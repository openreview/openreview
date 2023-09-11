# How to Post a Custom Conflict

If you are using automatic matching, you can generate conflicts automatically with Paper Matching Setup. Sometimes Program Chairs additionally want to create custom conflicts between certain users and certain papers. This can be done by posting an edge to the conflict edge invitation for your venue.&#x20;

1. If you have not done so, you will need to [install and instantiate the openreview-py client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).
2. Determine the conflict invitation you will be using. If you are creating a custom conflict edge for a reviewer, it would be \<your\_venue\_id>/Reviewers/-/Conflict. If it is for an Area Chair, it would be \<your\_venue\_id>/Area_\__Chairs/-/Conflict.

```python
invitation = "<your_venue_id>/Reviewers/-/Conflict"
```

1. Set a variable 'tail' to the user you are generating the conflict for. Set a variable 'head' to the forum of the submission you want to create the conflict for. For example:

```python
tail = "~User_One1"
head = "ggVj9Mmq2-a"
```

1. Set the readers of this conflict. In general, this should include the tail of the edge and anyone who might be making assignments for this group, such as Area Chairs or Senior Area Chairs. You can confirm who the readers should be by going to openreview.net/invitation/edit?id=\<conflict\_invitation> and checking the readers in the reply field.

```python
readers = [
    "~User_One1", 
    "your_venue_id",
    "<your_venue_id>/Area_Chairs"
]
```

1. Optionally set a label for your custom conflicts. This can help you query and retrieve them later.&#x20;

```python
label = "Custom Conflict"
```

1. Finally, create and post an edge with a weight of -1 between the user and the paper. This will make the conflict a hard constraint.&#x20;

```python
client.post_edge(openreview.Edge(
   invitation = invitation,
   label = label, 
   weight = -1, 
   head = head, 
   tail = tail,
   signatures = [
    "<your_venue_id>"
   ],
   readers = [
    "<your_venue_id>"
   ],
   writers = [
    "<your_venue_id>"
]))
```
