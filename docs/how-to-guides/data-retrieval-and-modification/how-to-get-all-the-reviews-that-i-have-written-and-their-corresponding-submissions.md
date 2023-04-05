# How to get all the Reviews that I have written and their corresponding Submissions

OpenReview does not currently provide an official document that certifies that a Reviewer has participated in the review process of a conference. However, it is still possible to retrieve the **public** **reviews** and **public** **submissions** that the Reviewer reviewed using the [python client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md) from OpenReview.

### Steps for getting Reviews

1. Instantiate your OpenReview client

```python
client = openreview.api.OpenReviewClient(
    baseurl='https://api2.openreview.net',
    username=<your username>,
    password=<your password>
)
```

2. Run the following method

```python
result = openreview.tools.get_own_reviews(client)

# Result is a list of dictionaries that have the following schema
# {
#     'submission_title': <Submission title>,
#     'submission_link': <Link to submission>,
#     'review_link': <Link to review>
# }
```
