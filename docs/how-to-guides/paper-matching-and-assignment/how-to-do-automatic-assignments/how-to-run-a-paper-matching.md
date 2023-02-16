# How to run a paper matching

In order to automatically assign Reviewers and Area Chairs, you must:

1. Enable the 'Review' or 'Post Submission' stage from your venue request form. This can only be done AFTER the submission deadline has passed.&#x20;
   1. The [Review Stage](../../../reference/stages/review-stage.md) sets the readership of reviews.
   2. The [Post Submission](../../../reference/stages/post-submission-stage.md) stage sets readership of submissions.
2. Use the 'Paper Matching Setup' button on your venue request form to [calculate affinity scores and conflicts.](how-to-setup-paper-matching-by-calculating-affinity-scores-and-conflicts.md)

After you complete these steps, a link for 'Paper Assignments' should appear on your Program Chair console.

![](<../../../.gitbook/assets/image (2) (1).png>)

Clicking on one of the assignment links will bring you to the assignment page, where you can create a new matching configuration. If members of your reviewer or area chairs group have profiles without publications, you will need to select ‘Yes’ for ‘Allow Zero Score Assignments’ in order to obtain a solution. Please note that all members of a group must have OpenReview profiles in order for the automatic assignment algorithm to run. Any members without profiles must be removed from the group before this step.

You can learn more about our automatic paper matching algorithm from its github repo: https://github.com/openreview/openreview-matcher. To create a new matching, click the 'New Assignment Configuration'. This will pull up a form with some default values pertaining to your matching settings:

<details>

<summary>User Demand</summary>

* The number of users that should be assigned to each paper &#x20;

</details>

<details>

<summary>Max Papers </summary>

* The maximum number of papers that can be assigned to each reviewer

</details>

<details>

<summary>Min papers </summary>

* The minimum number of papers that can be assigned to each reviewer

</details>

<details>

<summary>Alternates </summary>

* How many alternate reviewers should be saved per paper&#x20;

</details>

<details>

<summary>Paper Invitation </summary>

* Invitation of the submissions that will be assigned in this matching&#x20;
* Defaults to venue\_id/-/Submission for single blind and venue\_id/-/Blind\_Submission for double blind venues

</details>

<details>

<summary>Match Group </summary>

* The group ID of users to be assigned to submissions&#x20;

</details>

<details>

<summary>Scores Specification</summary>

* JSON providing further details and customization to scores.
*   Each key represents an edge invitation (affinity score, bid, etc.). Weight can be added to a given score value with the numerical field 'Weight'. 'Default' is a numerical value that is used when there is not an edge for a specific reviewer-paper pair. Finally, 'translate\_map' is a map function that translates an edge label value into a number.

    In the example below, the aggregate score being used by the optimizer is: weight \* (affinity score) + weight \* (translate\_map(bid))
* ```
  {
      "Example_Venue/2022/Conference/Reviewers/-/Affinity_Score": {
          "weight": 1,
          "default": 0
      },
      "Example_Venue/2022/Conference/Reviewers/-/Bid": {
          "weight": 1,
          "default": 0,
          "translate_map": {
              "Very High": 1,
              "High": 0.75,
              "Neutral": 0,
              "Low": -0.5,
              "Very Low": -1
          }
      }
  }
  ```

</details>

<details>

<summary>Conflicts Invitation</summary>

* Invitation for storing conflicts between users and papers&#x20;
* Defaults to venue\_id/user\_group/-/Conflict

</details>

<details>

<summary>Custom User Demand Invitation </summary>

* If certain papers require a custom number of assigned users, Program Chairs can create edges determining these requests and provide the invitation for used for those edges here.&#x20;
* Defaults to venue\_id/user\_group/-/Custom\_User\_Demands

</details>

<details>

<summary>Custom Max Papers Invitation </summary>

* Some reviewers may submit requests to only have a certain number of assigned papers. The matcher will convert those requests into edges. This determines the invitation that will be used for those edges.&#x20;
* Defaults to venue\_id/user\_group/-/Custom\_Max\_Papers

</details>

<details>

<summary>Solver </summary>

* Which algorithm (MinMax, Fairflow, or Randomized) will be used to generate automatic assignments.&#x20;
  * MinMax: Optimizes the scores while respecting the min and max quotas for each paper and reviewer. You can read more about MinMax [here](https://developers.google.com/optimization/flow/mincostflow).&#x20;
  * Fairflow: Tries to make every match have at least some minimum affinity. You can read more about Fairflow [here](https://arxiv.org/abs/1905.11924v1).
  * Randomized: Generates randomized assignments and selects the assignment that maximizes expected total affinity without breaking the probability limits. You can read more about the Randomized solver [here](https://arxiv.org/abs/2006.16437).
* You can read more about all solver options [here](https://github.com/openreview/openreview-matcher#solvers).
* Defaults to MinMax

</details>

<details>

<summary>Allow Zero Score Assignments </summary>

* Whether or not assignments with scores of 0 should be allowed. If a reviewer does not have any publications listed on their profile, they will have an affinity score of 0 with all submissions. Therefore, if you have users without publications, you will need to select "yes" in order to get a solution.&#x20;

</details>

<details>

<summary>Randomized Probability Limits </summary>

* If you select "Randomized" for the solver, it will select a random assignment that maximizes expected total affinity, subject to the probability limit provided here. What this means is that for each reviewer-paper assignment, the probability of that match being generated in a random assignment is limited to this value. This should make the outcome of the matching more difficult to predict.&#x20;

</details>

After filling out the matching configuration form and hitting submit, you should see the following:

![](<../../../.gitbook/assets/image (2) (1) (2).png>)

You can view, edit or copy the values you filled out in the matching form. When you are happy with your configuration, you should hit 'Run Matcher' and wait until its status is 'Complete'. This generates proposed assignments, with options to browse assignments, view statistics or deploy matching. If you click ‘Browse Assignments’ you will be brought to the edge browser, where you can browse, edit, and create proposed assignments.&#x20;

If you get "No Solution" after running the matcher, you can view the configuration to see the entire error message. If the message is something like the following:

* **Error Message:** Total demand (150) is out of range when min review supply is (34) and max review supply is (100)

that means that your constraints require more reviewers or area chairs than you currently have. The total demand is equal to (number of submissions \* user demand) + (number of submissions \* alternates). The max review supply is the number of reviewers available \* max papers and the review supply is the number of reviewers available \* min papers. Your total demand must fall within this range in order to obtain a solution.

**Note that completion of this step does not make assignments, it only creates a proposed assignment configuration. Those assignments will need to be deployed before Reviewers or Area Chairs will see them.**&#x20;
