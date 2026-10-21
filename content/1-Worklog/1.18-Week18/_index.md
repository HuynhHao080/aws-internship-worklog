---
title: "Week 18 Worklog"
date: 2026-11-01
weight: 18
chapter: false
pre: " <b> 1.18. </b> "
---

### Week 18 Objectives:

* Set up AWS IoT Core for device provisioning and management.
* Implement device certificates, thing groups, and fleet indexing.
* Create IoT rules for routing telemetry to Kinesis.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Create IoT Thing Type for weather station with attributes <br> - Register 10 test devices with X.509 certificates (JITR) | 26/10/2026 | 26/10/2026 | <https://docs.aws.amazon.com/iot/latest/developerguide/thing-types.html> |
| Tue | - Configure Thing Groups: lab-indoor, lab-outdoor, rooftop <br> - Enable Fleet Indexing for search and dynamic groups | 27/10/2026 | 27/10/2026 | <https://docs.aws.amazon.com/iot/latest/developerguide/thing-groups.html> |
| Wed | - Create IoT Rule: SELECT * FROM 'weather/+/telemetry' → Kinesis <br> - Add error action to DLQ (SQS) for failed rule evaluations | 28/10/2026 | 28/10/2026 | <https://docs.aws.amazon.com/iot/latest/developerguide/iot-rules.html> |
| Thu | - Implement device shadow for desired/reported state (firmware version, config) <br> - Test shadow delta updates from cloud to device | 29/10/2026 | 29/10/2026 | <https://docs.aws.amazon.com/iot/latest/developerguide/iot-device-shadows.html> |
| Fri | - Simulate 10 devices sending telemetry every 30s via MQTT <br> - Verify data flow: IoT Core → Rule → Kinesis → Lambda → DynamoDB <br> - Monitor IoT Core metrics: Connect, Publish, RuleMatch, RuleEval | 30/10/2026 | 30/10/2026 | <https://docs.aws.amazon.com/iot/latest/developerguide/monitoring-iot.html> |

### Week 18 Achievements:

* **PLANNED**: Provision 10 test devices with JITR certificates.
* **PLANNED**: Organize devices into logical Thing Groups.
* **PLANNED**: Create IoT Rule with SQL query routing to Kinesis + DLQ.
* **PLANNED**: Verify device shadow for OTA configuration updates.
* **PLANNED**: Confirm end-to-end telemetry pipeline at scale.
* **TODO**: Implement Jobs for fleet-wide firmware updates.