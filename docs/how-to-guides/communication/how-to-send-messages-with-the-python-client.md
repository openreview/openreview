# How to send messages with the python client

You can send messages through OpenReview using the python client [post\_message](https://openreview-py.readthedocs.io/en/latest/api.html?highlight=post\_message#openreview.Client.post\_message) function. You will first need to [install and setup the python client](https://openreview-py.readthedocs.io/en/latest/how\_to\_setup.html). Your recipients could be a list of OpenReview [profile IDs](../../getting-started/creating-an-openreview-profile/finding-your-profile-id.md), a list of email addresses, or an OpenReview group ID. To send a message to all of your venue's authors, or example:&#x20;

```
subject = 'Your message subject'
message = 'Hello, please go to your submission and do x, y, and z.'
recipients = ['Your/Conference/ID/Authors']
client.post_message(subject, recipients, message)
```

If you wanted to include a link to each author's paper in the email, you could instead iterate through each submission and send an email with the papers' authorids fields as recipients. If your venue is single blind, replace /-/Blind\_Submission with /-/Submission:&#x20;

```
submissions = list(openreview.tools.iterget_notes(client, invitation = 'Your/Conference/Id/-/Blind_Submission'))
for submission in submissions: 
    subject = f'Message regarding Paper #{submission.number}'
    message = f'Hello, please go to your submission and do x, y, z. Find your submission here: https://openreview.net/forum?id={submission.forum}'
    recipients = submission.content['authorids']
    client.post_message(subject, recipients, message)
```
