# Exercise: Posting LLM generated reviews

## Setup:

1. Complete [Prerequisites](prerequisites.md)
2. [Add submissions](../../how-to-guides/workflow/how-to-create-change-and-delete-notes.md) (make sure this is on the dev site)
3. Open the [Review Stage](../../reference/stages/review-stage.md) (step 17 in the [Example Workflow](../conferences.md))

## Posting LLM Generated Reviews Overview

In some cases, a venue may want to use LLMs to post reviews. This exercise assumes the venue already has access to an LLM that generates the review and covers how to post them as replies to submissions.

* Create a `venue_id/-/AI_Review` invitation using the [Custom Stage](../../how-to-guides/workflow/using-the-customstage.md).
* **(Optional)** Create an AI Reviewer group to sign the AI Reviews.
  *   Venues may want this to make it more clear that it's an AI Review, for example:

      <figure><img src="../../.gitbook/assets/ai-review-note-heading (1).png" alt=""><figcaption></figcaption></figure>
  * If you choose to do this, you can either create:
    * One overall AI Reviewer group (`venue_id/AI_Reviewer`) to sign all the notes
    * Or create an AI Reviewer group per paper (`venue_id/SubmissionXX/AI_Reviewer`)
  * Otherwise, the notes will be signed with the Program Chairs group.
* Write a script to iterate through all submissions and post the AI Review to each paper.

### 1. Create the AI Review invitation

You can use the [`CustomStage`](../../how-to-guides/workflow/using-the-customstage.md) to create the `venue_id/-/AI_Review` invitation and you will post your LLM generated reviews using this invitation.

The invitees can be the PCs and you can configure the other settings however you need (e.g. form content, readers, etc.).

### 2. (Optional) Create an AI reviewer group and update the AI Review invitation

#### Creating the AI reviewer group(s)

If you don't want to sign the AI reviews using the PC group, you can create AI Reviewer group(s). Otherwise you can skip this step.

The following creates a single AI Reviewer group you can use to sign all the reviews:

```python
client.post_group_edit(
    invitation='<venue_id>/-/Edit',
    signatures=['<venue_id>'],
    group=openreview.api.Group(
        id = '<venue_id>/AI_Reviewer',
        readers = ['<venue_id>'],
        signatures = ['<venue_id>'],
        writers = ['<venue_id>'],
        signatories = ['<venue_id>'],
    )
)
```

Alternatively, you can loop through [all submissions](../../how-to-guides/data-retrieval-and-modification/how-to-get-all-notes-for-submissions-reviews-rebuttals-etc.md#quickstart-getting-all-submissions) and create an AI Reviewer group for each paper. Note the change in the `id` format. The `venue_id/SubmissionXX` group contains all the groups for that paper, including the assigned reviewers, ACs, and now the AI Reviewer.

```python
for sub in submissions:
    client.post_group_edit(
        invitation='<venue_id>/-/Edit',
        signatures=['<venue_id>'],
        group=openreview.api.Group(
            id = '<venue_id>/Submission<sub.number>/AI_Reviewer',
            readers = ['<venue_id>'],
            signatures = ['<venue_id>'],
            writers = ['<venue_id>'],
            signatories = ['<venue_id>'],
        )
    )
```

**Check your work:** Go to one of the submission groups for the venue and check for your new reviewer group. (`https://openreview.net/group/edit?id=<venue_id>/Submission<number>`)

#### Updating the AI Review invitation

If you decide to use AI Reviewer groups, you will also need to update the AI Review invitation to allow you to sign the notes using the AI Reviewer group. The signature is different depending on whether you created 1 AI Reviewer group or a group for each paper.

The following posts an edit to the `/-/AI_Review` invitation. Note that the signature for the per-paper AI Reviewer groups use [dollar sign notation](../../reference/api-v2/entities/invitation/dollar-sign-notation.md) to retrieve the submission number:

```python
ai_review_inv_id = f"{venue_id}/-/AI_Review"

## Signature for single overall AI Reviewer group
ai_reviewer_group_id = f"{venue_id}/AI_Reviewer"

## Signature for per-paper AI Reviewer groups
# ai_reviewer_group_id = f"{venue_id}/Submission${{7/content/noteNumber/value}}/AI_Reviewer"

client_v2.post_invitation_edit(
    invitations=f'{venue_id}/-/Edit',
    readers=[venue_id],
    writers=[venue_id],
    signatures=[venue_id],
    invitation=openreview.api.Invitation(
        id=ai_review_inv_id,
        edit={
            "invitation": {
                "edit": {
                    "signatures": {
                        "param": {
                            "items": [ { "value": ai_reviewer_group_id, "optional": True } ]
                        }
                    }
                }
            }
        }
    )
)
```

### 3. Post reviews using your LLM

Follow the directions here to [post reviews](../../how-to-guides/workflow/how-to-create-change-and-delete-notes.md) using your LLM. You should be able to sign the notes using the PC group or the AI\_Reviewer group(s) you created.

**Check your work**: Go to one of the submission pages (you can navigate here from the PC Console), and check for your review.

### 4. Delete an LLM review

Follow the directions here to [delete reviews](../../how-to-guides/workflow/how-to-create-change-and-delete-notes.md) using your LLM.

**Check your work**: Go to the submission page (you can navigate here from the PC Console), and check that the review was deleted.
