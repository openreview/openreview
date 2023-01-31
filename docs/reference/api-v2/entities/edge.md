# Edge

Edges are being used to represent matching data, like affinity scores, conflicts, custom quotas, etc. The edge entity contains to main properties: head and tail and optinally a label and weight. Head and tail properties can point to other entities in the system like note, group and profile.

## Custom Max Papers

A member of the reviewing committee can specify a maximum number of papers to review. The range of max numbers is usually defined by the organizers of the venue. Users can specify this value during the recruitment period or request the value to the organizers. Organizers can post these edges using our Python library. See the example below:

```
client.post_edge(openreview.api.Edge(
    invitation='ICML.cc/2023/Conference/Reviewers/-/Custom_Max_Papers',
    head='ICML.cc/2023/Conference/Reviewers',
    tail='~Reviewer_ICMLOne1',
    signatures=['ICML.cc/2023/Conference/Program_Chairs'],
    weight=2
))
```

Please make sure the tail value is the profile id of the user. To get the profile id you can use the following call:

```
profile = openreview.tools.get_profile('reviewer@icml.cc')
profile.id
```

Once the edge is posted, it will appear in the reviewers console as the following image:

<figure><img src="../../../.gitbook/assets/Screen Shot 2023-01-31 at 11.14.16 AM.png" alt=""><figcaption></figcaption></figure>
