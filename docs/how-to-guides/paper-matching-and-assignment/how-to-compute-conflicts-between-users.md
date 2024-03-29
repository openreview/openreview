# How to Compute Conflicts Between Users

In general, conflicts can and should be computed using [Paper Matching Setup](how-to-do-automatic-assignments/how-to-setup-paper-matching-by-calculating-affinity-scores-and-conflicts.md). However, there may be cases where you do not want to re-compute conflicts for all reviewers and papers; for example, if an author requested to add new coauthors after reviewers were already assigned to their paper. You can use the python client to manually check for conflicts between the reviewers and the new authors like so:&#x20;

1. If you have not done so, you will need to [install and instantiate the openreview-py client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).&#x20;
2. Get the note that you are interested in computing conflicts for.&#x20;

```python
note = client.get_note(<submission_id>)
```

3\. Get the profiles of the authors of the submission and the reviewers. The reviewer group id should be something like your conference id/Paper#/Reviewers, for example robot-learning.org/CoRL/2022/Conference/Paper99/Reviewers if the submission of interest is paper number 99.

```python

author_profiles = openreview.tools.get_profiles(
    client, 
    submission.content['authorids']['value'],
    with_publications=True
)
reviewers = openreview.tools.get_profiles(
    client,
    client.get_group(reviewer_group_id).members,
    with_publications=True
)
```

4\. Compute the conflicts. If no conflicts are found, an empty array will be printed. Otherwise, the array will contain the shared groups that put them in conflict with each other.&#x20;

```python
for reviewer in reviewers:
    print(reviewer.id)
    conflicts = openreview.tools.get_conflicts(
        author_profiles,
        reviewer,
        policy='default',
        n_years=5
    )
    print(conflicts)
```
