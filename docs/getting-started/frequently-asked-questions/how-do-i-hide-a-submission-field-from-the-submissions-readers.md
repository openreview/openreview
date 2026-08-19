# How do I hide a submission field?

As the venue organizer, you can choose to hide specific submission fields from all submission readers. When a field is hidden, it becomes visible only to the Program Chairs and to the paper authors.

## For Conference Review Workflow Venues

1. In the Workflow Timeline, click Submission Change Before Reviewing
2. Under Restrict Field Visibility, edit the Content Readers JSON to hide the requested fields. The `authors`  and `authorids`  fields are hidden automatically - these fields can be used as a guide for how to hide readers for other fields.
3. Under Edit Dates, set the Activation Date to now to trigger the stage.

## For Request Form Venues

1. From your venue request form, click [Post Submission](../../reference/stages/post-submission-stage.md).
2. Under `hide_fields`, select from the dropdown all the fields you would like to hide.

After the submission deadline, submissions will be updated to be visible to all users selected under `submission_readers` in the request form, and all fields selected under `hide_fields` will be visible only to Program Chairs and paper authors.
