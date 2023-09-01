# How to Upload Paper Decisions in Bulk

There are two possible ways to post decisions to papers:&#x20;

1. Manually, using individual Decision buttons on each paper&#x20;
2. In bulk, using the Decision Stage.&#x20;

The first option will be automatically enabled after the [Decision Stage](../../reference/stages/decision-stage.md) is run.&#x20;

To upload decisions in bulk, you can do the following:&#x20;

1. Create your decisions csv. The csv should contain the paper\_number, decision, and comment for each paper, with one paper per line. It should not include column names. The comment field is optional, so you can leave that column empty if you wish. Your csv should look something like the following:&#x20;
2.

    <figure><img src="../../.gitbook/assets/Screen Shot 2023-05-19 at 11.57.25 AM.png" alt="Three columns: column A contains paper_number, column B contains the decision, column C contains decision comments (optional)"><figcaption><p>Example of decision csv file</p></figcaption></figure>

or like this:&#x20;

```
1,Accept,A decision comment 
2,Reject, 
3,Accept,A decision comment
```

1. From your [venue request form](../../getting-started/hosting-a-venue-on-openreview/navigating-your-venue-pages.md#venue-request-form), click the Decision Stage button. Upload your decisions csv for the "Decisions File" field.&#x20;
2. Click submit.&#x20;

If you had already manually posted decisions, the bulk upload will overwrite them.&#x20;
