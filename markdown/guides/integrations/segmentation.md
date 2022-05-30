# Segmentation Integration Guide

## About this guide

Goal of this guide is to explain how you can use the Shopgate Segmentation API to synchronize segments from any other external system over to retail.red. So that in the end you have the same segments available in Shopgate as in your other system, and can use these segments to send notifications, configure promotions etc.

## Prerequisites

- You have read the support article about segments
- You have the swagger file of the segmentation service available to check
- You have the swagger file of the customer service available to check

## Segment type

Shopgate supports two different types of segments: Dynamic segments & fixed segments. This guide will only cover fixed segments, which allows you to manage a fixed list of members for a segment. If you want to use dynamic segments in your integration please refer to our Swagger specification and contact our support if you have additional questions.

## Creating a segment

You can create a new segment either in the Shopgate Admin or via API. If you just want to synchronize one or two specific segments, it might be sufficient to create the Segments manually in the Shopgate Admin, and then just keep the members in sync via API. But if you are planning to automatically create new segments at Shopgate when they are created in the external system, the creation of a new segment should get automized.

To create a new segment via the API you can use the route:

**POST ​/merchants​/{merchantCode}​/segments**

As “type” you should choose “fixed”, since only for fixed segments you are able to manage the individual members of the segment.

You could already pass the members when the segment is created, but for this guide we go with the alternative of adding the members afterwards.

## Adding & removing members to a segment

In order to add a new member to a segment, you need to know the ID of the customer at retail.red. If you do not know this ID, you can retrieve it via the Shopgate Customer Service.
Call the customer service with the route:

**GET /merchants/{merchantCode}/customers**

to get a list of the customers, which includes the ID of the customer. You can use the filter parameter to only get specific customers returned. For example you can filter by emailAddress to find the specific customer you want via the email address.

Please be aware that if you allow guest users in your shop, multiple users can have the same email address. You can also filter out guest users via the filter parameter.

Here an example of a filter for a specific user via email, excluding guest users:

**{"emailAddress":"john.doe@gmail.com", "isAnonymous":false}**

Once you know the ID of the customer, you can add the customer as a member to a segment via:

**POST /merchants/{merchantCode}/segments/{segmentCode}/members**

As “ID“ you have to pass the customer ID that we just found out via the customer service, and as “type“ use “customer”.

Of course you can also add multiple members with a single request using this route.

For deleting members from a segment use the route:

**DELETE /merchants/{merchantCode}/segments/{segmentCode}/members**

For deleting you also need to pass the customer ID and type “customer”.