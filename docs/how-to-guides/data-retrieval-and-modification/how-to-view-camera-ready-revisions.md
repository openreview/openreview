# How to view Camera-Ready Revisions

If you enabled a Submission Revision Stage to allow authors to revise their submissions or submit Camera-Ready Revisions, the authors will have used these invitations to directly revise their existing submissions. This means that in order to see the final versions you can simply go to the forum of each submission. You can also click 'show revisions'  to see the revision history for each paper.&#x20;

If you need a way to see which authors have submitted camera-ready revisions, you will first need to [install and setup the python client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md). If your venue is running on API 1 then follow instructions for API 1, and if it is running on API 2 then follow instructions for API 2.

#### API 2 venues

The first step is to [get all submissions](how-to-get-all-submissions.md) with a corresponding camera-ready invitation. The following example will be for "accepted" papers.&#x20;

Get all accepted papers. Make sure to replace `venue_id` with your specific venue ID, it will be a string.

```python
accepted_notes = client.get_all_notes(content={'venueid':venue_id} )
```

Check the notes to see if the Camera\_Ready\_Revision invitation is stored in note.invitations. This means the note was edited by the authors with the camera\_ready\_revision invitation. If the invitation you created for the camera-ready isn't named "Camera\_Ready\_Revision", please use the correct invitation for your venue. Again, replace `venue_id` with your own.

```python
camera_ready_notes = []
for note in accepted_notes:
    camera_ready_invitation = f'venue_id/Submission{note.number}/-/Camera_Ready_Revision'
    if camera_ready_invitation in note.invitations:
        camera_ready_notes.append(note)
```

You can check the `camera_ready_notes` list for the submissions that did submit their camera-ready draft.

#### API 1 venues

Retrieve all of the revision invitations for your venue. You will want to replace the super invitation with the submission revision invitation for your venue, which is your conference id + /-/ + the name you chose for your Submission Revision stage.&#x20;

```python
revision_invitations = list(openreview.tools.iterget_invitations(client, super = 'Your/Conference/ID/-/Camera_Ready_Revision', expired=True))
```

Create a dictionary mapping the submission number to each submission for your venue. Replace the submission invitation with that of your venue:&#x20;

```python
submissions_by_number = {p.number: p for p in client.get_all_notes(invitation = 'Your/Conference/ID/-/Submission')}
```

Iterate through all of the camera-ready revision invitations and for each one, try to get the revisions made under that invitation. If there aren't any, don't add them to the dictionary revisions by forum. Finally, print the number of revision invitations vs the number of actually completed revisions so that you know how many papers are missing revisions.

```python
revisions_by_forum = {}
for invitation in revision_invitations: 
    number = int((invitation.id.split('/-/')[0]).split('Paper')[1])
    submission = submissions_by_number[number]
    references = client.get_references(referent = submission.id, invitation = invitation.id)
    if references:
        revisions_by_forum[submission.forum] = references
    else:
        print(f'no revisions for {number}')
print(f'Number of revision invitations: {len(revision_invitations)}')
print(f'Number of revisions submitted: {len(revisions_by_forum.keys())}')
```
