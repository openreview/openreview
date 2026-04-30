# Prerequisites

All exercises should be practiced on the dev site. For any exercise, you will need to have the following prerequisites set up.&#x20;

1. Review the documentation [here](../../how-to-guides/workflow/how-to-test-your-venue-workflow.md) and create a venue request for the dev site.
2. Instantiate the python client using your dev profile credentials:

```python
import openreview
# API 1 client
dev_client_v1 = openreview.Client(
    baseurl='https://devapi.openreview.net',
    username=dev_username,
    password=dev_pass
)

# API 2 client
dev_client_v2 = openreview.api.OpenReviewClient(
    baseurl='https://devapi2.openreview.net',
    username=dev_username,
    password=dev_pass
)
```

5. Recommended: Familiarize yourself with the [Example Workflow](../conferences.md)
