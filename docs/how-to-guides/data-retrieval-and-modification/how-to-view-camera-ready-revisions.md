# How to view Camera-Ready Revisions

If you enabled a Submission Revision Stage to allow authors to revise their submissions or submit Camera-Ready Revisions, the authors will have used these invitations to directly revise their existing submissions. This means that in order to see the final versions you can simply go to the forum of each submission. You can also click 'show revisions'  to see the revision history for each paper.&#x20;

If you need a way to see which authors have submitted camera-ready revisions, you can use the following code:&#x20;

1. You will first need to [install and setup the python client](https://openreview-py.readthedocs.io/en/latest/how\_to\_setup.html).
2. Next, retrieve all of the revision invitations for your venue. You will want to replace the regex with the submission revision invitation for your venue, which is your conference id + Paper.\*/-/ + the name you chose for your Submission Revision stage.&#x20;

```
revision_invitations = list(openreview.tools.iterget_invitations(client, super = 'Your/Conference/ID/-/Camera_Ready_Revision'))
```

2\. Create a dictionary mapping the submission number to each submission for your venue. Replace the submission invitation with that of your venue:&#x20;

```
submissions_by_number = {p.number: p for p in client.get_all_notes(invitation = 'Your/Conference/ID/-/Submission')}
```

Iterate through all of the camera-ready revision invitations and for each one, try to get the revisions made under that invitation. If there aren't any, add them to the dictionary revisions by forum. Finally, print the number of revision invitations vs the number of actually completed revisions so that you know how many papers are missing revisions.

```
revisions_by_forum = {}
for invitation in revision_invitations: 
    number = int((invitation.id.split('/-/')[0]).split('Paper')[1])
    submission = submissions_by_number[number]
    try:
        references = client.get_references(referent = submission.id, invitation = invitation.id)
        revisions_by_forum[submission.forum] = references
    except: 
        print(f'no revisions for {number}')
print(f'Number of revision invitations: {len(revision_invitations)}')
print(f'Number of revisions submitted: {len(revisions_by_forum.keys())}')
```
