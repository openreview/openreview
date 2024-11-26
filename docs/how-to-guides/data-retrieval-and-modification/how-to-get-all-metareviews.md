# How to Get All MetaReviews

To get all meta-reviews for a venue, you can do the following:&#x20;

1. Get all submissions for your venue. You can do this by passing your venue's submission invitation into `get_all_notes`_._ You should also pass in `details = "directReplies"` to obtain any Notes that are a direct reply to each submission.&#x20;

```python
venue_id = 'Your/Venue/ID'
venue_group = client.get_group(venue_id)
submission_name = venue_group.content['submission_name']['value']
submissions = client.get_all_notes(invitation=f'{venue_id}/-/{submission_name}', details='directReplies')
```

2\. For each submission, add any replies with the Meta-Review invitation to a list of meta-reviews.&#x20;

```python
meta_review_name = venue_group.content['meta_review_name']['value']

meta_reviews=[openreview.api.Note.from_json(reply) for s in submissions for reply in s.details['directReplies'] if f'{venue_id}/{submission_name}{s.number}/-/{meta_review_name}' in reply['invitations']]
```

3\. The meta\_reviews list now contains all of the meta-reviews for your venue.
