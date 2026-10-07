# How to Recruit and Remind Recruited Reviewers

Every committee group of your venue (Reviewers, and Area Chairs and Senior Area Chairs if your venue uses them) has its own recruitment tools. You send invitations and reminders from the **Workflow Groups** section at the top of your [Workflow Timeline](../../getting-started/hosting-a-venue-on-openreview/navigating-your-venue-pages.md#workflow-timeline). The steps below use the Reviewers group; the steps for Area Chairs and Senior Area Chairs are the same.

{% hint style="info" %}
Recruitment can be done at any point. The recruitment request is available as soon as your venue is deployed.
{% endhint %}

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2FYA2cgxqkMqGlE7WAdiTq%2Fimage.png?alt=media&#x26;token=745aae79-efbb-44f6-9582-bc54f611b4b8" alt="The Reviewers group in the Workflow Groups section, with the Recruitment Request and Recruitment Request Reminder buttons."><figcaption><p>The Reviewers group in the Workflow Groups section, as Program Chairs see it.</p></figcaption></figure>

## Send recruitment invitations

1. Open your Workflow Timeline and go to the **Workflow Groups** section.
2. In the row for the Reviewers group, click the **Recruitment Request** button.
3. Fill out the form:
   *   **Invitee Details**: enter the invitees, one per line. Invitees must be formatted in a specific way, or they will be reported as errors and will not receive the email:

       > Enter a list of invitees with one per line. Either tilde IDs (\~Captain\_America1), emails (captain\_rogers@marvel.com), or email,name pairs (captain\_rogers@marvel.com, Captain America) expected. If only an email address is provided for an invitee, the recruitment email is addressed to "Dear invitee". Do not use parentheses in your list of invitees.
   * **Invite Message Subject Template** and **Invite Message Body Template**: the recruitment email. You can use `{{fullname}}` (the name of the invitee) and `{{invitation_url}}` (the link to accept or decline). The body must contain `{{invitation_url}}`, otherwise the request is rejected.
4. Click **Submit**.

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2FgIoy50DpYtZ15AXxFcGY%2Fimage.png?alt=media&#x26;token=7655cf27-48b4-4198-90e3-abd3ff19e9ce" alt="The Recruitment Request form for the Reviewers group, with the Invitee Details, Invite Message Subject Template and Invite Message Body Template fields."><figcaption><p>The Recruitment Request form, as Program Chairs see it.</p></figcaption></figure>

When the request has been processed, the Program Chairs receive an email titled "Recruitment request status for \<venue> Reviewers Group", and the same status is posted as a comment on your venue request form. It lists how many users were invited, how many were already invited, how many were already members, and any errors, with links to the invited list.

{% hint style="info" %}
You can also send recruitment invitations from the Reviewers group page: open the group, go to the **Content** tab, click **Edit** and select **Recruitment Request**.
{% endhint %}

## Where invitees end up

* Every invited user is added to the group venue\_id/Reviewers/Invited.
* Users who accept are added to venue\_id/Reviewers.
* Users who decline are added to venue\_id/Reviewers/Declined.

Invitees can change their answer later with the link in the invitation email. Accepting removes them from the Declined group, and declining removes them from the Reviewers group. A reviewer who has already been assigned to a submission cannot decline and is asked to contact the Program Chairs or the submission's Area Chair. The Reviewers group and its Invited and Declined subgroups are listed in the **Workflow Groups** section, with the number of members in each.

## Remind invitees who have not responded

**Automatic reminder:** seven days after each recruitment request, OpenReview sends a reminder to the users from that request who have neither accepted nor declined. The reminder reuses your invitation email, with "\[Reminder]" added to the subject.

**Manual reminder:** to send a reminder at any other time:

1. In the **Workflow Groups** section, click the **Recruitment Request Reminder** button in the row for the Reviewers group.
2. Edit the **Invite Reminder Message Subject Template** and **Invite Reminder Message Body Template**. The body must contain `{{invitation_url}}`.
3. Click **Submit**.

The reminder is sent to every member of venue\_id/Reviewers/Invited who is not in venue\_id/Reviewers or venue\_id/Reviewers/Declined.

## Configure the recruitment steps

Recruitment also has two steps in the Workflow Timeline, which you can change with the **Edit** link under each step:

* **Create Reviewers Recruitment Request**
  * **Dates**: when Program Chairs can send recruitment invitations.
  * **Request Emails**: the default subject and body of the recruitment email.
* **Reviewers Recruitment Response**
  * **Dates**: when invitees can accept or decline. By default, invitees can respond until 12 weeks after your venue was deployed.
  * **Reduced Load**: the reduced load options invitees can choose from, and whether they can pick one when they accept. By default, reduced loads are not offered.
  * **Response Emails**: the emails sent when an invitee accepts or declines.
  * **Overlap Committees**: committee groups whose members cannot accept this invitation. If your venue has Area Chairs, by default a user cannot serve as both a Reviewer and an Area Chair: a user who has already accepted the Area Chair invitation is asked to decline it before accepting the Reviewer invitation. Delete the value to allow users to serve in both roles.
