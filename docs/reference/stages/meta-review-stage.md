# Meta Review Stage

**What it Does**

The Meta Review Stage creates paper groups and [meta review invitations](../default-forms/default-meta-review-form.md). It should be used to edit the Meta Review form. Use of the Meta Review Stage will overwrite any changes made to the Meta Review Invitations through the Invitation Editor. It also sets the visibility of all existing and future meta reviews.&#x20;

**When to Use it**

For API 1 venues, the Meta Review Stage should not be run until after the submission deadline, except in the case of public, single blind venues. They can begin the Meta Review Stage before the submission deadline if they first run 'Post Submission Stage' to create paper groups. &#x20;

For API 2 venues, you can run the Meta Review Stage to configure the settings and the form, and once the start date is reached the invitation will become active.&#x20;

Skip this if your venue does not have Area Chairs.

**Options**

<details>

<summary>Meta Review Start Date</summary>

* Enter a time and date you want the Meta Review period to begin in GMT using the following format: YYYY/MM/DD HH:MM (e.g. 2019/01/31 23:59)
* Optional
* Defaults to now

</details>

<details>

<summary>Meta Review Deadline</summary>

* This is the official, soft deadline Area Chairs will see.&#x20;
* Enter a time and date in GMT using the following format: YYYY/MM/DD HH:MM (e.g. 2019/01/31 23:59)
* Optional

</details>

<details>

<summary>Meta Review Expiration Date</summary>

* After this date, no more meta reviews can be submitted. This is the hard deadline area chairs will not be able to see.&#x20;
* Enter a time and date in GMT using the following format: YYYY/MM/DD HH:MM (e.g. 2019/01/31 23:59)
* Optional
* Default is 30 minutes after the meta review deadline

</details>

<details>

<summary>Make Meta Reviews Public (required)</summary>

* This setting determines whether or not meta reviews are public the moment they are posted.
* Default is "No, meta reviews should NOT be revealed publicly when they are posted"

</details>

<details>

<summary>Release Meta Reviews To Authors (required)</summary>

* This setting determines whether or not meta reviews are visible to authors the moment they are posted.
* Default is "No, meta reviews should NOT be revealed when they are posted to the paper's authors"

</details>

<details>

<summary>Release Meta Reviews To Reviewers (required)</summary>

* This setting determines whether or not meta reviews are visible to reviewers the moment they are posted.
* Default is "Meta review should not be revealed to any reviewer"

</details>

<details>

<summary>Additional Meta Review Form Options </summary>

* This is where you can configure additional options in the meta review form.
* Use lowercase for the field names and underscores to represent spaces. The UI will auto-format the names, for example: supplementary\_material -> Supplementary Material.
* Valid JSON is expected.
* The [Default Meta Review Form](../default-forms/default-meta-review-form.md).
* Optional

</details>

<details>

<summary>Remove Meta Review Form Options</summary>

* Removes fields from the Default Meta Review form. Expects a comma separated list of field names in lowercase.
* Optional

</details>
