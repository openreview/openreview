# Review Stage

#### What it Does

The Review Stage creates paper groups and [review invitations](../default-forms/default-review-form.md). It should be used to edit the Review form. Use of the Review Stage will overwrite any changes made to the Review Invitations through the Invitation Editor. It also sets the visibility of all existing and future reviews.

#### When to Use it

The Review Stage should not be run until after the submission deadline, except in the case of public, single blind venues. They can begin the Review Stage before the submission deadline if they first run 'Post Submission Stage' to create paper groups.

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

* If yes, sets the readers of existing and future reviews to 'everyone'.
* Required
* Overrides 'Release Reviews to Authors'. When this is set to yes, the readers are 'everyone' and nothing else is added, so the paper's Authors group is not added even if that option is also set to yes.
* Cannot be used while the venue keeps every submission private. The stage is rejected with 'Reviews cannot be released to the public since all papers are private'. It is allowed when the venue makes accepted submissions public and hides rejected ones.

</details>

<details>

<summary>Release Reviews to Authors</summary>

* If yes, sets the readers of existing and future reviews to include paper authors.
* Required
* Ignored whenever 'Make Reviews Public' is set to yes, regardless of whether the submissions are public. To release reviews to authors, set 'Make Reviews Public' to no. See [How to release reviews](../../how-to-guides/workflow/how-to-release-reviews.md).

</details>

<details>

<summary>Release Reviews to Reviewers</summary>

* Sets the visibility of existing and future reviews.
* Required

</details>

<details>

<summary>Email Program Chairs About Reviews</summary>

* Determines if PCs will be notified of future review submissions.
* Required

</details>

<details>

<summary>Review Rating Field Name</summary>

* Determines which field should be used to calculate the average "rating" on the PC console. You should enter a field that has been added via "Additional Review Form Options".
* The selected field should have options that follow the format "number: description". For example, "1: Very poor".
* Required
* Defaults to "rating"

</details>

<details>

<summary>Review Confidence Field Name</summary>

* Determines which field should be used to calculate the average "confidence" on the PC console. You should enter a field that has been added via "Additional Review Form Options".
* The selected field should have options that follow the format "number: description". For example, "1: Not confident".
* Required
* Defaults to "confidence"

</details>

<details>

<summary>Additional Review Form Options</summary>

* Adds or overwrites fields to the Review Form. Expects valid JSON surrounded by a single pair of curly braces {}. Read more about the accepted field types [here](../../getting-started/customizing-forms.md#essential-structure-of-custom-fields).
* Optional
* Defaults to [default Review Form](../default-forms/default-review-form.md).

</details>

<details>

<summary>Remove Review Form Options</summary>

* Removes fields from the Review form. Expects a comma separated list of field names in lowercase.
* Optional
* Defaults to [default Review Form](../default-forms/default-review-form.md).

</details>
