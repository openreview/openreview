# How to Sync Manual and Automatic Assignments

Using manual assignment adds Reviewers to PaperX/Reviewers groups without posting an assignment edge. If you originally used automatic assignment but then add assignments manually through the PC console instead of the edge browser, your groups and assignment edges will become out of sync. Here is how you can restore them:&#x20;

1. If you have not done so, you will need to [install and instantiate the openreview-py client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).&#x20;
2. Retrieve all of the assignment edges and all of the Paper#/Reviewers groups for your venue. Make two dictionaries: one mapping groups by their IDs, the other mapping edges by their head. The head of each edge is the forum of the paper that the reviewer is assigned to.

```
edges = list(openreview.tools.iterget_edges(client, invitation = 'Your/Venue/ID/Reviewers/-/Assignment'))
papers = list(openreview.tools.iterget_notes(client, invitation = 'Your/Venue/ID/-/Blind_Submission'))
groups = [client.get_group(f"Your/Venue/ID/Paper{paper.number}/Reviewers")for paper in papers]
groups_by_ids = {group.id: group for group in groups}
edges_by_paper = {}
for edge in edges: 
    if edge.head in edges_by_paper: 
        edges_by_paper[edge.head].append(edge.tail)
    else: 
        edges_by_paper[edge.head] = [edge.tail]
```

3\. Iterate through all assignment edges. For each one, check that the reviewer in the assignment edge is in the correct PaperX/Reviewers group. For example, if an edge has a tail of \~User\_One1 and a head corresponding to Paper2, then Paper2/Reviewers should include  \~User\_One1. If the reviewer is not in the correct group, add them.&#x20;

```
count = 0
for edge in tqdm(edges): 
    # Check if reviewer is in paperX reviewer group 
    paper = client.get_note(edge.head)
    reviewer_group = groups_by_ids[f'Your/Venue/ID/Paper{paper.number}/Reviewers']
    if edge.tail not in reviewer_group.members: 
        print(edge.tail, reviewer_group.id, paper.forum)
        count = count+1
        # If edge exists but reviewer is not in group, add reviewer to group 
        reviewer_group = client.add_members_to_group(reviewer_group, edge.tail)
print(count)
```

4\. Now we want to check for the opposite case, where a Reviewer has been added to a group but there is not an associated assignment edge. Retrieve all papers, then create a dictionary with paper numbers as the keys. Iterate through the papers, get the associated Reviewers' group for each, and check that there is an assignment edge for each group member. If there is not, remove the reviewer.&#x20;

```
papers_by_number = {}
for paper in papers: 
    papers_by_number[paper.number] = paper.forum
count = 0
# Go through every PaperX/Reviewers group and for each one check if reviewers have an edge 
# go through each paper, for each one get the reviewers group, for each reviewer in the group see if the edge exists 
for number, forum in papers_by_number.items(): 
    reviewers_group = groups_by_ids[f'Your/Venue/ID/Paper{number}/Reviewers']
    tails = edges_by_paper[forum]
    for reviewer in reviewers_group.members: 
        if reviewer not in tails: 
            count = count + 1
            print(reviewer, reviewers_group.id, forum)
            reviewers_group = client.remove_members_from_group(reviewers_group, reviewer)
print(count)
```
