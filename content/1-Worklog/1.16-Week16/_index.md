---
title: "Week 16 Worklog"
date: 2026-10-18
weight: 16
chapter: false
pre: " <b> 1.16. </b> "
---

### Week 16 Objectives:

* Build serverless backend: Lambda functions for data ingestion, processing, and API.
* Implement API Gateway REST API with Lambda proxy integration.
* Set up DynamoDB tables for device registry and time-series weather data.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Create DynamoDB tables: DeviceRegistry (PK: device_id), WeatherData (PK: device_id, SK: timestamp) <br> - Enable TTL on WeatherData for 90-day retention | 12/10/2026 | 12/10/2026 | <https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.CoreComponents.html> |
| Tue | - Develop Lambda: IoT Core rule action → Kinesis Data Streams <br> - Implement data validation and transformation logic | 13/10/2026 | 13/10/2026 | <https://docs.aws.amazon.com/iot/latest/developerguide/iot-rules.html> |
| Wed | - Develop Lambda: Kinesis consumer → DynamoDB batch write <br> - Handle partial failures and implement retry with exponential backoff | 14/10/2026 | 14/10/2026 | <https://docs.aws.amazon.com/lambda/latest/dg/with-kinesis.html> |
| Thu | - Create API Gateway REST API: GET /devices, GET /devices/{id}/telemetry <br> - Configure Lambda proxy integration, request validation, CORS | 15/10/2026 | 15/10/2026 | <https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-create-api-as-simple-proxy-for-lambda.html> |
| Fri | - Unit test Lambda functions with mock events <br> - Deploy to dev via SAM/CloudFormation <br> - End-to-end test: simulated device → IoT Core → Kinesis → Lambda → DynamoDB → API | 16/10/2026 | 16/10/2026 | <https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html> |

### Week 16 Achievements:

* **PLANNED**: Create DynamoDB tables with appropriate key schema and TTL.
* **PLANNED**: Build Lambda pipeline: IoT Core → Kinesis → DynamoDB.
* **PLANNED**: Expose REST API via API Gateway with Lambda proxy.
* **PLANNED**: Verify end-to-end data flow with simulated device payloads.
* **TODO**: Add integration tests and implement dead-letter queue for failed Kinesis records.