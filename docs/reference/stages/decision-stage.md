# Decision Stage

#### What it Does&#x20;

The Decision Stage creates decision invitations for Program Chairs. Use of the Decision Stage will overwrite any changes made to the Decision Invitations through the Invitation Editor. It will also overwrite any customized readership settings that were not made through the venue request form.

#### When to Use it&#x20;

The Decision Stage should not be run until after the submission deadline, except in the case of public, single blind venues. They can begin the Decision Stage before the submission deadline if they first run 'Post Submission Stage' to create paper groups.&#x20;

<details>

<summary>Decision Start Date</summary>

* When Decision Invitations open for Program Chairs, in GMT.&#x20;
* Optional&#x20;
* Defaults to now

</details>

<details>

<summary>Decision Deadline</summary>

* When decisions will close for Program Chairs, in GMT.&#x20;
* Required

</details>

<details>

<summary>Decision Options </summary>

* Decision types. Expects comma-separated list&#x20;
* Optional&#x20;
* Defaults to "Accept (Oral)", "Accept (Poster)", "Reject"

</details>

<details>

<summary>Make Decisions Public</summary>

* If yes, sets the readers of existing and future decisions to 'everyone'.&#x20;
* Required&#x20;
* Will not work if submissions are not public

</details>

<details>

<summary>Release Decisions to Authors</summary>

* If yes, sets the readers of existing and future decisions to include paper authors.&#x20;
* Required&#x20;
* Will not work if 'Make Decisions Public' is selected while submissions are not public

</details>

<details>

<summary>Release Decisions to Reviewers</summary>

* Sets the visibility of existing and future Decisions.&#x20;
* Required

</details>

<details>

<summary>Release Decisions to Area Chairs </summary>

* Sets the visibility of existing and future Decisions.&#x20;
* Required

</details>

<details>

<summary>Additional Decision Form Options</summary>

* Adds or overwrites fields to the Decision Form. Expects valid JSON surrounded by a single pair of curly braces {}. Read more about the accepted field types [here](../../getting-started/frequently-asked-questions/what-field-types-are-supported-in-the-forms.md).&#x20;
* Optional&#x20;
* Defaults to [default Decision Form](../default-forms/default-decision-form.md)

</details>

<details>

<summary>Decisions File</summary>

* Allows for bulk upload of decisions. Expects a csv containing the paper\_number, decision, and comment for one paper per line. Does not expect a header/column names. The comment column is optional.&#x20;
* Optional&#x20;
* Defaults to manual Decisions.&#x20;

</details>
