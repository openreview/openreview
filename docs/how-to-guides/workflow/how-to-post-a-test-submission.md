# How to Post a Test Submission

If you are testing your venue on the dev site, you may want to generate some test submissions. This can be accomplished manually through the UI, but it may be faster to do it with python using the guide below.&#x20;

Instantiate the python client using your dev profile credentials:&#x20;

```python
import openreview
client = openreview.api.Client(
    baseurl='https://devapi2.openreview.net',
    username=<your dev username>,
    password=<your dev password>
)
```

Familiarize yourself with the submission invitation. Every content field of your note will need to be in the format specified by the submission invitation, and all the fields that do not contain `optional: true` are mandatory.

Create your note. If you were submitting a note through the [default submission form](../../reference/default-forms/default-submission-form.md#api-v2-json), it would look something like this:&#x20;

```python
note = openreview.Note(
    readers = [
        "Test/Venue",
        "~Test_Author1",
        "~Test_Author2"
    ],
    writers = [
        "Test/Venue",
        "~Test_Author1",
        "~Test_Author2"
    ],
    signatures = ["~PC_Id1"],
    content = {
        "title": "Test Submission 1",
            "~Test_Author1",
            "~Test_Author2"
            "~Test_Author1",
            "~Test_Author2"
            "machine learning", 
            "autonomous vehicles"
        "TL;DR": "A test submission",
    }
)
url = client.put_attachment('./paper.pdf', 'Test/Venue/-/Submission', 'pdf')
note.content['pdf'] = url
```

\
**Invitation**: The ID of your venue's submission invitation. It will take the form of your venue id + /-/Submission. \
\
**Readers/Writers**: A list of the venue ID  + all author profile IDs. After the submission deadline, these values will be automatically updated to match the visibility settings selected in the venue request form. \
\
**Signatures**: Your profile ID. \
\
**Content**: The actual content of the submission, which must match the invitation. If the invitation calls for any file uploads, such as a pdf or zip file, you can build the url to the file using put\_attachment and entering the path, the invitation ID, and the name of the field. You can then add that field to the note's content. \
The "authorids" field of the note's content should contain a list of the authors' profile IDs or emails, in the same order as the authors' names. If you registered dummy users, you can find their profile IDs by going to their profile page and copying the ID parameter of that page's url. For example, dev.openreview.net/profile?id=\~Test\_Author2 --> \~Test\_Author2. \


Post your note and output the resulting note's ID:&#x20;

```python
note = client.post_note_edit(
    invitation="Test/Venue/-/Submission",
    signatures="~Test_User1",
    readers=[
        "Test/Venue",
        "~Test_Author1",
        "~Test_Author2"
    ],
    writers = [
        "Test/Venue",
        "~Test_Author1",
        "~Test_Author2"
    ],
    note=note
)
print(note.id)
```

You can now view your note in the UI by going to dev.openreview.net/forum?id=\<note\_id>.&#x20;

