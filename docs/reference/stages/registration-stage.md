# Registration Stage

**What it Does**

The Registration Stage creates a registration task for Reviewers and/or Area Chairs which asks them to confirm that their profile is complete and has publications. Organizers can also ask other questions related to the venue related to reduced quota, emergency reviewers, etc. The responses can be accessed from the Organizer's console (e.g. [Program Chair console](../../getting-started/hosting-a-venue-on-openreview/navigating-your-venue-pages.md#program-chair-console)) or our API directly.

**When to Use it**

The Registration Stage can be run an any point during your workflow, though it is usually enabled at the beginning to collect information from Reviewers, Area Chairs and/or Senior Area Chairs.

**Options**

If your venue has area chairs, you will see two registration buttons, one for reviewers and one for ACs. Each form will have the following fields:

<details>

<summary>Registration Start Date</summary>

* When the registration task will become active, in GMT
* Optional
* Defaults to now

</details>

<details>

<summary>Registration Deadline</summary>

* The soft deadline reviewers and/or area chairs will see, in GMT
* Required

</details>

<details>

<summary>Registration Expiration Date</summary>

* The hard deadline when the task will expire, in GMT
* Required

</details>

<details>

<summary>Registration Name</summary>

* The name you choose will appear as a button in the Registration page
* Use underscores to represent spaces
* Optional
* Default: 'Registration'

</details>

<details>

<summary>Form Title</summary>

* Title of the registration form
* Required
* Default: 'Reviewer Registration' or 'Area Chair Registration'

</details>

<details>

<summary>Form Instructions</summary>

* Instructions reviewers or area chairs will see when completing the task
* Required

</details>

<details>

<summary>Additional Form Options</summary>

* Additional fields that can be added to the registration form. Expects a valid JSON surrounded by a single pair of curly braces {}. Read more about defining fields [here.](../../getting-started/frequently-asked-questions/what-field-types-are-supported-in-the-forms.md)
* Optional
* Defaults to [default Registration Form](../default-forms/default-registration-form.md)

</details>

<details>

<summary>Remove Form Options</summary>

* Fields to be removed from the default [registration form](../default-forms/default-registration-form.md)
* Optional

</details>

**How to Query**

To query all registration notes submitted to your venue, follow [these instructions](broken-reference).
