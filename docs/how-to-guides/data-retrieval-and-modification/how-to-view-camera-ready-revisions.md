# How to view Camera-Ready Revisions

If you enabled a Submission Revision Stage to allow authors to revise their submissions or submit Camera-Ready Revisions, the authors will have used these invitations to directly revise their existing submissions. This means that in order to see the final versions you can simply go to the forum of each submission. You can also click 'show revisions'  to see the revision history for each paper.&#x20;

If you need a way to see which authors have submitted camera-ready revisions, you will first need to [install and setup the python client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).

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
