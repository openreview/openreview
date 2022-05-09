# Getting all Decisions

To get all decisions for a venue, you can do the following:&#x20;

1. Get all submissions for your venue. You can do this by passing your venue's submission invitation into get\_all\_notes_._ You should also pass in details = "directReplies" to obtain any notes that reply to each submission.&#x20;

```
submissions = list(client.get_all_notes(invitation="Your/Venue/ID/-/Submission", details='directReplies'))
```

2\. For each submission, add any replies with the Decision invitation to a list of decisions.&#x20;

```
decisions = [] 
for submission in submissions:
    decisions = decisions + [reply for reply in submission.details["directReplies"] if reply["invitation"].endswith("Decision")]
    
    
```

3\. The decisions list now contains all of the decisions for your venue.
