# Edges

Bids, assignments, affinity scores, conflicts, etc. are saved as _Edges_ in OpenReview.

Simply speaking, _Edges_ are links between two OpenReview entities (_Notes_, _Groups_, _Profiles_, etc.).

Besides the fields that define user permissions, an _Edge_ would usually contain these fields: _head_, _tail_, _weight_, _label_.

For example, a OpenReview affinity score edge for a paper-reviewer pair may have the reviewer’s _Profile_ id set in the _edge.head_ field, paper id set in the _edge.tail_ field, and OpenReview affinity score set in the _edge.weight_ field.

All _Edges_ respond to some OpenReview _Invitation_, which specifies the possible content and permissions that an _Edge_ is required to contain.

Users can query for edges using any combination of the following fields: \* the ID of the _Invitation_ that it responds to \* head \* tail \* label.&#x20;

Consider the following example which gets the first 10 _Edges_ representing the “Personal” conflicts in ICLR 2020:

```
>>> conflict_edges = client.get_edges(
        invitation='ICLR.cc/2020/Conference/-/Conflict',
        label='Personal',
        limit=10)
```

Note that since conflict data is sensitive, you may not have permissions to access conflict edges mentioned in the above example.

By default, _get\_edges_ will return up to the first 1000 corresponding _Edges_ (_limit=1000_). To retrieve _Edges_ beyond the first 1000, you can adjust the _offset_ parameter, or use the function _get\_all\_edges_ which returns a list of all corresponding _Edges_:

```
conflict_edges_iterator = openreview.tools.iterget_edges(
        client,
        invitation='ICLR.cc/2020/Conference/Reviewers/-/Conflict',
        label='Personal')
```

Since edges usually are very large in numbers, it is possible to get just the count of edges by using the function client.get\_edges\_count

```
>>> conflict_edges_count = client.get_edges_count(
        invitation='ICLR.cc/2020/Conference/Reviewers/-/Conflict',
        label='Personal')
```

Since most of the common tasks performed using _Edges_ require _Edges_ to be grouped, it’s also possible to query for already grouped _Edges_. Consider the following example that gets all reviewers grouped by papers they have conflicts with for the ICLR 2020 Conference

```
grouped_conflict_edges = client.get_grouped_edges(
        invitation='ICLR.cc/2020/Conference/Reviewers/-/Conflict',
        groupby='head',
        select='tail,weight,label')
```

Consider the following example that gets all papers grouped by reviewers who have conflicts with for the ICLR 2020 Conference.

```
grouped_conflict_edges = client.get_grouped_edges(
        invitation='ICLR.cc/2020/Conference/Reviewers/-/Conflict',
        groupby='tail',
        select='head,weight,label')
```

To group _Edges_, one must already know what the _edge.head_ and _edge.tail_ represent in an _Edge_ and that information can be seen from the _Edge_’s invitation.
