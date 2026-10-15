# Installing and Instantiating the Python client

1. You will need to install the **openreview-py** package.

```bash
pip install openreview-py
```

2\. Create a client object with your OpenReview credentials. If you do not yet have an OpenReview profile, [you will need to make one now](../creating-an-openreview-profile/signing-up-for-openreview.md).

```python
import openreview

# API V2
client = openreview.api.OpenReviewClient(
    baseurl='https://api2.openreview.net',
    username=<your username>,
    password=<your password>
)

# API V1
client_v1 = openreview.Client(
    baseurl='https://api.openreview.net',
    username=<your username>,
    password=<your password>
)
```

{% hint style="info" %}
If your profile has multi-factor authentication enabled, the client asks for a verification code in your terminal or notebook when it logs in. For scripts that run unattended, where nobody can enter the code, see [Using multi-factor authentication in unattended scripts](using-multi-factor-authentication-in-unattended-scripts.md).
{% endhint %}
