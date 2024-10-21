# How to Compute Conflicts Between Users

In general, conflicts can and should be computed using [Paper Matching Setup](how-to-do-automatic-assignments/how-to-setup-paper-matching-by-calculating-affinity-scores-and-conflicts.md). However, there may be cases where you do not want to re-compute conflicts for all reviewers and submissions or you want to use your own conflict policy to compute conflicts between users and submissions. You can use the python client to manually check for conflicts between the reviewers and authors like so:&#x20;

### Compute conflicts between reviewers and authors of a single submission

1. If you have not done so, you will need to [install and instantiate the openreview-py client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).&#x20;
2. Get the note that you are interested in computing conflicts for.&#x20;

```python
note = client.get_note(<submission_id>)
```

3\. Get profiles for the submission authors and the reviewers. The reviewer group id should be something like your conference id/Submission#/Reviewers, for example if your submission of interest is submission 99:

```python
reviewer_group_id = "robot-learning.org/CoRL/2022/Conference/Submission99/Reviewers"
```

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

4\. Compute the conflicts. If no conflicts are found, `get_conflicts` will return an empty list. Otherwise, the list will contain the shared groups that put them in conflict with each other.&#x20;

```python
for reviewer in reviewers:
    conflicts = openreview.tools.get_conflicts(
        author_profiles,
        reviewer,
        policy='default',
        n_years=5
    )
```

5. The last step is posting the conflict Edges. You can follow this [guide to post custom conflicts](how-to-post-a-custom-conflict.md), and keep the label as 'Conflict'.

### Compute conflicts between reviewers and authors of all submissions

1. [install and instantiate the openreview-py client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).&#x20;
2. Get all submissions, authorids of all submissions, and the author and reviewer Profiles. If you're using a custom policy that does not need the author's publications, you can exclude `with_publications=True`

```python
venue_id = '<add your venueID>'

all_submissions = client.get_all_notes(invitation=f'{venue_id}/-/Submission')
author_ids = set()
for submission in all_submissions:
    author_ids.update(submission.authorids['value'])

reviewer_ids = client.get_group(f'{venue_id}/Reviewers').members

author_ids = list(author_ids)

author_profiles = openreview.tools.get_profiles(client, author_ids, with_publications=True)
reviewers = openreview.tools.get_profiles(client, reviewer_ids, with_publications=True)
```

3. Get conflicts for each reviewer.

{% code overflow="wrap" %}
```python
for reviewer in reviewers:
    conflicts_for_reviewer = openreview.tools.get_conflicts(author_profiles, reviewer, policy=own_policy, n_years=None) # Returns a list of conflicts
```
{% endcode %}

4. The last step is posting the conflict Edges. You can follow this [guide to post custom conflicts](how-to-post-a-custom-conflict.md), and keep the label as 'Conflict'.
