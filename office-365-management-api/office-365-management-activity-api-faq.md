---
ms.TocTitle: Office 365 Management Activity API frequently asked questions
title: Office 365 Management Activity API frequently asked questions
description: Frequently asked questions about using the Office 365 Management Activity API
ms.ContentId:
ms.topic: reference (API)
ms.date: 09/21/2018
---

# Office 365 Management Activity API frequently asked questions

#### What events are audited for a specific Office 365 service?

Office 365 Management Activity API schema documentation has a comprehensive list of events. For details, see [Office 365 Management Activity API schema](office-365-management-activity-api-schema.md).

#### How do I onboard to the Management Activity API?

To get started with the Office 365 Management Activity API, see [Get started with Office 365 Management APIs](get-started-with-office-365-management-apis.md).
 
#### What is the throttling limit for the  Management Activity API?

The current throttling limit is 60K requests/minute per Publisher ID. 

#### We want to programmatically capture all events in all workloads. What is the most reliable way to do this?

You can do this by using the Office 365 Management Activity API. Also, we recommend that you use the **pull model** due to issues with using webhooks. For more information, see the "Using webhooks" section in [Troubleshooting the Office 365 Management Activity API](troubleshooting-the-office-365-management-activity-api.md#using-webhooks).

#### Is it true that mailbox auditing in Exchange Online can only be enabled by using PowerShell?

Yes. However, we are working on enabling mailbox auditing by default for all mailboxes in an Office 365 organization. For more information, see "Exchange mailbox auditing will be enabled by default" in the [Microsoft Security, Privacy, and Compliance blog](https://techcommunity.microsoft.com/t5/Security-Privacy-and-Compliance/Exchange-Mailbox-Auditing-will-be-enabled-by-default/ba-p/215171).

#### Are there any differences in the records that are fetched by the Management Activity API versus the records that are returned by using the audit log search tool in the Office 365 Security & Compliance Center?

The data that is returned by both methods is the same. There is no filtering that happens. The only difference is that with the API, you can get data for the last 7 days at a time. When searching the audit log in the Security & Compliance Center (or by using the corresponding [Search-UnifiedAuditLog](https://docs.microsoft.com/powershell/module/exchange/policy-and-compliance-audit/search-unifiedauditlog) cmdlet in Exchange Online), you can get data for the last 90 days. 
 
#### Aren’t webhook notifications more immediate? After all, aren’t they event-driven?

No. Webhook notifications aren't event-driven in the sense that the event triggers the notification. The content blob must still be created, and the creation of the content blob is what triggers the notification delivery. Recently, there have been longer wait times for notifications when using a webhook compared to querying the API directly with the */content* operation. Therefore, the Management Activity API shouldn’t be thought of as a real-time security alert system. Microsoft has other products for that. As far as security is concerned, Management Activity API event notifications can more appropriately be used to determine usage patterns over extended periods of time.

#### When pulling the data from the Management Activity API, there is sometimes a delay of more than 3 to 5 days. Why is this?

At times, there are instances of a temporary outage or other issues in the Office 365 service. In those cases, some audit records are dropped and the service tries to backfill them. Although this happens for only about 5% to 10% of the records, these are the records that may be delayed in certain situations. If the delay is more than 5 days, check the Service Health Dashboard in the Office 365 admin center. If needed, you can also open a ticket with [Microsoft support](https://support.office.com/article/contact-support-for-business-products-admin-help-32a17ca7-6fa0-4870-8a8d-e25ba4ccfd4b#ID0EAADAAA=online).

#### I am encountering a throttling error in the Management Activity API. What should I do?

Open a ticket with [Microsoft Support](https://support.office.com/article/contact-support-for-business-products-admin-help-32a17ca7-6fa0-4870-8a8d-e25ba4ccfd4b#ID0EAADAAA=online) and request a new throttling limit, and include a business justification for increasing the limit. 

#### What is the maximum time I will have to wait before a notification is sent about a given Office 365 event?

There is no guaranteed maximum latency for notification delivery (in other words, no SLA). Microsoft Support’s experience has been that most notifications are sent within one hour of the event. Often the latency is much shorter, but often it’s longer as well. This varies somewhat from workload to workload, but a general rule is that most notifications will be delivered within 24 hours of the originating event.


#### Can I query the Management Activity API for a particular event ID or RecordType or other properties in the content blob?

No. Don’t think of the data available through the Management Activity API as being a “log” in the traditional sense. Rather, think of it as a dump of event details. It’s up to you to gather all those event details, store and index them locally, and then implement your own query logic, by using either a custom application or a third-party tool.

#### How do I know the data coming from my existing auditing solution, which collects data from the Management Activity API, is accurate and complete?

The short answer is that Microsoft doesn’t provide any kind of a log that will allow you to cross-check any given application or third-party (ISV) application. There are other Microsoft security products that draw their data from the same pipeline, but those products fall outside the scope of this discussion and can’t be used to directly cross-check the Management Activity API. If you’re concerned about discrepancies between what your existing solution is providing and what you expect, you should implement the operations illustrated above. But this can be difficult, depending on how your existing tool or solution lists and indexes data. If your existing solution only presents data sorted by the creation time of the actual event, there’s no way to query the API by event creation time so that you could compare result sets. In this scenario, you’d have to collect the notified content blobs for several days, index or sort them manually, and then do a rough comparison.

#### How long will the content blobs remain available?

Content blobs are available 7 days after the notification of the content blob’s availability. This means that if there is a significant delay in the creation of the content blob, you will have more time (the delay plus 7 days) after the actual event creation date before the content blob is no longer available.

#### If there is a 24-hour delay in getting a notification, doesn’t that mean I will have only 6 days to retrieve the content blob?

No. Even if the notification is delayed for an unusually long period (for example, in the case of a service interruption), you would still have 7 days after the first availability of the notification to download the content blob related to the originating event.
