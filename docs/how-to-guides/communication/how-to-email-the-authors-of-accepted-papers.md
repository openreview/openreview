# How to email the authors of accepted papers

### Through the UI&#x20;

Under the ‘Overview’ tab of the PC console for your venue, you will find a ‘Venue Roles’ section. Click on the ‘Accepted’ link next to ‘Authors’ to be taken to the Accepted Authors group. On this page, click 'Edit group'. You will then have the option to email members of the group.

### Through the Python Client

#### Single Blind Venues&#x20;

Since the Submissions do not contain the decisions, we first need to retrieve all the Decision notes, filter the accepted notes and use their forum ID to locate its corresponding Submission. We break down these steps below.

Retrieve Submissions and Decisions:

```
submissions = list(openreview.tools.iterget_notes(client, invitation = 'Your/Venue/ID/-/Submission', details='directReplies'))
id_to_submission = {note.id: note for note in submissions}
all_decision_notes = [] 
for submission in submissions: 
    all_decision_notes = all_decision_notes + [reply for reply in submission.details["directReplies"] if reply["invitation"].endswith("Decision")]

```

It is convenient to place all the submissions in a dictionary with their id as the key so that we can retrieve an accepted submission using its id.

We then filter the Decision notes that were accepted and use their forum ID to get the corresponding Submission:

```
accepted_submissions = [id_to_submission[note["forum"]] for note in all_decision_notes if 'Accept' in note["content"]["decision"]]
```

You can then message the author ids of each accepted submission.&#x20;

```
for submission in accepted_submissions: 
    subject = f'Message regarding Paper #{submission.number}'
    message = f'Hello, please go to your submission and do x, y, z. Find your submission here: https://openreview.net/forum?id={submission.forum}'
    recipients = [f'Your/Venue/ID/Paper{submission.number}/Authors']
    client.post_message(subject, recipients, message)
```

#### Double Blind Venues&#x20;

This is very similar to the previous example. The only difference is that we need to get the blind notes with the added details parameter to get the Submission.

Retrieve Submissions and Decisions:

```
submissions = openreview.tools.iterget_notes(client, invitation = 'Your/Venue/ID/-/Blind_Submission', details='directReplies')
blind_notes = {note.id: note for note in submissions}
all_decision_notes = [] 
for submission in blind_notes: 
    all_decision_notes = all_decision_notes + [reply for reply in submission.details["directReplies"] if reply["invitation"].endswith("Decision")]
```

We then filter the Decision notes that were accepted and use their forum ID to get the corresponding Submission:

```
accepted_submissions = [blind_notes[decision_note.forum].details['original'] for decision_note in all_decision_notes if 'Accept' in decision_note["content"]['decision']]
```

You can then message the author ids of each accepted submission.&#x20;

```
for submission in accepted_submissions: 
    subject = f'Message regarding Paper #{submission.number}'
    message = f'Hello, please go to your submission and do x, y, z. Find your submission here: https://openreview.net/forum?id={submission.forum}
    recipients = [f'Your/Venue/ID/Paper{submission.number}/Authors']
    client.post_message(subject, recipients, message)
```
