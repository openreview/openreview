# How to Change the Expiration Date of the Submission Invitation

The submission deadline set through the venue request form is actually only the advertised due date. The true deadline is the expiration date, which is set to 30 minutes after the submission deadline. If you would like to change the expiration date to allow the authors more or less wiggle room around the submission deadline, you can do so using the python client.&#x20;

1. If you have not done so, you will need to [install and instantiate the openreview-py client](../../getting-started/using-the-api/installing-and-instantiating-the-python-client.md).&#x20;
2. Choose your desired expiration date. You will need to convert it into epoch time in milliseconds using an [epoch time converter.](https://www.epochconverter.com/) For example:&#x20;

```python
expdate = 1650639704000
```

Create an Invitation Edit

```python
client.post_invitation_edit(
    invitations='<invitation-id>',  # This is the Invitation used to create this Invitation Edit
    readers=[venue_id],
    writers=[venue_id],
    signatures=[venue_id],
    invitation=openreview.api.Invitation(
        id="<Your/Venue/ID/-/Submission",
        expdate=expate
        signatures=[venue_id]
    )
)
```

