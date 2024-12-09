# Can I automatically transfer my Expertise Selection to another venue?

Expertise Selection is a task for Program Committee members (such as reviewers) that gives them the opportunity to identify which previous publications they would like to be considered when being matched with submissions to review. If you want to use the same Expertise Selection configuration for multiple venues, it is possible to transfer the expertise from one venue to another using the OpenReview Python client using the following steps.

{% hint style="info" %}
These instructions are for venues that use the default OpenReview setting of using all papers except those that are **excluded** by the user in the Expertise Selection Task. Before transferring expertise, please check that both venues use this method for Expertise Selection.
{% endhint %}



### 1. [Download and install the Python Client](../using-the-api/installing-and-instantiating-the-python-client.md)

```python
import openreview
from openreview.api import OpenReviewClient

live_client_v2 = OpenReviewClient(
    baseurl='https://api2.openreview.net',
    username=YOUR_USERNAME,
    password=YOUR_PASSWORD
)
```

### 2. Get Expertise Selection edges from the previous venue

OpenReview by default considers all publications as relevant expertise for paper matching and assignment. In Expertise Selection, edges are posted indicating the papers that should be _excluded_ from paper matching. You can get the edges from a previous venue using the following method:

```python
#set up venue and user information
profile_id = YOUR_TILDE_ID 
from_venue_id = FROM_VENUE_ID 
from_group_name = FROM_GROUP_NAME  #Typically 'Reviewers' or 'Area_Chairs'

#get edges for from_venue
expertise_edges = openreview_client.get_all_edges(invitation=f"{from_venue_id}/{from_group_name}/-/Expertise_Selection",
 tail=profile_id)
```

You will need the following information:

* **profile\_id:** Your profile ID (such as \~First\_Last1). Also see [Finding your Profile ID](../creating-an-openreview-profile/finding-your-profile-id.md)&#x20;
* **from\_venue\_id:** The id of the venue you want to transfer expertise from. Also see: [How do I find a venue ID?](how-do-i-find-a-venue-id.md)
* **group\_name:** The name of the group you are in on the Program Committee (typically this is Reviewers or Area\_Chairs)

The number of edges should correspond with the number of papers that you are excluding from Expertise Selection.

### 3. Copy edges to new venue

Finally, post an edge to the new venue that you are doing Expertise Selection for:

```python
to_venue_id = TO_VENUE_ID
to_group_name = TO_GROUP_NAME
invitation = f"{to_venue_id}/{to_group_name}/-/Expertise_Selection"

for edge in expertise_edges:
    openreview_client.post_edge(openreview.api.Edge(
        invitation = invitation,
        head = edge.head,
        tail = edge.tail,
        signatures = [profile_id],
        label = 'Exclude'
    ))
```

