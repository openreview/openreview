# Creating a New Venue

These instructions are for those who would like to use OpenReview to host a conference, workshop, class project, symposium, or other event. If you are a journal hoping to use OpenReview, please email info@openreview.net.&#x20;

## Submitting a Venue Request Form

Submitting a venue request form is the first step towards hosting a venue on OpenReview. Go to [https://openreview.net/group?id=OpenReview.net/Support](https://openreview.net/group?id=OpenReview.net/Support) and click 'OpenReview Support Request Form'.

{% hint style="info" %}
If you want to create a Venue Instance for testing purposes, you should use the following link instead: [https://dev.openreview.net/group?id=OpenReview.net/Support](https://dev.openreview.net/group?id=OpenReview.net/Support).

Note that sending emails through the dev site is ONLY supported when creating a profile and resetting a password.
{% endhint %}

This is where you will select many of the settings for your venue such as general information about the venue, the committee roles, submission visibility, deadlines, etc.

For example, some venues have an abstract and a full paper deadline while others just have a full paper deadline. This can be specified in the form. Venue organizers can also decide if they want to force all the authors of the submission to have a profile or if they can enter the name/email address. The default setting is that the submission force accepts both options. Usually venue organizers give the authors 24 hours after the deadline to sign up before the submission is desk rejected.

The settings for readership permissions can be overwritten at later stages. If you initially make submissions private, you override the submission readers later on with [Post Submission Stage](../../reference/stages/post-submission-stage.md) or with [Post Decision Stage](../../reference/stages/post-decision-stage.md).&#x20;

## Deployment

After you submit the form, our team will review it, ask for necessary modifications and deploy it; making your venue live. The deployment process does the following:

* Creates a `venueid` of the form AAAI.org/2025/Conference.
* Creates a home page for the venue located at https://openreview.net/group?id=\[venueid]. This page contains the general information about the venue and the submission button.&#x20;
* Creates the submission invitation open to any logged in user.
* Creates the committee groups and author groups with their respective consoles. The default committee group `id` for each role is as follows: Program\_Chairs, Senior\_Area\_Chairs, Area\_Chairs,  Reviewers and Authors. They can be accessed from the home page or following the url https://openreview.net/group?id=\[committee\_group\_id]. The console pages show different information depending on the role: paper assignments, pending tasks, status of the reviews, etc.

You can then edit some of your selected settings from the venue request form. Note that if you do not enter a submission start date, submissions will open immediately upon venue deployment.

If you are following a journal-like workflow like TMLR, please [get in touch with us](https://openreview.net/contact).
