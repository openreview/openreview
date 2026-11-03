# How to rename or add a field on existing submissions

Some OpenReview features look for submission fields by name. If your submission form used a different name, you can rename the field after submissions have been posted: change the form, then edit every submission with the venue meta invitation.

## Field names that OpenReview recognizes

| Field name      | What depends on it                                                                                                                                                                                                             |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `subject_areas` | Submission search, including the search box in the Bidding Console. When your venue has a list of subject areas configured, the Bidding Console's subject area filter also reads this field.                                   |
| `keywords`      | Submission search, including the search box in the Bidding Console.                                                                                                                                                            |
| `track`         | Restricting a reviewer, area chair, or senior area chair group to one track for paper matching. See [How to support different tracks for a venue](how-to-have-different-tracks-or-types-of-submissions-for-a-single-venue.md). |

Search only covers a fixed set of fields, including `title`, `abstract`, `keywords` and `subject_areas`. A field with any other name, such as `research_area`, is not searched, so reviewers cannot find submissions by its value when they bid.

## Step 1: Rename the field in the submission form

Change the form first, so that authors who revise their submission after the change see the new field name.

1. Go to your Workflow Timeline.
2. Open the **Submission** step and select **Edit Fields**.
3. Add the field with its new name, using the same definition as the old field.
4. Remove the old field by setting it to `{ "delete": true }`.

For example, to rename `research_area` to `subject_areas`:

```json
{
  "subject_areas": {
    "order": 6,
    "description": "Primary research area of the submission",
    "value": {
      "param": {
        "type": "string",
        "enum": ["Computer Vision", "Natural Language Processing", "Theory"],
        "input": "select"
      }
    }
  },
  "research_area": {
    "delete": true
  }
}
```

If you only want to add a new field, include just the new field.

Changing the form does not change submissions that have already been posted. The next steps update them.

## Step 2: Get all submissions

Use the [Python client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md) and log in as a Program Chair:

```python
import openreview

client = openreview.api.OpenReviewClient(
    baseurl='https://api2.openreview.net',
    username='<your username>',
    password='<your password>'
)

venue_id = '<your venue id>'
venue_group = client.get_group(venue_id)
submission_invitation_id = venue_group.content['submission_id']['value']
meta_invitation_id = venue_group.content['meta_invitation_id']['value']

submissions = client.get_all_notes(invitation=submission_invitation_id)
```

The meta invitation is `<your venue id>/-/Edit`. It lets the venue edit any note in the venue, after the submission deadline as well.

## Step 3: Edit each submission with the meta invitation

For each submission, copy the old value into the new field and delete the old field:

```python
old_field = 'research_area'
new_field = 'subject_areas'

for submission in submissions:
    if old_field not in submission.content:
        continue
    new_value = { 'value': submission.content[old_field]['value'] }
    if 'readers' in submission.content[old_field]:
        new_value['readers'] = submission.content[old_field]['readers']
    client.post_note_edit(
        invitation=meta_invitation_id,
        signatures=[venue_id],
        note=openreview.api.Note(
            id=submission.id,
            content={
                new_field: new_value,
                old_field: { 'delete': True }
            }
        )
    )
```

The script copies the old field's `readers`, so a field you restricted keeps the same visibility under its new name.

To add a field instead of renaming one, leave out `old_field` and set the value you want:

```python
client.post_note_edit(
    invitation=meta_invitation_id,
    signatures=[venue_id],
    note=openreview.api.Note(
        id=submission.id,
        content={
            'track': { 'value': 'Main Track' }
        }
    )
)
```

{% hint style="warning" %}
Edits posted with the meta invitation skip the checks in your submission form, such as the allowed values of an `enum`. Make sure the values you post are ones the form allows.
{% endhint %}

## Check the result

Confirm that no submission still has the old field, and that the new field holds the values you expect:

```python
leftover = [s.number for s in client.get_all_notes(invitation=submission_invitation_id) if old_field in s.content]
print('Submissions still using the old field:', leftover)

vision_submissions = client.get_all_notes(
    invitation=submission_invitation_id,
    content={ new_field: 'Computer Vision' }
)
print('Computer Vision submissions:', [s.number for s in vision_submissions])
```

Search picks up the renamed field automatically, so reviewers can find these submissions by subject area when they bid.
