# How to create, change, and delete notes

Jump to:

* [Quickstart: posting a test submission](how-to-create-change-and-delete-notes.md#id-1.-posting-creating-a-note)
* [Quickstart: automatically posting desk rejections](how-to-create-change-and-delete-notes.md#quickstart-posting-a-desk-rejection-for-submissions-missing-pdfs)
* [Quickstart: changing readers](how-to-create-change-and-delete-notes.md#quickstart-update-the-readers-of-a-note)
* [Quickstart: posting and deleting test reviews](how-to-create-change-and-delete-notes.md#quickstart-posting-editing-and-deleting-a-test-review)
* [Quickstart: deleting fields](how-to-create-change-and-delete-notes.md#id-3.-deleting-notes-and-fields)

## 1. Posting (Creating) a Note

To **post a new Note**, create a `Note` object with required fields, then use `client_v2.post_note_edit()`.&#x20;

#### Required fields include:

* `invitation`: the invitation ID defining the note type (e.g., a submission, review, comment)
* `forum`: the ID of the main submission this note belongs to (for replies)
* `signatures`: who is posting (usually a profile ID)
* `readers`: who can read this note
* `writers`: who can modify the note
* `content`: a dictionary of the note content fields (title, authors, review text, etc.)

### Quickstart: Posting a submission with Python&#x20;

Posting a submission with Python is typically reserved for testing venue workflows and changes.

{% hint style="info" %}
PCs and Reviewers that are also authors of test submissions will be conflicted later on in the process. We recommend using test author profiles for the submission process.&#x20;
{% endhint %}

```python
#create the submission note
venue_id = test_venue_id
author_ids_or_emails = [ "~Author_One1",'email@mail.com']
your_id = "~PC_ID1"
i = 1
test_pdf_path = './data/paper.pdf'
note = openreview.api.Note(
        license = 'CC BY-SA 4.0',
        content = {
            'title': { 'value': 'Paper title ' + str(i) },
            'abstract': { 'value': 'This is an abstract ' + str(i) },
            'authorids': { 'value': author_ids_or_emails },
            'authors': { 'value': author_ids_or_emails},
            'keywords': { 'value': ['machine learning', 'nlp'] },
            'pdf': {'value': '/pdf/' + 'p' * 40 +'.pdf' },
        }
    )
#url = client.put_attachment(test_pdf_path, f'{venue_id}/-/Submission', 'pdf')
#note.content['pdf'] = url

#post the note
note = openreview_client.post_note_edit(
    invitation=f"{venue_id}/-/Submission",
    signatures=[your_id],
    readers=[venue_id] + author_ids_or_emails,
    writers = [venue_id] + author_ids_or_emails,
    note=note
)
```

### Quickstart: Posting a desk rejection for submissions missing PDFs

PCs may want to programmatically desk-reject submissions that are missing PDF files once the submission deadline has passed. You can use the script below to do so.&#x20;

```python
venue_id = "<VENUE_ID>"
​
submissions = client.get_all_notes(invitation=f'{venue_id}/-/Submission')
#gets the desk rejection name of the invitation
desk_rejection_name = client.get_group(venue_id).content['desk_rejection_name']['value']
    
# for each submission note that does not contain a pdf field, post a desk rejection note
for submission in submissions:
    #Check for a pdf field value
    if not submission.content.get('pdf', {}).get('value'): 
        desk_reject_note = client.post_note_edit(
                    #desk rejection invitation
                    invitation=f'{venue_id}/Submission{submission.number}/-/{desk_rejection_name}',
                    signatures=[f'{venue_id}/Program_Chairs'],
                    note=openreview.api.Note(
                        content = {
                        'desk_reject_comments': { 'value': 'No PDF.' }
                        }
                    )
                )
        print(submission.number, "is desk rejected")
```

### Quickstart: Posting a Support Request Form With Python&#x20;

While a support request form can most easily be [submitted through the UI](https://openreview.net/group?id=OpenReview.net/Support), some venues that have multiple deadlines a year and need to submit multiple venue requests with the same settings may find it easier to do this programmatically through the API. While most notes are posted with the API2 client, this note must be posted with the [API 1 client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).

```python
#change the settings below
program_chair_emails = ['pc_email1@mail.com','pc_email2@mail.com']
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
    
support_request = openreview.Note(
    invitation = 'OpenReview.net/Support/-/Request_Form', 
    readers = ["OpenReview.net/Support"] + program_chair_emails,
    writers = ["OpenReview.net/Support"] + program_chair_emails,
    content = content,
    signatures = [
        <your_profile_id>
    ]
)
client_v1.post_note(support_request)
```

## 2. Editing Notes

Editing notes in API2 is done the same way as creating notes, by posting a note edit with new values for the fields.&#x20;

### Quickstart: Update the readers of a note or field

One common edit to make in bulk to notes is updating the readers of a note. This can be done in the UI for common configurations, but if your venue needs further fine-grained control over readership (for example custom tracks, groups, etc), then you can use the Python Client to change the readers of any subset of notes programmatically.  The following script will change the readers of a note or field within a note to the new list.

```python
#venue_id
venue_id = "<VENUE_ID>"
note_type = 'Official_Review' #Taken from the last part of the note invitation. Could replace with "Rebuttal", "Decision", "Metareview"
new_readers = ["<LIST_OF_NEW_READERS>"]  # options are 1) ['everyone'] or 2) list of either group names ("Program_Chairs") or Submission + Group Name ("Submission Authors").
field_to_change = None #use None to change readers of the note, or use the field name to change the readers of a field

#set up invitations
edit_invitation = f'{venue_id}/-/Edit' 
submission_invitation = f'{venue_id}/-/Submission'

#get a list of notes to edit (in this case all reviews)
submissions = client_v2.get_notes(invitation=submission_invitation,details = 'replies')
#reviews to edit
reviews=[openreview.api.Note.from_json(reply) for s in submissions for reply in s.details['replies'] if f'{venue_id}/Submission{s.number}/-/{note_type}' in reply['invitations']]
print(len(reviews))
notes_to_change = reviews

#change the readers of the notes
for note_to_change in notes_to_change:
    submission_number = client_v2.get_note(note_to_change.forum).number
    note_content = note_to_change.content
    if new_readers == ['everyone']:

         note_readers = ['everyone']

    else:
        #parse the new_readers:
        group_ids = []
        for g in new_readers:
            if g.startswith('Submission'):
                group_name = g.split(' ')[1]
                new_reader_group = client_v2.get_group(f'{venue_id}/Submission{submission_number}/{group_name}').id
                group_ids.append(new_reader_group)
            else:
                new_reader_group = client_v2.get_group(venue_id + '/' + g)
                group_ids.append(new_reader_group.id)
        #parse the new_readers:
        if field_to_change == None:
            note_readers = list(set(group_ids + note_to_change.signatures + [venue_id]))
        else:
            note_content[field_to_change]['readers'] = list(set(group_ids + note_to_change.signatures + [venue_id]))
        
    client_v2.post_note_edit(invitation=edit_invitation,
            signatures=[venue_id],
            note=openreview.api.Note(
                id=note_to_change.id,
                readers=note_readers,
                content=note_content
            )
        )


```

For more examples, see: how to [hide/reveal fields](../submissions-comments-reviews-and-decisions/how-to-hide-reveal-fields.md).

## 3. Deleting Notes and fields

Deleting notes should be used with caution as most changes can be done by editing a note, and once deleted, changes can be difficult to recover. Typically, we recommend deleting notes only in the case of testing a venue workflow on the dev site. To delete a note, you would post a note edit that includes a value (in millisecond time) for the `ddate` field, which has a value of `None` by default. An example is below:

```python

venue_id = '<VENUE ID>'
note_to_delete = client_v2.get_note('<NOTE_ID>')
time_now = openreview.tools.datetime_millis(dt.datetime.now()))
)

client_v2.post_note_edit(
    invitation=f'{VENUE ID}/-/Edit',
    signatures=note_to_delete.signatures,
    note=openreview.api.Note(
        id=note_to_delete.id,
        content=note_to_delete.content,
        ddate = time_now
        )
```

### Quickstart: Remove a field from a submission

Removing fields from submissions should be done sparingly. Oftentimes, changing the readers of the field its sufficient (see above), but in the case that you want to remove a field entirely, you can use the approach below"

```python
venue_id = "<YOUR_VENUE_ID>"
forum_id_to_change = '123dsavdf'
field_to_delete = 'keywords'
submission = client_v2.get_note(forum_id_to_change)
time_now = openreview.tools.datetime_millis(dt.datetime.now()))
)

edit_content = submission.content
edit_content[field_to_delete][['value']['ddate'] = time_now

client_v2.post_note_edit(invitation=f'{venue_id}/-/PC_Revision',
    signatures=[f'{venue_id}/Program_Chairs'],
    note=openreview.api.Note(
        id = submission.id,
        content = edit_content
    ))
```

### Quickstart: Posting, editing, and deleting a test review

1. Posting a new review note:

```python
submission_number = '<SUBMISSION_NUMBER>'
venue_id = '<VENUE ID>'

anon_groups = client_v2.get_groups(prefix=f'{venue_id}/Submission{submission_number}/Reviewer_', signatory='~Reviewer_One1')
anon_group_id = anon_groups[0].id

review_edit = client_v2.post_note_edit(
    invitation=f'{venue_id}/Submission{submission_number}/-/Official_Review',
    signatures=[anon_group_id],
    note=openreview.api.Note(
        content={
            'title': { 'value': 'Good paper, accept'},
            'review': { 'value': 'Excellent paper, accept'},
            'rating': { 'value': 10},
            'confidence': { 'value': 5},
        }
    )
```

2. Editing an existing review note, you must specify the note ID:

```python
# Edit the review note

# Get review note
review_notes = client_v2.get_notes(
    invitation=f'{venue_id}/Submission{submission_number}/-/Official_Review',
    signatures=[anon_group_id]
)

# Assuming there's one review per reviewer
original_review = review_notes[0]

# Update the content of the review with the new content 
review_content = original_review.content
review_content['rating']['value'] = 6

# Step 2: Edit the review to update the rating
edited_review = client_v2.post_note_edit(
    invitation=f'{venue_id}/Submission{submission_number}/-/Official_Review',
    signatures=original_review.signatures, # Using the signatures from the original note
    note=openreview.api.Note(
        id=original_review.id,
        content=review_content
    )
)
```

3. Deleting a review note:

```python
# Assuming the first one is the one we want to delete
note_to_delete = review_notes[0]

# Step 2: Post a deletion edit - to delete the whole review
deleted_note = reviewer_client.post_note_edit(
    invitation=f'{venue_id}/Submission{submission_number}/-/Official_Review',
    signatures=[anon_group_id],
    note=openreview.api.Note(
        id=note_to_delete.id,
        content=note_to_delete.content,
        ddate = openreview.tools.datetime_millis(dt.datetime.now()))
)
```
