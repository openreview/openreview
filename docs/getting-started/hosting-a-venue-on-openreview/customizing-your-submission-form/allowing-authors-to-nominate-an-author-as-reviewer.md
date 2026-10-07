# Allowing authors to nominate an author as reviewer

You can add a field for authors to nominate co-authors as reviewers by editing the submission form.

To make changes to your submission form, navigate to your Workflow Timeline. Then go to the "Submission" Step — > Edit Fields — > Content JSON. Add the following code to the submission JSON.

```json
"serve_as_reviewer": {
    "order": 11,
    "description": "Enter the profile ids of the authors of this submission who will serve as reviewers.",
    "value": {
        "param": {
            "type": "string[]",
            "enum": ["${3/authors/value/*/username}"],
            "input": "select"
        }
    }
}
```

This code will show any users added as authors of the submission in a dropdown. The submitted author will be able to choose one or more authors to serve as reviewer. The validation that the user added is an author of the submission is done by the field, so no need to validate this field with a pre/post-process.
