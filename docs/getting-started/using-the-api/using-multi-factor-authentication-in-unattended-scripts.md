# Using multi-factor authentication in unattended scripts

If your profile has [multi-factor authentication](../creating-an-openreview-profile/setting-up-multi-factor-authentication.md) enabled, the Python client asks for a second verification step when it logs in. Whether you need this page depends on whether someone is there to answer it:

* **You run the script or notebook yourself:** nothing to change. The client asks in your terminal or notebook for the code from your authenticator app or your email, or opens your browser to confirm with a passkey, and then continues.
* **The script runs unattended:** nobody is there to answer, so the login fails with `MfaRequiredException`. This page shows how to set up an authenticator app so the script can generate the code itself.

Unattended login works with the **authenticator app** method only. Email codes and passkeys still need a person to complete them.

## How it works

An authenticator app generates a new 6-digit code every 30 seconds from a secret shared between you and OpenReview. Any program that knows the secret produces the same codes, so you can keep the secret in Google Authenticator on your phone **and** give it to your scripts, and both will work.

Your script generates the code with [PyOTP](https://pypi.org/project/PyOTP/) and hands it to the client through the `mfa_code_provider` parameter. The client never receives the secret itself, so you decide where it is stored.

## 1. Install PyOTP

```bash
pip install pyotp
```

## 2. Enable the authenticator app and get its secret

The **Password & Security** page on the website shows the setup secret only as a QR code. To use the secret in scripts you need it as text, so enable the authenticator app with the following script instead. Run it once, in a terminal.

```python
import base64

import openreview
import pyotp

client = openreview.api.OpenReviewClient(
    baseurl='https://api2.openreview.net',
    username=<your username>,
    password=<your password>
)

response = client.session.post(client.mfa_setup_totp_init_url, headers=client.headers)
response.raise_for_status()
setup = response.json()
secret = setup['secret']

with open('openreview-totp-qr.png', 'wb') as f:
    f.write(base64.b64decode(setup['qrCodeDataUrl'].split(',', 1)[1]))

response = client.session.post(client.mfa_setup_totp_verify_url, headers=client.headers, json={'code': pyotp.TOTP(secret).now()})
response.raise_for_status()

print(f'Authenticator app enabled. Secret: {secret}')
print('QR code saved to openreview-totp-qr.png')
recovery_codes = response.json().get('recoveryCodes')
if recovery_codes:
    print('Recovery codes:')
    print('\n'.join(recovery_codes))
```

When it finishes:

1. **Save the recovery codes.** They are printed only if your profile has no recovery codes yet (usually, when this is your first multi-factor method), and they cannot be shown again.
2. **Store the secret** somewhere safe, such as a password manager, your operating system's keychain, or your CI system's secret store. Never put it in a script or a Git repository.
3. **Optional:** scan `openreview-totp-qr.png` with Google Authenticator or another authenticator app to get the same codes on your phone.
4. **Delete `openreview-totp-qr.png`.** It contains the secret.

{% hint style="warning" %}
A profile has a single authenticator-app secret. Running this script again replaces it, and codes from the old secret stop working everywhere. That includes your phone, so scan the new QR code there too.
{% endhint %}

{% hint style="info" %}
If you already enabled the authenticator app on the website, the secret behind its QR code is not shown anywhere. Run the script above to replace it with one you can use in scripts.
{% endhint %}

## 3. Log in from your scripts

Pass a function that returns the current code as `mfa_code_provider`. This example reads the secret from the `OPENREVIEW_TOTP_SECRET` environment variable:

```python
import os

import openreview
import pyotp

client = openreview.api.OpenReviewClient(
    baseurl='https://api2.openreview.net',
    username=<your username>,
    password=<your password>,
    mfa_code_provider=lambda method: pyotp.TOTP(os.environ['OPENREVIEW_TOTP_SECRET']).now()
)
```

The API V1 client, `openreview.Client`, accepts the same parameter.

The function receives the name of the method, which is always `'totp'`, and must return the code as a string. You can get the code from anywhere: an environment variable, a secrets manager, or a password manager that can generate codes itself.

What the client does with it:

* The function is called only when your profile has the authenticator app enabled, and at most once per login.
* A wrong code is not retried. The login fails with an error that says how many attempts remain.
* If the function returns `None` or an empty string, the client falls back to the interactive prompt. With no terminal available, it raises `MfaRequiredException`.
* Errors raised inside the function reach your script unchanged.

{% hint style="warning" %}
After 5 incorrect codes in a row, verification is locked for 15 minutes. Codes depend on the current time, so keep the clock of the machine that runs your scripts synchronized (for example, with automatic time sync enabled).
{% endhint %}

## Keeping your account safe

Anyone who has both your password and your authenticator secret can sign in as you. Treat the secret like your password:

* Store it in a secrets manager or keychain, not in plain-text files or source code.
* Give it only to the machines that need it.
* If you think it has leaked, change your password and run the setup script again to replace the secret.
