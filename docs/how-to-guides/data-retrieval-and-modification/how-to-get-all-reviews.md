# How to Get All Reviews

## Venues using API V2

#### To get all reviews for a single-blind and double-blind venue, you can do the following:

1. Get all submissions for your venue. You can do this by passing your venue's submission invitation into get\_all\_notes_._ You should also pass in details = "directReplies" to obtain any notes that reply to each submission or details = "replies" to get all notes (direct replies and replies of replies that are nested).

```python
venue_id = 'Your/Venue/ID'
venue_group = client.get_group(venue_id)
submission_name = venue_group.content['submission_name']['value']
submissions = client.get_all_notes(invitation=f'{venue_id}/-/{submission_name}', details='replies')
```

2\. For each submission, add any replies with the review invitation to a list of reviews.&#x20;

```python
review_name = venue_group.content['review_name']['value']

reviews=[openreview.api.Note.from_json(reply) for s in submissions for reply in s.details['replies'] if f'{venue_id}/{submission_name}{s.number}/-/{review_name}' in reply['invitations']]
```

3\. The list reviews now contains all of the reviews for your venue.

## Venues using API V1

#### To get all reviews for a double-blind venue, you can do the following:&#x20;

1. Get all submissions for your venue. You can do this by passing your venue's submission invitation into get\_all\_notes_._ You should also pass in details = "directReplies" to obtain any notes that reply to each submission.&#x20;

```python
submissions = client.get_all_notes(
    invitation="Your/Venue/ID/-/Blind_Submission",
    details='directReplies'
)
```

2\. For each submission, add any replies with the Official Review invitation to a list of Reviews.&#x20;

```python
reviews = [] 
for submission in submissions:
    reviews = reviews + [reply for reply in submission.details["directReplies"] if reply["invitation"].endswith("Official_Review")]
```

3\. The list reviews now contains all of the reviews for your venue.

#### To get all reviews for a single-blind venue, you can do the following:

1. Get all submissions for your venue. You can do this by passing your venue's submission invitation into get\_all\_notes_._ You should also pass in details = "directReplies" to obtain any notes that reply to each submission.&#x20;

```python
submissions = client.get_all_notes(
    invitation="Your/Venue/ID/-/Submission",
    details='directReplies'
)
```

2\. For each submission, add any replies with the Official Review invitation to a list of Reviews.&#x20;

```python
reviews = [] 
for submission in submissions:
    reviews = reviews + [reply for reply in submission.details["directReplies"] if reply["invitation"].endswith("Official_Review")]
```

3\. The list reviews now contains all of the reviews for your venue.

