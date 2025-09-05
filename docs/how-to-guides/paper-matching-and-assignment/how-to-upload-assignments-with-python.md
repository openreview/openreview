# How to upload assignments with Python

## Overview

Follow these instructions if you are not using OpenReview’s matching system and are computing assignments locally to upload in bulk.

General steps to follow:

1. Add users to the paper committee groups
2. Post Assignment edges

## Prerequisites

1. Let the full paper deadline pass
2. [Set up the Python client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md)
3. Create the necessary invitations by calling `setup_committee_matching()` for all roles:

```python
# Use API 1 client
venue = openreview.helpers.get_conference(client_v1, request_form_id)
venue.setup_committee_matching(committee_id=venue.get_reviewers_id())
# For ACs: venue.setup_committee_matching(committee_id=venue.get_area_chairs_id())
# For SACs: venue.setup_committee_matching(committee_id=venue.get_senior_area_chairs_id())
```

## Uploading assignments

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

First, [get all active submissions](../data-retrieval-and-modification/how-to-get-all-notes-for-submissions-reviews-rebuttals-etc.md#quickstart-getting-all-submissions).

Next, get your venue group and retrieve the reviewer assignment invitation ID.

* If you were assigning ACs or SACs, you would instead retrieve the AC or SAC assignment invitations respectively.

```python
venue_group = client_v2.get_group(venue.venue_id)
assignment_invitation_id = venue_group.content['reviewers_assignment_id']['value']
# Result: venue_id/role_name/-/Assignment
# For ACs: venue_group.content['area_chairs_assignment_id']['value']
# For SACs: venue_group.content['senior_area_chairs_assignment_id']['value']
```

### 2. Add users to the paper committee groups

Then, create a function that takes in 1 submission note and processes the assignments for that paper.

We will retrieve all the paper committee group IDs. We use these group IDs to add the assigned members and configure the Assignment edges later.&#x20;

* It follows the format: `venue_id/SubmissionXX/role_name`.

<pre class="language-python"><code class="lang-python"><strong>def process_paper_assignments(paper):
</strong>    # Get all paper committee group IDs
    paper_reviewer_id = venue.get_reviewers_id(number=paper.number)
    paper_ac_id = venue.get_area_chairs_id(number=paper.number)
    paper_sac_id = venue.get_senior_area_chairs_id(number=paper.number)
    paper_author_id = venue.get_authors_id(number=paper.number)
</code></pre>

Since we're assigning reviewers, we'll add them to the paper reviewer group. This will automatically create anon IDs.&#x20;

* If you were assigning ACs or SACs, you would instead add them to the `paper_ac_id` or `paper_sac_id` groups respectively.

```python
def process_paper_assignments(paper):
    # ... previous code ...

    # Get assigned users: ['~profile_id1', '~profile_id2', ...]
    assigned_users = paper_id_to_assigned_users[paper.id]

    # Add assigned reviewers
    client_v2.add_members_to_group(group=paper_reviewer_id, members=assigned_users)
    # For assigning ACs: client_v2.add_members_to_group(group=paper_ac_id, members=assigned_users)
    # For assigning SACs: client_v2.add_members_to_group(group=paper_sac_id, members=assigned_users)
```

### 3. Build Assignment edges

Now we need to build Assignment edges for each user. All the edges need to be posted with: `invitation`, `head`, `tail`, `readers`, `nonreaders`, `writers`, `signatures`, and `weight`.

To see how Assignment edges should be configured for each role, you **must** view the respective Assignment invitation by going to: `https://openreview.net/invitation/edit?id=assignment_invitation_id`&#x20;

Since we're uploading assignments for reviewers, we will use the following configuration:

* **invitation**: The reviewer assignment invitation ID.
* **head**: The ID of the paper.
* **tail**: The reviewers profile ID.
* **readers**: List containing: venue ID, assigned SAC ID, assigned AC ID, the assigned reviewer's profile ID.
* **nonreaders**: List containing: paper authors group ID.
* **writers**: List containing: venue ID, assigned SAC ID, assigned AC ID.
* **signatures**: List containing the Program Chair group ID.
* **weight**: 1
  * Weight typically represents the aggregate score, but when we're manually uploading assignments we can just set this to 1.

{% hint style="warning" %}
For AC and SAC assignments, the `invitation`, `head`, `tail`, `readers`, and `writers` will be configured differently.\
\
If SACs are assigned to ACs (check "Senior Area Chairs Assignment" in your request form), the `head` and `tail` **will be** **profile IDs**. You must refer to their invitations for more info.
{% endhint %}

<pre class="language-python"><code class="lang-python"><strong>def process_paper_assignments(paper):
</strong>    # ... previous code ...

    # Initialize assignment edge list
    paper_assignment_edges = []

    # Build Edges for each reviewer
    for user_id in assigned_users:
        paper_assignment_edges.append(openreview.api.Edge(
            invitation = assignment_invitation_id,
            head = paper.id,
            tail = user_id, # profile.id
            readers = [ venue_id, paper_sac_id, paper_ac_id, user_id ],
            nonreaders = [ paper_author_id ],
            writers = [ venue_id, paper_sac_id, paper_ac_id ],
            signatures = [ venue.get_program_chairs_id() ], # E.g. venue_id/Program_Chairs
            weight = 1
        ))
    return paper_assignment_edges
</code></pre>

### 4. Post Assignment edges

Gather all the assignment edges. We use `concurrent_requests` which takes in a function and a list, where each item in the list will get passed to the function. Here, the list should be the list of submission notes:

```
assignment_edges = reduce(concat, openreview.tools.concurrent_requests(process_paper_assignments, submissions))
```

Finally, check if it is the expected number of assignments and post the edges in bulk:

```python
print('Number of assignment edges', len(assignment_edges))

openreview.tools.post_bulk_edges(client=client_v2, edges=assignment_edges)
```

### 5. Check edge count in API

You can check the edge count in your browser to verify that the API posted all the edges: `http://api2.openreview.net/edges/count?invitation=invitation_id`&#x20;

## How to undo deployed assignments with python

If you make a mistake when uploading assignments, you can undo them by:

1. Deleting all assignment edges
2. Removing the users from the associated paper committee groups

{% hint style="danger" %}
Do this if your review process hasn't started yet. Otherwise you will unassign users who may have already posted reviews.
{% endhint %}

### 1. Delete the edges

```python
client_v2.delete_edges(invitation=assignment_invitation_id, soft_delete=True)
```

[Verify the edge count](how-to-upload-assignments-with-python.md#check-edge-count-in-api) is 0 before re-uploading assignments.

### 2. Remove the users from the paper committee groups

Create a function that:

* Gets the paper group ID of the committee you are unassigning
* Get the assigned users and remove them from the group
* Call `concurrent_requests` and pass a list of the submission notes

```python
def remove_paper_assignments(paper):
    # Get paper committee group ID
    paper_reviewer_id = venue.get_reviewers_id(number=paper.number)
    # ACs: paper_ac_id = venue.get_area_chairs_id(number=paper.number)
    # SACs: paper_sac_id = venue.get_senior_area_chairs_id(number=paper.number)
    
    # Get assigned users: ['~profile_id1', '~profile_id2', ...]
    assigned_users = paper_id_to_assigned_users[paper.id]
    
    # Remove assigned users
    client_v2.remove_members_from_group(paper_reviewer_id, assigned_users)
    # Unassign ACs: client_v2.remove_members_from_group(paper_ac_id, assigned_users)
    # Unassign SACs: client_v2.remove_members_from_group(paper_sac_id, assigned_users)

# Call function for each submission
openreview.tools.concurrent_requests(remove_paper_assignments, submissions)
```
