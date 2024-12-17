# How to Get All Official Comments

Official Comments are used by venues for discussion of papers, including author rebuttals. To get all Official Comments, use the following steps.

{% hint style="info" %}
These instructions are for API2 venues (most venues since 2024).&#x20;
{% endhint %}

### 1. [Install and Instantiate the Python Client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md)

```python
import openreview
from openreview.api import OpenReviewClient

client = OpenReviewClient(
    baseurl='https://api2.openreview.net',
    username=YOUR_USERNAME,
    password=YOUR_PASSWORD
)
```

### 2. Get all submissions with replies

Next, [get all submissions](how-to-get-all-reviews.md) for the venues with replies:

```python
venue_id =VENUE_ID #ID of the venue you would like to get submissions for

#get the name for the submission invitation
venue_group = client.get_group(venue_id)
submission_name = venue_group.content['submission_name']['value']
invitation_name = f'{venue_id}/-/{submission_name}' #Put the name of the invitation here

#get all submissions
submissions = client.get_all_notes(
invitation= invitation_name,details = 'replies')
```

See [here](../../getting-started/frequently-asked-questions/how-do-i-find-a-venue-id.md) for information on how to find a venue ID

### 3. Get all official comments from replies

The following code will return a list with all official comments for all submissions.

<pre class="language-python"><code class="lang-python">official_comments=[]
for s in submissions:
<strong>    official_comments.extend([r for r in submission.details['replies'] if r['invitations'][0].endswith('Official_Comment')])
</strong></code></pre>

## How to filter comments

Once you have the Official Comments, you can filter them by certain properties to select a specific type of comments. For example, to get all comments made by the submission authors, you can filter by comments signed by the submission authors:

```python
author_comments = [
        r for r in official_comments if any('Authors' in mem for mem in r['signatures'])
    ]
```

You can replace 'Authors' with another group as well, such as '/Reviewer\_' or a profile id.

Some fields that may be helpful to filter comments by:

* **signatures:** The signature of the profile or group (such as Submission\[Number]/Authors) that posted the Note
* **readers:** The profile(s) or group(s) that can read the note
* **forum:** The unique ID of the submission that the comment is posted to
* **replyto:** The unique ID of the note that the comment is a reply to (e.g. if this comment is a response to a review on the forum page, then the replyto id will be that of the review)
