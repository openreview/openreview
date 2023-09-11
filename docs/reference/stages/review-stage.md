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

<summary>Review Expiration Date</summary>

* After this date, no more reviews can be submitted. This is the hard deadline reviewers will not be able to see.
* Enter a time and date in GMT using the following format: YYYY/MM/DD HH:MM (e.g. 2019/01/31 23:59)
* Optional
* Default is 30 minutes after the review deadline

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

<summary>Review Rating Field Name</summary>

* Determines which field should be used to calculate the average "rating" on the PC console. You should enter a field that has been added via "Additional Review Form Options".&#x20;
* The selected field should have options that follow the format "number: description". For example, "1: Very poor".&#x20;
* Required
* Defaults to "rating"

</details>

<details>

<summary>Review Confidence Field Name</summary>

* Determines which field should be used to calculate the average "confidence" on the PC console. You should enter a field that has been added via "Additional Review Form Options".&#x20;
* The selected field should have options that follow the format "number: description". For example, "1: Not confident".&#x20;
* Required
* Defaults to "confidence"

</details>

<details>

<summary>Additional Review Form Options </summary>

* Adds or overwrites fields to the Review Form. Expects valid JSON surrounded by a single pair of curly braces {}. Read more about the accepted field types [here](../../getting-started/frequently-asked-questions/what-field-types-are-supported-in-the-forms.md).&#x20;
* Optional&#x20;
* Defaults to [default Review Form](../default-forms/default-review-form.md).

</details>

<details>

<summary>Remove Review Form Options</summary>

* Removes fields from the Review form. Expects a comma separated list of field names in lowercase.
* Optional&#x20;
* Defaults to [default Review Form](../default-forms/default-review-form.md).

</details>
