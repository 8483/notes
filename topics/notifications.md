Push API is the W3C standard for web push notification protocol.

Push API is a web-push, unlike a native-push.

`Push` and `notification` use different, but complementary, APIs:

-   `push` is invoked when a server supplies information to a service worker.
-   `notification` is the action of a service worker or web page script showing information to a user.

Service workers (core for PWAs) are needed for the browsers to handle push notifications.

1. Backend endpoint is needed
2. Clients sends subscription object to server endpoint
3. Client shows notification

Running the front end Javascript to subscribe to your notifications, and getting the device token returned.
Granting a client access to your notifications is very simple using Javascript. If a valid push package is returned, access will be granted and a device token is given in a JSON response. A device token is a unique identifier for each Mac that subscribes to the notification service. When sending a notification, the device token determines which Mac the notification will be sent to.
