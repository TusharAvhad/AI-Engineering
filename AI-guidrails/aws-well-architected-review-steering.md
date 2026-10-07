---
inclusion: manual
description: "Use this steering file when conducting an AWS Well-Architected Framework review of a workload. The review must be evidence-based, balanced, and grounded in the actual workload implementation."
---

# AWS Well-Architected Review Steering

## Purpose

Use this steering file when conducting an AWS Well-Architected Framework review of a workload.

The review must be evidence-based, balanced, and grounded in the actual workload implementation. The goal is to identify architectural strengths, risks, gaps, and practical remediation options across all AWS Well-Architected pillars.

Do not produce generic findings. Every finding must be supported by observable evidence or clearly marked as "Cannot Determine".

## Mandatory Review Scope

Every AWS Well-Architected review must assess all six pillars:

1. Operational Excellence
2. Security
3. Reliability
4. Performance Efficiency
5. Cost Optimization
6. Sustainability

Focus pillars may receive deeper analysis when requested, but no pillar may be omitted.

If there is insufficient evidence to assess a pillar, mark the relevant area as "Cannot Determine" and explain what evidence is missing.

## Review Principles

Follow these principles throughout the review:

- Prefer evidence over assumption.
- Validate existing strengths as clearly as gaps.
- Calibrate findings to workload criticality.
- Do not manufacture critical or high-risk findings without strong evidence.
- Use precise file paths and line numbers wherever possible.
- Distinguish between confirmed risks, likely gaps, and unknowns.
- Treat static analysis as incomplete where runtime, AWS account, or operational evidence is required.
- Consider cross-pillar tradeoffs before recommending changes.
- Provide specific remediation guidance, not generic AWS advice.
- Keep recommendations proportionate to the workload's business criticality and operating model.

## Evidence Standards

Use evidence from:

- Infrastructure-as-code
- Application source code
- CI/CD definitions
- Deployment configurations
- Monitoring and logging configurations
- Security policies
- Architecture documentation
- Operational runbooks
- Test configurations
- Repository metadata such as CODEOWNERS, README files, and contribution guides

Each finding should include:

- File path
- Line number where practical
- Relevant configuration, code, or documentation reference
- Why the evidence matters
- Pillar relevance
- Confidence level

Use these confidence levels:

- High Confidence: Direct evidence exists in source code or configuration.
- Medium Confidence: Partial evidence exists, but some context is missing.
- Low Confidence: Evidence is indirect or incomplete.
- Cannot Determine: The state cannot be assessed from available files.

## Required Inputs

Before performing the review, identify or request:

- Workload name
- Code packages or directories to analyse
- Output location, if a report will be generated
- Business criticality: critical, high, standard, or low
- Any requested AWS Well-Architected Lens
- Any focus pillars for deeper analysis

Even when focus pillars are provided, assess all six pillars.

## Analysis Expectations

Analyse the workload using static evidence from the repository.

Inspect, where present:

- AWS CDK
- CloudFormation
- Terraform
- SAM
- Serverless Framework
- Pulumi
- Kubernetes and container manifests
- CI/CD pipeline definitions
- Application source code
- Configuration files
- IAM policies
- Network definitions
- Monitoring and alerting rules
- Backup and disaster recovery configuration
- Logging, tracing, and metrics instrumentation
- Test and quality automation
- Documentation and runbooks

Respect ignore files such as `.gitignore` where appropriate, but do not skip relevant infrastructure, application, or operational files without noting the reason.

## Pillar Assessment Guidance

### Operational Excellence

Assess how the workload is built, deployed, observed, operated, and improved.

Look for evidence of:

- Clear ownership and operational documentation
- CI/CD automation
- Deployment safety controls
- Rollback mechanisms
- Runbooks
- Health checks
- Structured logging
- Metrics and dashboards
- Alerts and incident routing
- Readiness and operational validation

Typical gaps include:

- Manual deployments
- No rollback plan
- Sparse observability
- Missing runbooks
- No operational readiness checks
- No clear ownership model

### Security

Assess how identities, permissions, network boundaries, data protection, detection, and incident response are implemented.

Look for evidence of:

- Least privilege IAM policies
- Strong identity boundaries
- Encryption at rest
- Encryption in transit
- Secrets management
- Secure network design
- Audit logging
- Threat detection
- Vulnerability management
- Incident response procedures
- Secure build and deployment practices

Flag high-risk issues when directly evidenced, including:

- Hardcoded credentials or secrets
- Broad administrative IAM permissions without justification
- Public ingress on sensitive ports
- Storage without encryption
- TLS versions below 1.2
- Missing audit trails for sensitive workloads
- Containers or functions running with unnecessary privileges

### Reliability

Assess whether the workload can recover from failures, scale with demand, and meet availability expectations.

Look for evidence of:

- Multi-AZ design where appropriate
- Auto scaling
- Health checks
- Retries with backoff
- Timeouts
- Idempotency
- Queues or asynchronous decoupling
- Backup configuration
- Restore procedures
- Disaster recovery planning
- Deployment rollback
- Fault isolation
- Failure testing

Flag high-risk issues when directly evidenced, including:

- Critical workloads deployed to a single availability zone without justification
- Stateful resources without backups
- No health checks on load-balanced targets
- No retry or timeout strategy for external calls
- No rollback mechanism
- No disaster recovery evidence for high-criticality workloads

### Performance Efficiency

Assess whether the workload uses appropriate compute, storage, database, and network resources for its access patterns.

Look for evidence of:

- Appropriate compute selection
- Right-sized memory, CPU, and timeout settings
- Efficient database access
- Connection pooling
- Caching
- CDN usage where relevant
- Asynchronous processing for long-running work
- Performance alarms
- Latency and throughput metrics
- Current runtime and dependency choices

Typical gaps include:

- Over-provisioned resources
- Missing caching for read-heavy workloads
- Synchronous workflows where asynchronous processing is more suitable
- Serverless database access without connection management
- Missing CDN for static or globally accessed content
- No performance telemetry

### Cost Optimization

Assess whether the workload manages cost intentionally without compromising required outcomes.

Look for evidence of:

- Cost allocation tags
- Budgets or cost alarms
- Autoscaling
- Right-sizing
- Lifecycle policies
- Log retention policies
- Storage tiering
- Pricing model selection
- Use of managed services where cost-effective
- VPC endpoints where they reduce unnecessary NAT Gateway usage
- Cleanup automation for unused resources

Typical gaps include:

- Missing cost allocation tags
- Always-on capacity for variable workloads
- No lifecycle policy for large or ageing data
- Indefinite log retention without justification
- Over-provisioned databases or compute
- NAT Gateway usage where endpoints would be appropriate
- No budget or anomaly detection evidence

### Sustainability

Assess whether the workload minimizes unnecessary resource consumption while still meeting business objectives.

Look for evidence of:

- Demand-based scaling
- Scale-to-zero patterns where appropriate
- Efficient data lifecycle management
- Managed services over self-managed infrastructure where suitable
- Graviton or efficient instance families where viable
- Efficient CI/CD builds
- Small deployment artifacts
- Caching and batching
- Data compression
- Region and architecture choices that reduce waste

Typical improvement opportunities include:

- Always-on resources that could scale down or scale to zero
- x86 instances where efficient alternatives are available
- Missing lifecycle rules
- Uncompressed or duplicated data
- Inefficient CI/CD pipelines
- Self-managed infrastructure where managed services would reduce operational overhead

## Risk Assessment

Assess each finding using impact and likelihood.

Impact levels:

- Minor: Limited blast radius, minimal user impact, no data loss, easy to remediate.
- Moderate: Service degradation, limited user impact, potential exposure, or moderate remediation effort.
- Severe: Full outage, data loss, data breach, regulatory impact, or significant blast radius.

Likelihood levels:

- Low: Requires unusual conditions or strong controls already exist.
- Medium: Possible during normal operations or controls are incomplete.
- High: Likely to occur, commonly exploited, or controls are weak or missing.

Risk levels:

- Critical: Severe impact and high likelihood.
- High: Severe impact with medium or low likelihood, or moderate impact with high likelihood.
- Medium: Moderate impact with medium or low likelihood, or minor impact with high likelihood.
- Low: Minor impact with medium or low likelihood.

Do not assign Critical or High risk unless the evidence supports both the impact and likelihood.

## Scoring Guidance

Score each pillar from 1 to 5.

Use this rubric:

1. No practices implemented. Fundamental gaps exist.
2. Basic practices. Some awareness exists, but implementation is incomplete or ad hoc.
3. Developing. Key practices exist, but gaps remain and the approach is partly reactive.
4. Mature. Most practices are implemented proactively, with minor gaps.
5. Optimized. Practices are comprehensive, automated, measured, and continuously improved.

Calibrate scores using evidence.

A workload with encryption, CI/CD, monitoring, autoscaling, multi-AZ design, structured logging, and rollback automation should generally score strongly in the relevant pillars.

Do not over-penalize standard or low-criticality workloads for not implementing enterprise-grade patterns such as multi-region active-active architecture unless the business criticality requires it.

## Output Expectations

When producing a review report, include:

- Executive summary
- Workload context
- Evidence sources reviewed
- Assessment confidence
- Pillar-by-pillar findings
- Pillar scorecard
- Critical and high-risk findings
- Medium and low-risk findings
- Cross-pillar tradeoffs
- Prioritized remediation plan
- Items that could not be determined from static analysis
- Recommended next steps

Every finding should include:

- Finding ID
- Pillar
- Title
- Description
- Evidence
- Impact
- Likelihood
- Risk level
- Recommendation
- Estimated effort
- Affected AWS services
- Confidence level

## Remediation Guidance

Group remediation actions by timeline.

Quick Wins, less than 1 week:

- Low effort changes
- Configuration improvements
- Missing tags
- Missing alarms
- Encryption enablement
- Log retention changes
- Simple lifecycle policies

Foundation, 1 to 4 weeks:

- Medium effort improvements
- Observability implementation
- CI/CD improvements
- Backup and restore automation
- Resilience patterns
- Security control improvements

Strategic, 1 to 3 months:

- Major architecture changes
- Disaster recovery strategy
- Multi-region patterns
- Platform-level security improvements
- Major refactoring
- Advanced automation

Order remediation by risk reduction first, then effort.

Recommendations must be specific to the workload evidence. Avoid generic recommendations unless clearly labelled as an optional improvement.

## Cross-Pillar Tradeoffs

Identify and explain tradeoffs, including:

- Security controls that affect performance or developer flow
- Cost reductions that reduce reliability
- Reliability patterns that increase cost
- Performance optimizations that increase complexity
- Sustainability improvements that require architectural change
- Operational controls that increase short-term delivery effort but reduce long-term risk

Recommend balanced options rather than optimizing one pillar at the expense of the others.

## Cannot Determine Handling

Use "Cannot Determine" when evidence is unavailable from static analysis.

Examples:

- Runtime metrics are unavailable.
- AWS account configuration is not included.
- Operational processes are not documented.
- Incident response evidence is not present.
- Cost data is not available.
- Actual backup restore testing cannot be verified.
- Production traffic patterns are unknown.

For each "Cannot Determine" item, state:

- What could not be assessed
- Why it could not be assessed
- What evidence would be needed
- Whether the uncertainty should be treated as a risk

## Review Tone

The review should be professional, balanced, and constructive.

Acknowledge good engineering decisions prominently. Mature workloads should feel accurately assessed, not unfairly criticized.

Use direct language for confirmed risks, but avoid alarmist wording when evidence is incomplete.

## Final Review Checklist

Before completing the review, verify that:

- All six AWS Well-Architected pillars were assessed.
- Findings are evidence-backed.
- File paths and line numbers are included where practical.
- "Cannot Determine" is used where evidence is unavailable.
- Risk levels follow the impact and likelihood model.
- Scores are justified by strengths and gaps.
- Remediation actions are specific and prioritized.
- Cross-pillar tradeoffs are considered.
- Critical and high-risk findings have clear next steps.
- Existing strengths are acknowledged.
