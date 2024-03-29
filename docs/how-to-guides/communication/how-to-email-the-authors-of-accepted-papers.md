# How to email the authors of accepted papers

### Through the UI&#x20;

Under the ‘Overview’ tab of the PC console for your venue, you will find a ‘Venue Roles’ section. Click on the ‘Accepted’ link next to ‘Authors’ to be taken to the Accepted Authors group. On this page, click 'Edit group'. You will then have the option to email members of the group.

### Through the Python Client

#### Single Blind Venues&#x20;

Since the Submissions do not contain the decisions, we first need to retrieve all the Decision notes, filter the accepted notes and use their forum ID to locate its corresponding Submission. We break down these steps below.

Retrieve Submissions and Decisions:

```python
submissions = client.get_all_notes(invitation = 'Your/Venue/ID/-/Submission', details='directReplies')
id_to_submission = {note.id: note for note in submissions}
all_decision_notes = [] 
for submission in submissions:
    for reply in submission.details["directReplies"]:
        for invitation in reply["invitations"]:
            if invitation.endswith("Decision"):
                all_decision_notes = all_decision_notes + [reply]
```

It is convenient to place all the submissions in a dictionary with their id as the key so that we can retrieve an accepted submission using its id.

We then filter the Decision notes that were accepted and use their forum ID to get the corresponding Submission:

```python
accepted_submissions = [id_to_submission[note["forum"]] for note in all_decision_notes if 'Accept' in note["content"]["decision"]["value"]]
```

You can then message the author ids of each accepted submission.&#x20;

```python
for submission in accepted_submissions: 
    subject = f'Message regarding Paper #{submission.number}'
    message = f'Hello, please go to your submission and do x, y, z. Find your submission here: https://openreview.net/forum?id={submission.forum}'
    recipients = [f'Your/Venue/ID/Paper{submission.number}/Authors']
    client.post_message(subject, recipients, message)
```
