---
description: >-
  This page is for manually computing affinity scores using Python to call our
  Expertise API.
---

# How to Compute Affinity Scores Manually

## Introduction

Affinity, or similarity, scores are numbers between 0 and 1 that are used to create matches between the different OpenReview objects. Scores between users and submissions are usually implicitly computed as part of the [**Paper Matching Setup**](https://docs.openreview.net/how-to-guides/paper-matching-and-assignment/how-to-do-automatic-assignments/how-to-setup-paper-matching-by-calculating-affinity-scores-and-conflicts). However, there may be times where you would like to compute scores in a way that is not offered by the previously mentioned step, like scores between two groups of users or scores between a group of users and desk rejected submissions. This can be done by querying our Expertise API through the Python client.

{% hint style="info" %}
**Note**: Users must have readership permission to the objects they are trying to compute the scores. The Expertise API uses the permission of the user to gather the data from the OpenReview API.
{% endhint %}

## Requesting Scores for two venue groups

First, log into OpenReview using the Python client:

```python
client = openreview.api.OpenReviewClient(
    baseurl='https://api2.openreview.net', 
    username=username,
    password=password
)
```

Next, request a job using the `request_expertise` function of the client. In the following example, we'll request a job that computes scores between the area chairs:

<pre class="language-python"><code class="lang-python"><strong>response = client.request_expertise(
</strong>    name='AC_Scores',
    group_id='Conference/Year/Area_Chairs',
    venue_id=None,
    alternate_match_group='Conference/Year/Area_Chairs',
    expertise_selection_id='Conference/Year/Area_Chairs/-/Expertise_Selection',
    alternate_expertise_selection_id='Conference/Year/Area_Chairs/-/Expertise_Selection',
    model='specter2+scincl',
)
</code></pre>

{% hint style="danger" %}
**`name`**, **`group_id`**, and **`venue_id`** are required arguments
{% endhint %}

The list below describes the arguments for the function call above:

* **`name`** is a string to describe the type of scores you are computing.
* **`group_id`** is the ID of a group where the publications of all the members will be retrieved.
* **`venue_id`** is the value of the `venueid` property for each submission. The value of this distinguishes between active, withdrawn and desk-rejected submissions.

{% hint style="info" %}
In this case, **`venue_id`** is `None` because we're looking to compute scores between users, instead of users to submissions
{% endhint %}

* **`alternate_match_group`** is the ID of the group which you would like to score against the first group. If this value is not `None`, then the Expertise API will perform a user-to-user score computation
* **`expertise_selection_id`** is the ID of the Expertise Selection invitation for the `group_id`. This is how the expertise excludes publications that the users selected.
* **`alternate_expertise_selection_id`** is the ID of the Expertise Selection invitation for the `alternate_match_group`. This is how the expertise excludes publications that the users selected.
* **`model`** is the name of the model that will be used to compute the scores. We support several models that you find described [here](https://github.com/openreview/openreview-expertise), but we strongly suggest that you use `specter2+scincl`.

The `response` variable will contain a dictionary with your job ID, for example: `{'jobId': '12345'}`. This is the value that you will use to query the status and results of your scores.

{% hint style="warning" %}
When computing scores between SACs and ACs, the `group_id` should be the SACs and the `alternate_match_group` should be the ACs.
{% endhint %}

## Requesting scores between a group and all papers

You can use `request_expertise` to compute scores between any group and papers. The following example computes scores between reviewers and active papers:

```python
response = client.request_expertise(
   name='venue-reviewers1',
   group_id='Conference/Year/Reviewers',
   venue_id='Conference/Year/Submission',
   expertise_selection_id='Conference/Year/Reviewers/-/Expertise_Selection',
   model='specter2+scincl'
)
```

## Requesting Scores for a subset of submissions

Use `request_paper_subset_expertise` function of the client to compute affinity scores for a subset of submissions and a venue group:

<pre class="language-python"><code class="lang-python"><strong>response = client.request_paper_subset_expertise(
</strong>    name='AC_Subset_Scores',
    submissions=[list of submissions],
    group_id='Conference/Year/Area_Chairs',
    expertise_selection_id='Conference/Year/Area_Chairs/-/Expertise_Selection',
    model='specter2+scincl',
)
</code></pre>

This job will compute affinity scores between all the area chairs of the conference and the list of submissions passed as a parameter. The submission list should be a list of note objects, you can get them by using the function client.get\_notes() or client.get\_all\_notes() or you can follow this [link](../data-retrieval-and-modification/how-to-get-all-notes-for-submissions-reviews-rebuttals-etc.md) for more information.

## Requesting Scores for a subset of users

Use `request_user_subset_expertise` function of the client to compute affinity scores for all the submissions and a subset of users:

<pre class="language-python"><code class="lang-python"><strong>response = client.request_user_subset_expertise(
</strong>    name='AC_Subset_Scores',
    members=['~Area_Chair1', '~Area_Chair2'],
    venue_id='Conference/Year/Submission',
    expertise_selection_id='Conference/Year/Area_Chairs/-/Expertise_Selection',
    model='specter2+scincl',
)
</code></pre>

This job will compute affinity scores between a subset of area chairs and all the active submissions.

## Weighing publications

All of these request expertise functions can take an optional `weight` argument specifying how certain types of publications should be weighted in someone's expertise. `weight` is a list of dictionaries. The default weight is 1 for all publications.

For example, you can pass the following weights:

```python
weight = [
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
```

This config tells the expertise to:

* Weigh OpenReview publications twice as much as others.
* Down-weight Archived and Anonymous Preprint papers.
* DBLP papers would use the default weight of 1.

You can also weigh publications from specific venues. For example:

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

This config tells the expertise to:

* Upweight all NeurIPS publications across all years.
  * Use `prefix` to match all years of a venue.
* Upweight _only_ ICLR 2025 Conference publications.

## Check the status of the job

Each request of expertise will return a job id that you can use to get the status and to retrieve the results.

```python
job_id = response['jobId']
status = client.get_expertise_status(job_id)
```

Once the status is complete then you can start downloading the scores.

## Retrieving The Scores

You can use the function `get_expertise_results` to fetch your scores. Here is an example of the function call:

```python
results = client.get_expertise_results(
    '12345',
    wait_for_complete=True
)
```

The first argument is the `jobId` that you received from your call to `request_expertise`. The **wait\_for\_complete** argument will continue to query the Expertise API automatically until the scores are complete. When the scores are complete, `get_expertise_results` will return a dictionary with the following schema:

```json
{
    'metadata': {
        'no_profile': string[],
        'no_profile_submission': string[],
        'no_publications': string[],
        'no_publications_count': int,
        'submission_count': int
    }, 
    'results': Object[]
}
```

The scores for our example will be stored in `results['results']`.&#x20;

* If you computed user-to-user scores, the objects in this array may look like: `{'match_member': '~UserA1', 'score': 0.61, 'submission_member': '~UserB1'}`, where:
  * `match_member` is a member from the group whose ID is `group_id`.
  * `submission_member` is a member from the group whose ID is `alternate_match_group`.
* If you computed a user-to-submission scores, the objects in this array may look like: `{'submission': 'paperID1', 'user': '~UserA1', 'score': 0.90}`, where:
  * `submission` is the paper ID.
  * `user` is a member from the group whose ID is `group_id` or from the `members` list if you [computed scores with a subset of users](how-to-compute-affinity-scores.md#requesting-scores-for-a-subset-of-users).

## Converting expertise results to a CSV

You may need to convert the expertise results to a CSV so you can upload them to the live site. For scores between:

* Users and papers, the columns should be: paper ID, profile ID, score
* SACs to ACs, the columns should be: AC profile ID, SAC profile ID, score

If you set the group IDs correctly in the expertise request, you can just run the following to convert results to a CSV:

```python
import pandas as pd
pd.DataFrame.from_records(result['results']).to_csv('expertise_results.csv', index=False, header=False)
```

Otherwise, if you for example mixed up the group IDs for SACs and ACs, you may need to switch the columns before upload.

### Uploading scores from a CSV

To upload scores, call `venue.setup_committee_matching()` and pass:

* **`committee_id`**: The ID of the group for which you are uploading scores.
* **`compute_affinity_scores`**: The path to the affinity score CSV.

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

## Converting expertise results to edges

You may need to convert them to edges for upload. Example use cases:

* You computed scores for a subset of users to all papers
* You computed scores for all users to a subset of papers
* Or you just want to manually upload affinity score edges

In this example, we'll create Area Chair affinity score edges to papers.

First, **check the affinity score invitation** to see how the edge is structured, such as the `head`, `tail`, and `readers`. You can view the invitation by going to: `https://openreview.net/invitation/edit?id=venue_id/role_name/-/Affinity_Score`.&#x20;

Next, use the expertise results to create a list of lists where each sublist contains the data for 1 edge.

```python
scores = [
    [ entry['submission'], entry['user'], entry['score'] ]
    for entry in results['results']
]
```

Then, we will loop through `scores` and create an Edge for each item.&#x20;

Since we're creating Area Chair affinity scores, we'll use the following configuration:

* **`invitation`**: The Area Chair affinity score invitation ID.
* **`head`**: The ID of the paper.
* **`tail`**: The profile ID of the Area Chair.
* **`weight`**: The score between the paper and user.
* **`readers`**: List containing: venue ID, the SAC group ID, and the tail of this edge.
* **`writers`**: List containing: venue ID.
* **`signatures`**: List containing: the Program Chair group ID.

{% hint style="warning" %}
This is only an example, you **must** refer to the invitation for the correct edge configuration.
{% endhint %}

```python
edges = []
for score_line in scores:
    paper_note_id, profile_id, score = score_line
    formatted_score = max(round(float(score), 4), 0) # Round to 4 decimal places, make it non-negative
    edges.append(openreview.api.Edge(
        invitation = "venue_id/Area_Chairs/-/Affinity_Score",
        head = paper_note_id,
        tail = profile_id,
        weight = formatted_score,
        readers = [
            "venue_id",
            "venue_id/Senior_Area_Chairs",
            profile_id
        ],
        writers = ["venue_id"]
        signatures = ["venue_id"]
    ))
```

Now you have a list of edges ready to upload, follow [How to Upload Edges in Bulk](how-to-upload-edges-in-bulk.md).
