# How to Enable Reviewer Recommendations for Area Chairs

Reviewer Recommendations let Area Chairs nominate the Reviewers they would like assigned to each of their papers and score each nomination from 1 to 10. The scores are stored as edges, which Paper Matching can then use as a scoring signal alongside affinity scores and bids.

There is no field for this on the venue request form. To have it enabled for your venue, [add a comment to your venue request form](../../getting-started/hosting-a-venue-on-openreview/navigating-your-venue-pages.md#venue-request-form) or reach OpenReview Support through the [Contact Form](https://openreview.net/contact). Include the recommendation start date, the due date, and the number of Reviewers each Area Chair should recommend per paper.

{% hint style="info" %}
Deploy your Area Chair assignments before recommendations open. The recommendation console lists each Area Chair's papers from their deployed assignments, so an Area Chair with no deployed assignments will see an empty list.
{% endhint %}

## What gets created

Enabling the stage creates an edge invitation at `<your venue id>/Reviewers/-/Recommendation`:

* Each edge links a submission to a Reviewer's profile, and its weight is the recommendation score, an integer from 1 to 10.
* Area Chairs are invited to post these edges. If your venue has Senior Area Chairs, they are invited as well.
* Each edge is readable by the venue and by that paper's Area Chairs and Senior Area Chairs. Authors are explicitly excluded.
* The number of Reviewers to recommend per paper defaults to **7**. It is shown to Area Chairs in the instructions and is used to measure completion in the Program Chair Console.

## What Area Chairs see

Area Chairs reach the stage from the task in their console, which opens a page of instructions:

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2F44LeWJv878bHczzPF3iD%2Fimage.png?alt=media&#x26;token=940515b2-2089-414b-a986-a00e21eabdb1" alt="The Reviewer Recommendation instructions page, listing how many reviewers to recommend, the 10 to 1 scoring scale, and a Recommend Reviewers button"><figcaption><p>The instructions page as an Area Chair sees it. The number of Reviewers to recommend is the value set when the stage was opened.</p></figcaption></figure>

Clicking **Recommend Reviewers** opens the Edge Browser. The left column lists the Area Chair's assigned papers and how many recommendations each one has so far; selecting a paper lists the eligible Reviewers on the right.

<figure><img src="https://622636955-files.gitbook.io/~/files/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FVorH499wd7ipUjYX5etp%2Fuploads%2FDTwOOXIIocQP4BmbFccc%2Fimage.png?alt=media&#x26;token=1aa34371-f1e2-423a-a580-a6730b8cabfc" alt="The Edge Browser showing an Area Chair&#x27;s assigned papers on the left and, for the selected paper, a list of reviewers with a Recommendation score dropdown and affinity score on the right"><figcaption><p>Recommending Reviewers for a submission. Recommended Reviewers are highlighted and show the score the Area Chair gave them.</p></figcaption></figure>

In the Reviewer column, Area Chairs can:

* Set a score from 1 to 10 with the **Recommendation** dropdown, or remove a recommendation with the trash icon.
* Sort by **Affinity Score**, or by bid if you ran a [Bid Stage](../../reference/stages/bid-stage.md) for Reviewers.
* Search for a Reviewer by name or institution.
* See how many papers each Reviewer has already been recommended for, which helps spread recommendations across the pool.

Reviewers who have a conflict with the selected paper are not shown, provided you computed conflicts when you [set up paper matching](how-to-do-automatic-assignments/how-to-setup-paper-matching-by-calculating-affinity-scores-and-conflicts.md). Recommendations can be changed or removed until the due date passes.

## Using recommendations in paper matching

When paper matching data is set up for the Reviewers group, if the recommendation invitation already exists it is added to the matching configuration's [Scores Specification](how-to-do-automatic-assignments/how-to-run-a-paper-matching.md#scores-specification) with a weight of 1 and a default of 0, so recommendations count towards the aggregate score the matcher optimizes.

The Scores Specification is written when the matching data is set up. If you set up matching for the Reviewers group before the recommendation stage was opened, add the entry to the configuration yourself before running the matcher:

```
"<your venue id>/Reviewers/-/Recommendation": {
    "weight": 1,
    "default": 0
}
```

Raise the weight to make recommendations count for more than affinity scores, or lower it to treat them as a weak preference.

## Tracking progress

In the Program Chair Console:

* The **Overview** tab shows **Recommendation Progress**: the number of Area Chairs who have posted at least the target number of recommendations, counted across all of their papers rather than per paper.
* The **Area Chair Status** tab shows **Reviewers Recommended** for each Area Chair, with a link to their recommendations in the Edge Browser.
