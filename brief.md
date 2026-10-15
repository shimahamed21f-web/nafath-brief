Version: NAFATH-V2
Track: Networking 

## Overview
The emergency responder service depends on six fields working together. Networking provides the 5G connection, while the mobile app lets responders keep working even when the connection drops. Software APIs must handle automatic retries safely, so a repeated request does not create duplicate actions. Data teams turn usage records into daily reports per region, which helps the operator plan capacity. AI models use this data to predict congestion before it harms the service, and cross-validation helps check that the predictions are reliable. Cybersecurity protects the whole system and gives the team a clear incident response process when an attack happens. If one field fails, the others are affected, so the service must be designed as one connected system.

## Software
In HTTP, a method is safe if it is read-only and does not change the server state, for example GET and HEAD. A method is idempotent if repeating the same request has the same effect as sending it once; PUT and DELETE are idempotent, while POST is not. This matters for automatic retries: a client can retry GET or PUT after a failure, but retrying POST could create duplicate actions, such as two emergency reports.

Resource: RFC 9110 - HTTP Semantics - https://www.rfc-editor.org/rfc/rfc9110

## AI
Overfitting happens when a model learns the training data too closely, including noise, so it performs well on training data but poorly on new data. Cross-validation splits the data into several parts, trains the model on some parts and tests it on the remaining part, then repeats this. If scores on the held-out parts are much worse than training scores, the model is probably overfitting the congestion data.

Resource: 3.1. Cross-validation: evaluating estimator performance - https://scikit-learn.org/stable/modules/cross_validation.html

## Mobile
When offline, the responder's app should keep working by reading and writing to a local database, so critical information such as incident details stays available. Changes should be queued and marked as pending. When the connection returns, the app should synchronize in the background, send the queued changes, fetch updates from the server and resolve conflicts with a clear rule. It should retry safely if the sync fails.

Resource: Build an offline-first app - https://developer.android.com/topic/architecture/data-layer/offline-first

## Networking
A network slice is a logical network that provides specific network capabilities and characteristics. A slice instance includes the core network control plane and user plane functions, plus at least one access part, such as NG-RAN. An operator can deploy several slices with the same features for different groups, for example because each has a different committed service. A dedicated slice could serve emergency responders, using the standardised URLLC type (SST 2) for ultra-reliable, low-latency communication.

Resource: ETSI TS 123 501 V18.5.0 (2024-05) 5G; System architecture for the 5G System (5GS)-https://www.etsi.org/deliver/etsi_ts/123500_123599/123501/18.05.00_60/ts_123501v180500p.pdf

## Cybersecurity
A star schema has a central fact table, which holds numeric measures, connected directly to dimension tables that describe business entities, such as date and region. A snowflake schema normalizes a dimension into several related tables, which means more tables to join. For daily usage reports per region, a star schema suits better: a usage fact table with date and region dimensions is simple to query, and dimensions can filter and group the data.

Resource: Understand star schema and the importance for Power BI - Power BI | Microsoft Learn - https://learn.microsoft.com/power-bi/guidance/star-schema

## Data
A star schema has a central fact table, which holds numeric measures, connected directly to dimension tables that describe business entities, such as date and region. A snowflake schema normalizes a dimension into several related tables, which means more tables to join. For daily usage reports per region, a star schema suits better: a usage fact table with date and region dimensions is simple to query, and dimensions can filter and group the data.

Resource: Understand star schema and the importance for Power BI - Power BI | Microsoft Learn - https://learn.microsoft.com/power-bi/guidance/star-schema

## Misconception in My Track

## Open Question
