# Installing and Instantiating the Python client

1. You will need to install the openreview-py client.&#x20;

```
git clone git@github.com:openreview/openreview-py.git
cd openreview-py
pip3 install -e .
```

2\. Create a client object with your OpenReview credentials. If you do not yet have an OpenReview profile, [you will need to make one now](../creating-an-openreview-profile/signing-up-for-openreview.md).&#x20;

```
import openreview
client = openreview.Client(baseurl='https://api.openreview.net', username=<your username>, password=<your password>)
```
