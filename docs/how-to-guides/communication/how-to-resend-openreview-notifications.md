# How to Resend OpenReview Notifications

[First, create the openreview-py client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).

The [resend\_emails](https://github.com/openreview/openreview-py/blob/9e76b22752926082ba5fa1459a9eccb716e0ae88/openreview/tools.py#L1840) tool in the openreview-py library will allow program organizers to resend email notifications. To do this you'll need the requestID, this is the request made to send the emails.  For example, if Decisions were sent to authors via the Post Decision Stage there would be a single request ID for each authors group. The request ID is found in the message logs \***I want to be more specific here**\*

`"requestId": "XXXXXXXXXX"`

Once you have the request ID, determine who you want to resend the emails to. If you want to resend the email to all the members of the group the emails were originally sent to, include the group. If you only want to resend the email to one person in the group, use that email.

```python
# Resending emails to all group members
resend_emails(client, request_id='XXXXXXXXXX', groups=['My/Venue/Id/Submission14/Authors'])

# Resend an email to a single group member
resend_emails(client, request_id='XXXXXXXXXX', groups=['myemail@email.com'])
```



