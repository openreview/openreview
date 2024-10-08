# How to enable bidding for Senior Area Chair Assignment

If you want to allow SACs to bid on ACs in assignment, you will need to run the bid stage from the python client. To do so you will need to:

1. [Install and connect to the Python client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md), if you haven't already. Please use the API 1 client.
2. Find your request form id (in the id= portion of the URL of the [venue request form](../../getting-started/hosting-a-venue-on-openreview/navigating-your-venue-pages.md)) and add it to `request_form_id`
3. Find your venue ID (listed in the venue ID field of the venue request form) and add it to `venue_id`
4. Update the `bid_start_date` and `bid_due_date`&#x20;

Then you can run the following block of code to start the SAC bidding stage

```
# Add the request form id as a string
request_form_id = ''

# Assign the venueID to a variable
venue_id = ''

# Change dates
bid_start_date = datetime.datetime(year=2024,month=9,day=30,hour=12,minute=0,second=0)
bid_due_date = datetime.datetime(year=2024,month=10,day=8,hour=12,minute=30,second=0)

venue = openreview.get_conference(client, request_form_id, setup=False)

sac_bid_stage = openreview.stages.BidStage(
    f'{venue_id}/Senior_Area_Chairs',
    start_date = bid_start_date,
    due_date = bid_due_date, 
    request_count = 25,
    score_ids=[f'{venue_id}/Senior_Area_Chairs/-/Affinity_Score']
)
venue.bid_stages = [sac_bid_stage]

venue.create_bid_stages()
```
