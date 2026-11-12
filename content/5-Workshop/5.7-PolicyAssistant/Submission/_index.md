---
title: "Policy Assistant submission pack"
weight: 0
---


**Primary application model:** Global Claude Haiku 5.5. The application ranks extracted PDF and policy text by keywords. Dated verification reports record their respective releases. [Model cutover evidence](https://github.com/HuynhHao080/aws-policy-assistant/blob/main/docs/haiku55-migration.md).
## Four links for the presentation

1. The [AWS application](https://djreo5eptvwk5.cloudfront.net/) runs with DEMO policies for an accessories shop.
2. The [bilingual proposal]({{< relref "/2-Proposal/CurrentSolution" >}}) covers the problem, scope, architecture, risks and cost inputs.
3. The [source and README](https://github.com/HuynhHao080/aws-policy-assistant) include the AWS SAM template, tests and operator instructions.
4. The [staff operator guide](https://github.com/HuynhHao080/aws-policy-assistant/blob/6225d5c66ed5f114be1e3cf004df937ab0e58f67/docs/HUONG-DAN-PHOI-HOP-HO-TRO.md) describes the workflow. [API and artifact evidence]({{< relref "/5-Workshop/5.7-PolicyAssistant/SupportTeam" >}}).

## Submission revision and evidence

- Use application commit **c25e278** for this submission. CloudFormation reported `UPDATE_COMPLETE` with `global.anthropic.claude-haiku-5-5`, and CloudFront publication completed for the frontend built from this revision. [Pinned source](https://github.com/HuynhHao080/aws-policy-assistant/tree/c25e27829b15251206b54982bfcd7b7274a1a88e).
- This revision passed **93 frontend tests**, including sign-in return and expired callback recovery. The live Cognito sign-in, sign-up and recovery pages served the new branding. The HTTP check covers form display. Keep simulated checks, dated live API results and your own recorded demo as separate evidence. [Application CI](https://github.com/HuynhHao080/aws-policy-assistant/actions/runs/43180702320) · [Handover]({{< relref "/5-Workshop/5.7-PolicyAssistant/CurrentRelease" >}}).

## Local lab

Clone the application repository, install Python 3.12 and Node.js 22, then open PowerShell in the new repository.

```powershell
git switch --detach c25e27829b15251206b54982bfcd7b7274a1a88e
py -3.12 -m venv .venv
& ./.venv/Scripts/python.exe -m pip install -r policy_assistant/requirements.txt -r tests/requirements.txt
Remove-Item Env:AWS_SAM_STACK_NAME, Env:POLICY_ASSISTANT_ID_TOKEN -ErrorAction SilentlyContinue
& ./.venv/Scripts/python.exe -m pytest tests -q
npm ci --prefix frontend --ignore-scripts
npm run build --prefix frontend
npm test --prefix frontend
```

Check each command’s exit code. Python tests use mocked services and frontend tests use a simulated DOM. The built interface can be inspected locally. The example configuration has blank Cognito connection settings. [Reproduction guide](https://github.com/HuynhHao080/aws-policy-assistant/tree/main/docs/reproducibility).

## Four-minute demonstration

| Time | Action and explanation |
|---|---|
| 0:00–0:40 | Open the app and explain the DEMO shop scope. Walk through your diagram from browser to CloudFront, Cognito and API. |
| 0:40–1:40 | Open the warranty policy and choose **Ask from this policy**. Ask for the charger warranty period and who pays warranty shipping. Compare each part with its excerpt and record how the assistant handles missing information. |
| 1:40–2:50 | Submit a synthetic support request. As administrator, claim it and add a note. Return to the customer account and compare the visible conversation. |
| 2:50–4:00 | Explain PDF ownership, concurrent revisions, queues and cleanup. Open the evidence and identify the checks you performed. |

Hide tokens, presigned upload URLs and account details in the recording. If a step fails, record the cause and present dated evidence with its original scope. Remove test data through account or item deletion after the demo.

## Items to complete before submission

Prioritize your own architecture diagram and workshop presentation. The published scoring rules treat both as required for workshop points. Use the official AWS icons and be ready to explain the identity boundaries, data flow and trade-offs. [FCAJ scoring rules](https://hcm-rules.awsfcaj.com/5-scoring/).

| Item | Student action |
|---|---|
| Architecture | Draw your AWS flows and practise explaining identity boundaries, Singapore and Global routing, failure and retry. [Review worksheet](https://github.com/HuynhHao080/aws-policy-assistant/blob/main/docs/fcaj/ARCHITECTURE-REVIEW-WORKSHEET.md). |
| COST | Enter proposal assumptions into Calculator and save the total, date and share URL or export. Target budget is USD 30–50/month. [Cost inputs]({{< relref "/2-Proposal/CurrentSolution" >}}). |
| Demo | Perform it personally and capture evidence with the date of the check. |
| Worklog and evidence | Review daily entries, published blog posts and event evidence, then add actual links. [Evidence review]({{< relref "/0-Audit/student-evidence" >}}). |

This technical material used AI assistance. Review the submission, identify your personal contributions and refer to the [FCAJ workshop requirements](https://hcm-rules.awsfcaj.com/3-project/).
