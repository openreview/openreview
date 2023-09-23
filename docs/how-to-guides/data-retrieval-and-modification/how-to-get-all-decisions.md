# How to Get All Decisions

To get all decisions for a venue, you can do the following:&#x20;

1. Get all submissions for your venue. You can do this by passing your venue's submission invitation into get\_all\_notes_._ You should also pass in details = "directReplies" to obtain any notes that reply to each submission.&#x20;

```python
# API V2
venue_group_settings = client.get_group(venue_id).content
submission_invitation = venue_group_settings['submission_id']['value']
submissions = client.get_all_notes(
    invitation=submission_invitation,
    details='directReplies'
)

# API V1 (Single Blind Submissions)
submissions = client.get_all_notes(
    invitation="Your/Venue/ID/-/Submission",
    details='directReplies'
)

# API V1 (Double Blind Submissions)
submissions = client.get_all_notes(
    invitation="Your/Venue/ID/-/Blind_Submission",
    details='directReplies'
)

```

2\. For each submission, add any replies with the Decision invitation to a list of decisions. Depending on the API version, the code varies slightly.

```python
decisions = []
   
# API V2
venue_group_settings = client.get_group(venue_id).content
decision_invitation_name = venue_group_settings['decision_name']['value']
for submission in submissions:
    for reply in submission.details['directReplies']:
        if any(invitation.endswith(f'/-/{decision_invitation_name}') for invitation in reply['invitations']):
            decisions.append(reply)
            
# API V1
for submission in submissions:
    decisions = decisions + [reply for reply in submission.details["directReplies"] if reply["invitation"].endswith("Decision")]
```

3\. The decisions list now contains all of the decisions for your venue.
