# Notes

Most data in OpenReview are represented as _Notes_. Each _Note_ responds to an _Invitation_, which specifies the possible content and permissions that the _Note_ is required to contain.

Users can query notes using the ID of the Invitation that they respond to.

By default, _get\_notes_ will return up to the first 1000 corresponding _Notes (limit=1000)._ To retrieve Notes beyond the first 1000, you can adjust the `after` parameter, or use the function `client.get_all_notes` which returns a list of all corresponding _Notes._

```python
submissions = client.get_all_notes(invitation="<submission-invitation-id>")
```

It’s also possible to query for Notes that are replies to other Notes. All reviews, decisions, and comments posted to a particular submission will share that submission's forum. To get all of the notes posted to a particular submission forum, you can do the following:&#x20;

```python
notes = client.get_all_notes(forum="<forum-id>")
```

If you would like to get all types of replies for a Conference, like all Reviews, you can use `details=directReplies`.

```python
submissions = client.get_all_notes(
    invitation="<submission-invitation-id>",
    details="directReplies"
)
```

Invitation IDs follow a loose convention that contains the ID of the conference (e.g. `ICLR.cc/2019/Conference`) and an identifying string (e.g. `Submission`), joined by `/-/`. Older conferences often use the format `ConferenceID/-/Paper#/Official_Comment`, whereas newer venues use the format `ConferenceID/Paper#/-/Official_Comment`.
