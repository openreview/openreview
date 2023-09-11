# Getting all MetaReviews

To get all meta-reviews for a venue, you can do the following:&#x20;

1. Get all submissions for your venue. You can do this by passing your venue's submission invitation into get\_all\_notes_._ You should also pass in details = "directReplies" to obtain any notes that reply to each submission.&#x20;

```python
submissions = client.get_all_notes(
    invitation="Your/Venue/ID/-/Submission",
    details='directReplies'
)
```

2\. For each submission, add any replies with the Meta-Review invitation to a list of meta-reviews.&#x20;

```python
metareviews = [] 
for submission in submissions:
    metareviews = metareviews + [reply for reply in submission.details["directReplies"] if reply["invitation"].endswith("Meta_Review")]
```

3\. The  metareviews list now contains all of the meta-reviews for your venue.
