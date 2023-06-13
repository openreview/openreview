# Adding members to a group

Program committee is represented as group, like Reviewers, Area Chairs, Action Editors, etc. Each of these group has a property members that can contain a set of groups. Email addresses and profile ids are also represented as groups. If you want to build you own group of reviewers you can just simply add them as members of the group.

You have two options to edit the Reviewers group:

1. Using the group editor. You can find the group editor using the url: https://openreview.net/group/edit?id=group\_id&#x20;
2. Using our Python library:\
   \
   `client.add_members_to_group(group, ['melisa@mail.com', '~Melisa_Bok1'])`

