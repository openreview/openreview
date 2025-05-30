# How to Programmatically Post Support Request Form

{% hint style="info" %}
Currently all request forms are being posted with API 1. If you want to submit a request using the python client, then you should be creating an API 1 client.
{% endhint %}

While a support request form can most easily be [submitted through the UI](https://openreview.net/group?id=OpenReview.net/Support), some venues that have multiple deadlines a year and need to submit multiple venue requests with the same settings may find it easier to do this programmatically through the API.&#x20;

1. If you have not done so, you will need to [install and instantiate the openreview-py client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).&#x20;
2. Familiarize yourself with the venue request form. Note which fields are required, and which are optional. You can [view the form through the UI](https://openreview.net/group?id=OpenReview.net/Support) or [view it in JSON](https://api.openreview.net/invitations?id=OpenReview.net/Support/-/Request_Form). &#x20;
3. Choose your program chairs, and create a list of their email addresses.&#x20;

```python
program_chair_emails = ["pc1@gmail.com", "pc2@gmail.com"]
```

4\. Now it is time to choose the settings for your venue. These make up the 'content' field of your note. Go through the fields on the form in UI, and cross reference the JSON of the invitation to make sure each key matches that in the invitation exactly. For the Program Chairs field, you can enter your program\_chai&#x72;_\__&#x65;mails list. For example:&#x20;

```python
content = {
        "title": "My test venue",
        "Official Venue Name": "My test venue", 
        "Abbreviated Venue Name": "MTV", 
        "Official Website URL": "mtv.com", 
        "program_chair_emails": program_chair_emails, 
        "contact_email": "pc@venue.org", 
        "Area Chairs (Metareviewers)": "Yes, our venue has Area Chairs",
        "Paper Matching": [
            "Reviewer Bid Scores", 
            "OpenReview Affinity"
        ],
        "Venue Start Date": "2022/05/01",
        "Author and Reviewer Anonymity": "Double-blind",
        "reviewer_identity": [
            "Assigned Area Chair"
        ],
        "submission_readers": "All program committee (all reviewers, all area chairs, all senior area chairs if applicable)",
        "venue_organizer_agreement":['OpenReview natively supports a wide variety of reviewing workflow configurations. However, if we want significant reviewing process customizations or experiments, we will detail these requests to the OpenReview staff at least three months in advance.',
  'We will ask authors and reviewers to create an OpenReview Profile at least two weeks in advance of the paper submission deadlines.',
  'When assembling our group of reviewers and meta-reviewers, we will only include email addresses or OpenReview Profile IDs of people we know to have authored publications relevant to our venue.  (We will not solicit new reviewers using an open web form, because unfortunately some malicious actors sometimes try to create "fake ids" aiming to be assigned to review their own paper submissions.)',
  "We acknowledge that, if our venue's reviewing workflow is non-standard, or if our venue is expecting more than a few hundred submissions for any one deadline, we should designate our own Workflow Chair, who will read the OpenReview documentation and manage our workflow configurations throughout the reviewing process.",
  'We acknowledge that OpenReview staff work Monday-Friday during standard business hours US Eastern time, and we cannot expect support responses outside those times.  For this reason, we recommend setting submission and reviewing deadlines Monday through Thursday.',
  'We will treat the OpenReview staff with kindness and consideration.']
    }
```

5\. Now create your openreview [Note](https://openreview-py.readthedocs.io/en/latest/api.html?highlight=get%20note#openreview.Note). You need to include the invitation for the request form, as well as signatures, readers, writers, and content. Your signature should be your OpenReview profile ID that is linked to the email address you entered in the program\_chai&#x72;_\__&#x65;mails.

```
support_request = openreview.Note(
    invitation = 'OpenReview.net/Support/-/Request_Form', 
    readers = ["OpenReview.net/Support"] + program_chair_emails,
    writers = ["OpenReview.net/Support"] + program_chair_emails,
    content = content,
    signatures = [
        <your_profile_id>
    ]
)
```

6\. Post your note.

```
client.post_note(support_request)
```

7\. You can check for your support request here: [https://openreview.net/group?id=OpenReview.net/Support](https://openreview.net/group?id=OpenReview.net/Support)
