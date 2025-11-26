# Data Retrieval for API 1 Venues

### Submissions

To get all 'active' submissions for a double-blind venue **,** pass your venue's blind submission invitation into get\_all\_note&#x73;_._

```python
submissions = client.get_all_notes(
    invitation="Your/Venue/ID/-/Blind_Submission"
    )
```

To get all submissions for a double-blind venue regardless of their status (active, withdrawn or desk rejected), pass your venue's submission invitation to get\_all\_notes().

```python
submissions = client.get_all_notes(
    invitation="Your/Venue/ID/-/Submission"
    )
```

As a program organizer, to get only the "accepted" submissions for double-blind venues, query using the Blind submission invitation and include 'directReplies' and 'original' in the details.&#x20;

```python
# Double-blind venues

submissions = client.get_all_notes(invitation = 'Your/Venue/ID/-/Blind_Submission', details='directReplies,original')
blind_notes = {note.id: note for note in submissions}
all_decision_notes = [] 
for submission_id, submission in blind_notes.items(): 
        all_decision_notes = all_decision_notes + [reply for reply in submission.details["directReplies"] if reply["invitation"].endswith("Decision")]

accepted_submissions = []

for decision_note in all_decision_notes:
    if 'Accept' in decision_note["content"]['decision']:
        accepted_submissions.append(blind_notes[decision_note['forum']].details['original'])
```

As a program organizer, to get only the "accepted" submissions, query using the Submission invitation and include 'directReplies' in the details.

```python
# Single-blind venues

submissions = client.get_all_notes(invitation = 'Your/Venue/ID/-/Submission', details='directReplies')
notes = {note.id: note for note in submissions}
all_decision_notes = [] 
for submission_id, submission in notes.items(): 
        all_decision_notes = all_decision_notes + [reply for reply in submission.details["directReplies"] if reply["invitation"].endswith("Decision")]

accepted_submissions = []

for decision_note in all_decision_notes:
    if 'Accept' in decision_note["content"]['decision']:
        accepted_submissions.append(notes[decision_note['forum']])
```

Parameters you can use when [querying API 1 notes](https://api.openreview.net/notes).



## Reviews

#### To get all reviews for a double-blind venue, you can do the following:&#x20;

1. Get all submissions for your venue. You can do this by passing your venue's submission invitation into get\_all\_note&#x73;_._ You should also pass in details = "directReplies" to obtain any notes that reply to each submission.&#x20;

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
    reviews = reviews + [openreview.Note.from_json(reply) for reply in submission.details["directReplies"] if reply["invitation"].endswith("Official_Review")]
```

3\. The list reviews now contains all of the reviews for your venue.

#### To get all reviews for a single-blind venue, you can do the following:

1. Get all submissions for your venue. You can do this by passing your venue's submission invitation into get\_all\_note&#x73;_._ You should also pass in details = "directReplies" to obtain any notes that reply to each submission.&#x20;

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
    reviews = reviews + [openreview.Note.from_json(reply) for reply in submission.details["directReplies"] if reply["invitation"].endswith("Official_Review")]
```

3\. The list reviews now contains all of the reviews for your venue.

## Exporting data



Say you want to export all of the reviews for a given venue into a csv file.&#x20;

1. If you have not done so, you will need to [install and instantiate the openreview-py client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).&#x20;
2. Retrieve all of the Reviews into a `reviews` object following the instructions [here](/broken/pages/ILgM2uZNzr0XKkEu0K5i).&#x20;
3. Next, get the super review invitation. This is the overall review invitation which each of the Paper#/-/Official\_Review invitations are based off of, and it follows the format Venue/ID/-/Official\_Review.&#x20;

```python
invitation = client.get_invitation("<Your/Venue/Id/-/Official_Review>")
print(invitation.content)
```

4. Generate a list of the fields in the content in the Review invitation. For reference, this is what the default review invitation content looks like in JSON:&#x20;

```python
{
  "title": {
      "order": 1,
      "value-regex": ".{0,500}",
      "description": "Brief summary of your review.",
      "required": true
  },
  "review": {
      "order": 2,
      "value-regex": "[\\S\\s]{1,200000}",
      "description": "Please provide an evaluation of the quality, clarity, originality and significance of this work, including a list of its pros and cons (max 200000 characters). Add formatting using Markdown and formulas using LaTeX. For more information see https://openreview.net/faq",
      "required": true,
      "markdown": true
  },
  "rating": {
      "order": 3,
      "value-dropdown": [
          "10: Top 5% of accepted papers, seminal paper",
          "9: Top 15% of accepted papers, strong accept",
          "8: Top 50% of accepted papers, clear accept",
          "7: Good paper, accept",
          "6: Marginally above acceptance threshold",
          "5: Marginally below acceptance threshold",
          "4: Ok but not good enough - rejection",
          "3: Clear rejection",
          "2: Strong rejection",
          "1: Trivial or wrong"
      ],
      "required": true
  },
  "confidence": {
      "order": 4,
      "value-radio": [
          "5: The reviewer is absolutely certain that the evaluation is correct and very familiar with the relevant literature",
          "4: The reviewer is confident but not absolutely certain that the evaluation is correct",
          "3: The reviewer is fairly confident that the evaluation is correct",
          "2: The reviewer is willing to defend the evaluation, but it is quite likely that the reviewer did not understand central parts of the paper",
          "1: The reviewer's evaluation is an educated guess"
      ],
      "required": true
  }
}
```

so we would expect a list like \["title", "review", "rating", "confidence"]. This is how we get the list:

```python
keylist = list(review_invitation.reply['content'].keys())
```

5. If you haven't already, import csv. Then iterate through the list of reviews stored in 'reviews' and for each one, append the values associated to the keys in your keylist. If a value does not exist for that key, put an empty string in its place.&#x20;

```python
import csv
with open('reviews.csv', 'w') as outfile:
    csvwriter = csv.writer(outfile, delimiter=',')
    # Write header 
    t = csvwriter.writerow(keylist)
    for review in reviews:
        valueList = []
        for key in keylist:
            if review.content.get(key):
                if 'value' in review.content[key].keys():
                    valueList.append(review.content.get(key)['value'])
                else:
                    valueList.append(review.content.get(key))
            else:
                valueList.append('')
        s = csvwriter.writerow(valueList)
outfile.close()  
```

6. There should now be a csv of exported reviews in the directory in which you are working.&#x20;

### Adding submission information to the exported csv (both API2 and API1)

You may also want to know which submission each review is associated with. You can get the forum of each review, which corresponds to the forum page of its associated submission. For example, if a review's forum is aBcDegh, you could find that submission at https://openreview.net/forum?id=aBcDegh. To create a csv that includes the review forums, do this:

```python
with open('reviews.csv', 'w') as outfile:
    csvwriter = csv.writer(outfile, delimiter=',')
    # Write header 
    keylist.insert(0,'forum')
    t = csvwriter.writerow(keylist)
    for review in reviews:
        valueList = []
        valueList.append(review.forum)
        for key in keylist:
            if (key != 'forum'):
                if 'value' in review.content[key].keys():
                    valueList.append(review.content.get(key)['value'])
                else:
                    valueList.append(review.content.get(key))
            else:
                valueList.append('')
        s = csvwriter.writerow(valueList)
outfile.close()      
```

### Adding reviewer information to the exported csv (both API2 and API1)

You may also want to know the profile information of the reviewer associated with a review. First, create a mapping between the Reviewer ID (anonymous) and Profile ID:

```python
submission_groups = client.get_all_groups(prefix=f'{venue_id}/Submission')
reviewerID_to_profileID =  {group.id.split('/')[-1]  : group.members[0] for group in submission_groups if '/Reviewer_' in group.id}
```

Then you can use this map to find the profile ID of the reviewer for a review:

```python
with open('reviews.csv', 'w') as outfile:
    csvwriter = csv.writer(outfile, delimiter=',')
    # Write header 
    keylist.insert(0,'profileID')
    t = csvwriter.writerow(keylist)
    for review in reviews:
        valueList = []
        valueList.append(reviewerID_to_profileID[review.signatures[0].split('/')[-1]])
        for key in keylist:
            if (key != 'profileID'):
                if review.content.get(key):
                    valueList.append(review.content.get(key))
                else:
                    valueList.append('')
        s = csvwriter.writerow(valueList)
outfile.close()    
```

If you want additional information about the reviewer, you can get their profile using the Profile ID (see [How to Get Profiles and Their Relations](how-to-get-profiles-and-their-relations.md) for more guidance).

