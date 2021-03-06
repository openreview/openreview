# Getting all Assignments for a User

Assignments are stored as edges. You can view all of the assignment edges for a user in two ways:&#x20;

#### View them at api.openreview.net

1. In your browser search bar, enter the following. If you want to view assignments for an Area Chair, change 'Reviewers' to 'Area\_Chairs'. Replace your\_venue\_id with your full venue id. This will bring up all of the assignment edges for all of your venue's reviewers.

```
api.openreview.net/edges?invitation=<your_venue_id>/Reviewers/-/Assignment
```

2\. Now we want to filter by a particular reviewer. If you look at a specific edge, you will see that the field "tail" corresponds to the profile ID of the assigned reviewer. So to see only the assignments for reviewer \~User\_One1, change your search query to the following:

```
api.openreview.net/edges?invitation=<your_venue_id>/Reviewers/-/Assignment&tail=~User_One1
```

#### Retrieve them with the Python Client

1. If you have not done so, you will need to [install and instantiate the openreview-py client](../installing-and-instantiating-the-python-client.md).&#x20;
2. Set a variable 'tail' to the profile ID of the user you are interested in, for example:

```
tail = "~User_One1"
```

3\. Set a variable 'invitation' to the invitation of the edges you are trying to view. If your user of interest is a reviewer, the invitation would be in the format \<your\_venue\_id>/Reviewers/-/Assignment.

```
invitation = "<your_venue_id>/Reviewers/-/Assignment
```

4\. Retrieve all of the edges posted to that invitation for the user of interest.&#x20;

```
edges = client.get_all_edges(invitation = invitation, tail = tail)
```
