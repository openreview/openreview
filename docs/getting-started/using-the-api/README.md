# Using the API

There are currently two APIs supported. The current API (referred to in the documentation as API or API 2) is the current API version, and is the default version used for all operations unless otherwise specified.

The legacy API v1 is being phased out, but is still used for some conferences (primarily those before 2024)

While most operations will work on both APIs, pay careful attention when that is not the case, for example, the JSON format for each API is different.

## You need an OpenReview account

You need an OpenReview account to use the API. You authenticate with the same username and password you use on the website, and the API enforces the same permissions as the site does: you can only read and edit what your account is allowed to read and edit.

If you do not have an account yet, [sign up for one](../creating-an-openreview-profile/signing-up-for-openreview.md) before going any further. Once you have one, see [Installing and Instantiating the Python client](installing-and-instantiating-the-python-client.md) for how to authenticate.
