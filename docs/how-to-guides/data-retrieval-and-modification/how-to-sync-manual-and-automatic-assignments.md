# How to Sync Manual and Automatic Assignments

Using manual assignment adds Reviewers to SubmissionX/Reviewers groups without posting an assignment edge. If you originally used automatic assignment but then add assignments manually through the PC console instead of the edge browser, your groups and assignment edges will become out of sync. Here is how you can restore them:&#x20;

1. If you have not done so, you will need to [install and instantiate the openreview-py client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).  To get the edges use the API 1 client, however for everything else you will need to use the API 2 client.
2. Retrieve all of the assignment edges and all of the Submission#/Reviewers groups for your venue. Make two dictionaries: one mapping groups by their IDs, the other mapping edges by their head. The head of each edge is the forum of the paper that the reviewer is assigned to.

```
edges = client.get_all_edges(invitation = 'Your/Venue/ID/Reviewers/-/Assignment')
submissions = client.get_all_notes(invitation = 'Your/Venue/ID/-/Submission')
groups = [client.get_group(f"Your/Venue/ID/Submission{submissions.number}/Reviewers") for submission in submissions]
groups_by_ids = {group.id: group for group in groups}
edges_by_submission = {}
for edge in edges: 
    if edge.head in edges_by_submission: 
        edges_by_submission[edge.head].append(edge.tail)
    else: 
        edges_by_submission[edge.head] = [edge.tail]
```

3\. Iterate through all assignment edges. For each one, check that the reviewer in the assignment edge is in the correct SubmissionX/Reviewers group. For example, if an edge has a tail of \~User\_One1 and a head corresponding to Submission2, then Submission2/Reviewers should include \~User\_One1. If the reviewer is not in the correct group, add them.&#x20;

```
count = 0
for edge in tqdm(edges): 
    # Check if reviewer is in submissionX reviewer group 
    submissions = client.get_note(edge.head)
    reviewer_group = groups_by_ids[f'Your/Venue/ID/Submission{submissions.number}/Reviewers']
    if edge.tail not in reviewer_group.members: 
        print(edge.tail, reviewer_group.id, submission.forum)
        count = count+1
        # If edge exists but reviewer is not in group, add reviewer to group 
        reviewer_group = client.add_members_to_group(reviewer_group, edge.tail)
print(count)
```

4\. Now we want to check for the opposite case, where a Reviewer has been added to a group but there is not an associated assignment edge. Retrieve all submissions, then create a dictionary with submission numbers as the keys. Iterate through the submissions, get the associated Reviewers' group for each, and check that there is an assignment edge for each group member. If there is not, remove the reviewer.&#x20;

```
submissions_by_number = {}
for submission in submissions: 
    submissions_by_number[submission.number] = submission.forum
count = 0
# Go through every SubmissionX/Reviewers group and for each one check if reviewers have an edge 
# go through each submission, for each one get the reviewers group, for each reviewer in the group see if the edge exists 
for number, forum in submissions_by_number.items(): 
    reviewers_group = groups_by_ids[f'Your/Venue/ID/Submission{number}/Reviewers']
    tails = edges_by_submission[forum]
    for reviewer in reviewers_group.members: 
        if reviewer not in tails: 
            count = count + 1
            print(reviewer, reviewers_group.id, forum)
            reviewers_group = client.remove_members_from_group(reviewers_group, reviewer)
print(count)
```
