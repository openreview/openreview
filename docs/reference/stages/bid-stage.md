# Bid Stage

**What it Does**

The Bid Stage is a feature of the OpenReview Paper Matching system that allows reviewers to indicate their preference of which papers they would like to review and place bids. See [Score Specification](https://docs.openreview.net/how-to-guides/paper-matching-and-assignment/how-to-do-automatic-assignments/how-to-run-a-paper-matching#scores-specification) to see how bids are then converted into scores.&#x20;

**When to Use it**

The Bid Stage should not be run until after the submission deadline, except in the case of public, single blind venues. For public and single-blind venues, the Bid Stage can be run before the submission deadline if they first run 'Post Submission Stage' to create paper groups.&#x20;

It is recommended that venues first run Paper Matching to compute conflict of interest before opening the Bid Stage, but this is not required. If the conflict of interest scores are taken into account, the reviewers' own papers will be excluded from their bid selections.

<details>

<summary>Bid Start Date</summary>

* When the bidding invitation opens, time in GMT.
* Optional (will open immediately if start date is not selected)

</details>

<details>

<summary>Bid Due Date</summary>

* When bidding closes, time in GMT.
* Required

</details>

<details>

<summary>Bid Count</summary>

* The minimum number of bids required for the task to be complete.
* Defaults to 50.
* Optional

</details>

