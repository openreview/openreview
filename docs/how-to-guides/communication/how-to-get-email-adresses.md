# How to get email adresses

Emails in profiles are obfuscated for everyone including program chairs. However, to perform necessary actions, program chairs sometimes need to retrieve a user's email address to contact them.

To get the preferred email address from a profile, a program chair can do the following:

1. Install the [openreview python library and create a client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).
2. Impersonate the venue and get the preferred email of a profile.

<pre class="language-python"><code class="lang-python">venue_id = '' # For example, 'aclweb.org/ACL/ARR/2025/February'
profile_id = '' # For example, '~Firstame_Lastname1' or an email 'firstname@email.com'

client.impersonate(venue_id)
profiles = openreview.tools.get_profiles(client, [profile_id]) # you can also pass a list containing multiple profile ids or email addresses
<strong>user_email = profiles[0].get_preferred_email()
</strong></code></pre>

For more information on the params excepted for get\_profiles(), please see the [documentation](https://openreview-py.readthedocs.io/en/latest/api.html#openreview.tools.get_profiles).
