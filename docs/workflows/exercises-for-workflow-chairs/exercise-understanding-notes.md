# Exercise: Understanding Notes

#### Objectives

Learn to use the OpenReview API to:

1. Post new notes
2. Retrieve a specific note
3. Fetch all submissions for a given venue
4. Edit a submission (readers and content)
5. Delete a note

Use the [OpenReview documentation](https://docs.openreview.net/getting-started/using-the-api/objects-in-openreview/introduction-to-notes) and linked guides as your primary reference. Ensure that you have instantiated the API client in a variable `client` before beginning.

#### 1. Post New Test Submissions

**Task**: Create and post at least two submissions to a venue of your choice.

**Hints**:

* See [**How to create, change, and delete notes**](https://docs.openreview.net/how-to-guides/workflow/how-to-create-change-and-delete-notes) — _Posting a submission with Python_.

**Check your work:**\
Open the [PC Console](../../getting-started/hosting-a-venue-on-openreview/navigating-your-venue-pages.md) for your venue. Your new submissions should appear under the Submission Status tab.&#x20;

#### 2. Get a Single Note

**Task**: Fetch and inspect one of the submissions you just posted by its note id.

**Hints**:

* Read [**Introduction to Notes**](https://docs.openreview.net/getting-started/using-the-api/objects-in-openreview/introduction-to-notes).
* Look up [`get_note(id)` in the API reference](https://openreview-py.readthedocs.io/en/latest/api.html#openreview.api.OpenReviewClient.get_note).

**Check your work:**\
Run the following code, the output should match the note ID you requested.

```python
print(note.id)
```

#### 3. Get All Submissions for a Venue

**Task**: Retrieve all submissions for your chosen venue.

**Hints**:

* See [**How to get all notes for submissions, reviews, rebuttals, etc.**](https://docs.openreview.net/how-to-guides/data-retrieval-and-modification/how-to-get-all-notes-for-submissions-reviews-rebuttals-etc).

**Check your work:**\
Run the following code, the output should match the number of submissions in your venue.

```python
print(len(submissions))
```

#### 4. Edit a Submission

**Task**: Choose one of your submissions and edit the title\
**Hints**:

* Same guide: [**How to create, change, and delete notes**](https://docs.openreview.net/how-to-guides/workflow/how-to-create-change-and-delete-notes) — _Update the content of a note_.

**Check your work:**\
Check the submission page on the OpenReview site. The submission should show the new title. When you click 'Show Revisions', you should see the old title in the original edit, and the current title in a more recent edit.&#x20;

#### 5. Delete a Note

**Task**: Remove one of your test submissions from the system.

**Hints**:

* Same guide: [**How to create, change, and delete notes**](https://docs.openreview.net/how-to-guides/workflow/how-to-create-change-and-delete-notes) — _Delete a note_.

**Check your work:**\
Run the following code, the output should indicate the note does not exist

```python
client.get_note("<NOTE_ID>")
```

