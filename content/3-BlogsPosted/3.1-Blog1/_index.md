---
title: "Blog 1"
date: 2026-06-23
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Automating CI/CD for GitHub Monorepos with AWS CodePipeline

In modern microservice architectures, a GitHub monorepo is a natural way to keep multiple services in one shared repository. But monorepos create a CI/CD problem: a single commit can touch many folders, and without filtering, every CodePipeline may run even when only one service changed.

This post is based on the AWS blog on integrating GitHub Monorepo with AWS CodePipeline to run project-specific pipelines. I also draw on the example architecture that introduces a webhook, API Gateway, Lambda decision engine, and targeted pipeline execution.

---

## Why Monorepo CI/CD is Different

A monorepo contains several projects in one repository, for example:

```text
repo/
├── frontend/
├── backend/
├── auth-service/
├── internship-service/
└── docs/
```

The naive CI/CD approach is to attach each pipeline to the same repository webhook. Then any commit in the repo triggers all pipelines, even if only one project changed.

That leads to three main issues:

- Wasted build time when unrelated pipelines run.
- Higher AWS cost from unnecessary CodeBuild and CodePipeline executions.
- Slower feedback for developers on the actual changed service.

---

## The Better Architecture: Path-Based Decision Engine

The key idea is to separate event ingestion from pipeline execution. Instead of CodePipeline listening to the repository directly, GitHub pushes events to a lightweight filter layer. That layer decides which pipeline should run.

The architecture looks like this:

1. GitHub Monorepo sends a webhook event on push.
2. Amazon API Gateway receives the webhook.
3. AWS Lambda validates the GitHub signature and parses the payload.
4. The Lambda decision engine checks which folders changed.
5. The Lambda starts only the corresponding CodePipeline(s).

---

## Example Flow

If a commit changes `frontend/src/App.jsx`, the decision engine can route it like this:

- `frontend/` changed → start `FrontendPipeline`
- `backend/` not changed → do not start `BackendPipeline`
- `internship-service/` not changed → do not start `InternshipServicePipeline`
- `docs/` not changed → no pipeline for docs

If the commit only changes documentation under `docs/`, the system can skip all pipelines entirely.

---

## Why this Works

This model turns CI/CD into an event-driven system instead of a reactive “build everything” system.

Old behavior:

- Repository changes → run all pipelines

New behavior:

- Repository changes → analyze changed paths → run only relevant pipelines

That means:

- faster CI cycles for developers
- less cost from builds and artifacts
- clearer pipeline ownership by project

---

## AWS Services Used

### Webhook Receiver

- **GitHub Webhook**: delivers push events with changed files.
- **Amazon API Gateway**: provides a secure HTTP endpoint for the webhook.
- **AWS Lambda**: validates the webhook signature and executes the decision logic.

### Decision Engine

- **AWS Lambda**: extracts changed file paths and matches them to folder rules.
- **AWS CodePipeline**: starts the relevant pipeline using `StartPipelineExecution`.
- **AWS IAM**: ensures the Lambda has permission only to start pipelines, not modify them.

### Optional enhancements

- **Amazon EventBridge**: publish filtered events for observability or audit.
- **Amazon SNS**: notify teams when a pipeline is skipped or started.
- **AWS Secrets Manager**: store the GitHub webhook secret.

---

## Sample Lambda Decision Logic

The Lambda function can be small and deterministic. Here is a simplified JavaScript example:

```js
const { CodePipelineClient, StartPipelineExecutionCommand } = require('@aws-sdk/client-codepipeline');

const pipelineMap = [
  { prefix: 'frontend/', pipeline: 'FrontendPipeline' },
  { prefix: 'backend/', pipeline: 'BackendPipeline' },
  { prefix: 'auth-service/', pipeline: 'AuthServicePipeline' },
  { prefix: 'internship-service/', pipeline: 'InternshipServicePipeline' },
];

exports.handler = async (event) => {
  const payload = JSON.parse(event.body);
  const changedFiles = payload.commits.flatMap(c => [...c.added, ...c.modified, ...c.removed]);

  const pipelines = new Set();
  for (const file of changedFiles) {
    for (const rule of pipelineMap) {
      if (file.startsWith(rule.prefix)) {
        pipelines.add(rule.pipeline);
      }
    }
  }

  if (pipelines.size === 0) {
    return { statusCode: 200, body: 'No relevant pipeline triggered.' };
  }

  const client = new CodePipelineClient({});
  for (const name of pipelines) {
    await client.send(new StartPipelineExecutionCommand({ name }));
  }

  return { statusCode: 200, body: `Started pipelines: ${[...pipelines].join(', ')}` };
};
```

This simple function is the “decision engine” in the architecture. It avoids needless pipeline executions by only starting pipelines for changed folders.

---

## Implementation Considerations

### Validate the webhook

Always verify the GitHub webhook signature in Lambda before trusting the payload. This prevents unauthorized requests from starting pipelines.

### Keep the mapping flexible

Use a configuration file or environment variable to map repo paths to pipeline names, so you can change the monorepo structure without redeploying code.

### Handle multiple pipelines

Some commits may affect multiple services. The Lambda can start more than one pipeline in a single request.

### Skip docs-only changes

For changes under `docs/`, you can explicitly return early and skip all pipeline executions. This saves costs for non-code updates.

---

## Cost and Efficiency Benefits

This approach is especially useful for monorepos and microservice landscapes because it:

- reduces AWS CodeBuild minutes by running only what changed
- lowers CodePipeline execution costs
- improves developer turnaround time for relevant services
- avoids noisy alerts from unrelated pipelines

For large repositories with many services, the savings can be substantial.

---

## Conclusion

Automating CI/CD for a GitHub monorepo with AWS CodePipeline does not require a complex orchestrator. A lightweight Lambda decision engine between GitHub and CodePipeline is enough to make the system smart and efficient.

With this architecture, monorepo CI/CD becomes:

- more targeted
- more cost-effective
- easier to maintain
- better aligned with microservice ownership

If you are building multiple projects in one repository, this pattern is a great fit.

---

## References

- AWS Blog: Integrate GitHub Monorepo with AWS CodePipeline to run project-specific CI/CD pipelines
