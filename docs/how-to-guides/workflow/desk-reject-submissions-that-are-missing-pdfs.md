# Desk Reject Submissions that are Missing PDFs

PCs may want to programmatically desk-reject submissions that are missing PDF files once the submission deadline has passed.&#x20;

Make sure to replace \<venue\_id> with the venue ID of your conference. For example, "ICLR.cc/2024/Conference".

```python
venue_id = "<venue_id>"

submissions = client.get_all_notes(invitation=f'{venue_id}/-/Submission')
#gets the desk rejection name of the invitation
desk_rejection_name = client.get_group(venue_id).content['desk_rejection_name']['value']
    
# for each submission note that does not contain a pdf field, post a desk rejection note
for submission in submissions:
    if not submission.content.get('pdf', {}).get('value'):
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
