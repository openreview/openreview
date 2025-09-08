# How to get email addresses

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



### Get Preferred Emails

Emails can be retrieved for users by including `with_preferred_emails` in the call to `get_profiles`. &#x20;

For more information on the params accepted for get\_profiles(), please see the [documentation](https://openreview-py.readthedocs.io/en/latest/api.html#openreview.tools.get_profiles).

<pre class="language-python"><code class="lang-python"><strong>venue_id = '' # For example, 'aclweb.org/ACL/ARR/2025/February'
</strong>preferred_emails_invitation_id = venue_id + '/-/Preferred_Emails'
profile_ids_or_emails = [] # For example, '~Firstame_Lastname1' or an email 'firstname@email.com' in a list of one or multiple

profiles = openreview.tools.get_profiles(client_v2, profile_ids_or_emails, with_preferred_emails=preferred_emails_invitation_id)
</code></pre>

For each profile you can then use `profile.get_preferred_email()`  to get the preferred email for the profile.

#### Example: How to get Author emails

In this example, the accepted submissions are being retrieved to get the `authorids`. The `authorids` are passed in `get_profiles()`  and the `preferred_name` ,`preferred_email` , submission number, and submission title are stored in `author_info`.

```python
venue_id = '' # For example, 'aclweb.org/ACL/ARR/2025/February'
preferred_emails_invitation_id = venue_id + '/-/Preferred_Emails'

all_accepted_authors = set()

accepted_submissions = client_v2.get_all_notes(content={'venueid':venue_id} )

for submission in accepted_submissions:
    for author in submission.content['authorids']['value']:
        all_accepted_authors.add(author)
        
profiles = openreview.tools.get_profiles(client_v2, list(all_accepted_authors), with_preferred_emails=preferred_emails_invitation_id)
```

Please contact us using the [Feedback form](https://openreview.net/contact) if you're having trouble getting the preferred emails for authors or if Publication Chairs need access to the preferred emails.
