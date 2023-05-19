# Submission Revision Stage

#### What it Does&#x20;

The Submission Revision Stage creates invitations that allow authors to revise their original submissions.

#### When to Use it

Double blind venues can only run the Submission Revision Stage after the submission deadline has passed. Single-blind venues can run the Submission Revision Stage before the submission deadline if they first run Post Submission Stage.&#x20;

<details>

<summary>Submission Revision Name</summary>

* The name you choose will appear as a button on the forum of each revisable submission.&#x20;
* Optional&#x20;
* Default: 'Revision'

</details>

<details>

<summary>Submission Revision Start Date </summary>

* When the Revision invitation should open for authors, in GMT.&#x20;
* Optional&#x20;
* Default: now

</details>

<details>

<summary>Submission Revision Deadline</summary>

* When the Revision invitation will close for authors, in GMT.&#x20;
* Required

</details>

<details>

<summary>Accepted Submissions Only </summary>

* Whether or not revisions should be allowed for only accepted submissions.&#x20;
* Required

</details>

<details>

<summary>Submission Revision Additional Options </summary>

* Additional options that can be added to submissions. Expects valid JSON surrounded by a single pair of curly braces {}. Read more about the accepted field types [here](../../getting-started/frequently-asked-questions/what-field-types-are-supported-in-the-forms.md).
* Optional&#x20;
* Default options for revision are all fields of the [Submission Form](../default-forms/default-submission-form.md).&#x20;

</details>

<details>

<summary>Submission Revision Remove Options</summary>

* Fields that the authors will not be able to edit. Expects a comma separated list of field names in lowercase.
* Optional&#x20;
* Default options for revision are all fields of the [Submission Form](../default-forms/default-submission-form.md).&#x20;

</details>
