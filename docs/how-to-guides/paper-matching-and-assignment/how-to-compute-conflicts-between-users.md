# How to Compute Conflicts Between Users

### Compute conflicts between reviewers and authors of a single submission

If you have not done so, you will need to [install and instantiate the openreview-py client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).&#x20;

{% hint style="info" %}
Read for more information about [conflict policies](how-to-do-automatic-assignments/how-to-setup-paper-matching-by-calculating-affinity-scores-and-conflicts.md).
{% endhint %}

### Compute conflicts between multiple reviewers and one submission

1. Get the note that you are interested in computing conflicts for.&#x20;

```python
note = client.get_note(<submission_id>)
```

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
    with_publications=True,
    with_relations=True
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

### Compute conflicts between multiple reviewers and all submissions

1. Get [all active submissions under review](broken-reference) and save them to a variable "submissions".
2. Get the profiles for the new reviewers.

```python
reviewers = openreview.tools.get_profiles(
    client,
    ids_or_emails=['reviewer1@gmail.com', '~Reviewer_Name1'],
    with_publications=True,
    with_relations=True
)
```

3. For each submission, get the author\_profiles and then get conflicts for each reviewer.

```python
for submission in submissions:
    author_profiles = openreview.tools.get_profiles(
        client, 
        submission.content['authorids']['value'],
        with_publications=True
    )
    for reviewer in reviewers:
        #print(reviewer.id, " conflicts computed")
        conflicts = openreview.tools.get_conflicts(
            author_profiles,
            reviewer,
            policy='default',
            n_years=5
        )
        #print(conflicts)
```

Getting the author profiles for each submission can be inefficient if the venue has too many submissions, you can first query all the author profile ids and get all the profiles together:\


```
all_authorids = []
for submission in submissions:
    authorids = submission.content['authorids']['value']
    all_authorids = all_authorids + authorids
    
author_profile_by_id = openreview.tools.get_profiles(client, list(set(all_authorids)), with_publications=True, with_relations=True, as_dict=True)

```
