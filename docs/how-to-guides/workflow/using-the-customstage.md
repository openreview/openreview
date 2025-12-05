# Using the CustomStage

## Overview

The `CustomStage` is a feature that allows users to define and manage custom stages within the venue management process. This stage can be tailored to fit specific requirements that are not covered by the predefined stages. The CustomStage creates a modifiable Invitation that enables users to submit or revise any submission reply such as reviews, meta reviews, rebuttals or a reply already created by the CustomStage.

## How to Use CustomStage

### 1. Configuring the CustomStage

To configure the CustomStage, you'll need to use the python client.

First, [install the openreview python library](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md) and create a client. **This client should be created with API 1**.

Next, provide your request form id to create the venue object.

```python
request_form_id = '<ADD-REQUEST-FORM-ID-HERE>'
venue = openreview.get_conference(client_v1, request_form_id)
```

The params passed to the Custom Stage are listed below: &#x20;

* `name`: The name of the custom stage or the Invitation you're creating (ex, "Review\_Revision").
* `reply_to`: Specifies what the replies are related to. [Possible values for ReplyTo](https://github.com/openreview/openreview-py/blob/c0b2f2a0665eeb1c3667a58380507fbba6ab34cc/openreview/stages/venue_stages.py#L1493).
* `source`: Specifies the submission source, whether it's all submissions or a subset. [Possible values for Source](https://github.com/openreview/openreview-py/blob/c0b2f2a0665eeb1c3667a58380507fbba6ab34cc/openreview/stages/venue_stages.py#L1487). Besides the possible values, you can use the expert mode where you can [specify the status of the submissions](using-the-customstage.md#id-6.-opt-in-release-for-rejected-submissions) you want to enable the custom stage.&#x20;
* `reply_type`: Specifies whether the the type is a reply or a revision to a note. [Possible values for ReplyType](https://github.com/openreview/openreview-py/blob/c0b2f2a0665eeb1c3667a58380507fbba6ab34cc/openreview/stages/venue_stages.py#L1500).
* `start_date`: Sets the activation/start date for the invitation. Accepts timestamp in milliseconds.
* `due_date`: Sets the displayed due date for the invitation. Accepts timestamp in milliseconds.
* `exp_date`: Sets the hard deadline for the invitation. Accepts timestamp in milliseconds.
* `invitees`: A list of invitees for the custom stage. Invitees are the ones being asked to complete the task, for example, for review revisions, the invitees would be the Assigned Reviewers.
* `readers`: A list of readers for the custom stage. The readers are who you want to be able to read the reply but not a revision. If you're creating a stage for a revision and you add readers it will result in an error. The revision will use the readers of the note that is being revised.
* `content`: A dictionary defining the content fields and their descriptions. The content field contains the fields that are needed to build the form. Please scroll down to see example use cases.
* `multi_reply`: Boolean indicating if multiple replies are allowed. Default is False.&#x20;
* `email_pcs`: Boolean indicating if program chairs should be emailed. Default is False.&#x20;
* `email_sacs`: Boolean indicating if senior area chairs should be emailed. Default is False.&#x20;
* `notify_readers`: Boolean indicating if readers should be notified. Default is False.&#x20;
* `email_template`: Template for the email notification.&#x20;
* `allow_de_anonymization`: Boolean indicating if de-anonymization is allowed. If set to True, this means a user with an anon ID will be able to sign with their profile ID instead, which would likely reveal their identity to the readers of that note. Default is False.

Please see the openreview.py repo for more details on the [CustomStage class](https://github.com/openreview/openreview-py/blob/7c5bcb5099dc543cc82a273b6ad99e6ec2fde500/openreview/stages/venue_stages.py#L1423).

### 3. Creating form fields

Check out this [guide to customizing forms](../../getting-started/customizing-forms.md).

### 4. Add CustomStage to the Venue

Once you have defined the CustomStage, you need to add it to the venue's workflow. This can be done by running: `venue.create_custom_stage()`

Creating the custom stage will not add a new stage button to the venue configuration, you can instead access the created invitation from the Related Invitations in the Venue Group if you want to see the invitation in the UI.

### 5. Manage CustomStage

You can manage the `CustomStage` by updating its parameters and running it with the python client or you can modify them directly in the created Invitation. The most common modifications made after the Invitation is posted with the `CustomStage` are date changes, updating readers, and changing `content` fields.

## Example Usage

### 1. Making Date Changes

To make changes to the `start_date`, `due_date`, and `end_date` , run the customStage with the new dates.

```python
venue.custom_stage = openreview.stages.CustomStage(name='<Insert-Invitation-Name>',
            reply_to=openreview.stages.CustomStage.ReplyTo.REVIEWS,
            source=openreview.stages.CustomStage.Source.ALL_SUBMISSIONS,
            reply_type=openreview.stages.CustomStage.ReplyType.REVISION,
            start_date=start,
            due_date=due_date,
            exp_date=exp_date
            )

venue.create_custom_stage()
```

### 2. Review Revision

Often following the rebuttal/discussion period, PCs will give reviewers the opportunity to update their reviews based on author feedback. The CustomStage allows a revision invitation to be created so PCs have more control over what reviewers can edit. The downside of reopening the review stage is it would allow reviewers to submit new reviews or edit all fields. The revision invitation will limit editing to specific fields as well as add new ones. Some PCs have found it helpful to create a new rating field for the final rating because they want to more easily compare the original rating with the final rating.

{% hint style="info" %}
If a new rating field is added, you can update the PC Console to use the new field in the review stats. This can be done from the Review Stage in the Review Rating Field Name setting.
{% endhint %}

```python
request_form_id = ''
venue = openreview.get_conference(client, request_form_id, setup=False)

start = datetime.datetime(year=2024,month=4,day=20,hour=,minute=0,second=0)
due_date = datetime.datetime(year=2024,month=6,day=20,hour=0,minute=0,second=0)
exp_date = datetime.datetime(year=2024,month=6,day=21,hour=0,minute=0,second=0)
```

The start, due\_date, and exp\_date are timestamps in milliseconds which can be achieved using the [datetime python library](https://docs.python.org/3/library/datetime.html), like in this example.

The review\_revision\_content variable contains a dictionary of the fields you want modified or created by the Review\_Revision invitation. In this example, the final\_recommendation and justification fields are added to the original review and they are the only editable fields. If you want reviewers to be able to edit fields created in the original review, add them to the revision content.&#x20;

<pre class="language-python"><code class="lang-python">review_revision_content = {
<strong>    "final_recommendation": {
</strong>            "value": {
              "param": {
                "type": "integer",
                "enum": [
                  {
                    "value": 4,
                    "description": "Accept"
                  },
                  {
                    "value": 3,
                    "description": "Weak Accept"
                  },
                  {
                    "value": 2,
                    "description": "Weak Reject"
                  },
                  {
                    "value": 1,
                    "description": "Reject"
                  }
                ],
                "input": "radio"
              }
            }
    },
    "justification": {
        "value": {
          "param": {
            "type": "string",
            "minLength": 1,
            "maxLength": 2000,
            "input": "textarea",
            "markdown": True,
            "optional": True
          }
        },  
        "description": "If necessary, provide a justification for your final recommendation."
      }
}
</code></pre>

Create the Review Revision, naming the invitation Review\_Revision. In the example, we've selected this invitation to include all submissions, the replyType is a Revision, and we are inviting the assigned reviewers to edit their reviews. The dates and content are passed. The readers were not included in this example because it will include the readers of the Review note. All other params can use the default value.

```python
venue.custom_stage = openreview.stages.CustomStage(name='Review_Revision',
            reply_to=openreview.stages.CustomStage.ReplyTo.REVIEWS,
            source=openreview.stages.CustomStage.Source.ALL_SUBMISSIONS,
            reply_type=openreview.stages.CustomStage.ReplyType.REVISION,
            invitees=[openreview.stages.CustomStage.Participants.REVIEWERS_ASSIGNED],
            start_date=start,
            due_date=due_date,
            exp_date=exp_date,
            content=review_revision_content)

venue.create_custom_stage()
```

Once the invitation is posted, it's listed in the Related Invitations of the main venue group.

### 3. Replies to Rebuttals

Program organizers may want Reviewers to respond directly to the Author's Rebuttals, but may not want them to use the Official Comment invitations. To create a reply with the CustomStage, follow the example below.

Create the venue object.

```python
request_form_id = ''

venue = openreview.helpers.get_conference(client, request_form_id, setup=False)
```

Set the dates and time. If you want the stage to open immediately, leave out the start date.

```python
start = datetime.datetime(year=2024,month=4,day=20,hour=,minute=0,second=0)
due_date = datetime.datetime(year=2024,month=6,day=20,hour=0,minute=0,second=0)
exp_date = datetime.datetime(year=2024,month=6,day=21,hour=0,minute=0,second=0)
```

The start, due\_date, and exp\_date are timestamps in milliseconds which can be achieved using the [datetime python library](https://docs.python.org/3/library/datetime.html), like in this example.

Define the content.

```python
content={
        "comment": {
            "order": 2,
            "description": "Leave a comment to the authors",
            "value": {
                "param": {
                    "maxLength": 5000,
                    "type": "string",
                    "input": "textarea",
                    "optional": True,
                    "deletable": True,
                    "markdown": True
                }
            }
        }
    }
```

When creating the stage in this example, REPLYTO\_REPLYTO\_SIGNATURES allows the author of the review to reply to directly to the rebuttal. Readers are also limited to reviewers who submitted their reviews and the authors. PCs are alway default readers. notify\_readers is True because we want the authors to be notified when the rebuttal comment is posted.

```python
venue.custom_stage = openreview.stages.CustomStage(name='Rebuttal_Comment',
    reply_to=openreview.stages.CustomStage.ReplyTo.REBUTTALS,
    source=openreview.stages.CustomStage.Source.ALL_SUBMISSIONS,
    due_date=due_date,
    exp_date=due_date + datetime.timedelta(minutes=30),
    invitees=[openreview.stages.CustomStage.Participants.REPLYTO_REPLYTO_SIGNATURES],
    readers=[openreview.stages.CustomStage.Participants.REVIEWERS_SUBMITTED, openreview.stages.CustomStage.Participants.AUTHORS],
    content=content,
    notify_readers=True,
    email_sacs=False
    )

venue.create_custom_stage()
```

### 4. Review Rating by Authors

By default a Review Rating Stage is provided in the venue request form upon deployment. This stage allows for Area Chairs to rate the quality of a review by posting a rating note to a review as a reply. PCs have expressed interest in authors being able to rate the reviews as well. To enable this for authors, the CustomStage can be used to create this invitation.

```python
request_form_id = ''
venue = openreview.get_conference(client, request_form_id, setup=False)
```

The start, due\_date, and exp\_date are timestamps in milliseconds which can be achieved using the [datetime python library](https://docs.python.org/3/library/datetime.html), like in this example.

```python
start = datetime.datetime(year=2024,month=4,day=20,hour=,minute=0,second=0)
due_date = datetime.datetime(year=2024,month=6,day=20,hour=0,minute=0,second=0)
exp_date = datetime.datetime(year=2024,month=6,day=21,hour=0,minute=0,second=0)
```

The content field accepts a dictionary of fields the invitation uses to build the form. Normally, "review\_quality" is the field we provide upon request, but PCs can customize the fields based on their needs.

<pre class="language-python"><code class="lang-python"><strong>content = {
</strong>        'review_quality': {
            'order': 1,
            'description': 'How helpful is this review?',
            'value': {
                'param': {
                    'type': 'integer',
                    'input': 'radio',
                    'enum': [
                        {
                            "value": 0,
                            "description": "0: below expectations"
                        },
                        {
                            "value": 1,
                            "description": "1: meets expectations"
                        },
                        {
                            "value": 2,
                            "description": "2: exceeds expectations"
                        }
                    ]
                }
            }
        }
    }
</code></pre>

Create the custom stage, and invite the authors as participants.

```python
venue.custom_stage = openreview.stages.CustomStage(name='Author_Review_Rating',
            reply_to=openreview.stages.CustomStage.ReplyTo.REVIEWS,
            source=openreview.stages.CustomStage.Source.ALL_SUBMISSIONS,
            due_date=due_date,
            exp_date=exp_date,
            invitees=[openreview.stages.CustomStage.Participants.AUTHORS],
            readers=[openreview.stages.CustomStage.Participants.PROGRAM_CHAIRS],
            content=content,
            allow_de_anonymization=True)

venue.create_custom_stage()
```

### 5. Post a note as a Reply to the Submission Forum

For this use case, the organizers of a venue want Authors of Accepted Submissions to post a note as a Reply to their own Submission Forum, much like Official Reviews, Meta Reviews and Comments are posted as replies to a submission forum. The note the authors are posting indicates they give their consent for their submission and reviews to be public or to indicate they want to keep this information private.

First create the venue object.

```python
request_form_id = ''
venue = openreview.get_conference(client, request_form_id, setup=False)
```

Set the period of time authors have to accomplish this task. The start, due\_date, and exp\_date are timestamps in milliseconds which can be achieved using the [datetime python library](https://docs.python.org/3/library/datetime.html), like in this example.

```python
start = datetime.datetime(year=2024,month=6,day=28,hour=0,minute=0,second=0)
due_date = datetime.datetime(year=2024,month=7,day=12,hour=12,minute=0,second=0)
exp_date = datetime.datetime(year=2024,month=7,day=12,hour=12,minute=30,second=0)
```

Specify the content for the consent form field.&#x20;

```python
consent_content = {
          "public_or_private": {
            "value": {
              "param": {
                "type": "string",
                "enum": [
                    "I want my submission and reviews to be public.",
                    "I want my submission to remain private."
                ],
                "input": "radio"
              }
            },
            "order": 2,
            "description": "Authors of accepted papers have the option to make their submission and reviews public or leave the submission private. Please indicate your choice. All submission authors will be able to have a say, and if all but one of the authors indicates that they want the submission public then it will stay private."
          }
        }
```

Create the customStage, inviting the authors as participants and creating invitations only for Accepted Submissions.

```python
venue.custom_stage = openreview.stages.CustomStage(name='Release_Consent',
            reply_to=openreview.stages.CustomStage.ReplyTo.FORUM,
            source=openreview.stages.CustomStage.Source.ACCEPTED_SUBMISSIONS,
            reply_type=openreview.stages.CustomStage.ReplyType.REPLY,
            invitees=[openreview.stages.CustomStage.Participants.AUTHORS],
            start_date=start,
            due_date=due_date,
            exp_date=exp_date,
            content=consent_content)

venue.create_custom_stage()
```

### 6. Opt in Release for rejected submissions

In this case, you can specify a dictionary in the source parameter to indicate that you want to enable this invitations for rejected submission only.&#x20;

```
venue.custom_stage = openreview.stages.CustomStage(name='Opt_In_Public_Release',
    reply_to=openreview.stages.CustomStage.ReplyTo.FORUM,
    source={ 'venueid': ['PRL/2023/ICAPS', 'PRL/2023/ICAPS/Submission', 'PRL/2023/ICAPS/Rejected_Submission'], 'with_decision_accept': False },
    due_date=due_date,
    exp_date=due_date + datetime.timedelta(days=1),
    invitees=[openreview.stages.CustomStage.Participants.AUTHORS],
    readers=[openreview.stages.CustomStage.Participants.PROGRAM_CHAIRS, openreview.stages.CustomStage.Participants.SIGNATURES],
    content={
        'opt_in_public_release': {
            'order': 1,
            'description': 'Check the option to agree to release your submisison to the public.',
            'value': {
                'param': {
                    'type': 'string',
                    'input': 'checkbox',
                    'enum': ['I and my co-authors agree to release our submission to the public.']
                }
            }
        }
    },
    notify_readers=True,
    email_sacs=False)
```
