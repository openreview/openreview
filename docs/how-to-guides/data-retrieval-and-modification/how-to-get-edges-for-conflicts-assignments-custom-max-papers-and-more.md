# How to get edges for Conflicts, Assignments, Custom Max Papers, and more

If you want to get all edges for any type of Edge Invitation you can use the [`get_grouped_edges`](https://github.com/openreview/openreview-py/blob/faf9e0c94886150101b709407491f3c431f302ce/openreview/api/client.py#L1718) API call. The most common edges to retrieve are for Conflicts, Assignments and Custom Max Papers.

`get_grouped_edges` requires at least 2 fields:

* [**Invitation**](../../reference/api-v2/entities/invitation.md): The ID of the Edge Invitation.
* **Groupby**: The field of the [Edge](../../reference/api-v2/entities/edge/) you want to group the edges by. Typically you will group by the edge's `head` or `tail`.  The value of head and tail are determined by their Invitation. You can view the invitation by going to: `https://openreview.net/invitation/edit?id=invitation_id`

`get_grouped_edges` will return list of dictionaries each containing 2 keys: `id` and `values`. The `id` will contain the field you grouped by and `values` will contain all the edges in that group.

## Getting all Conflict Edges for the Reviewers Group

This is an example of how to use `get_grouped_edges` to retrieve Reviewer Conflict edges. You can use this method to get any type of Edge.

### Mapping reviewer IDs to their conflict edges

1. Get the conflict invitation ID for the API call:

Get the venue group. The content of the venue group contains many settings for your venue including invitation IDs. The Reviewer Conflict invitation ID is stored as `reviewers_conflict_id` in the content. It follows the format: `venue_id/reviewers_name/-/Conflict`.

```python
venue_group = client.get_group(venue_id)
conflict_invitation_id = venue_group.content['reviewers_conflict_id']['value']
```

2. Get the edges using `get_grouped_edges`:

Call `get_grouped_edges`, pass the invitation ID, and group by the `tail`. Conflict edges are configured so that the `head` is the paper ID and the `tail` is the user with the conflict. When you group by the `head` you can access all the conflicts for a specific paper. Alternatively, if you group by the `tail`, you can access all conflicts for a specific user:

```python
grouped_edges = client.get_grouped_edges(
    invitation = conflict_invitation_id,
    groupby = 'tail'
)
```

3. Map each reviewer profile ID with their conflict edges:

```python
rev_conflict_map =  { group['id']['tail']: group['values'] for group in grouped_edges }
```

### Mapping paper IDs to their conflict edges

If you want to see all conflicts for a specific paper, group by the head instead.

```python
grouped_edges = client.get_grouped_edges(
    invitation = conflict_invitation_id,
    groupby = 'head'
)

paper_conflict_map =  { group['id']['head']: group['values'] for group in grouped_edges }
```

## How to get other types of edges

You can use the same method to get any type of Edge by simply changing the invitation ID. For example:

* To get all Reviewer Assignment edges, pass the Assignment invitation ID:

```python
assignment_invitation_id = venue_group.content['reviewers_assignment_id']['value']
```

* To get all Reviewer Custom Max Paper edges, pass the Custom Max Papers invitation ID:

```python
custom_max_papers_invitation_id = venue_group.content['reviewers_custom_max_papers_id']['value']
```

* To get all Reviewer Affinity Score edges, pass the Affinity Score invitation ID:

```python
affinity_score_invitation_id = venue_group.content['reviewers_affinity_score_id']['value']
```

To get the corresponding invitation IDs for Area Chairs or Senior Area Chairs, just replace `reviewers` with `area_chairs` or `senior_area_chairs`. For example, `area_chairs_assignment_id` or `senior_area_chairs_assignment_id`. If your venue uses other role names, those names will automatically get replaced in the invitation IDs.

## Optional parameters and other use cases

It may be helpful to pass other parameters to `get_grouped_edges` to further filter the edges.

### Using `label` to map reviewer IDs to their proposed assignments

Some Edge Invitations allow for edges to be posted with a `label`. To know if this is possible, you have to examine the invitation and see if `label` is configured. If so, you may pass `label='Some label'` to `get_grouped_edges` to retrieve only a subset of those edges.

This is often used when retrieving Proposed Assignment edges. The matcher will post these edges where the label is the title of the matching configuration:

```python
proposed_assignment_invitation_id = venue_group.content['reviewers_proposed_assignment_id']['value']

grouped_edges = client.get_grouped_edges(
    invitation = proposed_assignment_invitation_id,
    groupby = 'tail',
    label = 'rev-matching-1'
)

rev_proposed_assignment_map =  { group['id']['tail']: group['values'] for group in grouped_edges }
```

### Using `select` to map reviewer IDs to their assignment count

You can pass `select = 'count'` to get the number of edges for each grouped entity. In this case, we will group by the user ID with `tail` so that we can map them to their assignment counts.&#x20;

```python
grouped_edges = client.get_grouped_edges(
    invitation = assignment_invitation_id,
    groupby = 'tail',
    select = 'count'
)

rev_assignment_count_map =  { group['id']['tail']: group['count'] for group in grouped_edges }
```

{% hint style="info" %}
Notice how `group['values']` changed to `group['count']` in the mapping. This happens only when we select the count.
{% endhint %}

You may also select by any field(s) of the Edge if you don't need to retrieve all the data in the edges. For example, you may pass `select='head,tail'` if you only want to see the head and tail of the edge.
