# Exercise: Getting profile information

## Setup:

1. Complete [Prerequisites](prerequisites.md)

## Part 1: Getting all profiles from a list

### 1. Create a list of profiles to query

First, create a `list` of profiles to query. This could be a custom list, all authors of the submissions, all members of a group, such as reviewers, etc. This should be a list of profile IDs or emails that are associated with profiles.

Some useful examples from the documentation:

* [How to get all author profiles](../../how-to-guides/data-retrieval-and-modification/how-to-loop-through-accepted-papers-and-print-the-authors-and-their-affiliations.md)
* [How to use get\_group](../../getting-started/using-the-api/retrieving-posting-a-group/copying-members-from-one-group-to-another.md)



**Check your work:** Print the list of profiles. You should see a list of profiles in the format `['~First_Last1'....]` or `['example@email.com',.....]`

### 2. Get all profiles from that list

Review the example [here](../../getting-started/objects-in-openreview/introduction-to-profiles.md) to get profiles from a list. This will return a list of profile objects. The relevant code is:

```python
profiles = openreview.tools.get_profiles(client_v2,profile_id_list,as_dict=True)
```

**Check your work:**  The result should be a dictionary with the query profile ID and the OpenReview Profile Object.&#x20;

### 3. Parse the profiles

Review the example [here](../../getting-started/objects-in-openreview/introduction-to-profiles.md) to understand how profile information is stored, and one example of how to flatten and tabulate the data.&#x20;

**Check your work**: Use Python to get the profile ID property, the preferred email, and the current institutional affiliation. Then print the profile information and check that the data you extracted is correct.



