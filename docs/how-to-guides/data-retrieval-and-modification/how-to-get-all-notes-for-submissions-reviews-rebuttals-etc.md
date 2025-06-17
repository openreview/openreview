# How to Get all Notes (for submissions, reviews, rebuttals, etc)

Jump to:

* [Getting submissions](how-to-get-all-notes-for-submissions-reviews-rebuttals-etc.md#quickstart-getting-all-submissions)
* [Getting reviews, rebuttals, metareviews, decisions, comments](how-to-get-all-notes-for-submissions-reviews-rebuttals-etc.md#quickstart-getting-reviews-meta-reviews-comments-decisions-rebuttals)

See also:

* [Introduction to Notes](../../getting-started/using-the-api/objects-in-openreview/introduction-to-notes.md) for the structure of notes
* [Custom Submission Exports](how-to-loop-through-accepted-papers-and-print-the-authors-and-their-affiliations.md) for generating structured exports for submissions and other information

This covers getting notes in API v2. All current venues, and most past venues have migrated to API v2. However, if you want to get data from older venues, you may need to follow the [directions for API v1](data-retrieval-for-api-1-venues.md).&#x20;

## Getting Notes

The `get_all_notes()` method in API v2 is used to retrieve a list of notes that match a given filter. It's useful for batch processing or searching submissions, comments, reviews, etc.

Important parameters for this function include:&#x20;

| Parameter    | Description                                                              |
| ------------ | ------------------------------------------------------------------------ |
| `invitation` | Filter notes by invitation ID (e.g., submissions, reviews).              |
| `forum`      | Filter notes that are in the same forum (e.g., replies to a submission). |
| `replyto`    | Filter notes that are direct replies to a specific note.                 |
| `signatures` | Filter notes by signer(s).                                               |
| `content`    | Filter notes by certain content fields. E.g., `{'venueid': <venue_id>}`  |
| `details`    | Include additional notes, options are `'replies'`and `'directReplies'`   |

1. The most common way to get notes is to use the invitation used to create them. Invitations are schema used to define and govern notes. They typically have the structure described below:

```python
venue_id = "YOUR_VENUE_ID"
group_name = "Reviewers"#optional #represent spaces with _
stage_name = "Registration" #represent spaces with _

invitation_id = f"{venue_id}/{group_name}/-/{stage_name}"

```

2. Once you have the invitation, you can get all notes with that invitation:&#x20;

```python
all_notes = client.get_all_notes(invitation=invitation_id)
```

3. Then, you can iterate through each of the note objects and access their data and content. in the `value` field. To get PDF attachments, you would use the `get_attachment()` function:&#x20;

```python
for note in notes:
    if(note.content.get("pdf",{}).get('value')):
        f = client_v2.get_attachment(field_name='pdf', id=note.id)
        with open(f'submission{note.number}.pdf','wb') as op: 
            op.write(f)
```

### Quickstart: Getting all submissions

To get all submissions, regardless of status:&#x20;

```python
client_v2 = #openreview client with your credentials
venue_id = "<VENUE_ID>"
venue_group = client_v2.get_group(venue_id)
submission_name = venue_group.content['submission_name']['value']
submissions = client_v2.get_all_notes(invitation=f'{venue_id}/-/{submission_name}')
```

Note, the <kbd>`venueid`</kbd> field in the content of the note is used to differentiate between accepted, withdrawn, rejected, and desk rejected submissions. Accepted submissions use the original venue id:

```python
accepted_submissions = client.get_all_notes(content={'venueid':venue_id} )
```

All other statuses can be used by finding the appropriate ID name in the venue group:

* Papers under review: `submission_venue_id`&#x20;
* Withdrawn papers: `withdrawn_venue_id`&#x20;
* Desk Rejected papers: `desk_rejected_venue_id`

```python
status_key_to_query = 'submission_venue_id'
status_id = venue_group.content[status_key_to_query]['value'] # withdrawn_venue_id
submissions_with_status = client.get_all_notes(content={'venueid': status_id})
```

One special case is Camera Ready submissions. Since Camera Ready submissions are revisions to the original submission note, we need to check the `invitations`  field for the Camera Ready Invitation:

```python
accepted_submissions = client.get_all_notes(content={'venueid':venue_id} )
camera_ready_notes = []
for note in accepted_submissions:
    camera_ready_invitation = f'{venue_id}/Submission{note.number}/-/Camera_Ready_Revision'
    if camera_ready_invitation in note.invitations:
        camera_ready_notes.append(note)
```

## Getting Replies

When you're working with OpenReview, you might want to retrieve all the reviews, rebuttals, comments, and decisions for submissions. At first, it might seem like you can just call `get_all_notes` , but there is a catch: while these are notes, they are posted with specific per-paper invitations in the format of `f"{venue_id}/Submission{submission_number}/Review"` , rather than a general invitation for all Reviews. In fact, these are notes that are posted as replies to submission notes, so we can use the `details` field of `get_all_notes` to help identify the correct note, then filter for the desired notes (example below)

### Quickstart: Getting Reviews, Meta Reviews, Comments, Decisions, Rebuttals

<pre class="language-python"><code class="lang-python">venue_id = "&#x3C;YOUR_VENUE_ID>"
reply_type = "Official_Review" #also: "Meta_Review","Official_Comment", "Decision", "Rebuttal" etc.
submissions = client_v2.get_all_notes(invitation=f'{venue_id}/-/{submission_name}',details = 'replies')
<strong>replies = [reply for submission in submissions for reply in submission.details['replies'] if reply_type in reply['invitation']]
</strong></code></pre>

