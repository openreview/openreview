# Introduction to Groups

See also:

* [Technical reference for Groups](../../../reference/api-v2/entities/group/)
* [Managing Groups for Venue Workflows](../../../how-to-guides/managing-groups/)

In OpenReview, a **Group** object represents a collection of users (individuals or other groups) and serves as the core mechanism for managing permissions, roles, and access control. Groups define who can read, write, sign, or modify content such as Notes and Invitations. They are hierarchical and addressable using a path-like ID (e.g., `ICLR.cc/2025/Conference/Reviewers`).

To give fine-grained control over readership, individuals within a conference are typically members of many groups. For example, a reviewer for the conference `Test/2025/Conference` assigned to Submissions 1 and 10 would likely be in the following groups:

* **Reviewer group:** This contains all reviewers that accepted the recruitment invitation`Test/2025/Conference/Reviewers`&#x20;
* **Relevant submission groups:** These groups give assigned reviewers (rather than all reviewers) the ability to view and post reviews for submissions they are assigned to. In this case it would be: `Test/2025/Conference/Submission1/Reviewers` `Test/2025/Conference/Submission10/Reviewers`&#x20;
* **Relevant anonymous groups:** These groups give the reviewer the ability to remain anonymous during the review process: `Test/2025/Conference/Submission1/Reviewer_Axyz` , `Test/2025/Conference/Submission10/Reviewer_Xyab`&#x20;

### **Structure**

You can find a description of all fields of the `Group` object in the [API Reference](../../../reference/api-v2/entities/group/). You will most often interact with the following key fields:

* **id**: Unique identifier for the group (e.g., `ICLR.cc/2025/Conference/Reviewers`).
* **members**: List of user IDs or group IDs that are members.

### **Retrieving Groups**

You can retrieve groups using the `get_group` or `get_groups` method:

<pre class="language-python"><code class="lang-python"><strong>reviewer_group = client.get_group('Test/2025/Conference/Reviewers')
</strong>conference_groups = client.get_groups(prefix='Test/2025/Conference')
</code></pre>

The `prefix` parameter allows filtering by group ID hierarchy.

#### Quickstart: Get a list of all active venues

```python
venues = client_v2.get_group(id='active_venues').members
print(venues)
```

#### Quickstart: See a venue's information

```python
venue_group = client_v2.get_group("<VENUE_ID>")
```

Within this group, `content`  is a dictionary that contains information about the venue.&#x20;

To see all possible keys contained in a venue group's `content` object, you can use the following:

```python
venue_group.content.keys()
```

You can query the information within this dictionary to get more information about the venue. For example: `venue_group.get_content_value('submission_id')`  will tell you the name of the Submission invitation.&#x20;

#### Quickstart: Get pending invited Area Chairs/ Reviewers

```python
#venue_id
venue_id = "<VENUE_ID>"
role = "Area_Chairs" #Alternatively, Reviewers, Senior_Area_Chairs

accepted = client_v2.get_group(venue_id + '/' + role)
invited = client_v2.get_group(venue_id + '/' + role +'/Invited')
declined = client_v2.get_group(venue_id + '/' + role + '/Declined')

pending_members = [m for m in invited.members if not m in accepted.members and not m in declined.members]
```

### Working with groups

Most of the changes that you want to make with groups can be made from the UI. See the section [Managing Groups](../../../how-to-guides/managing-groups/) for specific instructions on working with groups. In some specific cases, you may need to programmatically create or modify groups.&#x20;

#### Creating Groups

The majority of groups are created automatically in the workflow. However, Program Chairs (but not other roles) have the access necessary to create groups, should they need to.&#x20;

To add a group, you would use `post_group_edit`  function. This creates a group with a specific id that you can then add members to. As an example, if you ant to add an LLM Reviewer to each Submission Group 1:&#x20;

The new group ID would be: `MyConference/2025/Conference/Submission1/Reviewer_LLM`

The full `post_group_edit` call will look like this:

```python
client.post_group_edit(
    invitation='<venue_id>/-/Edit',
    signatures=['<venue_id>'],
    group=openreview.api.Group(
        id = '<venue_id>/Submission<number>/Reviewer_New_Group',
        readers = ['<venue_id>'],
        signatures = ['<venue_id>'],
        writers = ['<venue_id>'],
        signatories = ['<venue_id>'],
    )
)
```

#### Adding Members

To add members to a group, you use the function `add_members_to_group` , where `group` is the group id to add to, and `members` is group or list of profile ids to add.

```python
client.add_members_to_group(group='<venue_id>/Submission<number>/Reviewers', 
members='<venue_id>/Submission<number>/Reviewer_New_Group')
```

An alternative approach uses the more general `post_group_edit` function to modify groups which gives more flexibility how to modify groups. To use this function to add mamebers:&#x20;

```python
group = client_v2.get_group('<GROUP_ID>')
members = []
self.post_group_edit(invitation = f'{group.domain}/-/Edit', 
    signatures = group.signatures, 
    group = Group(
        id = group.id, 
        members = {
            'add': list(set(members)) #the keyword 'add' is used to add the members here
        }
    ), 
    readers=group.signatures, 
    writers=group.signatures
)
```

#### Removing Members

The function `post_group_edit` can also be used to remove members from a group by using the keyword `remove`:&#x20;

```python

group = client_v2.get_group('<GROUP_ID>')
members = []
self.post_group_edit(invitation = f'{group.domain}/-/Edit', 
    signatures = group.signatures, 
    group = Group(
        id = group.id, 
        members = {
            'remove': list(set(members)) #the keyword 'remove' is used to remove the members here
        }
    ), 
    readers=group.signatures, 
    writers=group.signatures
)
```

#### Replacing Members

Finally, `post_group_edit` can also be used to replace the members of a group with new members with the keyword `replace` .

```python
members = []
self.post_group_edit(invitation = f'{group.domain}/-/Edit', 
    signatures = group.signatures, 
    group = Group(
        id = group.id, 
        members = {
            'replace': { 'index': 2, 'value': '~OpenReview_Profile1' }
        }
    ), 
    readers=group.signatures, 
    writers=group.signatures
)
```



