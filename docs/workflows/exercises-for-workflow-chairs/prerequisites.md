# Prerequisites

All exercises should be practiced on the dev site. For any exercise, you will need to have the following prerequisites set up.&#x20;

1. Create a profile on the dev site: [https://dev.openreview.net/signup](https://dev.openreview.net/signup)
2. Create a venue on the dev site: [https://dev.openreview.net/group?id=OpenReview.net/Support](https://dev.openreview.net/group?id=OpenReview.net/Support)
3. Send an email to info@openreview.net to request that the venue is deployed
4. Instantiate the python client using your dev profile credentials:

```
import openreview
client = openreview.api.Client(
    baseurl='https://devapi2.openreview.net',
    username=<your dev username>,
    password=<your dev password>
)
```

5. Recommended: Familiarize yourself with the [Example Workflow](../conferences.md)
