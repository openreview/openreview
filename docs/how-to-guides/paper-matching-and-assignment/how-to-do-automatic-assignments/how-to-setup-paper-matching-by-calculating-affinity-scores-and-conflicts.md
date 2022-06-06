# How to setup paper matching by calculating affinity scores and conflicts

Before calculating affinity scores and conflicts, you should make sure that your submission deadline has passed and that you have run either the ‘Post Submission Stage’ or the ‘Review Stage’. You can calculate affinity scores and conflicts for your venue using OpenReview's 'Paper Matching Setup' feature. Paper Matching Setup is enabled for any venue that selected an option for the 'Paper Matching' question on the venue request form. This feature allows Program Chairs to compute or upload affinity scores and/or compute conflicts.

You can find the 'Paper Matching Setup' button on your venue request form next to 'Remind Recruitment'.

![](<../../../.gitbook/assets/image (4) (1).png>)

Clicking it should bring up the following form. The 'Matching Group' is a dropdown menu of the groups you can use in the matcher (Reviewers, Area Chairs, Senior Area Chairs), depending on whichever you selected for your venue. You can select if you would like affinity scores and/or conflicts computed. Alternatively, you can compute and upload your own affinity scores using the OpenReview expertise API: https://github.com/openreview/openreview-expertise

![](<../../../.gitbook/assets/image (1) (1).png>)

Running the paper matching setup should output a comment on your venue request page. If there were members missing profiles or publications, the message will identify them and say 'Affinity scores and/or conflicts could not be computed for these users. Please ask these users to sign up in OpenReview and upload their papers. Alternatively, you can remove these users from the Reviewers group.' This message does not mean that the process failed, but that those members were excluded from the calculations. You have two options:&#x20;

1. Remove reviewers without profiles from the reviewers group.&#x20;
2. Remind the reviewers that they need OpenReview profiles and wait for them to create them. You can run the Paper Matching Setup as many times as you want or until all users have completed profiles.&#x20;

Note that when a reviewer creates a profile, their email address will not be automatically updated to their profile ID in the reviewers' group. The matcher will still detect email addresses as users without profiles, so any email addresses will either need to be removed or replaced with tilde IDs. This can be done automatically by re-running Paper Matching Setup.

You can confirm that the affinity scores were computed by checking if an invitation for the scores was created: https://api.openreview.net/edges?invitation=your/venue/id/role/-/Affinity\_Score. Next, you should be able to run a paper matching from the ‘Paper Assignments’ link in your PC console.
