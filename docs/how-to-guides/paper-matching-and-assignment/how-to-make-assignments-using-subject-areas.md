# How to Make Assignments using Subject Areas

There are cases of program organizers wanting to enable authors and reviewers to select one or more subject areas/disciplines and assign reviewers to papers by subject area. This can be accomplished by creating a new "score" to be used in the matching process.  This process can also be applied to Area Chairs and you can follow the same steps replacing Reviewers with Area Chairs.

You can use Subject Scores in addition to Affinity Scores. The benefit of calculating affinity scores is the score is based on the reviewer's publications, and Reviewers may select a subject area they desire without the publication history in that subject.

### Submission form customization

First, you will need to add a field to the submission form that will allow authors to select the subject areas for their submission.

```json
"subject_area": {
        "order": 3,
        "description": "Please select one primary Machine Learning subject area.",
        "value": {
            "param": {
                "type": "string",
                "enum": [
                    "Bayesian Inference",
                    "Computer Vision",
                    "Deep Learning",
                    "Graph Mining",
                    "Natural Language Processing",
                    "Reinforcement Learning",
                    "Representation Learning",
                    "Statistics",
                    "Supervised Learning",
                    "Unsupervised Learning"
                ],
                "input": "radio"
            }
        }
    }
```

If you're allowing authors to select more than one value, change `type` to `"type": "string[]"` and use `items` instead of `enum`.

```json
"subject_area": {
        "order": 3,
        "description": "Please select one primary Machine Learning subject area.",
        "value": {
            "param": {
                "type": "string[]",
                "items": [
                  { "value": "Bayesian Inference", "description": "Bayesian Inference", "optional": true},
                  { "value": "Computer Vision", "description": "Computer Vision", "optional": true},
                  { "value": "Deep Learning", "description": "Deep Learning", "optional": true},
                  { "value": "Graph Mining", "description": "Graph Mining", "optional": true},
                  { "value": "Natural Language Processing", "description": "Natural Language Processing", "optional": true}
                ],
                "input": "checkbox"
            }
        }
    }
```

### Reviewer Registration Form

Reviewers will also need to select subject areas of expertise, so they can be matched with submissions of the same subject area.

The best way to do this is to create a [Reviewer Registration](../../reference/stages/registration-stage.md) form from the venue configuration page. There are default fields included in this form, you can choose to keep them or not. You'll need to add the same `subject_area` field and values you added to the submission form to accurately create a match between reviewers and submissions.

### Next Steps

To continue with the setup you can either use the python client to post the invitation edit and edges that will be shared below or you can email support at info@openreview.net for assistance.

### Score Invitation

An invitation will need to be created to allow the posting of edges. These edges are used to connect two entities, the submission note and the reviewer's profile ID. The edge will contain four properties:

* `head` : the submission note
* `tail` : the reviewer's profile ID
* `weight` : the weight between two entities
* `label` : the subject area in common

An example of posting a score invitation for subject areas:

Add your venue ID to `venue_id`.

```python
venue_id = ''

client.post_invitation_edit(
    invitations=f'{venue_id}/-/Edit', 
    readers=[venue_id],
    writers=[venue_id],
    signatures=[venue_id],
    invitation=openreview.api.Invitation(
        id = f"{venue_id}/Reviewers/-/Subject_Score",
        invitees = [venue_id,"OpenReview.net/Support"],
        readers = [venue_id],
        writers = [venue_id],
        signatures = [venue_id],
        edge = {
                "id": {
                    "param": {
                        "withInvitation": f"{venue_id}/Reviewers/-/Subject_Score",
                        "optional": True
                    }
                },
                "ddate": {
                    "param": {
                        "range": [ 0, 9999999999999 ],
                        "optional": True,
                        "deletable": True
                    }
                },
                "cdate": {
                    "param": {
                        "range": [ 0, 9999999999999 ],
                        "optional": True,
                        "deletable": True
                    }
                },
                "readers": [
                    venue_id,
                    "${2/tail}"
                  ],
                  "nonreaders": [],
                  "writers": [
                    venue_id
                  ],
                "signatures": {
                    "param": {
                      "regex": f"{venue_id}/Program_Chairs",
                      "default": [
                        f"{venue_id}/Program_Chairs"
                      ]
                    }
                  },
                "head": {
                    "param": {
                      "type": "note",
                      "withInvitation": f"{venue_id}/-/Submission"
                    }
                  },
                "tail": {
                    "param": {
                      "type": "profile",
                      "options": {
                        "group": f"{venue_id}/Reviewers"
                      }
                    }
                  },
                "weight": {
                    "param": {
                      "minimum": -1
                    }
                  },
                "label": {
                    "param": {
                      "regex": ".*",
                      "optional": True,
                      "deletable": True
                    }
                  }
            }
    )
)
```

### Posting Edges

To post the edges, you'll first need to get your reviewer's responses to the registration form.

For example, how to get the forum responses and set the reviewer's profile ID as the key and the content of their response as the value. The reviewer's ID is located in the signature of the response.

```python
#get the forum id of the reviewers' registration page
registration_forum = ''
# get the venue id
venue_id = ''

# Get all replies to the registration forum
notes = client.get_all_notes(replyto=registration_forum)

# Create a dictionary with profile_id : [subject_area]
registrations = {}
for n in notes:
    signature = n.signatures[0]
    profile = client.get_profile(signature)
    profile_id = profile.id
    registrations[profile_id] = n.content['subject_area']['value']

```

Once you have the registration entries, iterate through submissions and and post an edge between the submission and every reviewer that shares the matching subject area.&#x20;

<pre class="language-python"><code class="lang-python"><strong># get all active submissions under review
</strong>venue_group = client.get_group(venue_id)
under_review_id = venue_group.content['submission_venue_id']['value']
submissions = client.get_all_notes(content={'venueid': under_review_id})

# the subject area field in the submission note may be different per venue, for this example we'll use 'subject_area'
for submission in submissions:
    submission_id = submission.id
    if 'subject_area' in submission.content:
        submission_subject_area = submission.content['subject_area']['value']
        for s in submission_subject_area:
            print(f'submission: {submission.id}, subject area: {s}')
            # Check registrations for any value that matches s and print the corresponding key
            for reviewer_id, subject in registrations.items():
                if s in subject:
                    #print(f"Reviewer ID: {reviewer_id}, Subject: {subject}")
                    client.post_edge(openreview.api.Edge(
                        invitation=f'{venue_id}/Reviewers/-/Subject_Score',
                        signatures=[venue_id],
                        head=submission_id,
                        tail=reviewer_id,
                        weight=1,
                        label=s
                    ))

</code></pre>

### Score Specification

For the Subject\_Score edges to be included in the matching, you will need to add a specification to the Score Specification in the Assignment Configuration.

Add:

```
 Your/Venue/ID/Reviewers/-/Subject_Score: {
    weight: 1,
    default: 0
 }
```

To:

<figure><img src="../../.gitbook/assets/Screenshot 2025-03-13 at 2.48.33 PM.png" alt=""><figcaption><p>Subject_Score is added to the Scores Specification</p></figcaption></figure>

The Subject\_Score is added to the Score Specification box, so it is included in the matching. Affinity scores should help make better matchings within a subject area.

After you run the Matching with the set configuration, you can check the assignment distribution in "View Statistics" by score when the proposed assignments are complete. The results will be as follows:

* Matchings with high affinity and matching subject area will be maximized.
* Subject area match will take priority over a low affinity score.

In this configuration, affinity scores will be a decimal ranging from 0 to 1 and the subject score is **either** 0 or 1. The resulting aggregated scores could then range between 0 and 2. Ideally in the histogram title "**Distribution of Assignments by Scores**" you would see a large clustering in the 1.7 to 2 interval and a smaller clustering in the 0.7 to 0.9 interval.

A matching may not entirely contain assignments that have a subject score or match, and a few reasons are:

* Submissions may be conflicted with many, or all reviewers, in their designated subject area
* Reviewers in that subject area might already be saturated and there are no more available assignments, so the matcher falls back to the affinity score

