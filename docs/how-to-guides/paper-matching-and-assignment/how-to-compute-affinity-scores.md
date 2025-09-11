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
* **`model`** is the name of the model that will be used to compute the scores. We support several models that you find described [here](https://github.com/openreview/openreview-expertise), but we strongly suggest that you use `specter2+scincl`.

The `response` variable will contain a dictionary with your job ID, for example: `{'jobId': '12345'}`. This is the value that you will use to query the status and results of your scores.

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

## Converting expertise results to edges

Once you retrieve the results, you may need to convert them to edges for upload. In this example, we'll create Area Chair affinity scores to papers.

First, check the affinity score invitation to see how the edge is structured, such as the `head`, `tail`, and `readers`. You can view the invitation by going to: `https://openreview.net/invitation/edit?id=venue_id/role_name/-/Affinity_Score`.&#x20;

Next, use the expertise results to create a list of lists where each sublist contains the data for 1 edge.

```python
scores = [
    [ entry['submission'], entry['user'], entry['score'] ]
    for entry in results['results']
]
```

Then, we will loop through `scores` and create an Edge for each item.&#x20;

Since we're creating Area Chair affinity scores, we'll use the following configuration:

* **invitation**: The Area Chair affinity score invitation ID.
* **head**: The ID of the paper.
* **tail**: The profile ID of the Area Chair.
* **weight**: The score between the paper and user.
* **readers**: List containing: venue ID, the SAC group ID, and the tail of this edge.
* **writers**: List containing: venue ID.
* **signatures**: List containing: the Program Chair group ID.

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
