# How to Add and Remove Members from a Group

The program committee is represented as a group, like Reviewers, Area Chairs, Action Editors, etc. Each of these groups have a property 'members' that can contain a set of groups. Email addresses and profile ids are also represented as groups.&#x20;

### **Adding Members to the Group**

If you want to build you own group of reviewers, you can simply add them as members to the group.

**Using the Group Editor**

1. Access the group editor at the following URL, replacing `group_id` with the actual ID of your group: `https://openreview.net/group/edit?id=group_id`&#x20;
2. Under the Group Members section select the text box area, add their profile ID or email, and click on Add to Group

<figure><img src="../../.gitbook/assets/image (1) (2).png" alt=""><figcaption><p>Group UI</p></figcaption></figure>

**Using the Python Library**

To remove members using the python client, utilize the `add_members_to_group` function:\
\
`client.add_members_to_group(group, ['~Jane_Doe1', '~Melisa_Bok1'])`

Replace `group` with your target group's ID, and include the profile ids or emails of the members you wish to remove in the list.

### Removing Members from the Group

Should you need to remove members from a group, you can also use the group editor or the python library.

**Using the Group Editor**&#x20;

1. Access the group editor at the following URL, replacing `group_id` with the actual ID of your group: `https://openreview.net/group/edit?id=group_id`
2. In the Group Members section, locate the profiles/emails of the members you wish to remove.
3. Select the profile ID or email and click 'Remove' or 'Remove Selected'  to delete them from the group.

<figure><img src="../../.gitbook/assets/Screen Shot 2024-02-05 at 4.35.17 PM.png" alt=""><figcaption><p>Group UI</p></figcaption></figure>

**Using the Python Library**

To programmatically remove members using the python library, utilize the `remove_members_from_group` function:

```python
client.remove_members_from_group(group, ['~Jane_Doe1', '~Melisa_Bok1'])
```

Replace `group` with your target group's ID, and include the profile ids or emails of the members you wish to remove in the list.
