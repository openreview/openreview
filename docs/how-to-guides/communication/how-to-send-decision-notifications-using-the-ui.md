# How to Send Decision Notifications Using the UI

Venues created with the Conference Review Workflow send decision emails to authors through one Workflow Timeline step per decision option. By default there are two:

* **Create Author Accept Decision Notification**, which emails the authors of submissions with an "Accept" decision.
* **Create Author Reject Decision Notification**, which emails the authors of submissions with a "Reject" decision.

Each step runs automatically on its activation date. It emails the authors of every submission whose decision matches that step's decision option.

{% hint style="warning" %}
No fields are selected under **Fields To Include** when your venue is created. Until you select at least one field in each notification step, that step sends no emails.
{% endhint %}

{% hint style="info" %}
If you change the decision options in the **Decision** step (**Edit** next to **Decision Options**), the notification steps are updated to match. A **Create Author \<Option> Decision Notification** step is added for each new option, with its activation date set one week after your change. The step for each option you removed is deleted. Parentheses are dropped from the step name, so "Accept (Oral)" becomes **Create Author Accept Oral Decision Notification**.
{% endhint %}

To send decision notifications:

1. Post the decisions, either in the **Decision** step or by uploading a CSV file in the **Create Decision Upload** step.
2. Go to your venue's [Workflow Timeline](../../getting-started/hosting-a-venue-on-openreview/navigating-your-venue-pages.md#workflow-timeline) and expand the notification step for a decision option, for example **Create Author Accept Decision Notification**.
3. Click **Edit** next to **Fields To Include** and select which decision fields (`decision`, `comment`) to include in the email. Click **Submit**. **You must select at least one field.** If no field is selected when the step runs, no emails are sent and an "Author \<Option> Decision Notification Failed" comment is posted to your [venue request form](../../getting-started/hosting-a-venue-on-openreview/navigating-your-venue-pages.md#venue-request-form).
4.  (Optional) Click **Edit** next to **Templates** to customize the **Email Subject** and **Email Content**. The accept and reject steps have different default messages. **Do not remove the tokens in curly braces.** They are filled in for each submission:

    * `{submission_number}` and `{submission_title}`: the submission's number and title.
    * `{formatted_decision}`: the fields you selected under **Fields To Include**, taken from the submission's decision.
    * `{submission_forum}`: the submission's ID, used in the link to the submission.
    * `{{{{fullname}}}}`: the recipient's name. Keep all four braces on each side.

    Don't add any other text in curly braces. The **Email Subject** can only use `{submission_number}` and `{submission_title}`.
5. Click **Edit** next to **Dates** and set the **Activation Date** to the time you want the emails sent. To send them right away, set it to the current time. Click **Submit**.
6. Repeat steps 2–5 for each notification step.

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2Fwda3y5UP4QFx6zGBowVz%2Fimage.png?alt=media&#x26;token=d207c41c-16d5-4cc9-b6e6-4d6768d2ad70" alt="The Create Author Accept Decision Notification step expanded in the Workflow Timeline, with the Templates editor open showing the default Email Subject and Email Content."><figcaption><p>The Templates editor of Create Author Accept Decision Notification, as Program Chairs see it.</p></figcaption></figure>

Emails are sent to the authors of each matching submission. Replies go to your venue's contact email.

{% hint style="warning" %}
The notification steps don't depend on the **Decision Release** step. Authors are emailed the decision fields you selected even if the decision isn't yet visible to them on OpenReview. If you want authors to see the decision on the submission page when they get the email, set the **Decision Release** activation date before the notification dates. By default, the notification steps are scheduled after **Decision Release**.
{% endhint %}

### Re-sending notifications

A step can run more than once, because it runs again each time you edit it after its activation date. OpenReview skips any submission whose authors already got an email with the same subject. To send the emails again, for example after changing some decisions, change the **Email Subject** under **Templates**.
