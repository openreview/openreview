# Prerequisites

All exercises should be practiced on the dev site. For any exercise, you will need to have the following prerequisites set up.&#x20;

1. Review the documentation [here](../../how-to-guides/workflow/how-to-test-your-venue-workflow.md) and create a venue request for the dev site.
2. Instantiate the python client using your dev profile credentials:

```
import openreview
client = openreview.api.Client(
    baseurl='https://devapi2.openreview.net',
    username=<your dev username>,
    password=<your dev password>
)
```

5. Recommended: Familiarize yourself with the [Example Workflow](../conferences.md)
