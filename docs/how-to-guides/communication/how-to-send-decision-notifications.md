# How to Send Decision Notifications



If you selected not to send decision notifications to authors automatically, you will need to post notifications using the OpenReview python client. You can do this using the following guide:&#x20;

1. You will first need to [install and setup the python client](https://openreview-py.readthedocs.io/en/latest/how\_to\_setup.html).
2. Define a function containing a message for each type of decision you used. If you used 'conditional accept', 'accept', and 'reject', for example, you might write this for the conditional accept:&#x20;

```
def get_conditional_accept_message(submission):
    return f'''Dear {{{{firstname}}}},

We are delighted to inform you that your submission {submission.number}: 
{submission.content['title']} has been conditionally accepted. Congratulations!

You can find the final reviews for your paper on the submission's page in OpenReview:
https://openreview.net/forum?id={submission.id}

Best, 

Conference Program Chairs

'''
```

3\. Then you should retrieve all of the submissions for your venue. For a double blind venue, do the following:&#x20;

```
submissions= client.get_all_notes(invitation='Your/Venue/ID/-/Blind_Submission', details='original'))
```

For a single blind venue, do this instead:&#x20;

```
submissions=client.get_all_notes(invitation='Your/Venue/ID/-/Submission'))
```

4\. Next, you can retrieve all of the decision notes and create a dictionary mapping the forum of each submission to its associated decision.

```
decision_by_forum={ d.forum: d for d in openreview.tools.iterget_notes(client, invitation='Your/Venue/ID/Paper.*/-/Decision'))}
```

5\. Finally, for each submission, get the associated decision and send a message by calling the respective function defined for that decision.&#x20;

```

for submission in tqdm(submissions):
    decision = decision_by_forum.get(submission.id)
    if decision:
        message=None
        if 'Accept' == decision.content['decision']:
            message = get_accept_message(submission)
        if 'Conditional Accept' == decision.content['decision']:
            message = get_conditional_accept_message(submission)
        if 'Reject' == decision.content['decision']:
            message = get_reject_message(submission)
        if message:
            r=client.post_message(
                subject=f'[Conference 2022] Decision notification for submission {submission.number}', 
                message=message, 
                recipients=submission.details['original']['content']['authorids'])
        else:
            print('Message not found', submission.id)
    else:
        print('Decision not found', submission.id)
```
