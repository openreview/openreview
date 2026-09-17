# How to release reviews

When you are ready to release the reviews, run the [Review Stage](../../reference/stages/review-stage.md) from the [venue request form](../../getting-started/hosting-a-venue-on-openreview/navigating-your-venue-pages.md#venue-request-form) and update the visibility settings to determine who should be able to see them. This will change the readers of all existing reviews in bulk.

Please note that if you want to release the reviews to the public, you will first need to make all submissions public if they are not already. If you select to release the reviews to the public while trying to release them to authors, it will not work if the submissions are not already public. If your decision stage has passed, you can use the 'Post Decision Stage' to release submissions. If you need to make submissions public but have not yet posted the decisions, [contact OpenReview support](https://openreview.net/contact) for assistance.

### Make Reviews Public overrides Release Reviews to Authors

The two settings are not independent. When **Make Reviews Public** is set to yes, the readers of every review are set to `everyone` and nothing else is added to the list — the paper's Authors group included. **Release Reviews to Authors is not consulted at all in that case**, even when it is also set to yes.

This matters for venues that release accepted and rejected papers together. With the venue configured to make accepted submissions public and hide rejected ones, running the Review Stage with both settings on gives the reviews of every paper the same `everyone` readers. The reviews on the accepted, public papers are released as expected, and the authors of the rejected papers are left seeing nothing, because they were never added as readers.

### Releasing reviews to authors

To make sure authors can read the reviews of their own paper, run the [Review Stage](../../reference/stages/review-stage.md) with:

* **Make Reviews Public**: no
* **Release Reviews to Authors**: yes

The readers are then built explicitly — Program Chairs, Senior Area Chairs, Area Chairs, the reviewers, and the paper's Authors — for every submission, whether or not the submission itself is public.

{% hint style="warning" %}
The Review Stage applies one visibility policy to every review it governs. There is no setting that makes reviews public on accepted papers and author-visible on rejected papers in the same run, and the Post Decision Stage changes the readers of submissions only, not of their reviews. If you need that split, [contact OpenReview support](https://openreview.net/contact).
{% endhint %}

### If the stage would not submit at all

If your venue keeps **all** submissions private, setting Make Reviews Public to yes is rejected outright with:

```
Reviews cannot be released to the public since all papers are private
```

The whole stage fails, so nothing is applied — including Release Reviews to Authors, even if you set it to yes in the same run. Set Make Reviews Public back to no and submit again. The same check applies to the Meta Review and Decision stages.
