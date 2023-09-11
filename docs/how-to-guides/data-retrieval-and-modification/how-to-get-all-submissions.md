# Getting all Submissions

### API 2 venues

Get all the submission notes of your venue regardless of their status (active, withdrawn or desk rejected). To do this you'll need to pass the submission invitation using get\_all\_notes().

```python
venue_group = client.get_group('Your/Venue/ID')
submission_name = venue_group.content['submission_name']['value']
submissions = client.get_all_notes(invitation=f'Your/Venue/ID/-/{submission_name}')
```



To only get "accepted" submissions, you'll need to query the notes by venueid.

```python
submissions = client.get_all_notes(content={'venueid':'Your/Venue/ID'} )
```

To get active submissions under review:

<pre class="language-python"><code class="lang-python"><strong>venue_group = client.get_group('Your/Venue/ID')
</strong>under_review_id = venue_group.content['submission_venue_id']['value']
submissions = client.get_all_notes(content={'venueid': under_review_id})
</code></pre>

To get withdrawn submissions:

```python
venue_group = client.get_group('Your/Venue/ID')
withdrawn_id = venue_group.content['withdrawn_venue_id']['value']
submissions = client.get_all_notes(content={'venueid': withdrawn_id})
```

To get desk-rejected submissions:

```python
venue_group = client.get_group('Your/Venue/ID')
desk_rejected_venue_id = venue_group.content['desk_rejected_venue_id']['value']
submissions = client.get_all_notes(content={'venueid': desk_rejected_venue_id})
```

Parameters you can use when [querying API 2 notes](https://docs.openreview.net/reference/api-v2/openapi-definition#notes).

### API 1 venues

To get all 'active' submissions for a double-blind venue **,** pass your venue's blind submission invitation into get\_all\_notes_._

```python
submissions = client.get_all_notes(
    invitation="Your/Venue/ID/-/Blind_Submission"
    )
```

To get all submissions for a double-blind venue regardless of their status (active, withdrawn or desk rejected), pass your venue's submission invitation to get\_all\_notes().

```python
submissions = client.get_all_notes(
    invitation="Your/Venue/ID/-/Submission"
    )
```

As a program organizer, to get only the "accepted" submissions for double-blind venues, query using the Blind submission invitation and include 'directReplies' and 'original' in the details.&#x20;

```python
# Double-blind venues

submissions = client.get_all_notes(invitation = 'Your/Venue/ID/-/Blind_Submission', details='directReplies,original')
blind_notes = {note.id: note for note in submissions}
all_decision_notes = [] 
for submission_id, submission in blind_notes.items(): 
        all_decision_notes = all_decision_notes + [reply for reply in submission.details["directReplies"] if reply["invitation"].endswith("Decision")]

accepted_submissions = []

for decision_note in all_decision_notes:
    if 'Accept' in decision_note["content"]['decision']:
        accepted_submissions.append(blind_notes[decision_note['forum']].details['original'])
```

As a program organizer, to get only the "accepted" submissions, query using the Submission invitation and include 'directReplies' in the details.

```python
# Single-blind venues

submissions = client.get_all_notes(invitation = 'Your/Venue/ID/-/Submission', details='directReplies')
notes = {note.id: note for note in submissions}
all_decision_notes = [] 
for submission_id, submission in notes.items(): 
        all_decision_notes = all_decision_notes + [reply for reply in submission.details["directReplies"] if reply["invitation"].endswith("Decision")]

accepted_submissions = []

for decision_note in all_decision_notes:
    if 'Accept' in decision_note["content"]['decision']:
        accepted_submissions.append(notes[decision_note['forum']])
```

Parameters you can use when [querying API 1 notes](https://api.openreview.net/notes).
