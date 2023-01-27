# Note

## Editing a note as an organizer

Organizers of the venue have access to an invitation that let them edit any part of the note, like add, edit or remove fields. This invitation is being called "meta invitation" and it usually ends with the word "Edit".

### Remove fields

Fields can be removed from the note using the value `{ delete: true}`

```
client.post_note_edit(
    invitation = 'ICML.cc/2023/Conference/-/Edit',
    readers = ['ICML.cc/2023/Conference'],
    writers = ['ICML.cc/2023/Conference'],
    signatures = ['ICML.cc/2023/Conference'],
    note = openreview.api.Note(
        id = note_id,
        content = {
            'supplementary_material': { 'delete': True }
        }
    )
)
```

Only the invitation ending with "Edit" can be used to edit any part of the note. Other invitations can be used but the property "deletable: true" must be declared in the field to be removed.

```
"supplementary_material": {
    "value": {
        "param": {
            "type": "file",
            "extensions": [
                "zip",
                "pdf",
                "tgz",
                "gz"
            ],
            "maxSize": 500,
            "optional": True,
            "deletable": True
        }
    },
    "description": "All supplementary material must be self-contained and zipped into a single file. Note that supplementary material will be visible to reviewers and the public throughout and after the review period, and ensure all material is anonymized. The maximum file size is 500MB.",
    "order": 8
}
```

Post a note edit using an invitation that is not meta:

```
client.post_note_edit(invitation='ICML.cc/2023/Conference/-/PC_Revision',
    signatures=['ICML.cc/2023/Conference/Program_Chairs'],
    note=openreview.api.Note(
        id = submission.id,
        content = {
            'title': { 'value': submission.content['title']['value'] + ' Version 2' },
            'abstract': submission.content['abstract'],
            'authorids': { 'value': submission.content['authorids']['value'] + ['melisa@yahoo.com'] },
            'authors': { 'value': submission.content['authors']['value'] + ['Melisa ICML'] },
            'keywords': submission.content['keywords'],
            'pdf': submission.content['pdf'],
            'supplementary_material': { 'value': { 'delete': True } },
            'financial_aid': { 'value': submission.content['financial_aid']['value'] },                    
        }
    ))
```
