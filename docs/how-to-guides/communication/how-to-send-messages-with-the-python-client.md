# How to send messages with the python client

You can send messages through OpenReview using the python client [post\_message](https://openreview-py.readthedocs.io/en/latest/api.html#openreview.api.OpenReviewClient.post\_message) function. You will first need to [install and setup the python client](https://openreview-py.readthedocs.io/en/latest/how\_to\_setup.html).

To send an email you need:

* **subject**: Subject line of the email
* **message**: Text of the email
* **recipients**: List of OpenReview [profile IDs](../../getting-started/creating-an-openreview-profile/finding-your-profile-id.md), email addresses, or OpenReview group IDs.
* **invitation**: An invitation that allows you to send an email to your recipients (this is usually the  [meta invitation](../../reference/api-v2/entities/invitation/types-and-structure.md#meta-invitations) invitation for venue organizers).

Below are some examples of common cases for sending emails.

#### Sending a message to all of your venue's authors

```python
subject = 'Your message subject'
message = 'Hello, please go to your submission and do x, y, and z.'
recipients = ['Your/Conference/ID/Authors']
invitation = f'Your/Conference/ID/-/Edit'
client.post_message(subject, recipients, message, invitation=invitation)
```

If you wanted to include a link to each author's paper in the email, you could instead iterate through each submission and send an email with the papers' authorids fields as recipients:&#x20;

```python
submissions = client.get_all_notes(invitation = 'Your/Conference/Id/-/Submission')
for submission in submissions: 
    subject = f'Message regarding Paper #{submission.number}'
    message = f'Hello, please go to your submission and do x, y, z. Find your submission here: https://openreview.net/forum?id={submission.forum}'
    recipients = submission.content['authorids']
    invitation = f'Your/Conference/ID/-/Edit'
    client.post_message(subject, recipients, message, invitation=invitation)
```

#### Sending an email to the reviewers of submission 123

```python
subject = 'Your message subject'
message = 'Hello, please go to your review and do x, y, and z.'
recipients = [f'Your/Conference/ID/Submission123/Reviewers']
invitation = f'Your/Conference/ID/-/Edit'
client.post_message(subject, recipients, message, invitation=invitation)
```
