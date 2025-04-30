# How to post/delete an Official Review using Python

## Requirements

[Install and instantiate the python client](https://docs.openreview.net/getting-started/using-the-api/installing-and-instantiating-the-python-client).

## Posting a review

To post a review, you will need to post a [Note Edit](https://docs.openreview.net/reference/api-v2/entities/edit) by calling [`post_note_edit`](https://github.com/openreview/openreview-py/blob/791983ca7794cdf53affce9d0e12c6a8bcb1dc36/openreview/api/client.py#L2275) . It requires at least 3 fields, `invitation` , `signatures` , and `note`:

* [**Invitation**](https://docs.openreview.net/reference/api-v2/entities/invitation): The ID of the paper's specific Official Review invitation. Any time you post data you need to specify an invitation.
* [**Signatures**](https://docs.openreview.net/reference/api-v2/entities/edit/fields#signatures): The ID of the user creating the edit. Reviews are usually anonymous so we sign them with the reviewer's anon group ID to hide their identity. The anon group contains the user's profile ID as a member. Only the member of the group has permissions to use the anon group to sign a Note Edit. The reviewer anon group ID is in the format of: `<venue_id>/Submission<number>/<reviewer name>_` .
  * To find the anon ID for a reviewer of a paper, call [`get_groups`](https://github.com/openreview/openreview-py/blob/791983ca7794cdf53affce9d0e12c6a8bcb1dc36/openreview/api/client.py#L836) . The following will look for all groups that  match the specified prefix and signatory:

```python
anon_groups = client.get_groups(prefix='<venue_id>/Submission<number>/Reviewer_', signatory='<Reviewer profile ID>')
anon_group_id = anon_groups[0].id
# Result: <venue_id>/Submission<number>/Reviewer_ABCD>
```

* [**Note**](https://docs.openreview.net/reference/api-v2/entities/note): This is the object that will contain the contents of your review inside the Note.content. To know what fields are required in the Note content, look at what is specified in the /-/Official\_Review invitation.&#x20;

The full `post_note_edit` call will look like this:

```python
review_note = client.post_note_edit(
    invitation='<venue_id>/Submission<number>/-/Official_Review',
    signatures=[anon_group_id],
    note=openreview.api.Note(
        content={
            'title': { 'value': 'Review for Paper 1' },
            'review': { 'value': 'good paper' },
            'rating': { 'value': 10 },
            'confidence': { 'value': 5 }
        }
    )
)
```

To view the note in the UI, go to: openreview.net/forum?id=\<review\_note.forum>

## Deleting a review

To delete a review, you need to post a Note Edit with `post_note_edit` and pass 2 fields to the Note, the `id` and `ddate` :&#x20;

* [**ID**](https://docs.openreview.net/reference/api-v2/entities/note/fields#id)**:** The ID of the Note you wish to delete. The simplest way to find the ID of a specific Note is to find the review in the UI, copy the URL, and get the ID from the `noteId` param.
* [**Ddate**](https://docs.openreview.net/reference/api-v2/entities/note/fields#ddate)**:** The deletion date is a unix timestamp in milliseconds.

For the `invitation` passed in the Edit, there are 2 options:

* `Submission<number>/-/Official_Review` : Can be used by the reviewer or by venue organizers. If this is used, you _must_ pass the content of the Note.
* `venue_id/-/Edit` : Can be used only by venue organizers. If this is used, you only need to pass the Note ID and ddate.

The full `post_note_edit` calls will look like this:

```python
# With the /-/Official_Review invitation
review_note = client.post_note_edit(
    invitation='<venue_id>/Submission<number>/-/Official_Review',
    signatures=[anon_group_id],
    note=openreview.api.Note(
        id='<review_note.id>',
        ddate=openreview.tools.datetime_millis(datetime.datetime.now()),
        content={
            'title': { 'value': 'Review for Paper 1' },
            'review': { 'value': 'good paper' },
            'rating': { 'value': 10 },
            'confidence': { 'value': 5 }
        }
    )
)

# With the meta invitation
review_note = client.post_note_edit(
    invitation='<venue_id>/-/Edit',
    signatures=[anon_group_id],
    note=openreview.api.Note(
        id='<review_note.id>',
        ddate=openreview.tools.datetime_millis(datetime.datetime.now())
    )
)
```
