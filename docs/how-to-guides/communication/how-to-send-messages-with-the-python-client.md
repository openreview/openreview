# How to send messages with the python client

You can send messages through OpenReview using the python client [post\_message](https://openreview-py.readthedocs.io/en/latest/api.html?highlight=post\_message#openreview.Client.post\_message) function. You will first need to [install and setup the python client](https://openreview-py.readthedocs.io/en/latest/how\_to\_setup.html). Your recipients could be a list of OpenReview [profile IDs](../../getting-started/creating-an-openreview-profile/finding-your-profile-id.md), a list of email addresses, or an OpenReview group ID. **The important thing is that whichever ID you use for the recipient, it has to be a member of a group you are a writer of.** This property is called the `parentGroup`. This is how OpenReview gives permission to organizers to send emails. If you do not know what the corresponding group of your recipient is, you can find it with the following query:

[https://api.openreview.net/groups?members=\<email or profile ID>\&details=writable\&select=id,details.writable](https://api.openreview.net/groups?members=\~Bum\_Jun\_Kim1\&details=writable\&select=id,details.writable)

This will return a JSON with the property `id` and `details.writable`. If a group has `details.writable=true`, that group `id` can be used as your `parentGroup`.

To send a message to all of your venue's authors, or example:&#x20;

```
subject = 'Your message subject'
message = 'Hello, please go to your submission and do x, y, and z.'
recipients = ['Your/Conference/ID/Authors']
parentGroup = f'Your/Conference/ID/Paper{submission_number}/Authors'
client.post_message(subject, recipients, message, parentGroup=parentGroup)
```

If you wanted to include a link to each author's paper in the email, you could instead iterate through each submission and send an email with the papers' authorids fields as recipients. If your venue is single blind, replace /-/Blind\_Submission with /-/Submission:&#x20;

```
submissions = client.get_all_notes(invitation = 'Your/Conference/Id/-/Blind_Submission')
for submission in submissions: 
    subject = f'Message regarding Paper #{submission.number}'
    message = f'Hello, please go to your submission and do x, y, z. Find your submission here: https://openreview.net/forum?id={submission.forum}'
    recipients = submission.content['authorids']
    parentGroup = f'Your/Conference/ID/Paper{submission.number}/Authors'
    client.post_message(subject, recipients, message, parentGroup=parentGroup)
```

If your venue is set up to use API 1, and the submission deadline has not passed then you can send a message to submission authors by doing the following:&#x20;

```
submissions = client.get_all_notes(invitation = 'Your/Conference/Id/-/Submission')
for submission in submissions: 
    subject = f'Message regarding Paper #{submission.number}'
    message = f'Hello, please go to your submission and do x, y, z. Find your submission here: https://openreview.net/forum?id={submission.forum}'
    recipients = submission.content['authorids']
    client.post_message(subject, recipients, message)
```

{% hint style="info" %}
If you don't know what API version you're using, you can check this by viewing your [venue request form](../../getting-started/hosting-a-venue-on-openreview/navigating-your-venue-pages.md), look for the field "API Version". If there is no such field on your venue request form you are using API 1 by default.
{% endhint %}
