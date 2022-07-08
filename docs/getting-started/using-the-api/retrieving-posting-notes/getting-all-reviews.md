# Getting all Reviews

#### To get all reviews for a double-blind venue, you can do the following:&#x20;

1. Get all submissions for your venue. You can do this by passing your venue's submission invitation into get\_all\_notes_._ You should also pass in details = "directReplies" to obtain any notes that reply to each submission.&#x20;

```
submissions = client.get_all_notes(invitation="Your/Venue/ID/-/Blind_Submission", details='directReplies')
```

2\. For each submission, add any replies with the Official Review invitation to a list of Reviews.&#x20;

```
reviews = [] 
for submission in submissions:
    reviews = reviews + [reply for reply in submission.details["directReplies"] if reply["invitation"].endswith("Official_Review")]
```

3\. The list reviews now contains all of the reviews for your venue.

#### To get all reviews for a single-blind venue, you can do the following:&#x20;



1. Get all submissions for your venue. You can do this by passing your venue's submission invitation into get\_all\_notes_._ You should also pass in details = "directReplies" to obtain any notes that reply to each submission.&#x20;

```
submissions = client.get_all_notes(invitation="Your/Venue/ID/-/Submission", details='directReplies')
```

2\. For each submission, add any replies with the Official Review invitation to a list of Reviews.&#x20;

```
reviews = [] 
for submission in submissions:
    reviews = reviews + [reply for reply in submission.details["directReplies"] if reply["invitation"].endswith("Official_Review")]
```

3\. The list reviews now contains all of the reviews for your venue.
