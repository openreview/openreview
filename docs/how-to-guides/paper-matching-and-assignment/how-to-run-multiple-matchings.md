# How to Run Multiple Matchings

## Prerequisites and Setup

Follow these instructions if you’ve already deployed assignments and want to use the matcher to assign more users to papers.

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
5. Convert proposed assignments to assignment edges.
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

### Convert proposed assignments to assignment edges

First, set up both API 1 and API 2 clients, set the role names for the group we are assigning, retrieve the Venue object, and get submissions:

```python
client_v1 = ...
client_v2 = ...

role_name = 'Area_Chairs'
group_id = f'venue_id/{role_name}'

venue = openreview.conference.helpers.get_conference(client_v1, request_form_id) # Uses API 1 client
papers = venue.get_submissions(sort='number:asc')
```

Next, we will retrieve the Proposed Assignment edges for the new matching config and map the paper IDs (the `head`) to the edges for that paper. To do this, you need the `title` of the **new** config found in the assignments page:

```python
proposed_assignment_edges =  { g['id']['head']: g['values'] for g in client_v2.get_grouped_edges(
    invitation=venue.get_assignment_id(group_id),
    label='name-of-new-matching', 
    groupby='head', 
    select=None)
}
```

Then, we will create a function that process each submission, checks if there is a proposed assignment edge for that paper, and creates an Assignment edge. It also adds the user to the correct paper group:

```python
assignment_invitation_id = venue.get_assignment_id(group_id, deployed=True)

def process_paper_assignments(paper):
    paper_assignment_edges = []
    if paper.id in proposed_assignment_edges:
        paper_committee_id = venue.get_committee_id(name=role_name, number=paper.number)
        proposed_edges=proposed_assignment_edges[paper.id]
        assigned_users = []

        for proposed_edge in proposed_edges:
            assigned_user = proposed_edge['tail']
            paper_assignment_edges.append(openreview.api.Edge(
                invitation = assignment_invitation_id,
                head = paper.id,
                tail = assigned_user,
                readers = proposed_edge['readers'],
                nonreaders = proposed_edge.get('nonreaders'),
                writers = proposed_edge['writers'],
                signatures = proposed_edge['signatures'],
                weight = proposed_edge.get('weight')
            ))
            assigned_users.append(assigned_user)
        client_v2.add_members_to_group(paper_committee_id, assigned_users)
        return paper_assignment_edges
    else:
        print('assignment not found', paper.id)
        return []
```

**Note**: If your venue has SACs assigned to ACs and you are deploying AC assignments, you will need to modify the script to add the assigned SAC to the paper group as well. We do this by retrieving the SAC assignments and in `process_paper_assignments` we post a group edit to add the SAC to the paper's SAC group:

```python
sac_assignment_edges =  { g['id']['head']: g['values'] for g in client_v2.get_grouped_edges(
    invitation=venue.get_assignment_id(venue.get_senior_area_chairs_id(), deployed=True),
    groupby='head', 
    select=None)} if not venue.sac_paper_assignments else {}

def process_paper_assignments(paper):
    paper_assignment_edges = []
    if paper.id in proposed_assignment_edges:
        paper_committee_id = venue.get_committee_id(name=role_name, number=paper.number)
        proposed_edges=proposed_assignment_edges[paper.id]
        assigned_users = []
        for proposed_edge in proposed_edges:
            assigned_user = proposed_edge['tail']
            # -- New code block --
            if sac_assignment_edges:
                sac_assignments = sac_assignment_edges.get(assigned_user, [])
                for sac_assignment in sac_assignments:
                    assigned_sac = sac_assignment['tail']
                    sac_group_id = venue.get_senior_area_chairs_id(number=paper.number)
                    client_v2.post_group_edit(
                        invitation = venue.get_meta_invitation_id(),
                        readers = [venue.venue_id],
                        writers = [venue.venue_id],
                        signatures = [venue.venue_id],
                        group = openreview.api.Group(
                            id = sac_group_id,
                            members = [assigned_sac]
                        )
                    )
# ... rest of the function ...
```

Now we will call `openreview.tools.concurrent_requests()` which takes in a function and a list, where each item in the list will get passed to the function for processing. We will pass `process_paper_assignments` and the list of papers. The following will return a list of edges:

```python
assignment_edges = reduce(concat,openreview.tools.concurrent_requests(process_paper_assignments, papers))
```

Finally, we will post the edges in bulk:

```python
print('Posting assignment edges', len(assignment_edges))
openreview.tools.post_bulk_edges(client=client_v2, edges=assignment_edges)
```

### Delete the “Current Assignment” conflicts

This is so that the assignment browser doesn't show the extra conflicts. Pass the invitation and edge `label`:

```python
client_v2.delete_edges(invitation='venue_id/role_name/-/Conflict', label='Current Assignment')
```

### Viewing the new assignments

The new assignments can be seen using the same "Edit Assignments" link next to the "Deployed" configuration. This shows all assignments.
