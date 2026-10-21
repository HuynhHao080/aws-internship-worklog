---
title: "Week 22 Worklog"
date: 2026-11-29
weight: 22
chapter: false
pre: " <b> 1.22. </b> "
---

### Week 22 Objectives:

* Build CI/CD pipeline with CodePipeline, CodeBuild, CodeDeploy.
* Implement automated testing: unit, integration, contract tests.
* Set up blue/green deployment for Lambda and API Gateway.

### Tasks to be carried out this week:

| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Mon | - Create CodePipeline: Source (GitHub) → Build (CodeBuild) → Deploy (CodeDeploy) <br> - Configure buildspec.yml: install, test, package (SAM), deploy | 23/11/2026 | 23/11/2026 | <https://docs.aws.amazon.com/codepipeline/latest/userguide/welcome.html> |
| Tue | - Implement CodeBuild project: run pytest unit tests, cdk synth validation <br> - Add integration test stage with LocalStack/moto mocking | 24/11/2026 | 24/11/2026 | <https://docs.aws.amazon.com/codebuild/latest/userguide/welcome.html> |
| Wed | - Configure CodeDeploy for Lambda: blue/green with traffic shifting (10% → 100% over 10 min) <br> - Set up pre/post-traffic hooks for validation | 25/11/2026 | 25/11/2026 | <https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-steps.html> |
| Thu | - Add API Gateway stage deployment with canary release (5% canary) <br> - Implement contract testing with Pact for API consumers | 26/11/2026 | 26/11/2026 | <https://docs.aws.amazon.com/apigateway/latest/developerguide/canary-release.html> |
| Fri | - Run full pipeline: feature branch → PR → main → production <br> - Measure deployment time, rollback time <br> - Document pipeline architecture and troubleshooting guide | 27/11/2026 | 27/11/2026 | |

### Week 22 Achievements:

* **PLANNED**: Build end-to-end CI/CD pipeline with 4 stages.
* **PLANNED**: Achieve 85% unit test coverage, 12 integration tests passing.
* **PLANNED**: Implement blue/green Lambda deployment with automated rollback.
* **PLANNED**: Configure API Gateway canary release for safe rollouts.
* **PLANNED**: Pipeline execution time: ~12 min (build + test + deploy).
* **TODO**: Add chaos engineering experiments (Lambda failure injection).