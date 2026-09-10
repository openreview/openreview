# Introduction to Notes

See also:

* [How-to guide for notes](../../how-to-guides/workflow/how-to-create-change-and-delete-notes.md)
* [Technical Reference for Notes](../../reference/api-v2/entities/note/)
* [Guide for getting notes (submissions, reviews, etc)](../../how-to-guides/data-retrieval-and-modification/how-to-get-all-notes-for-submissions-reviews-rebuttals-etc.md)
* [Guide for exporting submission and other information](../../how-to-guides/data-retrieval-and-modification/how-to-loop-through-accepted-papers-and-print-the-authors-and-their-affiliations.md)

### What is a note?

The **Note** is the core data object in OpenReview that is used to represent content such as submissions, reviews, comments, official responses, and more. Notes are created and governed by **invitations**, which define their schema and permissions.

For detailed information about the Note object and its fields, see [here](../../reference/api-v2/entities/note/). A few fields that are particularly important for interacting with notes are `id`, `invitation` ,`readers` , and `content`.

* `id` - unique identifier for the Note
* `invitation` is used to identify the schema used to generate and validate the note. In practice, this is most commonly used for identifying all notes for example identifying all Reviews or all Rebuttals.
* `readers` identifies which groups can retrieve the Note. Individual fields inside `content` can carry their own `readers` as well, see [Readers of a Note and readers of a field](introduction-to-notes.md#readers-of-a-note-and-readers-of-a-field)
* `content` is a dictionary that contains all of the fields of a note.

### Notes, Replies, and Forums

A **Reply** is a specific type of Note that is **linked to another Note**—such as a comment on a submission or a review in response to a paper.

**`id`**

* The unique identifier of a specific Note.
* Every Note, whether standalone or a reply, has its own `id`.

**`forum`**

* The `forum` field indicates the **top-level Note thread** the current Note belongs to.
* For a submission, `forum == id`, since it starts its own thread.
* For a reply, `forum` is the `id` of the original submission Note it belongs to.

**`replyto`**

* The `replyto` field indicates the **direct parent** of the current Note.
* A top-level Note (like a submission) has `replyto=None`.
* A review or comment will have `replyto` set to the `id` of the Note it is directly responding to (could be a submission or another comment).

In OpenReview, typically Submissions are the top-level Note, and other Notes posted in relationship to the Submission is a reply. The example below shows a forum page for an example submission. In this case, the Submission is the top-level note, the Official Review is a reply to the Submission, and the Official Comment is a reply to the Official Comment.

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2FTc4rtkSKZoHY7Q6SDw7E%2FScreenshot%202025-06-09%20at%201.34.03%E2%80%AFPM.png?alt=media&#x26;token=fa8cafe7-3840-4900-8678-cb0807f9e1e5" alt=""><figcaption></figcaption></figure>

The data structure of the above forum is as below:

```
{'cdate': 1749500872124,
 'content': {'abstract': {'value': 'This is an abstract 1'},
             'authorids': {'readers': ['TC/2024/Conference',
                                       'TC/2024/Conference/Submission1/Authors'],
                           'value': ['~SomeFirstName_User1',
                                     'author@mail.com',
                                     'andrew1@amazon.com']},
             'authors': {'readers': ['TC/2024/Conference',
                                     'TC/2024/Conference/Submission1/Authors'],
                         'value': ['SomeFirstName User',
                                   'First Last',
                                   'Andrew Mc']},
             'keywords': {'value': ['machine learning', 'nlp']},
             'paperhash': {'readers': ['TC/2024/Conference',
                                       'TC/2024/Conference/Submission1/Authors'],
                           'value': 'user|paper_title_1'},
             'pdf': {'value': '/pdf/pppppppppppppppppppppppppppppppppppppppp.pdf'},
             'title': {'value': 'Paper title 1'},
             'venue': {'value': 'TC 2024 Conference Submission'},
             'venueid': {'value': 'TC/2024/Conference/Submission'}},
 'ddate': None,
 
 'domain': 'TC/2024/Conference',
 'forum': 'WEIzUBsDX6',
 'id': 'WEIzUBsDX6',
 'invitations': ['TC/2024/Conference/-/Submission',
                 'TC/2024/Conference/-/Post_Submission'],
 'license': 'CC BY-SA 4.0',
 'mdate': 1749500934613,
 'nonreaders': None,
 'number': 1,
 'odate': None,
 'parent_invitations': None,
 'pdate': None,
 'readers': ['TC/2024/Conference',
             'TC/2024/Conference/Submission1/Senior_Area_Chairs',
             'TC/2024/Conference/Submission1/Area_Chairs',
             'TC/2024/Conference/Submission1/Reviewers',
             'TC/2024/Conference/Submission1/Authors'],
 'replyto': None,
 'signatures': ['TC/2024/Conference/Submission1/Authors'],
 'tcdate': 1749500872124,
 'tmdate': 1749500934613,
 'writers': ['TC/2024/Conference', 'TC/2024/Conference/Submission1/Authors']},
 'details': {'replies': [{'cdate': 1749501156614,
                          'content': {'confidence': {'value': 5},
                                      'rating': {'value': 10},
                                      'review': {'value': 'Excellent paper, '
                                                          'accept'},
                                      'title': {'value': 'Good paper, accept'}},
                          'domain': 'TC/2024/Conference',
                          'forum': 'WEIzUBsDX6',
                          'id': 'OvggXPAvUK',
                          'invitations': ['TC/2024/Conference/Submission1/-/Official_Review'],
                          'license': 'CC BY 4.0',
                          'mdate': 1749501156614,
                          'nonreaders': ['TC/2024/Conference/Submission1/Authors'],
                          'number': 1,
                          'parentInvitations': 'TC/2024/Conference/-/Official_Review',
                          'readers': ['TC/2024/Conference/Program_Chairs',
                                      'TC/2024/Conference/Submission1/Senior_Area_Chairs',
                                      'TC/2024/Conference/Submission1/Area_Chairs',
                                      'TC/2024/Conference/Submission1/Reviewer_Nxce'],
                          'replyto': 'WEIzUBsDX6',
                          'signatures': ['TC/2024/Conference/Submission1/Reviewer_Nxce'],
                          'tcdate': 1749501156614,
                          'tmdate': 1749501156614,
                          'version': 2,
                          'writers': ['TC/2024/Conference',
                                      'TC/2024/Conference/Submission1/Reviewer_Nxce']},
                         {'cdate': 1749501221454,
                          'content': {'comment': {'value': 'Thank you for your '
                                                           'review!'}},
                          'domain': 'TC/2024/Conference',
                          'forum': 'WEIzUBsDX6',
                          'id': '9mJiUp5fsF',
                          'invitations': ['TC/2024/Conference/Submission1/-/Official_Comment'],
                          'license': 'CC BY 4.0',
                          'mdate': 1749501221454,
                          'number': 1,
                          'parentInvitations': 'TC/2024/Conference/-/Official_Comment',
                          'readers': ['TC/2024/Conference/Program_Chairs',
                                      'TC/2024/Conference/Submission1/Senior_Area_Chairs',
                                      'TC/2024/Conference/Submission1/Area_Chairs',
                                      'TC/2024/Conference/Submission1/Reviewer_Nxce'],
                          'replyto': 'OvggXPAvUK',
                          'signatures': ['TC/2024/Conference/Submission1/Senior_Area_Chairs'],
                          'tcdate': 1749501221454,
                          'tmdate': 1749501221454,
                          'version': 2,
                          'writers': ['TC/2024/Conference',
                                      'TC/2024/Conference/Submission1/Senior_Area_Chairs']}]},
}
```

### Readers of a Note and readers of a field

`readers` appears at two different levels, and the distinction decides what a user actually gets back when they query a Note.

**Note level.** The `readers` field at the top of the Note lists the groups that can retrieve the Note at all. A user who is not in any of these groups will not see the Note in the UI and will not get it back from the API.

**Field level.** Any field inside `content` can carry its own `readers` list. Users outside that list still retrieve the Note, but the field is omitted from the `content` they receive. This is how a submission can be visible to reviewers while its `authors` and `authorids` stay hidden from them.

In the forum example above, the submission is readable by the venue, the Senior Area Chairs, the Area Chairs, the Reviewers, and the Authors, but `authors`, `authorids`, and `paperhash` each narrow their own readers to the venue and the paper's Authors:

```
'readers': ['TC/2024/Conference',
            'TC/2024/Conference/Submission1/Senior_Area_Chairs',
            'TC/2024/Conference/Submission1/Area_Chairs',
            'TC/2024/Conference/Submission1/Reviewers',
            'TC/2024/Conference/Submission1/Authors'],
'content': {'title': {'value': 'Paper title 1'},
            'authors': {'readers': ['TC/2024/Conference',
                                    'TC/2024/Conference/Submission1/Authors'],
                        'value': ['SomeFirstName User', 'First Last', 'Andrew Mc']}}
```

A Reviewer querying this Note receives it with `title` but without `authors`.

Two consequences are worth keeping in mind:

* Field readers can only **narrow** access. A group that cannot read the Note cannot see any of its fields, no matter what the field's `readers` say.
* A field with no `readers` of its own is visible to everyone who can read the Note.

Which level to change depends on what you are trying to do. To change who can see a whole submission, review, or comment, change the readers of the Note, usually by running the relevant stage. To hide one field from part of the committee, set the readers of that field. See [How to hide/reveal fields](../../how-to-guides/submissions-comments-reviews-and-decisions/how-to-hide-reveal-fields.md) for the procedure, and [Setting the Readers of a Field](../customizing-forms.md#setting-the-readers-of-a-field) for how to declare field readers when customizing a form.

### Creating, Editing, and Deleting Notes (Inference)

Operations on Notes are performed via [**Edits**](introduction-to-edits.md)**.** The first Edit posted is for the creation of the note, any revisions of the notes can be made by posting Edits with the relevant fields, and deleting a Note object is done by posting and Edit adding a date to the `ddate` field. See below for examples of each operation.

To create/modify a Note, you post an Edit with the necessary fields:

* **`invitation`**: The ID of the Invitation governing the Note. This is used to ensure that the Edit is well-formed and conforms to the specifications in the invitation.
* **`signatures`**: List of group IDs authorizing the Edit.
* **`note`**: The Note object containing fields like `readers`, `writers`, `signatures`, and `content` . All notes need to have these four fields. `readers`, `writers`, and `signatures` are all lists of strings consisting of the IDs of groups or profiles. `content` is a dictionary containing the fields that will show up in the final Note. the dictionary needs to include only fields specified in the invitation. Between the notes and all edits of the note, all required fields must be present in the note.

### Note Validation

Before the [Edit](introduction-to-edits.md) is posted, the content of both the Edit and the Note within is checked to ensure that it fits the parameters of the Invitation. In particular, the dictionary `content` needs to include only fields specified in the invitation. Between the notes and all edits of the note, all required fields must be present in the note.
