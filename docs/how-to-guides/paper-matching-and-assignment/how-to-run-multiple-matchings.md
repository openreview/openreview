# How to Run Multiple Matchings

## Prerequisites and Setup

Follow these instructions if you’ve already deployed assignments and want to use the matcher to assign more users to papers. For example:

* Create phase 2 assignments
* Assign emergency reviewers to papers missing 3 reviews
* Assign any subset of users to any subset of papers

[Create an OpenReview client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).

## Overview

The matcher is not aware of the current assignment data in OpenReview. To deploy additional assignments, we must set up the edges so that the matcher can create the correct assignments.

The UI currently doesn’t support deploying multiple assignment configurations, so you will also need to retrieve the Proposed Assignment edges created by the matcher and convert them to Assignment edges.

The general steps are:

1. Post current assignments as conflicts
   1. This tells the matcher to not assign the same users to the same papers.
   2. We post them with a label “Current Assignment” so we can easily identify them later to delete. These are temporary conflicts.
2. Post or update Custom Max Paper edges for all users
   1. This is the user’s paper quota and tells the matcher to not over-assign papers to users.
3. Post Custom User Demand edges for all papers
   1. This is the paper’s user quota and tells the matcher how many users to assign to each paper.
4. Create a new Assignment Configuration and run the matcher.
5. Deploy assignments
6. Delete the “Current Assignment” conflicts.

## Setting up the matcher and deploying assignments

### Post current assignments as conflicts

1. [Get all Assignment edges](../data-retrieval-and-modification/how-to-get-edges-for-conflicts-assignments-custom-max-papers-and-more.md).
2. Loop through the assignment edges. For each edge, create a conflict edge with the same `head` and `tail`, where the `head` is the paper ID and the `tail` is the user ID, and with the `label` “Current Assignment”. Follow [these instructions](how-to-post-a-custom-conflict.md) for building conflict edges.
3. [Post edges in bulk](how-to-upload-edges-in-bulk.md).

Example:

```python
assignments_per_user = { g['id']['tail']: g['values'] for g in client_v2.get_grouped_edges(
    invitation=f'{venue_id}/role_name/-/Assignment', 
    groupby='tail')
}

conflict_edges = []
for user, edges in assignments_per_user.items():
    for edge in edges:
        conflict_edges.append(openreview.api.Edge(
            invitation=f'{venue_id}/role_name/-/Conflict',
            head = edge['head'],
            tail = edge['tail'],
            weight = -1,
            label = 'Current Assignment',
            signatures = [venue_id],
            readers = [venue_id],
            writers = [venue_id]
        ))

print('Posting assignments as conflicts... ', len(conflict_edges))
openreview.tools.post_bulk_edges(client=client_v2, edges=conflict_edges)
```

### Post or update Custom Max Paper edges for all users

1. [Get all Custom Max Paper edges](../data-retrieval-and-modification/how-to-get-edges-for-conflicts-assignments-custom-max-papers-and-more.md#mapping-reviewer-ids-to-their-conflict-edges) and map users to their edges.
   1. Not all users will have Custom Max Papers edges. They will have an edge if they select a reduced load during recruitment or if a Program Chair sets it manually.
2. Follow the same steps to get all Assignment edges and map users to their edges. You will use this to set their quota.
   1. Not all users will have Assignment edges.
3. [Get all members of the group](../../getting-started/objects-in-openreview/groups.md#retrieving-groups) you are assigning.
4. For each member, check if they have an existing edge.&#x20;
   1. If yes, update the quota and post the edge.
   2. If not, build a new edge and collect them in a list to post them in bulk later.
   3. The edge `weight` is their paper quota. It should be:
      1. The user’s `current quota - the number of their assignments`.&#x20;
      2. If the user has no existing quota, you should use your venue's `default quota - the number of their assignments`.
      3. If you want to exclude specific reviewers from getting assigned, set their quota to 0.
5. [Post new edges in bulk](how-to-upload-edges-in-bulk.md).

Example:

{% hint style="warning" %}
Refer to the Custom Max Papers invitation for edge configuration, e.g. for `readers`. You can view the invitation by going to: `https://openreview.net/invitation/edit?id=venue_id/role_name/-/Custom_Max_Papers`&#x20;

Replace `${2/tail}` in the `readers` with the actual value of `tail`. In this example it is `mem`.
{% endhint %}

```python
quotas_per_user = { g['id']['tail']: g['values'] for g in client_v2.get_grouped_edges(
    invitation=f'{venue_id}/role_name/-/Custom_Max_Papers', 
    groupby='tail')
}

assignments_per_user = { g['id']['tail']: g['values'] for g in client_v2.get_grouped_edges(
    invitation=f'{venue_id}/role_name/-/Assignment', 
    groupby='tail')
}

members = client_v2.get_group(id=f'{venue_id}/role_name').members

new_quota_edges = []
default_quota = 4

for mem in members:
    num_assignments = len(assignments_per_user[mem]) if mem in assignments_per_user else 0
    existing_edge = quotas_per_user[mem][0] if mem in quotas_per_user else None # Each user only has 1 custom max papers edge
    current_quota = existing_edge['weight'] if existing_edge else default_quota
    new_quota = max(0, current_quota - num_assignments)

    if existing_edge:
        print('Updating edge for... ', mem)
        existing_edge['weight'] = new_quota
        updated_edge = openreview.api.Edge.from_json(existing_edge)
        client_v2.post_edge(updated_edge)
        print('Edge posted.')
    else:
        print('Building new edge for... ', mem)
        new_quota_edges.append(openreview.api.Edge(
            invitation = f'{venue_id}/role_name/-/Custom_Max_Papers',
            head = f'{venue_id}/role_name',
            tail = mem,
            weight = new_quota,
            signatures = [venue_id],
            writers = [venue_id],
            readers = [venue_id, ...other readers...]
        ))

openreview.tools.post_bulk_edges(client=client_v2, edges=new_quota_edges)
```

### Post Custom User Demand edges for all papers

Custom User Demand edges are very similar to Custom Max Paper edges except that the `head` is the paper ID and `tail` is the group ID of the users you are assigning (e.g. `venue_id/Reviewers`).&#x20;

1. [Get all active papers](../data-retrieval-and-modification/how-to-get-all-notes-for-submissions-reviews-rebuttals-etc.md#quickstart-getting-all-submissions)
2. For each paper, build the Custom User Demand edges. The `weight` is the number of users to assign to that paper.
   1. If you want to assign 2 new users for every paper, you set all the `weight`s to 2.
   2. If you want to assign users based on the number of missing reviews, you will need to [get all reviews](../data-retrieval-and-modification/how-to-get-all-notes-for-submissions-reviews-rebuttals-etc.md#quickstart-getting-reviews-meta-reviews-comments-decisions-rebuttals) and for each paper find how many are missing. Use that to set the `weight`.
   3. If you don’t want new assignments for some papers, you set the `weight`s of those papers to 0.

[Post edges in bulk](how-to-upload-edges-in-bulk.md).

Example:

{% hint style="warning" %}
Refer to the Custom User Demands invitation for edge configuration, e.g. for `readers`. You can view the invitation by going to: `https://openreview.net/invitation/edit?id=venue_id/role_name/-/Custom_User_Demands`&#x20;

Replace `${2/tail}` in the `readers` with the actual value of `tail`. In this example it is `venue_id/role_name`.
{% endhint %}

```python
user_demand_edges = []

for sub in submissions:
    user_demand_edges.append(openreview.api.Edge(
        invitation=f'{venue_id}/role_name/-/Custom_User_Demands',
        head=sub.id,
        tail=f'{venue_id}/role_name',
        weight = 2,
        signatures=[venue_id],
        writers = [venue_id],
        readers = [venue_id, ...other readers...]
    ))

openreview.tools.post_bulk_edges(client=client_v2, edges=user_demand_edges)
```

### Create a new Assignment Configuration and run the matcher

Now you can run the matcher for a new Assignment Configuration. This will create Proposed Assignments and you can view the stats page to determine if you need a new matching.

In the Assignment Configuration:

* Set a new `title`&#x20;
* The `User Demand` value doesn't matter since you already posted edges for each paper setting their User Demand. You can set it to your venue's default quota.
* Similarly, the `Max Papers` and `Min Papers` values don't matter since you should have already modified everyone's Custom Max Paper edges. You can set your venue's default quota. If any users are added to the group later, they will use the default.

### Deploy assignments using python

The UI currently doesn't support deploying multiple matchings, so you will need to use python.

1. Retrieve the `Venue` object for your venue. Use an **API 1** client.
2. Call `venue.set_assignments()` and pass:
   1. **`assignment_title`**: Title of new Assignment Configuration created in the previous step.
   2. **`committee_id`**: ID of the Group getting assigned.
   3. **`overwrite`**: Boolean that determines if the current assignments should be overwritten.&#x20;
      1. If False, it will only post the new assignments.
      2. If True, it will delete all current assignments and replace them with the new assignments.

{% hint style="warning" %}
Only use `overwrite = True` if you want to completely redo assignments.
{% endhint %}

```python
venue_id = 'venue/year/conference'
venue_group = client_v2.get_group(venue_id) # API 2 client
venue = openreview.get_conference(client_v1, venue_group.content['request_form_id']['value']) # API 1 client

venue.set_assignments(
    assignment_title = 'new_assignment_configuration_title',
    committee_id = f'{venue_id}/role_name',
    overwrite = False
)
```

### Delete the “Current Assignment” conflicts

This is so that the assignment browser doesn't show the extra conflicts. Pass the invitation and edge `label`:

```python
client_v2.delete_edges(invitation='venue_id/role_name/-/Conflict', label='Current Assignment')
```

### Viewing the new assignments

The new assignments can be seen using the same "Edit Assignments" link next to the "Deployed" configuration. This shows all assignments.

### How to undo assignments

If you want to undo assignments for any reason, you can call `venue.unset_assignments()`. This will undeploy assignments based on the Proposed Assignment edges in the specified Assignment Configuration. So if you made changes to the assignments after deployment, those reassignments won't be removed. You will need to handle them manually either through the UI or by deleting their edges and removing the users from the paper groups.

Use this if you deployed assignments, made no modifications, then want to undeploy:

```
venue.unset_assignments(
    assignment_title = 'new_assignment_configuration_title',
    committee_id = f'{venue_id}/role_name'
)
```
