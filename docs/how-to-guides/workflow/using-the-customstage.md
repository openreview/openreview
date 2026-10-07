# Using the CustomStage

## Overview

The `CustomStage` creates a new form for a task that the default steps of your venue do not cover. The form can be a reply to a submission, review, meta review, rebuttal, or custom invitation, or a revision of an existing review, meta review, rebuttal, or custom invitation reply.

Once created, it appears in the timeline like any other step.

## How to create a CustomStage

1. [Install the openreview Python library](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md) and create an API 2 client with your Program Chair account.

```python
import datetime
import openreview

client = openreview.api.OpenReviewClient(
    baseurl='https://api2.openreview.net',
    username=<your username>,
    password=<your password>
)
```

2. Load your venue using its venue ID.

```python
venue = openreview.venue.helpers.get_venue(client, '<your venue id>')
```

3. Define the CustomStage and create it. For complete examples, see [Examples](using-the-customstage.md#examples).

```python
venue.custom_stage = openreview.stages.CustomStage(
    name='<Stage_Name>',
    reply_to=openreview.stages.CustomStage.ReplyTo.FORUM,
    source=openreview.stages.CustomStage.Source.ALL_SUBMISSIONS,
    ...
)
venue.create_custom_stage()
```

Once it is created, the stage appears as a step in the Workflow Timeline, named after the stage (for example, `Review_Revision` appears as **Review Revision**). From the timeline you can change its **Dates** and its **Form Fields**. To change any other setting, run `create_custom_stage()` again with the same `name` and the new values.

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2FV1e269RGExv1B7OkxWI0%2Fimage.png?alt=media&#x26;token=f719c9a5-14f8-4efc-a611-4973ed86f987" alt="A Review Revision step in the Workflow Timeline, created with the CustomStage, showing only its Dates and Form Fields controls."><figcaption><p>A CustomStage named Review_Revision as it appears in the Workflow Timeline.</p></figcaption></figure>

{% hint style="warning" %}
If you run `create_custom_stage()` again, pass the full `content` of the form. Fields of the existing form that are not in `content` are removed.
{% endhint %}

## Parameters

* `name`: The name of the stage, for example `'Review_Revision'`.
* `reply_to`: What the form replies to or revises. One of `CustomStage.ReplyTo.FORUM` (the submission), `WITHFORUM` (any note in the submission forum), `REVIEWS`, `METAREVIEWS`, or `REBUTTALS`, or the name of an invitation as a string, for example `'Official_Review'`, `'Meta_Review'`, `'Author_Rebuttal'`, or a custom invitation.
* `source`: Which submissions get the form. One of `CustomStage.Source.ALL_SUBMISSIONS`, `ACCEPTED_SUBMISSIONS`, `PUBLIC_SUBMISSIONS`, or `FLAGGED_SUBMISSIONS` (flagged for ethics review). You can also pass a dictionary, for example `{ 'venueid': '<your venue id>/Desk_Rejected_Submission' }`.
* `reply_type`: `CustomStage.ReplyType.REPLY` (default) posts a new note. `CustomStage.ReplyType.REVISION` edits the note in `reply_to`; only the author of that note can revise it. A revision cannot be used for submissions.
* `start_date`, `due_date`, `exp_date`: Python `datetime` objects for when the form opens, the deadline shown to users, and when the form closes. If `start_date` is not set, the form opens immediately. If `exp_date` is not set, the form closes 30 minutes after `due_date`.
* `invitees`: Who can post the reply, as a list of `CustomStage.Participants` values, for example `AUTHORS`, `REVIEWERS_ASSIGNED`, `AREA_CHAIRS_ASSIGNED`, or `REPLYTO_REPLYTO_SIGNATURES`.
* `readers`: Who can read the reply, as a list of `CustomStage.Participants` values, for example `PROGRAM_CHAIRS`, `SIGNATURES` (the person who posted it), or `AUTHORS`. Not used for revisions, which keep the readers of the note being revised.
* `content`: A dictionary with the fields of the form. See [Customizing Forms](../../getting-started/customizing-forms.md).
* `multi_reply`: Whether more than one reply can be posted with each form (for example, per submission when `reply_to` is `FORUM`). Default `False`, which allows a single reply.
* `notify_readers`: Whether to email the readers when a reply is posted. Default `False`. No emails are sent unless this is `True`, and no emails are sent when a reply is edited.
* `email_pcs`, `email_sacs`: Whether to also email the Program Chairs or the assigned Senior Area Chairs. Default `False`. They only take effect when `notify_readers` is `True`.
* `email_template`: A custom email body. It can use `{submission_number}`, `{submission_id}`, and `{note_id}`.
* `allow_de_anonymization`: Whether users can sign the reply with their profile instead of their anonymous ID, which may reveal their identity. Default `False`.

{% hint style="info" %}
To reply to or revise reviews, meta reviews, or rebuttals, pass the invitation name as `reply_to`, for example `reply_to='Official_Review'`.
{% endhint %}

## Examples

### Desk Rejection Challenge

Authors of desk-rejected submissions can post a challenge, visible only to the Program Chairs and to themselves.

```python
due_date = datetime.datetime(2026, 7, 1, 12, 0)

venue.custom_stage = openreview.stages.CustomStage(
    name='Desk_Rejection_Challenge',
    reply_to=openreview.stages.CustomStage.ReplyTo.FORUM,
    source={ 'venueid': '<your venue id>/Desk_Rejected_Submission' },
    due_date=due_date,
    exp_date=due_date + datetime.timedelta(days=1),
    invitees=[openreview.stages.CustomStage.Participants.AUTHORS],
    readers=[
        openreview.stages.CustomStage.Participants.PROGRAM_CHAIRS,
        openreview.stages.CustomStage.Participants.SIGNATURES
    ],
    content={
        'desk_reject_challenge': {
            'order': 1,
            'description': 'Explain why you think the desk-rejection was not appropriate.',
            'value': {
                'param': {
                    'type': 'string',
                    'input': 'textarea',
                    'maxLength': 5000
                }
            }
        }
    },
    notify_readers=True
)

venue.create_custom_stage()
```

### Review Revision

After the rebuttal period, reviewers can update their reviews. The revision only allows editing the fields in `content`: here it adds a `final_rating` and a `justification` to each review. To let reviewers also change fields of the original review, add those fields to `content`.

```python
start_date = datetime.datetime(2026, 6, 20, 0, 0)
due_date = datetime.datetime(2026, 6, 27, 0, 0)

venue.custom_stage = openreview.stages.CustomStage(
    name='Review_Revision',
    reply_to='Official_Review',
    source=openreview.stages.CustomStage.Source.ALL_SUBMISSIONS,
    reply_type=openreview.stages.CustomStage.ReplyType.REVISION,
    start_date=start_date,
    due_date=due_date,
    content={
        'final_rating': {
            'order': 20,
            'description': 'Your final rating after the rebuttal.',
            'value': {
                'param': {
                    'type': 'integer',
                    'enum': [
                        { 'value': 4, 'description': '4: Accept' },
                        { 'value': 3, 'description': '3: Weak Accept' },
                        { 'value': 2, 'description': '2: Weak Reject' },
                        { 'value': 1, 'description': '1: Reject' }
                    ],
                    'input': 'radio'
                }
            },
            'readers': [
                '<venue_id>/Program_Chairs',
                '<venue_id>/Submission${7/content/noteNumber/value}/Senior_Area_Chairs',
                '<venue_id>/Submission${7/content/noteNumber/value}/Area_Chairs',
                '${5/signatures}'
            ]
        },
        'justification': {
            'order': 21,
            'description': 'If necessary, provide a justification for your final rating.',
            'value': {
                'param': {
                    'type': 'string',
                    'maxLength': 2000,
                    'input': 'textarea',
                    'markdown': True,
                    'optional': True
                }
            }
        }
    }
)

venue.create_custom_stage()
```
