# Ethics Review Stage

#### What it Does&#x20;

The Ethics Review stage is available to any venue that selected "Yes, our venue has Ethics Chairs and Reviewers" on their venue request form. Running the Ethics Review stage will create Paper#/Ethics\_Reviewers groups and Ethics Review Form invitations for each flagged paper.&#x20;

#### When to Use It&#x20;

The Ethics Review Stage can be run at any time after the Submission Deadline for Double Blind conferences and any time after Post Submission Stage is run for Single Blind conferences. It can be run multiple times with different paper selections; if you run it again, excluding a paper that was originally flagged, that paper will be removed unless it already has an ethics reviewer assigned to it. The flagged papers list must always contain all of the papers that should be flagged for ethics review.&#x20;

<details>

<summary>Ethics Review Start Date</summary>

* When ethics reviewers can start writing their reviews, in GMT
* Optional&#x20;
* Defaults to now

</details>

<details>

<summary>Ethics Review Deadline </summary>

* When the ethics review invitations expire, in GMT
* Required

</details>

<details>

<summary>Make Ethics Reviews Public</summary>

* Whether the ethics reviews should be made immediately public. Only available if submissions are public.&#x20;
* Required&#x20;

</details>

<details>

<summary>Release Ethics Reviews to Authors</summary>

* Whether the ethics reviews should be made immediately available to the authors.&#x20;
* Required

</details>

<details>

<summary>Release Ethics Reviews to Reviewers </summary>

* Which reviewers and ethics chairs the reviews should be immediately released to&#x20;
* Required&#x20;

</details>

<details>

<summary>Ethics Review Submissions</summary>

* A comma-separated list of the paper numbers of submissions requiring ethics review.&#x20;
* Required&#x20;

</details>

<details>

<summary>Additional Ethics Review Form Options</summary>

* Additional options that will be added to the default Ethics Review Form. Expects valid JSON surrounded by a single pair of curly braces {}. Read more about the accepted field types [here](../../getting-started/frequently-asked-questions/what-field-types-are-supported-in-the-forms.md).
* Optional&#x20;
* Defaults to the default [Ethics Review Form](../default-forms/default-ethics-review-form.md)

</details>

<details>

<summary>Remove Ethics Review Form Options </summary>

* Removes fields from the Review form. Expects a comma separated list of field names in lowercase.
* Optional&#x20;
* Defaults to default [Ethics Review Form](../default-forms/default-ethics-review-form.md).

</details>

