# Computing Affinity Scores and Conflicts for large venues

**Note:** This document is for large venues (>2000 submissions) that need to manually calculate scores. Smaller venues should refer to the directions [here](../../workflows/conferences.md).

### Overview

The sections below give detailed step-by-step instructions on how to complete this process. Below is an overview and important information about the process.&#x20;

There are 3 separate steps:

1. Compute affinity scores  with `request_expertise(...)`
2. Upload affinity scores with  `venue.setup_committee_matching(compute_affinity_scores=...)`
3. Compute and upload conflicts with  `venue.setup_committee_matching(compute_conflicts=..., compute_conflicts_n_years=...)`&#x20;

{% hint style="info" %}
Computing and uploading scores are two separate steps because you first have to call to the Expertise API, then retrieve the scores to upload later as CSV. However, for conflicts, `setup_committee_matching` handles both the computation and the upload of conflicts, so there is no need to run it twice.
{% endhint %}

#### **Estimated times:**

The times depend on the number of Edges being posted. The larger the venue, the more edges you will post.

* Compute affinity scores: \~12+ hours
* Upload affinity scores: \~3+ hours
* Compute and upload conflicts: \~3+ hours

{% hint style="info" %}
Make sure you have good internet connection since these processes take a long time. This also means you shouldn't upload scores and compute conflicts at the same time.
{% endhint %}

### 1. Setup

* Create all the necessary invitations by calling `setup_committee_matching` and passing no parameters:

```python
venue = openreview.helpers.get_conference(client_v1, request_form_id) # Use API 1 client
venue.setup_committee_matching()
```

### 2. Computing scores

If SACs are assigned to ACs, you can compute scores in the request form using Paper Matching Setup.

For computing any role (SACs, ACs, Reviewers, etc) against submissions, call the expertise function below:

```python
venue = openreview.helpers.get_conference(client_v1, request_form_id) # Use API 1 client

role = venue.get_reviewers_id() #Role to compute

# Use API 2 client
job_id = client_v2.request_expertise(
   name='venue-reviewers1', #Name of the scores
   group_id=role,
   venue_id=venue.get_submission_venue_id(),
   expertise_selection_id=f'{role}/-/Expertise_Selection',
   model='specter2+scincl',
   weight=[ #update weights
       {
           "articleSubmittedToOpenReview": True,
           "weight": 2
       },
       {
           "value": "OpenReview.net/Archive",
           "weight": 0.2
       },
       {
           "value": "OpenReview.net/Anonymous_Preprint",
           "weight": 0.2
       }
   ]
)

```

Some important details about `request_expertise`:&#x20;

**Returns**: A string representing the job ID.&#x20;

**Parameters:**

* **name**: Name of the job. Make each job name unique so you can query your job later.
* **group\_id**: The group you for which you are computing scores
* **venue\_id**: This refers to the submission’s venueid field.&#x20;
  * This tells the status of the submission, so this param is how we query submissions.
* **expertise\_selection\_id**: The ID of the role’s Expertise Selection invitation.
  * This is how users exclude publications from their expertise. So this param is saying to take into account any exclusions.
* **model**: The expertise model used to compute scores.
  * We recommend specter2+scincl, but we have many models.
  * Model info: [https://github.com/openreview/openreview-expertise?tab=readme-ov-file#model-descriptions](https://github.com/openreview/openreview-expertise?tab=readme-ov-file#model-descriptions)
* **weight**: List of dictionaries that specify weights for publications.
  * The default weight is 1, but weights can be specified
  * For example, the above config says:&#x20;
    * Weigh OR publications twice as much as others.
    * Downweight Archived and Anonymous Preprint papers.
    * This leaves DBLP papers with the weight of 1.
  * You can also weigh publications from specific venues. The following says:
    * Upweight all NeurIPS publications across all years.
      * Note: uses “prefix”
    * Upweight only ICLR 2025 publications

```python
{
   "prefix": "NeurIPS.cc",
   "weight": 2
},
{
   "value": "ICLR.cc/2025/Conference",
   "weight": 2
}
```

#### Retrieve Job ID

```python
# Use API 2 client
res = client_v2.get_expertise_status(group_id=venue.get_reviewers_id())

for job in res['results']:
    if job['name'] == 'venue-reviewers1':
        print(job['jobId'])
```

### Check the status of score computation

```python
# Use API 2 client
results = client_v2.get_expertise_status(job_id=job_id)
print(results['status'])
```

**Statuses**:

* Completed
* Error
* Running Expertise

If for any reason it errors, you can call `request_expertise` again to recompute scores.

### 3. Retrieve scores and convert to CSV

#### Retrieving scores: When the scores are complete, you can retrieve the scores

```python
results = client_v2.get_expertise_results(job_id)
```

The schema of the results can be found [here](how-to-compute-affinity-scores.md).

#### Convert to CSV

```python
import pandas as pd
pd.DataFrame.from_records(result['results']).to_csv('expertise_results.csv', index=False, header=False)
```

This creates a CSV with columns: Paper ID, Profile ID, Score.

### 3. Upload Scores

```python
venue = openreview.helpers.get_conference(client_v1, request_form_id) # Use API 1 client

no_profiles = venue.setup_committee_matching(
   committee_id=venue.get_reviewers_id(),
   compute_affinity_scores='expertise_results.csv'
)
print(no_profiles)
```

**Returns**: A dictionary showing users with no profiles and no publications.

* These users should either be removed or be reminded to sign up.

{% hint style="info" %}
By assignment time there should be no email members in the group.
{% endhint %}

`setup_committee_matching` params:

* committee\_id: The ID of the group for which you are uploading scores.
* compute\_affinity\_scores: The path to the affinity score CSV.

**If uploading fails:**&#x20;

1. Delete the score edges
2. Check that the edge count is 0
3. Re-run setup\_committee\_matching

```python
# API 2 client
client_v2.delete_edges(invitation='venue.get_affinity_score_id(venue.get_reviewers_id())')

client_v2.get_edges_count(invitation='venue.get_affinity_score_id(venue.get_reviewers_id())')
```

### 4. Computing and uploading conflicts

```python
venue = openreview.helpers.get_conference(client_v1, request_form_id) # Use API 1 client

no_profiles = venue.setup_committee_matching(
   committee_id=venue.get_reviewers_id(),
   compute_conflicts='NeurIPS',
   compute_conflicts_n_years=5
)
print(no_profiles)
```

**Returns**: A dictionary showing users with no profiles and no publications.

* These users should either be removed or be reminded to sign up.

{% hint style="info" %}
By assignment time there should be no email members in the group.
{% endhint %}

**setup\_committee\_matching** params:

* **committee\_id**: The ID of the group for which you are computing conflicts.
* **compute\_conflicts**: The conflict policy.
  * We have 2 policies: `Default` and `NeurIPS`.&#x20;
  * More information on policies can be found [here](how-to-do-automatic-assignments/how-to-setup-paper-matching-by-calculating-affinity-scores-and-conflicts.md).
* **compute\_conflicts\_n\_years**: The number of years to consider when looking at a user's History.

#### If uploading fails:&#x20;

1. Delete the conflict edges.
2. Check that the edge count is 0.
3. Re-run setup\_committee\_matching.

```python
# Use API 2 client
client_v2.delete_edges(invitation='venue.get_conflict_score_id(venue.get_reviewers_id())')

client_v2.get_edges_count(invitation='venue.get_conflict_score_id(venue.get_reviewers_id())')
```
