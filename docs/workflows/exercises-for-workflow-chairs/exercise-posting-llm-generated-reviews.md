# Exercise: Posting LLM generated reviews

## Setup:

1. Complete [Prerequisites](prerequisites.md)
2. [Add submissions](../../how-to-guides/workflow/how-to-create-change-and-delete-notes.md) (make sure this is on the dev site)
3. Open the [Review Stage](../../reference/stages/review-stage.md) (step 17 in the [Example Workflow](../conferences.md))

## Part 1: Posting LLM generated reviews

In some cases, a venue may want to use LLMs to post reviews. There are 2 steps to enable this:

1. Create an LLM reviewer group for each paper.
2. Add the LLM reviewer groups to each submission reviewers group.

### 1. Create an LLM reviewer group for each paper

First, you will need to create a new reviewer Group for each paper. This will add the LLM as a Reviewer for the paper.

The reviewer group should have the following format:`<venue_id>/Submission<number>/Reviewer_<new_name>` . So it's important to have the `id` of the new group follow this format.&#x20;

For example: `MyConference/2025/Conference/Submission1/Reviewer_LLM`

The full `post_group_edit` call will look like this:

```python
client.post_group_edit(
    invitation='<venue_id>/-/Edit',
    signatures=['<venue_id>'],
    group=openreview.api.Group(
        id = '<venue_id>/Submission<number>/Reviewer_New_Group',
        readers = ['<venue_id>'],
        signatures = ['<venue_id>'],
        writers = ['<venue_id>'],
        signatories = ['<venue_id>'],
    )
)
```

Then you will loop through all of the submissions in the venue.&#x20;



**Check your work:** Go to one of the submission groups for the venue and check for your new reviewer group. (https://openreview.net/group/edit?id=\<venue\_id>/Submission\<number>)

### 2. Add the LLM reviewer groups to each submission reviewers group

Next, you need to add the LLM Reviewers as members to each submission's overall reviewers group. You can do this by calling [`add_members_to_group`](https://github.com/openreview/openreview-py/blob/791983ca7794cdf53affce9d0e12c6a8bcb1dc36/openreview/api/client.py#L2059).  It requires 2 fields, `group` and `members` .

* **Group**: The group ID to which you are adding members.
* **Members**: The ID or IDs you are adding to the group.

The full `add_members_to_group` call will look like this:

```python
client.add_members_to_group(group='<venue_id>/Submission<number>/Reviewers', 
members='<venue_id>/Submission<number>/Reviewer_New_Group')
```

{% hint style="info" %}
Typically when adding to the SubmissionX/Reviewers group we always add a user's profile ID. This will automatically create their anon ID. In this case, the signature of the review is not a real user, so we can just add the LLM reviewer group ID.
{% endhint %}

**Check your work:** Go to one of the submission groups for the venue and check for your new reviewer group in the reviewers group (https://openreview.net/group/edit?id=\<venue\_id>/Submission\<number>/Reviewers)

### 3. Post reviews using your LLM

Follow the directions here to [post reviews](../../how-to-guides/workflow/how-to-create-change-and-delete-notes.md) using your LLM.

**Check your work**: Go to one of the submission pages (you can navigate here from the PC Console), and check for your review.&#x20;

### 3. Delete an LLM review

Follow the directions here to [delete reviews](../../how-to-guides/workflow/how-to-create-change-and-delete-notes.md) using your LLM.

**Check your work**: Go to the submission page (you can navigate here from the PC Console), and check that the review was deleted.

