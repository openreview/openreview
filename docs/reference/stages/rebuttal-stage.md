# Rebuttal Stage

#### What it Does&#x20;

The Rebuttal Stage creates rebuttal invitations. It should be used to allow authors to post their Rebuttals to the reviews posted to their submissions. It also sets the visibility of all existing and future rebuttals.&#x20;

#### When to Use it

The Rebuttal Stage should be run if PCs want to allow authors to post rebuttals to reviews. This stage is usually run after the review deadline, but it can be run before as well.&#x20;

**Options**

<details>

<summary>Rebuttal Start Date</summary>

* When Rebuttal Invitations will open for Authors, in GMT
* Optional
* Defaults to now

</details>

<details>

<summary>Rebuttal Deadline</summary>

* When Review Invitations will close for Authors, in GMT
* Required

</details>

<details>

<summary>Number of Rebuttals</summary>

* The number of rebuttals authors will be able to submit per paper:
  * one rebuttal per paper
  * one rebuttal per review
  * multiple rebuttals per paper
* Required

</details>

<details>

<summary>Rebuttal Readers</summary>

* Selection of who will be able to see posted rebuttals
* PCs and paper authors are added by default
* Optional
* **Note:** if rebuttals are to be posted as replies of submission

</details>

<details>

<summary>Additional Rebuttal Form Options</summary>

* Adds or overwrites fields to the Rebuttal Form. Expects valid JSON surrounded by a single pair of curly braces {}. Read more about the accepted field types [here](../../getting-started/frequently-asked-questions/what-field-types-are-supported-in-the-forms.md).&#x20;
* Optional
* Defaults to [default Rebuttal Form](../default-forms/default-rebuttal-form.md)

</details>

<details>

<summary>Email Program Chairs about Rebuttals</summary>

* Determines if PCs will be notified of future rebuttals.
* Required

</details>
