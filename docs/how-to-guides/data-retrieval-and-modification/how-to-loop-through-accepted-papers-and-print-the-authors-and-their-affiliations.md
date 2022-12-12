# How to loop through Accepted Papers and print the Authors and their Affiliations

1. If you have not done so, you will need to [install and instantiate the openreview-py client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).&#x20;
2. Get all the accepted papers for your venue. The value for 'venue' is the Abbreviated Venue Name which can be located on the venue request form and the venue homepage underneath the official venue name. For example, 'SaTML 2023' is an abbreviated venue name, be sure to include yours in its place.

```
accepted_papers = client.get_all_notes(content={ 'venue': 'SaTML 2023'})
```

3\. Iterate through each accepted paper, printing the number and title of the paper. To get the affiliations for each author, get the profiles by passing the author IDs in the content of the accepted paper. Iterate through the author profile and print the author's preferred name and history containing the affiliation.

```
for accepted_paper in accepted_papers:
    print(accepted_paper.number, accepted_paper.content['title'])
    author_profiles = openreview.tools.get_profiles(client, accepted_paper.content['authorids'])
    for author_profile in author_profiles:
        print(author_profile.get_preferred_name(pretty=True), author_profile.content.get('history', [{}])[0])
```
