# How to Get All Registration Notes

If you enabled the Registration Stage for reviewers and/or area chairs, you will be able to programmatically query these registration notes using the [python client](https://docs.openreview.net/\~/changes/kVYMUwjISEF9x7H1lfSE/getting-started/using-the-api/installing-and-instantiating-the-python-client).

1. Instantiate your OpenReview client

```python
client = openreview.api.OpenReviewClient(
    baseurl='https://api2.openreview.net',
    username=<your username>,
    password=<your password>
)
```

2. Get all the registration notes for your venue. The invitation is composed of your venue's id (found in the request form), the group you want to query notes for (`Reviewers` or `Area_Chairs`) and the[ name of the registration stage](https://docs.openreview.net/\~/changes/kVYMUwjISEF9x7H1lfSE/reference/stages/registration-stage#registration-name) (by default, this is `Registration`):

`{venue_id}/(Reviewers|Area_Chairs)/-/{registration_stage_name}` ,\
e.g., NeurIPS.cc/2023/Conference/Reviewers/-/Registration

```python
registration_notes = client.get_all_notes(invitation='NeurIPS.cc/2023/Conference/Reviewers/-/Registration')
```

3. Iterate through every note to access the note's content. You will be able to access all fields you configured for the registration stage.

```python
for note in registration_notes:
    print(note.content['expertise_confirmed']['value'])
```
