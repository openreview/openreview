# Desk Reject Submissions that are Missing PDFs

PCs may want to programmatically desk-reject submissions that are missing PDF files once the submission deadline has passed.&#x20;

Make sure to replace \<venue\_id> with the venue ID of your conference. For example, "ICLR.cc/2024/Conference".

```python
// API 2

venue_id = "<venue_id>"

submissions = client.get_all_notes(invitation=f'{venue_id}/-/Submission')
#gets the desk rejection name of the invitation
desk_rejection_name = client.get_group("<venue_id>").content['desk_rejection_name']['value']
    
# for each submission note that does not contain a pdf field, post a desk rejection note
for submission in submissions:
    if 'pdf' not in submission.content:
        desk_reject_note = client.post_note_edit(
                    invitation=f'{venue_id}/Submission{submission.number}/-/{desk_rejection_name}',
                    signatures=[f'{venue_id}/Program_Chairs'],
                    note=openreview.api.Note(
                        content = {
                        'desk_reject_comments': { 'value': 'No PDF.' }
                        }
                    )
                )
        print(submission.number, "is desk rejected")
```

```python
// API 1

venue_id = "<venue_id>"

submissions = client.get_all_notes(invitation=f'{venue_id}/-/Blind_Submission', details='original')
    
# for each submission note that does not contain a pdf field, post a desk rejection note
for submission in submissions:
    if 'pdf' not in submission.details['original']['content']:
        desk_reject_note = client.post_note(openreview.Note(
                    forum=submission.id,
                    replyto=submission.id,
                    invitation=f'{venue_id}/Paper{submission.number}/-/Desk_Reject',
                    readers = [
                        venue_id,
                        f'{venue_id}/Paper{submission.number}/Authors',
                        f'{venue_id}/Paper{submission.number}/Reviewers',
                        f'{venue_id}/Paper{submission.number}/Area_Chairs',
                        f'{venue_id}/Program_Chairs'],
                    writers = [f'{venue_id}', f'{venue_id}/Program_Chairs'],
                    signatures = [f'{venue_id}/Program_Chairs'],
                    content = {
                        'title': 'Submission Desk Rejected by Program Chairs',
                        'desk_reject_comments': 'No PDF.'
                    }
                ))
        print(desk_reject_note.id)
```

For API 1, make sure that the /-/Desk\_Reject invitation ID matches the one for your venue. You can find the invitation by going to [https://openreview.net/group/edit?id=](https://openreview.net/group/edit?id=)\<venue\_id>.&#x20;

The "readers" field needs to be adjusted to match the values of the Desk Reject invitation in order to post the desk reject note.
