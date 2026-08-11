# How to Edit the Source of an Invitation

Invitations use a `source` field to define which notes (submissions or replies) the invitation applies to. The `source` field is a JSON object, so Program Chairs can scope an invitation with far more precision than a simple "all submissions" or "accepted submissions only" toggle.

### Supported `source` keys

* `venueid`: An array of invitation ids. Only notes whose `venueid` matches one of these values are included. Defaults to the venue's active venue ids (e.g. the venue's Submission and Accepted/Rejected Submission venue ids).
* `with_decision_accept`: A boolean. When `True`, only submissions with an accepted decision are included.
* `decision_options`: An array of decision values (e.g. `['Accept']` or `['Accept', 'Accept (Poster)']`). Only submissions whose decision exactly matches one of these values are included. Use this instead of `with_decision_accept` when you need to target specific decision options rather than "accepted" as a whole.
* `reply_to`: The name of the invitation that the note must be a reply to (e.g. `'Official_Review'`).
* `readers`: An array of readers the note must have (e.g. `['everyone']`).
* `content`: An object of content field/value pairs that the note's content must match.

Example combining several keys to create Camera-Ready invitations for papers with `Accept (Poster)` decision only:

```python
source = {
    'venueid': [
        'ICLR.cc/2025/Conference/Submission',
        'ICLR.cc/2025/Conference',
        'ICLR.cc/2025/Conference/Rejected_Submission'
    ],
    'decision_options': ['Accept (Poster)']
}
```

Example scoping a `Meta_Review_Confirmation` invitation so it's only offered as a reply to Meta Reviews on submissions with Submission venueid:

```python
source = {
    'venueid': ['ICLR.cc/2025/Conference/Submission'],
    'reply_to': 'Meta_Review'
}
```

See also: [Introductions to Invitations](../../getting-started/objects-in-openreview/introductions-to-invitations.md), [Using the CustomStage](using-the-customstage.md), [Submission Revision Stage](../../reference/stages/submission-revision-stage.md).
