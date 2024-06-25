# What do the different 'status' values mean in the message logs?

**Sent**: The initial status value, meaning the message was sent to the email service.

**Dropped**: The email was not sent to the recipient for delivery.

**Deferred**: The email cannot immediately be delivered, but has not been completely rejected. The service will continue to try for 72 hours to deliver the deferred message. After 72 hours the defferal turns into a block.

**Bounce**: The server cannot or will not deliver the message. This is often caused by outdated or an invalid email address.&#x20;

**Delivered**: The email has been accepted by the receiving server. This does not mean that the email made it to the recipient's inbox.&#x20;

**Blocked**: The email was denied temporarily by the server. This is unrelated to the validity of the address.&#x20;

