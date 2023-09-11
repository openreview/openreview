# How to loop through Accepted Papers and print the Authors and their Affiliations

1. If you have not done so, you will need to [install and instantiate the openreview-py client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).&#x20;
2. [Get all the accepted papers](how-to-get-all-submissions.md) for your venue.&#x20;
3. Iterate through each accepted paper, printing the number and title of the paper. To get the affiliations for each author, get the profiles by passing the author IDs in the content of the accepted paper. Iterate through the author profile and print the author's preferred name and history containing the affiliation.

```python
# API 2

for accepted_submission in accepted_submissions:
    print(accepted_submission.number, accepted_submission.content['title']['value'])
    author_profiles = openreview.tools.get_profiles(client, accepted_submission.content['authorids']['value'])
    for author_profile in author_profiles:
        print(author_profile.get_preferred_name(pretty=True), author_profile.content.get('history', [{}])[0])
```

```python
# API 1

for accepted_submission in accepted_submissions:
    print(accepted_submission.number, accepted_submission.content['title'])
    author_profiles = openreview.tools.get_profiles(client, accepted_submission.content['authorids'])
    for author_profile in author_profiles:
        print(author_profile.get_preferred_name(pretty=True), author_profile.content.get('history', [{}])[0])
```
