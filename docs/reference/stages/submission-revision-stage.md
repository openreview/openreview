# Submission Revision Stage

#### What it Does

The Submission Revision Stage creates invitations that allow authors to revise their original submissions.

#### When to Use it

Double blind venues can only run the Submission Revision Stage after the submission deadline has passed. Single-blind venues can run the Submission Revision Stage before the submission deadline if they first run Post Submission Stage.

<details>

<summary>Submission Revision Name</summary>

* The name you choose will appear as a button on the forum of each revisable submission.
* Optional
* Default: 'Revision'

</details>

<details>

<summary>Submission Revision Start Date</summary>

* When the Revision invitation should open for authors, in GMT.
* Optional
* Default: now

</details>

<details>

<summary>Submission Revision Deadline</summary>

* When the Revision invitation will close for authors, in GMT.
* Required

</details>

<details>

<summary>Accepted Submissions Only</summary>

* Whether or not revisions should be allowed for only accepted submissions.
* Required

</details>

<details>

<summary>Submission Revision Additional Options</summary>

* Additional options that can be added to submissions. Expects valid JSON surrounded by a single pair of curly braces {}. Read more about the accepted field types [here](/broken/pages/Fl5aNHGvygJRuyYJu1Ig).
* Optional
* Default options for revision are all fields of the [Submission Form](../default-forms/default-submission-form.md).

</details>

<details>

<summary>Submission Revision Remove Options</summary>

* Fields that the authors will not be able to edit. Expects a comma separated list of field names in lowercase.
* Optional
* Default options for revision are all fields of the [Submission Form](../default-forms/default-submission-form.md).

</details>
