# Copying Members from One Group to Another

Some venues with multiple deadlines a year may want to reuse the same reviewer and area chairs from cycle to cycle. In those cases, it is useful to use the python client to copy group members from one group to another rather than recruiting the same people each time.&#x20;

1. If you have not done so, you will need to [install and instantiate the openreview-py client](../installing-and-instantiating-the-python-client.md).
2. Get the group you are taking members from. You can get an individual group by its id. If you are copying reviewers from one venue iteration to another, for example, do the following: &#x20;

```
first_reviewer_group = client.get_group("<first_venue_id>/Reviewers")
```

3\. Each group has a field 'members' which is a list of profile IDs or emails belonging to the members of that group. Extract the members:&#x20;

```
reviewers = first_reviewer_group.members
```

4\. Finally, you can use add\_members_\__to\_group to add those members to your new Reviewers group.&#x20;

```
client.add_members_to_group("<second_venue_id>/Reviewers", reviewers)
```
