# Review Stage

#### What it Does&#x20;

The Review Stage creates paper groups and [review invitations](../default-forms/default-review-form.md). It should be used to edit the Review form. Use of the Review Stage will overwrite any changes made to the Review Invitations through the Invitation Editor. It also sets the visibility of all existing and future reviews.&#x20;

#### When to Use it

The Review Stage should not be run until after the submission deadline, except in the case of public, single blind venues. They can begin the Review Stage before the submission deadline if they first run 'Post Submission Stage' to create paper groups.&#x20;

#### Options

<details>

<summary>Review Start Date</summary>

* When Review Invitations will open for Reviewers, in GMT
* Optional
* Defaults to now

</details>

<details>

<summary>Review Deadline</summary>

* When Review Invitations will close for Reviewers, in GMT
* Required

</details>

<details>

<summary>Make Reviews Public</summary>

* If yes, sets the readers of existing and future reviews to 'everyone'.&#x20;
* Required&#x20;
* Will not work if submissions are not public

</details>

<details>

<summary>Release Reviews to Authors</summary>

* If yes, sets the readers of existing and future reviews to include paper authors.&#x20;
* Required&#x20;
* Will not work if 'Make Reviews Public' is selected while submissions are not public

</details>

<details>

<summary>Release Reviews to Reviewers</summary>

* Sets the visibility of existing and future reviews.&#x20;
* Required

</details>

<details>

<summary>Email Program Chairs About Reviews</summary>

* Determines if PCs will be notified of future review submissions.
* Required

</details>

<details>

<summary>Additional Review Form Options </summary>

* Adds or overwrites fields to the Review Form. Expects valid JSON surrounded by a single pair of curly braces {}. Read more about the accepted field types [here](../accepted-field-types.md).&#x20;
* Optional&#x20;
* Defaults to [default Review Form](../default-forms/default-review-form.md).

</details>

<details>

<summary>Remove Review Form Options</summary>

* Removes fields from the Review form. Expects a comma separated list of field names in lowercase.
* Optional&#x20;
* Defaults to [default Review Form](../default-forms/default-review-form.md).

</details>
