# How to upload assignments with Python

## Overview

Follow these instructions if you are not using OpenReview’s matching system and are computing assignments locally to upload in bulk.

General steps to follow:

1. Upload assignments as Proposed Assignments
2. Use the assignment browser and stats page to spot check assignments
3. Deploy

## Prerequisites

1. In your request form, **Submission Reviewer Assignment** should be set to **Automatic**.
2. Let the full paper deadline pass
3. [Set up the Python client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md)
4. Create the necessary invitations by calling `setup_committee_matching()` for all roles:

```python
# Use API 1 client
venue = openreview.helpers.get_conference(client_v1, request_form_id)
venue.setup_committee_matching(committee_id=venue.get_reviewers_id())
# For ACs: venue.setup_committee_matching(committee_id=venue.get_area_chairs_id())
# For SACs: venue.setup_committee_matching(committee_id=venue.get_senior_area_chairs_id())
```

## Uploading assignments as Proposed Assignments

Proposed Assignments are a way to view your assignments in OpenReview without making them visible to the committee. This can be helpful if you want to spot check assignments, use the stats page to view the assignment distributions, and manually adjust them in the UI.

In this example, we will upload Reviewer assignments. The exact script depends on how you have your assignment data structured. We will assume you mapped paper IDs to a list of assigned reviewer profile IDs, e.g.:

```python
paper_id_to_assigned_users = {
    'paperid1': ['~profile_id1', '~profile_id2', ...]
}
```

{% hint style="warning" %}
The user IDs need to be their `profile.id`s. Any time you are adding users to groups or posting their edges, you use profile IDs. This is how we ensure data is synced between the consoles, assignment browser, and more. If you're unsure, get all profiles with: [`openreview.tools.get_profiles()`](../../getting-started/objects-in-openreview/introduction-to-profiles.md#getting-a-profile-or-profiles).
{% endhint %}

### 1. Setup

First, go to the Assignments page for your group and create a new **Assignment Configuration**. Since you are not using our matcher, only the `title` and `Max Papers` is important. `title` will be used to **label** the edges and `Max Papers` will be used as the default quota in case any users are added to the group later. You can input any values for the other settings.

Next, change the status of the configuration to `Complete`. Get the assignment configuration note using the `title` and post a note edit updating its `status`:

```python
assignment_config_note = client_v2.get_all_notes(
    invitation='venue_id/role_name/-/Assignment_Configuration', 
    content={'title': 'insert-assignment-config-title'})[0]

client_v2.post_note_edit(
    invitation='venue_id/role_name/-/Assignment_Configuration',
    signatures=[venue_id],
    note = openreview.api.Note(
        id=assignment_config_note.id,
        content = {
            'status': {
                'value': 'Complete'
            }
        }
    )
)
```

### 2. Build Proposed Assignment edges

Now we need to build Proposed Assignment edges for each user. All the edges need to be posted with: `invitation`, `head`, `tail`, `label`, `readers`, `nonreaders`, `writers`, `signatures`, and `weight`.&#x20;

Since we're uploading assignments for reviewers, we will use the following edge configuration:&#x20;

* **`invitation`**: The reviewer proposed assignment invitation ID.&#x20;
* **`head`**: The ID of the paper.&#x20;
* **`tail`**: The reviewer's profile ID.&#x20;
* **`label`**: The assignment configuration title. This is how we differentiate between edges of different matchings.
* **`readers`**: List containing: venue ID, assigned SAC ID, assigned AC ID, the assigned reviewer's profile ID.&#x20;
* **`nonreaders`**: List containing: paper authors group ID.&#x20;
* **`writers`**: List containing: venue ID, assigned SAC ID, assigned AC ID.&#x20;
* **`signatures`**: List containing the Program Chair group ID.&#x20;
* **`weight`**: 1&#x20;
  * Weight typically represents the aggregate score, but when we're manually uploading assignments we can just set this to 1.&#x20;

{% hint style="warning" %}
To see how Proposed Assignment edges should be configured for each role, you **must** view the respective Proposed Assignment invitation by going to: `https://openreview.net/invitation/edit?id=proposed_assignment_invitation_id`&#x20;

If SACs are assigned to ACs, the `head` and `tail` **will be profile IDs**. Refer to their invitations for more info.
{% endhint %}

We will create a function that processes the assignments for each paper and builds the edges using the title of the assignment configuration to `label` the edge:

```python
# Retrieve proposed assignment invitation ID
proposed_assignment_invitation_id = venue.get_assignment_id(
    committee_id = 'venue_id/role_name',
    deployed = False
)
# Result: venue_id/role_name/-/Proposed_Assignment

def process_paper_assignments(paper): 
    # Initialize assignment edge list
    paper_assignment_edges = []
    
    # Get group IDs for each role for this paper
    paper_reviewer_id = venue.get_reviewers_id(number=paper.number) # E.g. "venue_id/Submission5/Reviewers",
    paper_ac_id = venue.get_area_chairs_id(number=paper.number)
    paper_sac_id = venue.get_senior_area_chairs_id(number=paper.number)
    paper_author_id = venue.get_authors_id(number=paper.number)
    
    # Get users assigned to this paper
    assigned_users = paper_id_to_assigned_users[paper.id]
    
    # Build Edges for each reviewer
    for user_id in assigned_users:
        paper_assignment_edges.append(openreview.api.Edge(
            invitation = proposed_assignment_invitation_id,
            head = paper.id,
            tail = user_id, # profile.id
            label = 'insert-assignment-config-title',
            readers = [ venue_id, paper_sac_id, paper_ac_id, user_id ],
            nonreaders = [ paper_author_id ],
            writers = [ venue_id, paper_sac_id, paper_ac_id ],
            signatures = [ venue.get_program_chairs_id() ], # E.g. venue_id/Program_Chairs
            weight = 1
        ))
    return paper_assignment_edges
```

### 3. Post Proposed Assignment edges

Gather all the proposed assignment edges. We use `concurrent_requests` which takes in a function and a list, where each item in the list will get passed to the function. Here, the list should be the list of submission notes.

First, [get all active submissions](../data-retrieval-and-modification/how-to-get-all-notes-for-submissions-reviews-rebuttals-etc.md#quickstart-getting-all-submissions). This means we are only posting assignments to active papers.

Next, call `concurrent_requests` with the previously created function and use `reduce` and `concat` to get a list of edges:

```python
assignment_edges = reduce(concat, openreview.tools.concurrent_requests(process_paper_assignments, submissions))
```

Finally, check if it is the expected number of assignments and post the edges in bulk:

```python
print('Number of assignment edges', len(assignment_edges))

openreview.tools.post_bulk_edges(client=client_v2, edges=assignment_edges)
```

### 4. Check the data

Check the edge count in your browser to verify that the API posted all the edges: `http://api2.openreview.net/edges/count?invitation=invitation_id`&#x20;

Use the stats page by clicking **View Statistics** next to the assignment configuration. From here, you can view distributions such as papers to number of users or users to number of papers.

### 5. Deploy assignments

When you've finalized assignments, you can deploy them. This will make papers visible in the committee's consoles and give them access. This does not send emails.

Call `venue.set_assignments()` and pass:

* **`assignment_title`**: Title of Assignment Configuration.
* **`committee_id`**: ID of the Group getting assigned.
* **`overwrite`**: Boolean that determines if the current assignments should be overwritten.&#x20;
  1. If False, it will only post the new assignments.
  2. If True, it will delete all current assignments and replace them with the new assignments.

{% hint style="warning" %}
Only use `overwrite = True` if you want to completely redo assignments.
{% endhint %}

```python
venue.set_assignments(
    assignment_title = 'insert-assignment-config-title',
    committee_id = 'venue_id/role_name',
    overwrite = False
)
```

## How to undo deployed assignments

If you want to undo assignments for any reason, you can call `venue.unset_assignments()`. This will undeploy assignments based on the Proposed Assignment edges in the specified Assignment Configuration. So if you made changes to the assignments after deployment, those reassignments won't be removed. You will need to handle them manually either through the UI or by deleting their edges and removing the users from the paper groups.

Use this if you deployed assignments, made no modifications, then want to undeploy:

```
venue.unset_assignments(
    assignment_title = 'new_assignment_configuration_title',
    committee_id = f'{venue_id}/role_name'
)
```
