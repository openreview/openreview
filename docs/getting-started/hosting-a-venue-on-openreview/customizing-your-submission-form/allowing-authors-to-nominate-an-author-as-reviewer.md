# Allowing authors to nominate an author as reviewer

You can add a field for authors to nominate co-authors as reviewers by editing the submission form.

### For Conference Review Workflow Venues:

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

### For Request Form Venues:

You can customize the [default submission form](https://docs.openreview.net/reference/default-forms/default-submission-form) for your venue using the [Revision](https://docs.openreview.net/reference/stages/revision) button on your [venue request form](https://docs.openreview.net/getting-started/hosting-a-venue-on-openreview/navigating-your-venue-pages#venue-request-form). In the 'Additional Submission Options', field, enter valid JSON with the fields that you would like to add or change in your form.

{% tabs %}
{% tab title="Website" %}
<figure><img src="../../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>
{% endtab %}

{% tab title="JSON" %}
```json
{
   "serve_as_reviewer": {
      "value": {
          "param": {
            "type": "profile[]",
            "regex": "~.*"
          }
      },
      "description": "Please nominate an author to serve as a reviewer using their profile ID (e.g. ~First_Last1)",
      "order": 20
   }
}
```
{% endtab %}
{% endtabs %}

The resulting field in the submission form would look like this:

<figure><img src="../../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

The above field allows authors of submissions to nominate one or more co-authors to serve as reviewer. In order to check that the co-authors being added to this field are indeed an author of the submission, you need to add the following preprocess function to the submission invitation.

```
async function process(client, edit, invitation) {
    client.throwErrors = true
    
    const { note } = edit

    if (note.ddate) {
      return
    }

    const profiles = await client.tools.getProfiles(note.content.authorids.value)
    const profileIds = profiles.map(profile => profile.id)

    let reviewerAuthorField = note.content.serve_as_reviewer?.value
    if (reviewerAuthorField) {
      const reviewerAuthorFields = Array.isArray(reviewerAuthorField) ? reviewerAuthorField : [reviewerAuthorField]
      const reviewerAuthorProfiles = await client.tools.getProfiles(reviewerAuthorFields.map(field => field.includes('@') ? field.toLowerCase() : field))
      const invalidReviewerAuthor = reviewerAuthorProfiles.find(elem => !profileIds.includes(elem.id));
      if (invalidReviewerAuthor) {
        return Promise.reject(new OpenReviewError({ name: 'Error', message: 'Enter a paper co-author to serve as a reviewer. ' + invalidReviewerAuthor.id + ' does not appear in the author list' }))
      }
    }
}
```

To add this preprocess code, navigate to your venue's submission invitation: https://openreview.net/invitation/edit?id={venue\_id}/-/Submission, scroll down to the process functions and click on the "Pre Process" tab.&#x20;
