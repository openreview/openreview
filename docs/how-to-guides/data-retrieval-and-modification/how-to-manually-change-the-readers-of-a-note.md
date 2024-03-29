# How to manually change the readers of a note

First you'll need to query the API to get the notes. You can find directions on getting different notes (submissions, reviews, metareviews, decisions) listed under [Data Retrieval and Modification](./). When you have the note IDs, post an edit for each note.

```
client.post_note_edit(
    invitation = 'Your/Venue/ID/-/Edit',
    readers = ['Your/Venue/ID'],
    writers = ['Your/Venue/ID'],
    signatures = ['Your/Venue/ID'],
    note = openreview.api.Note(
        id = note_id,
        readers = [
            <add your reader groups here>
        ]
    )
)
```

The `readers` field is an array with Group ids that indicates who can retrieve the Note or see the Note in the UI. When deciding what the readers for the note need to be, keep in mind what the readers for the corresponding invitation are, what the anonymity of your venue is, and always double-check to make sure the right paper group is listed.

To set readers of the note to "public".

```
readers = [
    'everyone'
    ]
```

How to set the Program Chairs and the paper's Authors as readers. In this example, the X in SubmissionX represents the submission number. Please, check for the submission number and add the number in place of 'X'. The number is visible in the submission forum page or from the PC Console&#x20;

```
readers = [
    'Your/Venue/ID/Program_Chairs', 'Your/Venue/ID/SubmissionX/Authors'
    ]
```
