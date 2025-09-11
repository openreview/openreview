# How to Upload Edges in Bulk

## Prerequisites and Setup

1. Follow these instructions if you already have Edges ready to upload. For example, if you followed:
   1. [How to Compute Affinity Scores Manually](how-to-compute-affinity-scores.md)
   2. [How to get edges for Conflicts, Assignments, Custom Max Papers, and more](../data-retrieval-and-modification/how-to-get-edges-for-conflicts-assignments-custom-max-papers-and-more.md)&#x20;
   3. [How to Compute Conflicts Between Users](how-to-compute-conflicts-between-users.md)

{% hint style="warning" %}
If you are doing a **full** score re-computation you should instead upload scores as a CSV file using [`setup_committee_matching()`](computing-affinity-scores-and-conflicts-for-large-venues.md#id-5.-upload-scores). It also handles **full** [conflict re-computation](computing-affinity-scores-and-conflicts-for-large-venues.md#id-6.-computing-and-uploading-conflicts).
{% endhint %}

2. [Create an OpenReview client.](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md)
3. Have good internet connection. Posting a lot of edges takes time, so it's important that you can maintain connection while uploading.

## Different methods to posting edges in bulk

### 1. Calling `post_bulk_edges`

The simplest way to upload edges in bulk is to call `post_bulk_edges` and pass all your edges:

```python
openreview.tools.post_bulk_edges(client=client_v2, edges=edges)
```

This will post edges in batches of 50,000 edges. You can modify the batch size by passing the `batch_size` argument, e.g. `batch_size=10000`.

_However_, it can be easier to manage a lot of edges by posting them in bulk for each paper or for each user. This can also help you avoid posting duplicate edges for a `head`/`tail` combo, e.g. for scores or conflicts. See the methods below.

{% hint style="info" %}
Using `post_bulk_edges` ignores the validation set in the invitation.
{% endhint %}

### 2. Posting edges in bulk for each paper

Use this if you computed scores or conflicts for a subset of papers against all members of a group. This will assume you have a list of edges called `score_edges` ready to upload.

1. [Get submissions](../data-retrieval-and-modification/how-to-get-all-notes-for-submissions-reviews-rebuttals-etc.md#quickstart-getting-all-submissions)
2. Map paper IDs  to the list of edges for that paper.
   1. For affinity scores and conflicts, the `head` of the edge is usually the paper ID. Refer to the invitation for the correct edge configuration, e.g.: `https://openreview.net/invitation/edit?id=venue_id/role_name/-/Affinity_Score`.&#x20;
3. Loop through submissions and post edges for that paper:

```python
from collections import defaultdict
paper_id_to_edges = defaultdict(list)

for edge in score_edges:
    paper_id_to_edges[edge.head].append(edge)

for sub in submissions:
    print("Processing edges for paper number: ", sub.number, " ID: ", sub.id)
    openreview.tools.post_bulk_edges(client=client_v2, edges=paper_id_to_edges[sub.id])
```

The difference between this and 1) is that you are posting them using a grouping. So if it fails and you need to re-upload, you can delete edges for just that paper and trigger the re-upload:

```python
client.delete_edges(invitation=invitation_id, head=sub.id, soft_delete=True)
```

### 3. Posting edges in bulk for each user

Use this if you computed scores or conflicts for a subset of users against all papers. This will assume you have a list of edges called `score_edges` ready to upload and list of the subset of users called `user_list`.

1. [Get submissions](../data-retrieval-and-modification/how-to-get-all-notes-for-submissions-reviews-rebuttals-etc.md#quickstart-getting-all-submissions)
2. Map user IDs to the list of edges for that user.
   1. For affinity scores and conflicts, the `tail` of the edge is usually the profile ID of the user. Refer to the invitation for the correct edge configuration, e.g.: `https://openreview.net/invitation/edit?id=venue_id/role_name/-/Affinity_Score`.&#x20;
3. Loop through submissions and post edges for that user:

```python
from collections import defaultdict
profile_id_to_edges = defaultdict(list)

for edge in score_edges:
    profile_id_to_edges[edge.tail].append(edge)

user_list = ["~User1", "~User2" ...]

for user in user_list:
    print("Processing edges for user: ", user)
    openreview.tools.post_bulk_edges(client=client_v2, edges=profile_id_to_edges[user])
```

Similar to method #2, you are posting them using a grouping. So if it fails and you need to re-upload, you can delete edges for just that user and trigger the re-upload:

```python
client.delete_edges(invitation=invitation_id, tail="~User1", soft_delete=True)
```
