# How to modify the Review, Meta Review, and Decision Forms

The review, meta review, and decision forms are configured from the [Workflow Timeline](../../getting-started/hosting-a-venue-on-openreview/navigating-your-venue-pages.md#workflow-timeline). Each form has its own step in the timeline:

* **Official Review** – the review form filled out by assigned reviewers.
* **Meta Review** – the meta review form filled out by assigned Area Chairs. This step only exists if your venue has Area Chairs.
* **Decision** – the decision form used by the Program Chairs.

If your venue uses more than one reviewer role per submission, each additional role has its own review step named "_\<role name>_ Review", which you can edit in the same way as **Official Review**.

## Review form

1. From the Workflow Timeline, click **Official Review** to expand the step.
2. Next to **Form Fields**, click **Edit**.
3. Change the fields in the **Content** editor:
   * **Content JSON** lets you edit the JSON of the whole form. To remove an existing field, set it to `{ "delete": true }`.
   * **Widgets** has an **Add a field or Select a field to edit** dropdown to add a field or open an existing one. Use the trash icon next to a field to remove it.
   * **Preview** shows what the form will look like.
4. Check **Rating Field Name** and **Confidence Field Name**. Each one must name a field that is still in the form. If you remove or rename the `rating` or `confidence` field, enter the name of the field that replaces it, or the edit is rejected with an error such as `"rating" does not exist in the review form fields`.
5. Click **Submit**.

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2FH8viY7960dTlHXrY3t3v%2Fimage.png?alt=media&#x26;token=22133e0b-d9b2-488c-877c-5d7c9d7a9cec" alt="The Official Review step expanded in the Workflow Timeline, with the Form Fields editor open on the Widgets tab and the Rating Field Name and Confidence Field Name inputs below it."><figcaption><p>The Form Fields editor of the Official Review step, as Program Chairs see it.</p></figcaption></figure>

The rating and confidence field names are what the Program Chairs console uses for review statistics. See [Why are the rating and confidence fields in my PC console wrong?](../../getting-started/frequently-asked-questions/why-are-the-rating-and-confidence-fields-in-my-pc-console-wrong.md).

For the structure of form fields, see [Customizing Forms](../../getting-started/customizing-forms.md).

## Meta review form

1. From the Workflow Timeline, click **Meta Review** to expand the step.
2. Next to **Form Fields**, click **Edit**.
3. Change the fields in the **Content** editor as described for the review form.
4. Check **Recommendation Field Name**. It must name a field that is still in the form, or the edit is rejected with an error such as `"recommendation" does not exist in the meta review form fields`.
5. Click **Submit**.

{% hint style="info" %}
If your venue has Senior Area Chairs, the form fields you submit for **Meta Review** are also copied to the **Meta Review SAC Revision** step, the form Senior Area Chairs use to revise meta reviews.
{% endhint %}

## Decision form

The **Decision** step does not have a **Form Fields** control. You can change the decision options:

1. From the Workflow Timeline, click **Decision** to expand the step.
2. Next to **Decision Options**, click **Edit**.
3. In **Decision Options**, list every option the Program Chairs can choose from, for example `Accept (Oral), Accept (Poster), Reject`. Options can only contain letters, numbers, spaces, underscores, and parentheses.
4. In **Accept Decision Options**, select the options that mean the submission is accepted.
5. Click **Submit**.

## Other settings of these steps

Each of these steps also has controls for who can read the replies (**Readers**) and when the form opens and closes (**Dates**). The **Official Review** step also has a **Notifications** control to choose who is emailed when a review is posted.
