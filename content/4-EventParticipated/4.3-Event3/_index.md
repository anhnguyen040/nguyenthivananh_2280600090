---
title: "Event 3"
date: 2026-06-27
location: Online
role: Attendee
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Summary Report: “Meet up”

### Event Objectives

- Provide practical insights into the application of Agentic AI in Cloud Operations, DevOps, Security, and HR within Vietnamese enterprises.
- Discuss how AI can support (rather than replace) humans, while highlighting technical challenges and production-grade solutions.
- Connect the community, inspire participants, and promote the development of AI + Cloud skills among the younger generation.

### Speakers

- **Steve Trần** - Cloud Thinker
- **Nghi Danh** - AI Engineer, **Kiet Tran** - AI Engineer, **Trung Vu** - CEO Revve AI
- **Bao Phan, Nguyen Nguyen** - Cloud Kinetic 
- **Truong Tran, Anh Dang** - Noventiq
### Key Highlights

#### Career Journey

A career journey from university student to founder of Cloud Thinker in approximately four years.

The problem Cloud Thinker addresses: As enterprises migrate to the cloud, systems become increasingly complex and require more DevOps/SRE engineers to operate and maintain.

Cloud Thinker developed an agentic platform that supports human engineers in:

- Incident response: AI investigates incidents faster than humans (minutes instead of hours)
- Code review: Reviews infrastructure/code changes before deployment to production
- FinOps: Optimizes cloud costs
- Security: Automated penetration testing (simulating both white-hat and black-hat hacker behaviors)

#### Vietnamese Voice AI

Current speech-to-speech models primarily perform well for English because Vietnamese is considered a "low-resource language."

Solution: Use a three-step architecture:

STT (Speech-to-Text) → LLM (text processing, tool calling) → TTS (Text-to-Speech)

instead of relying on direct speech-to-speech models.

Unique challenges for Vietnamese:

- Voice-based gender recognition
- Natural interruption handling
- Regional accent and dialect processing

A complete production-ready infrastructure is required, including:

- Audit logs
- Version control
- Knowledge base
- Human handover mechanisms when AI cannot handle a request

#### DevOps AI Agent – AWS

Problem addressed: Logs and tracing data are fragmented, causing loss of context during manual incident investigation.

The six key pillars:

- Context: Agent space + system topology
- Control: Permission management based on tags
- Integration: MCP (Model Context Protocol)
- Collaboration: Integration with Slack/ServiceNow
- Convenience
- Cost-effective: Cost calculated based on runtime duration

Four-step workflow:

- Triage
- Investigation: Generate hypotheses and provide evidence
- Mitigation plan: Provide recommendations only, without autonomous execution
- System improvement recommendations

#### Amazon Quick for HR

HR challenges:
- Manual CV screening
- Missing potential talent
- Subjective evaluation processes
- Security risks when uploading sensitive data to public AI platforms

Amazon Quick:

An agentic AI solution that allows organizations to customize their own skills and connect with multiple platforms through connectors/MCP, including:

- Microsoft
 - Google Workspace
- S3
- Jira
- Salesforce
- GitHub

#### Security for Amazon Quick Connected to MCP Server

Problem:

Connecting MCP servers through the public internet introduces security risks such as:

- DDoS attacks
- Man-in-the-middle attacks

Solution:
- VPC Interface Endpoint
- Route 53 Resolver Private
- Application Load Balancer (ALB)
- TLS encryption through ACM


### Key Takeaways
- Agentic AI is a powerful supporting tool that can significantly improve productivity in Cloud Operations, DevOps, Security, and HR when properly designed and combined with human supervision.
- Successful AI agent implementation requires strong foundations in architecture, observability, security guardrails, and clear approval mechanisms.
- Continuous learning, building real-world projects, and contributing to the community are essential for long-term career growth in the AI era.
  
![Nguyen Thi Van Anh](/images/event3.png)