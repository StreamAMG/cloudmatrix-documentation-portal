---
stoplight-id: fo7u260nyu989
---

# Integrating with Cloudmatrix Lifecycle Events via AWS SNS

This guide will walk you through the process of integrating your application with our Cloudmatrix Lifecycle Events via AWS SNS service for receiving application lifecycle events. Our Cloudmatrix Admin UI will allow you to manage subscriptions, making it easy for you to configure and maintain your event notifications.


## Overview

Our solution is heavily based on AWS SNS integration, allowing you to receive real-time notifications about CM Articles lifecycle. By subscribing to our SNS topics, you can stay informed about changes to your video articles and any important in our application.

## Admin UI

### Webhook subscriptions management

Our platform provides an Admin UI that allows you to:

- Create and manage SNS subscriptions
- View and modify subscription settings
- Configure Event Type filtering
- Monitor subscription status
- Retry policy customization

### Audit log

Log of all outgoing notifications with basic details of the message including record of success/failure status.


## Cloudmatrix Event Types

This list shows our current event types. We will be adding new types as we expand our system. 

### Default Entitlements Update

Event occurs when default entitlement is being changed.

```json
{
  "type": "config",
  "operation": "update",
  "payload": {
    "defaultEntitlement": ["exampleEntitlement"]
  },
  "eventId": "e0b4e86f-42a4-4ed8-b9ca-293323e2f360",
  "site": "your-site.com",
  "tenantId": "bf2d4b67-ca97-11ee-9763-02ff621ec651",
  "timestamp": "2024-09-18T14:42:25.2525034Z"
}
```

### ArticleUpdate

Event occurs when changes has been made to an Article. Represents a snapshot of the current state of the Article at the time of publishing the message.

```json
{
  "type": "article",
  "id": "0197600b-782d-4d9f-9266-fe28076d5b25",
  "site": "your-site.com",
  "operation": "update",
  "timestamp": "2024-09-18T14:46:45.2892602Z",
  "tenantId": "bf2d4b67-ca97-11ee-9763-02ff621ec651",
  "payload": {
    "mediaData": {
      "type": "playback",
      "mediaType": "Live",
      "source": "CM",
      "playback": {
        "manifests": {
          "hls": "https://manifestUrl/master.m3u8"
        }
      }
    },
    "metadata": {
      "title": "title",
      "body": "body",
      "tags": ["testTag1", "testTag2"],
      "duration": 200,
      "sysEntryEntitlements": ["?"],
      "defaultEntitlementApplied": false
    },
    "adverts": {
      "enableAdsMode": 0
    }
  }
}
```


## Message Delivery and Retry Policy

Our Events configuration uses the following retry policy:

- Initial attempt: t = 0
- First retry: t = 20 seconds
- Second retry: t = 40 seconds
- Third retry: t ≈ 5 minutes (300 seconds)
- Fourth retry: t ≈ 10 minutes (600 seconds)
- Fifth retry: t = 20 minutes (1200 seconds)

Total retry time: ~ 40 minutes

Note: Ensure your endpoint can handle potential duplicate messages due to retries. You can use `x-amz-sns-message-id` header field to uniquely identify each message.


## Setting Up Your HTTPS Endpoint

To receive SNS notifications, you need to set up an HTTPS endpoint:

1. Implement an HTTPS server that can handle POST requests.
2. Ensure your server has a valid SSL certificate from a trusted Certificate Authority (CA).
3. Configure your server to consume:
   1. Subscription confirmation message automatically
      - or do it manually by opening the initial message `SubscribeURL` in your browser of choice
   2. Notification messages containing our CM Event payload that you subscribed to when created the subscription

### Handling SNS Messages

Your HTTPS endpoint should be able to handle three types of SNS envelope messages:

1. SubscriptionConfirmation
2. Notification
3. UnsubscribeConfirmation

## Security Considerations

- Use HTTPS for your endpoint to ensure secure communication.
- Verify the authenticity of SNS messages by validating the signature.
- Implement proper error handling and logging.

