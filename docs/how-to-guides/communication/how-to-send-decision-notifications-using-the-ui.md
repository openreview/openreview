# How to Send Decision Notifications Using the UI

After the decision stage closes, you will see the [Post Decision Stage](../../reference/stages/post-decision-stage.md) option on your venue request form. You will be able to use this stage to send bulk decision notifications to authors.&#x20;

1. Select "Yes, send an email notification to the authors" for the "Send Decision Notifications" field.
2. Customize the Email Content Fields. There should be one per decision type for your venue. **Do not remove the parenthesized tokens.** Any fields in curly braces will be populated with the information for each paper. You can customize anything in the message so long as you do not remove the curly braces.&#x20;
3. Click submit and notification emails will be sent to all authors. If decision notifications are sent successfully, a comment will be posted to your venue request form with a link to see the emails that were sent.&#x20;

Note that if you run the Post Decision Stage multiple times, new decision notifications will not be sent again even when "Yes, send an email notification to the authors" is selected. To re-send decision notifications, you should use the[ python client](https://docs.openreview.net/how-to-guides/communication/how-to-send-messages-with-the-python-client).
