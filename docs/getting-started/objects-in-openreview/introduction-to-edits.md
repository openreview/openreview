# Introduction to Edits

See also:

* [Technical Reference for Edits](../../reference/api-v2/entities/edit/)
* [How to create, edit, and delete notes](../../how-to-guides/workflow/how-to-create-change-and-delete-notes.md)

In OpenReview, an **Edit** is the object used for creating, modifying, or deleting entities (such as Notes, Groups, Edges, etc.). Edits are posted and then **applied via inference rules** defined by invitations.

See the[ API reference ](../../reference/api-v2/entities/edit/)for a comprehensive description of the fields of the Edit object. Most commonly, you will interact with the following key fields:

* `invitation`: ID of the invitation that governs the operation
* `signatures`: Who is performing the edit
* `note`: The data of the note (new content or modifications)
* `ddate`: A timestamp to **delete** a note (in ms time). The default value for this field is `None`

### Edits and Inference

Inference refers to **how the system processes edits**. The target invitation defines **what kind of edits are allowed**, **what metadata is required**, and **how the system responds** to them (e.g., assigning readers, creating edges, triggering workflows).&#x20;

* Invitations can specify a `process` function that determines side effects.
* The system checks permissions, field formats, and required values using the invitation’s rules.

Below is an example of an edit to a review. The Edit object includes the metadata for the Edit, and nested within is the metadata and content for the [Note](introduction-to-notes.md) to be updated.&#x20;

{% hint style="info" %}
There are two different reader fields here - one for the Edit (outer layer) and one within the Note object which are the readers of the Note to update. When updating the readers, it is helpful to double-check that the correct values are being update.
{% endhint %}

```
{'cdate': 1750702362496,
 'content': None,
 'ddate': None,
 'domain': 'TC/2024/Conference',
 'group': None,
 'id': 'a9tHyMftoj',
 'invitation': 'TC/2024/Conference/Submission1/-/Official_Review',
 'invitations': None,
 'nonreaders': ['TC/2024/Conference/Submission1/Authors'],
 'note': Note(id = 'vz9qfSrB0P',
              number = 1,
              cdate = None,
              pdate = None,
              odate = None,
              mdate = None,
              tcdate = None,
              tmdate = None,
              ddate = None,
              content = {'title': {'value': 'Good paper, accept'}, 
                        'review': {'value': 'Excellent paper, accept'}, 
                        'rating': {'value': 8}, 
                        'confidence': {'value': 5}},
              forum = 'lOns9yOgZf',
              replyto = 'lOns9yOgZf',
              readers = ['TC/2024/Conference/Program_Chairs', 'TC/2024/Conference/Submission1/Senior_Area_Chairs', 'TC/2024/Conference/Submission1/Area_Chairs', 'TC/2024/Conference/Submission1/Reviewer_4iHs'],
              nonreaders = ['TC/2024/Conference/Submission1/Authors'],
              signatures = ['TC/2024/Conference/Submission1/Reviewer_4iHs'],
              writers = ['TC/2024/Conference', 'TC/2024/Conference/Submission1/Reviewer_4iHs'],
              details = None,
              invitations = None,parent_invitations = None,domain = None,license = None),
 'readers': ['TC/2024/Conference/Program_Chairs',
             'TC/2024/Conference/Submission1/Senior_Area_Chairs',
             'TC/2024/Conference/Submission1/Area_Chairs',
             'TC/2024/Conference/Submission1/Reviewer_4iHs'],
 'signatures': ['TC/2024/Conference/Submission1/Reviewer_4iHs'],
 'tauthor': 'openreview.net',
 'tcdate': 1750702362496,
 'tmdate': 1750702362496,
 'writers': ['TC/2024/Conference']}
```

### How to Get Edits

To get all edits, use the following code:&#x20;

```python
note_id = ''
edits = client_v2.get_note_edits(note_id)
edits
```

