# Getting all Submissions

### API 2 venues

Get all the submission notes of your venue regardless of their status (active, withdrawn or desk rejected). To do this you'll need to pass the submission invitation using get\_all\_notes().

```
submissions = client.get_all_notes(
    invitation="Your/Venue/ID/-/Submission"
    )
```



To only get "accepted" submissions, you'll need to query the notes by venueid.

```
submissions = client.get_all_notes(content={'venueid':{'Your/Venue/ID'}} )
```

To get active submissions under review:

```
submissions = client.get_all_notes(content={'venueid':{'Your/Venue/ID/Submission'}} )
```

To get withdrawn submissions:

```
submissions = client.get_all_notes(content={'venueid':{'Your/Venue/ID/Withdrawn_Submission'}} )
```

To get desk-rejected submissions:

```
submissions = client.get_all_notes(content={'venueid':{'Your/Venue/ID/Desk_Rejected_Submission'}} )
```

Parameters you can use when [querying API 2 notes](https://docs.openreview.net/reference/api-v2/openapi-definition#notes).

### API 1 venues

To get all submissions for a double-blind venue regardless of their status (active, withdrawn or desk rejected)**,** pass your venue's blind submission invitation into get\_all\_notes_._

```
submissions = client.get_all_notes(
    invitation="Your/Venue/ID/-/Blind_Submission"
    )
```

To get all submissions for a single-blind venue regardless of their status (active, withdrawn or desk rejected), pass your venue's submission invitation to get\_all\_notes().

```
submissions = client.get_all_notes(
    invitation="Your/Venue/ID/-/Submission"
    )
```

To get only the "accepted" submissions, query using the venueid.

```
submissions = client.get_all_notes(content={'venueid':'Your/Venue/ID'} )
```

Parameters you can use when [querying API 1 notes](https://api.openreview.net/notes).
