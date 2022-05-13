# Notes

Most data in OpenReview are represented as _Notes_. Each _Note_ responds to an _Invitation_, which specifies the possible content and permissions that the _Note_ is required to contain.

Users can query notes using the ID of the Invitation that they respond to.

Consider the following example which gets the public _Notes_ that represent the 11th through 20th submissions to ICLR 2019:

```
blind_submissions = client.get_notes(
        invitation='ICLR.cc/2019/Conference/-/Blind_Submission',
        limit=10,
        offset=10)
```

By default, _get\_notes_ will return up to the first 1000 corresponding _Notes (limit=1000)._ To retrieve Notes beyond the first 1000, you can adjust the offset parameter, or use the function client.get\_all\_notes which returns a list of all corresponding _Notes_:

```
blind_submissions = client.get_all_notes(invitation='ICLR.cc/2019/Conference/-/Blind_Submission')
```

It’s also possible to query for Notes that are replies to other Notes. All reviews, decisions, and comments posted to a particular submission will share that submission's forum. To get all of the notes posted to a particular submission forum, you can do the following:&#x20;

```
notes = client.get_all_notes(forum = "")
```

If you would like to get all types of replies for a Conference, like all Reviews, you can use details = replies. Consider the following example that gets all _Official\_Comments_ for the ICLR 2021 conference:

```
blind_submissions = client.get_all_notes(invitation='ICLR.cc/2021/Conference/-/Blind_Submission', details='replies')
comments = [] 
for submission in blind_submissions:
    comments = comments + [reply for reply in submission.details["replies"] if reply["invitation"].endswith("Official_Comment")]
```

This code returns public comments left on ICLR 2021 Blind Submissions with invitations such as ICLR.cc/2021/Conference/Paper1234/-/Official\_Comment_._

Invitation IDs follow a loose convention that resembles the one in the example above: the ID of the conference (e.g. ICLR.cc/2019/Conference) and an identifying string (e.g. Blind\_Submission), joined by a dash (-). Older conferences often use the format ConferenceID/-/Paper#/Official_\__Comment, whereas newer venues use the format ConferenceID/Paper#/-/Official\_Comment.

Invitations can be queried with the _get\_invitations_ function to find the Invitation IDs for a particular conference. The following example retrieves the first 10 Invitations for the ICLR 2019 conference:

```
iclr19_invitations = client.get_invitations(
        regex='ICLR.cc/2019/Conference/.*',
        limit=10,
        offset=0)
```

Like _get\_notes_, _get\_invitations_ will return up to the first 1000 Invitations (_limit=1000_). To retrieve Invitations beyond the first 1000, you can adjust the _offset_ parameter, or use the function tools.iterget\_invitations:

```
iclr19_invitations_iterator = openreview.tools.iterget_invitations(
        client, regex='ICLR.cc/2019/Conference/.*')
```

The data of a note is stored in the "content" of that note. For example, the actual decision is stored in the content of decision notes and can be accessed like this:&#x20;

```
decision = decision_note.content["decision"]
```
