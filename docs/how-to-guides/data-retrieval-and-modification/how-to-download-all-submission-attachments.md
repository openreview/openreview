# How to download all submission attachments

If you get rate limit errors downloading files one at a time, use `openreview.tools.get_all_attachments`. It downloads files in batches of 50, so it makes far fewer requests.

```python
import openreview

client = openreview.api.OpenReviewClient(
    baseurl='https://api2.openreview.net',
    username='your-username',
    password='your-password'
)

files = openreview.tools.get_all_attachments(
    client,
    venueid='your-venue-id/Submission',  # or 'your-venue-id' for accepted papers
    field_name='pdf',                    # or 'supplementary_material', etc.
    output_dir='pdfs'
)
```

If `get_all_attachments` isn't found, upgrade with `pip install --upgrade openreview-py`. If nothing downloads, check that `venueid` matches the value on your submissions.
