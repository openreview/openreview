# How to transfer conflicts between AC pairs or triplets

Follow these docs if your venue is using AC pairs or triplets. This is a necessary step before you compute AC to paper assignments.

## Prerequisites and Setup

If you haven't already, [contact OpenReview Support](https://openreview.net/contact) so we can enable the Secondary AC setting for your venue.

You should have your pairs or triplets created based on AC-AC scores. They should be stored in a file that we will be read in the script below.

Compute AC to paper conflicts using [Paper Matching Setup](how-to-do-automatic-assignments/how-to-run-a-paper-matching.md). These are the conflicts we will transfer.

[Create an OpenReview client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).

## Transferring conflicts between AC pairs or triplets

1. Get all AC conflicts and map users to their conflict edges:

[More info on getting edges](../data-retrieval-and-modification/how-to-get-edges-for-conflicts-assignments-custom-max-papers-and-more.md).

```python
venue_id = '<your venue id>'
acs_to_conflicts =  { g['id']['tail']: g['values'] for g in client_v2.get_grouped_edges(
    invitation=f'{venue_id}/Area_Chairs/-/Conflict',
    groupby='tail',
    select='head,tail'
)}
```

2. Map paper IDs to the list of users who have conflicts with that paper:

```python
paper_to_conflicted_acs = defaultdict(set)
for edges in ac_to_conflicts.values():
    for edge in edges:
        paper_to_conflicted_acs[edge['head']].add(edge['tail'])
```

3. Read your pair/triplet file and collect each AC pair/triplet in a list `ac_circles`. We call the AC groupings "circles" to generalize the pair/triplet naming. We also gather the list of ACs in `all_file_acs` to run checks in Step 4. The following example assumes that your file:

* Is a CSV
* Has no column headers
* Columns are: AC1, AC2, AC3, etc.

The following works for AC groupings of any size:

```python
file_name = 'file.csv'
ac_circles = []
all_file_acs = set()

with open(file_name, 'r', newline='') as file:
    csv_reader = csv.reader(file)
    for row in csv_reader:
        members = [ac.strip() for ac in row if ac.strip()]
        if not members:
            continue  # skip empty lines

        ac_circles.append(members)
        all_file_acs.update(members)
```

4. Run checks to ensure that the AC IDs in the file are profile IDs and that they are in the AC group:

```python
ac_circle_profile_ids = [p.id for p in openreview.tools.get_profiles(client_v2, all_file_acs)]

print('AC profile IDs not in file: ', set(ac_circle_profile_ids) - set(all_file_acs))
print('IDs in file that do not map to a profile: ', set(all_file_acs) - set(ac_circle_profile_ids))

ac_mems = client_v2.get_group(f'{venue_id}/Area_Chairs').members
ac_profile_ids = [p.id for p in openreview.tools.get_profiles(client_v2, ac_mems)]

print('ACs in the group that are not in file: ', set(ac_profile_ids) - set(ac_circle_profile_ids))
print('ACs in the file that are not in the group: ', set(ac_circle_profile_ids) - set(ac_profile_ids))
```

5. Loop through the AC circles, gather the list of conflicted papers for each member, and build the conflict edges for any member who is missing the conflict. We avoid duping data by adding the user to the list of conflicted users for that paper:

```python
conflict_edges = []

for circle in ac_circles:
    # Gather paper IDs of all conflicted papers in circle
    circle_paper_conflicts = set()
    for member in circle:
        if member in ac_to_conflicts:
            circle_paper_conflicts.update(edge['head'] for edge in ac_to_conflicts[member])

    # Propagate conflicts to everyone in the circle
    for member in circle:
        for paper_id in circle_paper_conflicts:
            # If user has no conflict with this paper, create new edge
            if member not in paper_to_conflicted_acs[paper_id]:
                conflict_edges.append(openreview.api.Edge(
                    invitation=f'{venue_id}/Area_Chairs/-/Conflict',
                    label='Pair Conflict',
                    weight=-1,
                    head=paper_id,
                    tail=member,
                    signatures=[venue_id],
                    readers=[venue_id, member],
                    writers=[venue_id]
                ))

                # Add user to conflict list so we don't dupe data
                paper_to_conflicted_acs[paper_id].add(member)
```

6. Post the new conflicts:

```python
print(len(conflict_edges))
openreview.tools.post_bulk_edges(client=client_v2, edges=conflict_edges)
```
