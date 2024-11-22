# How to Get Reviewer Ratings

In order to get the existing Reviewer Ratings, you will need to use the [python client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).

1. [Get all submissions](how-to-get-all-submissions.md) for your venue and check all the replies in each submission for the `/-/Rating` invitation. The issue is, there is no data about the reviewer of the Official Review in the Rating note, so you will need to get the author of the review separately or add them to the reviewer\_rating list from the review data. For this example I will do the latter.

```python
# Add your venue ID as a string
venue_id = ''

submissions = client.get_all_notes(invitation=f'{venue_id}/-/Submission', details='replies')
reviewer_ratings = []

for submission in submissions:
    # for each submission get replies that have the Rating invitation
    reply_rating = [reply for reply in submission.details["replies"] if any(invitation.endswith("/-/Rating") for invitation in reply['invitations'])]
    # for each rating get the reviewer that's being evaluated
    for reply in reply_rating:
        reviewer = client.get_note(reply['replyto']).signatures[0]
        # the reviewer is anonomized in the signature for single and doubleblind venues
        if venue_id in reviewer:
            reviewer_id = client.get_group(reviewer).members[0]
            reply["reviewer"] = reviewer_id
        else:
            reply["reviewer"] = reviewer
        # add the rating to the reviewer_ratings list
        reviewer_ratings.append(reply)

```

Here is an example if you want to export certain fields to a CSV.

I chose the fields `forum`, `replyto`, `reviewer`, and `review_quality`:

* forum = the submission id
* replyto = the official review id
* reviewer = the profile ID of the reviewer
* review\_quality = the field name for the rating, this can change so check your Rating invitation

<pre class="language-python"><code class="lang-python">import csv
<strong>
</strong># specify the CSV file name
csv_file = 'reviewer_ratings.csv'

# open the CSV file for writing
with open(csv_file, mode='w', newline='') as file:
    writer = csv.writer(file)
    
    # write the header row
    writer.writerow(['submission', 'official_review', 'reviewer', 'review_quality'])
    
    # write the data rows
    for rating in reviewer_ratings:
        submission = rating.get('forum')
        official_review = rating.get('replyto')
        reviewer = rating.get('reviewer')
        review_quality = rating.get('content', {}).get('review_quality', {}).get('value')
        
        writer.writerow([submission, official_review, reviewer, review_quality])

print(f"Data has been written to {csv_file}")
</code></pre>

This file will be saved in your working directory.
