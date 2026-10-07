# How to release reviews, decisions, and metareviews

The **Official Review Release** step in your venue's [Workflow Timeline](../../getting-started/hosting-a-venue-on-openreview/navigating-your-venue-pages.md#workflow-timeline) runs automatically on its **Activation Date**. It changes the readers of all official reviews at once. When your venue is created, the step's Activation Date is the same as the Official Review due date, and the readers are:

* Program Chairs
* Assigned Senior Area Chairs, if your venue has Senior Area Chairs
* Assigned Area Chairs, if your venue has Area Chairs
* Assigned Reviewers
* Submission Authors

With these defaults, authors can read the reviews of their submission from the Activation Date.

{% hint style="info" %}
Meta reviews and decisions are released the same way, with the **Meta Review Release** and **Decision Release** steps. The **Meta Review Release** step exists only if your venue has Area Chairs.
{% endhint %}

To change who can read the reviews or when:

1. Go to the Workflow Timeline and find the **Official Review Release** step.
2. Click **Edit** next to **Readers** and select who should be able to read the reviews. Depending on your venue's roles, the options are Program Chairs, All Senior Area Chairs, Assigned Senior Area Chairs, All Area Chairs, Assigned Area Chairs, All Reviewers, Assigned Reviewers, Assigned Reviewers who already submitted their review, Reviewer who submitted the review, Submission Authors, and Public.
3. Click **Submit**.
4. Click **Edit** next to **Dates** and set the **Activation Date** to when the reviews should be released.
5. Click **Submit**.

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2FmYOjuocWcYNfjS9vNZcW%2Fimage.png?alt=media&#x26;token=58986006-c22d-4f7a-b4f2-3f6621428fd9" alt="The Official Review Release step expanded in the Workflow Timeline, with the Readers editor open showing the default readers."><figcaption><p>The Readers editor of the Official Review Release step, as Program Chairs see it.</p></figcaption></figure>

## Common errors

**`The "everyone" reader option cannot be included with other reader options.`**

**Public** makes the reviews readable by anyone, so it can't be combined with other readers. Select **Public** on its own.

**`If "everyone" is not selected as reader, the Program Chairs must be included as readers.`**

Unless you select **Public**, the readers must include **Program Chairs**. Add **Program Chairs** to your selection.

{% hint style="info" %}
After the Activation Date has passed, the step runs again each time you edit it. To change the readers of reviews that have already been released, edit **Readers** again.
{% endhint %}

{% hint style="info" %}
Authors are emailed that their reviews are available by a separate step, **Create Author Reviews Notification**, which runs on its own Activation Date. If you change the release date, check that step's date too.
{% endhint %}

{% hint style="warning" %}
If you select **Public**, reviews are only released on submissions that are already public. On private submissions, the readers of the reviews don't change, so authors don't get them either. Make the submissions public first, or don't select **Public**.
{% endhint %}
