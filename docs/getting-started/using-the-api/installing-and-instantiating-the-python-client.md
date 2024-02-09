# Installing and Instantiating the Python client

1. You will need to install the **openreview-py** client.&#x20;

```bash
pip install openreview-py
```

2\. Create a client object with your OpenReview credentials. If you do not yet have an OpenReview profile, [you will need to make one now](../creating-an-openreview-profile/signing-up-for-openreview.md).&#x20;

```python
import openreview

# API V2
client = openreview.api.OpenReviewClient(
    baseurl='https://api2.openreview.net',
    username=<your username>,
    password=<your password>
)

# API V1
client = openreview.Client(
    baseurl='https://api.openreview.net',
    username=<your username>,
    password=<your password>
)
```
