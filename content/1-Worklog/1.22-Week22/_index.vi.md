---
title: "Worklog Tuần 22"
date: 2026-11-29
weight: 22
chapter: false
pre: " <b> 1.22. </b> "
---

### Mục tiêu tuần 22:

* Xây dựng CI/CD pipeline với CodePipeline, CodeBuild, CodeDeploy.
* Triển khai automated testing: unit, integration, contract test.
* Thiết lập blue/green deployment cho Lambda và API Gateway.

### Các công việc cần triển khai trong tuần này:

| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Tạo CodePipeline: Source (GitHub) → Build (CodeBuild) → Deploy (CodeDeploy) <br> - Cấu hình buildspec.yml: install, test, package (SAM), deploy | 23/11/2026 | 23/11/2026 | <https://docs.aws.amazon.com/codepipeline/latest/userguide/welcome.html> |
| 3 | - Implement CodeBuild project: chạy pytest unit test, cdk synth validation <br> - Thêm integration test stage với LocalStack/moto mocking | 24/11/2026 | 24/11/2026 | <https://docs.aws.amazon.com/codebuild/latest/userguide/welcome.html> |
| 4 | - Cấu hình CodeDeploy cho Lambda: blue/green với traffic shifting (10% → 100% trong 10 phút) <br> - Thiết lập pre/post-traffic hook cho validation | 25/11/2026 | 25/11/2026 | <https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-steps.html> |
| 5 | - Thêm API Gateway stage deployment với canary release (5% canary) <br> - Implement contract testing với Pact cho API consumer | 26/11/2026 | 26/11/2026 | <https://docs.aws.amazon.com/apigateway/latest/developerguide/canary-release.html> |
| 6 | - Chạy full pipeline: feature branch → PR → main → production <br> - Đo deployment time, rollback time <br> - Viết tài liệu pipeline architecture và troubleshooting guide | 27/11/2026 | 27/11/2026 | |

### Kết quả đạt được trong tuần 22:

* **PLANNED**: Xây dựng CI/CD pipeline end-to-end với 4 stage.
* **PLANNED**: Đạt 85% unit test coverage, 12 integration test pass.
* **PLANNED**: Triển khai blue/green Lambda deployment với auto rollback.
* **PLANNED**: Cấu hình API Gateway canary release cho rollout an toàn.
* **PLANNED**: Pipeline execution time: ~12 phút (build + test + deploy).
* **TODO**: Thêm chaos engineering experiment (Lambda failure injection).