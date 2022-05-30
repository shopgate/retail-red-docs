# Notifications Integration Guide

## About this guide

Goal of this guide is to give you an overview of the different components used for sending a notification, and how you can use the Shopgate Notification System to send out notifications via an external system.

## Prerequisites

- You have read the support article about notifications and know how Campaigns & Templates work together.

## Overview

To send a notification to your customers, Push or SMS, you need to create a campaign. In the Campaign you configure:

- When the notification should be send out (at a defined date & time or event based)
- Who should receive the notification
- The title & message of the notification
- In case of a push notification: What should happen in the app when the notification is opened.

Optionally you can also first create a template to define the title & message of the notification, and then use this template when creating the campaign. Then you can reuse the template, if you plan on using the same title & message in multiple campaigns.

If the campaign is configured to be sent at a specific date & time, the Shopgate system will automatically take care of sending the notification out. You can also set “immediately” as sending time, in that case the notification is sent as soon as possible (max after 10 minutes).

Event based notifications will be sent automatically when the corresponding event is triggered. If you are using custom events, you need to trigger the custom event via the Notification Event Receiver Service.

A campaign with triggerType “event” can be dispatched multiple times. So you can use the same campaign multiple times, to send notifications to multiple different users at different times. Campaigns with triggerType “schedule”, that are sent at a specific time, can only be send out once.

Depending on the number of recipients, it can take up to a few minutes until all notifications are sent out. For a small amount of recipients it should only take a few seconds.
As a summary, the following steps have to be done:

1. Optional: Create a template
2. Create a campaign
3. For custom event campaigns: Trigger the Event

## Creating a template

You can create a template either in the Shopgate Admin or via API. If you plan to only send a few different types of notifications out, creating them manually in the Admin might be sufficient. Otherwise this can also be automated.

To create a template via the API you can use the route:

**POST ​/merchants​/{merchantCode}​/template**

See swagger file for details about this call.
For information about using variables and formatting in the message & title, please see the Support Article Using advanced formatting in Notification Messages. <!-- TODO Link to article -->

## Creating a campaign

Same as templates, campaigns can also be created via Shopgate Admin or via API.
To create a campaign via the API use the route:

**POST ​/merchants​/{merchantCode}​/campaigns**

See swagger file for details about this call.

If you want to send the campaign based on a custom event, you have to create the campaign with “triggerType” = “event”, as “settings.eventType” = “custom” and “settings.customEvent” = “yourEventName”.

You can use a template by specifying the “templateCode”, or you can directly define the message via “messages”.

For custom event campaigns you can also use custom variables in the message & title. These custom variables then have to be passed when triggering the event. Custom variables have to be in the following format:

**{{custom.myVariable}}**

## Triggering a custom event

To trigger a custom event, you have to call the Notification Event Receiver Service with the route:

**POST ​/merchants/{merchantCode}/customNotificationEvent**

When triggering a custom event, you can also further specify the recipients by passing additionalFilter. For example if you want to send the notification only to a specific customer, just pass an additionalFilter to filter for a specific email address.

If you used custom variables in the message, these also need to be passed along via customVariables.

## Creating a campaign

Same as templates, campaigns can also be created via Shopgate Admin or via API.
To create a campaign via the API use the route:

**POST ​/merchants​/{merchantCode}​/campaigns**

See swagger file for details about this call.

If you want to send the campaign based on a custom event, you have to create the campaign with “triggerType” = “event”, as “settings.eventType” = “custom” and “settings.customEvent” = “yourEventName”.

You can use a template by specifying the “templateCode”, or you can directly define the message via “messages”.

For custom event campaigns you can also use custom variables in the message & title. These custom variables then have to be passed when triggering the event. Custom variables have to be in the following format:

**{{custom.myVariable}}**

## Triggering a custom event

To trigger a custom event, you have to call the Notification Event Receiver Service with the route:

**POST ​/merchants/{merchantCode}/customNotificationEvent**

When triggering a custom event, you can also further specify the recipients by passing additionalFilter. For example if you want to send the notification only to a specific customer, just pass an additionalFilter to filter for a specific email address.

If you used custom variables in the message, these also need to be passed along via customVariables.
