# How to setup paper matching by calculating affinity scores and conflicts

{% hint style="info" %}
Before calculating affinity scores and conflicts, you should make sure that your submission deadline has passed and that you have run either the ‘[Post Submission Stage](../../../reference/stages/post-submission-stage.md)’ or the ‘[Review Stage](../../../reference/stages/review-stage.md)’.
{% endhint %}

You can calculate affinity scores and conflicts for your venue using OpenReview's 'Paper Matching Setup' feature. Paper Matching Setup is enabled for any venue that selected an option for the 'Paper Matching' question on the venue request form. This feature allows Program Chairs to compute or upload affinity scores and/or compute conflicts.

You can find the 'Paper Matching Setup' button on your venue request form next to 'Remind Recruitment'.

![](<../../../.gitbook/assets/image (4) (1).png>)

Clicking it should bring up the following form. The 'Matching Group' is a dropdown menu of the groups you can use in the matcher (Reviewers, Area Chairs, Senior Area Chairs), depending on whichever you selected for your venue. You can select if you would like affinity scores and/or conflicts computed. Alternatively, you can compute and upload your own affinity scores using the OpenReview expertise API: https://github.com/openreview/openreview-expertise

![](<../../../.gitbook/assets/image (1) (1).png>)

### Conflict Detection Policy

Conflict detection uses the information of the users' coauthors from publications in OpenReview as long as they are publicly visible and the users' Profile. Therefore, the more complete and accurate the information in the Profile is, the better the conflict detection.

The sections of the Profile used for conflict detection are the Emails section, the Education & Career History section, and the Advisors, Relations & Conflicts section.

Another parameter that can be controlled is the amount of years you want to consider when looking at conflicts. For example, there might be two users who worked at company A at some point. However, User A worked at Company C ten years ago and User B just started working at Company C. If the amount of years is set to 5, for example, a conflict won't be detected between User A and User B because only the history, relations and publications from the past 5 years will be taken into consideration. **By default, all relations, history, and publications are taken into consideration for conflict detection.**

{% hint style="info" %}
Since a lot of users use email services such as gmail.com, a list of common domains is used to filter them out before conflicts are computed.
{% endhint %}

There are two policies when computing conflicts: default and neurips.

#### Default Information Extraction Policy

1. Uses the domains and computes subdomains from the Education & Career History section.
2. Uses the domains and computes subdomains from the emails listed in the Advisors, Relations & Conflicts section.
3. Uses the domains and computes subdomains from the Emails listed in the Emails section.
4. Uses the publication ids in OpenReview that the user authored.

#### Neurips Information Extraction Policy

{% hint style="info" %}
Note that emails do not have a range of dates for when they were valid in the user's Profile. The Neurips policy addresses this issue.
{% endhint %}

1. Uses the domains and computes subdomains from the Education & Career History section.
2. Uses the domains and computes subdomains from the emails listed in the Advisors, Relations & Conflicts section.
3. Uses the domains and computes subdomains from the Emails listed in the Emails section, if and only if, no domains were available in the Education & Career History and Advisors, Relations & Conflicts sections.
4. Uses the publication ids in OpenReview that the user authored.

### Conflict Detection

Once all the information is extracted from the users' Profiles, the following rules apply to find a conflict between User A and User B:

* If any of the domains/subdomains from the Education & Career History section of user A matches at least one of the domains/subdomains of the same section of User B, then a conflict is detected.
* If any of the domains/subdomains from the Advisors, Relations & Conflicts section or the Emails section of user A matches at least one of the domains/subdomains of the same sections of User B, then a conflict is detected.
* If any of the publications of User A is the same as one of the publications of User B. In other words, if User A and User B are coauthors, then a conflict is detected.

### Troubleshoot Paper Matching

Running the paper matching setup should output a comment on your venue request page. If there were members missing profiles or publications, the message will identify them and say 'Affinity scores and/or conflicts could not be computed for these users. Please ask these users to sign up in OpenReview and upload their papers. Alternatively, you can remove these users from the Reviewers group.' This message does not mean that the process failed, but that those members were excluded from the calculations. You have two options:&#x20;

1. Remove reviewers without profiles from the reviewers group.&#x20;
2. Remind the reviewers that they need OpenReview profiles and wait for them to create them. You can run the Paper Matching Setup as many times as you want or until all users have completed profiles.&#x20;

Note that when a reviewer creates a profile, their email address will not be automatically updated to their profile ID in the reviewers' group. The matcher will still detect email addresses as users without profiles, so any email addresses will either need to be removed or replaced with tilde IDs. This can be done automatically by re-running Paper Matching Setup.

You can confirm that the affinity scores were computed by checking if an invitation for the scores was created: https://api.openreview.net/edges?invitation=your/venue/id/role/-/Affinity\_Score. Next, you should be able to run a paper matching from the ‘Paper Assignments’ link in your PC console.
