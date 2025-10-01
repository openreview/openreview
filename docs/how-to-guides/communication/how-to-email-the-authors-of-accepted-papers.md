# How to email the authors of accepted submissions

### Using the UI&#x20;

Under the ‘Overview’ tab of the PC console for your venue, you will find a ‘Venue Roles’ section. Click on the ‘Accepted’ link next to ‘Authors’ to be taken to the Accepted Authors group. On this page, click 'Edit group'. You will then have the option to email members of the group.

This option will work for use cases that do not need to customize messages. If you want to customize messages or have more control over who the recipient is, then please use the python client.

### Using the Python Client

PCs can utilize the python client to send customized messages or to a more controlled recipient group. Three different use cases are outlined below. **All use cases assume that PCs have already run the Post Decision Stage.**

#### I want to email authors of all accepted submissions:

First, you will need to [get all of the accepted submissions](../data-retrieval-and-modification/how-to-get-all-notes-for-submissions-reviews-rebuttals-etc.md#quickstart-getting-all-submissions) for your venue.

Second, loop through the submissions and for each submission send an email using the `authorids` in the submission.content. This message will be sent to all the authors of the submission.

```python
submissions = client.get_all_notes(content={'venueid':'Your/Venue/ID'})

for submission in submissions:
    subject = f'Message regarding Submission #{submission.number}'
    message = f'Hello, please go to your submission and do x, y, z. Find your submission here: https://openreview.net/forum?id={submission.forum}'
    recipients = submission.content['authorids']['value']
    client.post_message(subject, recipients, message, invitation='Your/Venue/ID/-/Edit')
```

#### I only want to email the corresponding author:

Unless you added a field to the submission form that required authors to indicate the corresponding author, OpenReview does not indicate which author in the list is the corresponding author.

If you want to email the author listed first in the submission, you can do this:

```python
for submission in submissions:
    subject = f'Message regarding Submission #{submission.number}'
    message = f'Hello, please go to your submission and do x, y, z. Find your submission here: https://openreview.net/forum?id={submission.forum}'
    recipients = submission.content['authorids']['value'][0]
    client.post_message(subject, recipients, message, invitation='Your/Venue/ID/-/Edit')
```

#### I only want to email accepted submissions with a specific decision:

To start, get all the accepted submissions for your venue, like in the sections above, except this time include the param `details='replies'` .&#x20;

```
submissions = client.get_all_notes(content={'venueid':'Your/Venue/ID'}, details='replies' )
```

Stored in the submission.details retrieve the replies that contain the Decision invitation.

Retrieve the decision notes for a specific decision. For this example, I will use 'Accept (Proceedings)', please change this to match the specific value in the decision note:

```python
proceeding_decision_notes = [] 
for submission in submissions:
    for reply in submission.details["replies"]:
        for invitation in reply["invitations"]:
            if (invitation.endswith("Decision")) and ('Accept (Proceedings)' in reply["content"]["decision"]["value"]):
                proceeding_decision_notes = proceeding_decision_notes + [reply]
                #print(reply["content"]["decision"]["value"])

len(proceeding_decision_notes) # check the number of items in the list and see if it matches the stats in the PC console
```

Now that the decision notes have been filtered by the decision, we need the corresponding submission note to get the submission information.

```python
# create a dictionary to map submission ids to submission objects
submission_dict = {submission.id: submission for submission in submissions}

proceedings_submissions = []

# iterate through the proceeding_decision_notes and find the corresponding submission
for note in proceeding_decision_notes:
    submission_id = note['forum']
    if submission_id in submission_dict:  # check if the submission_id exists in the dictionary
        proceedings_submissions.append(submission_dict[submission_id])  # add the corresponding submission to the new list
```

You can then message the author ids of each accepted submission either using the SubmissionX/Authors group or the authorids like in the previous examples.&#x20;

```python
for submission in proceedings_submissions: 
    subject = f'Message regarding Paper #{submission.number}'
    message = f'Hello, please go to your submission and do x, y, z. Find your submission here: https://openreview.net/forum?id={submission.forum}'
    recipients = [f'Your/Venue/ID/Paper{submission.number}/Authors']
    client.post_message(subject, recipients, message, invitation='Your/Venue/ID/-/Edit')
```
