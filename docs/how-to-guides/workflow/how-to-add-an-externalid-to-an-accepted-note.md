# How to add an externalId to an accepted note

An `externalId` is an external identifier (for example, a DOI) that can be attached to a Note. Once an externalId is added to an accepted submission, the note can be retrieved using that identifier instead of its OpenReview id.

externalIds are formatted as `prefix:identifier`, for example `doi:10.1609/aaai.v39i1.32013`. The supported prefixes are `doi`, `dblp` and `arxiv`.

### Add an externalId using the venue meta invitation

Program organizers can add an externalId to an accepted submission by posting a note edit using the venue meta invitation and signing with the venue id:

```python
venue_id = '<your venue id>'
meta_invitation_id = f'{venue_id}/-/Edit'

note_id = '<id of the accepted submission>'
doi_id = '<DOI of the paper>'

client.post_note_edit(
    invitation=meta_invitation_id,
    signatures=[venue_id],
    note=openreview.api.Note(
        id=note_id,
        external_id=f'doi:{doi_id}'
    )
)
```

Note that externalIds are unique across all notes: posting an externalId that already belongs to another note will return an error.

If you want to add DOIs to all of your accepted submissions, you can iterate over them. Accepted submissions are the ones where the `venueid` value is the venue id:

```python
venue_id = '<your venue id>'
meta_invitation_id = f'{venue_id}/-/Edit'

# a mapping from submission forum id to DOI
dois_by_id = {
    '<note id 1>': '<DOI 1>',
    '<note id 2>': '<DOI 2>'
}
accepted_submissions = client.get_all_notes(content={'venueid': venue_id})

for submission in accepted_submissions:
    doi_id = dois_by_id.get(submission.id)
    if doi_id:
        client.post_note_edit(
            invitation=meta_invitation_id,
            signatures=[venue_id],
            note=openreview.api.Note(
                id=submission.id,
                external_id=f'doi:{doi_id}'
            )
        )
```

### Retrieve a note by externalId

Once the edit is posted, the externalId is stored in the `externalIds` field of the note and can be used to retrieve it:

```python
notes = client.get_notes(external_id='doi:10.1609/aaai.v39i1.32013')
```
