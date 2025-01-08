# How to check the API version of a venue

OpenReview has two APIs. API 2 is the current version and is used by all new venues. API 1 is the legacy version which may be used by some venues through 2024. There are some differences in data retrieval and structure between the two API versions-  differences are noted at several places in the documentation.&#x20;

If you are not sure what version of the API your venue is using, you can check it by using the Venue ID, retrieving the venue group, and checking for the 'domain' field. If the domain field is absent, then it is an API 1 venue, if it is present, then it is an API 2 venue.&#x20;

An example for an API1 and API 2 venue is below:

<pre class="language-python"><code class="lang-python"><strong>api2_venueid = 'NeurIPS.cc/2024/Conference' #In 2024, NeurIPS used API 2
</strong>api2_venue_group = live_client_v2.get_group(api2_venueid)
api2_venue_domain = api2_venue_group.domain
print(api2_venue_domain) 
#Output: NeurIPS.cc/2024/Conference

api1_venueid = 'NeurIPS.cc/2022/Conference' #In 2022, NeruIPS used API1
api1_venue_group = live_client_v2.get_group(api1_venueid)
api1_venue_domain = api1_venue_group.domain
print(api1_venue_domain) 
#Output: None
```
</code></pre>

See also: [Installing and initiating the python client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md), [How to find a venue ID](../../getting-started/frequently-asked-questions/how-do-i-find-a-venue-id.md)
