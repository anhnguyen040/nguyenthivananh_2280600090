---
title: "Event 1"
date: 2026-05-23
location: Floor 36, Bitexco Financial tower
role: Attendee
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Summary Report: “FCAJ Community Day”

### Event Objectives

- The FCAJ Community Day aimed to explore modern Agentic AI, Cloud Architecture, and Enterprise-Grade System Design, connecting cutting-edge technology with practical business applications through expert sessions and workshops.
- Learned about Amazon Quick Suite, CloudFront as a foundation, non-determinism in LLMs, and the power of multi-agent systems for complex problems like startup credit scoring.
- Understood when to use single vs multi-agent architectures, the importance of guardrails, security, compliance, and how to move AI solutions from POC to production.

### Speakers

- **Tinh Truong** - Platform Engiiner, GoTymeX
- **Pham Nguyen Hai Anh** - G-AsiaPacific Vietnam, AWS Community Builder
- **Nguyen Tuan Thinh** - DevOps Engineer, First Cloud AI Journey
- **UTMorpho** - Team VIB LotusHacks 2026
- **Duc Dao** - Solution Architect - Cloud Kinetics
- **Vy Lam** - Senior Business Systems Analyst VPBank

### Key Highlights

#### Identifying the drawbacks of legacy application architecture

- Long product release cycles → Lost revenue/missed opportunities  
- Inefficient operations → Reduced productivity, higher costs  
- Non-compliance with security regulations → Security breaches, loss of reputation  
- Context engineering is becoming a core future skill.

#### Friendly AI assistant with Amazon quick

- Business users often face a painful daily routine: gathering information from multiple sources, consulting experts for analysis, and repeating time-consuming manual tasks.
- AWS introduced Amazon Quick Suite, a new agentic AI solution designed for enterprises.
- It creates a unified and friendly experience where people and AI agents collaborate seamlessly around the company’s entire knowledge base.
- The suite offers powerful capabilities in Insights, BI, Automation, data connectors, and strong governance, with practical use cases such as the PM Assistant (auto MoM, email, scheduling). 

#### CloudFront as Your Foundation

- Businesses struggle with unpredictable CDN costs, sudden bill spikes, complicated security management, and performance challenges in delivering content globally.
- Amazon CloudFront is positioned as the foundation for modern web and application delivery, offering a unified solution that combines content delivery, security, reliability, and performance.
- Key capabilities include predictable flat-rate pricing, global edge network, robust DDoS & WAF protection, origin cloaking, advanced caching, HTTP/3, and intelligent failover.
- It supports various customer personas — from small website owners to large scaling businesses — with strong security (HTTPS, Mutual TLS, OAC) and cost optimization features.

#### 36 hrs with LotusHacks

- **3 integration patterns**: Publish/Subscribe, Point-to-point, Streaming  
- **Benefits**: Loose coupling, scalability, resilience  
- **Sync vs async comparison**: Understanding the trade-offs  

#### Non-Determinism of "Deterministic" LLM Settings
While LLMs generate text token-by-token through logits, softmax, and sampling, researchers discovered that even with identical prompts and temp=0, outputs can vary significantly due to floating-point arithmetic on GPUs and inference batching optimizations.
- Temperature = 0 does not guarantee true determinism in real-world inference environments.
- Non-determinism stems from GPU architecture and commercial inference optimizations.
- Practical tips: Use temp=0.1 as a sweet spot, apply multiple runs with voting, structured outputs, and thorough testing.  

#### Enterprise-Grade Multi-Agent System - The Case of Startup Credit Scoring
The presentation introduces an Enterprise-Grade Multi-Agent System for startup credit scoring. It starts by highlighting the structural mismatch between traditional banking credit systems (designed for established companies) and the multi-dimensional, unstructured nature of startup data.
The speaker then explains why a single-agent approach is insufficient and proposes a multi-agent architecture — a “Virtual Credit Committee” consisting of specialized agents (Financial Analyst, Market Analyst, Team Evaluator, Risk Assessor, Compliance) coordinated by a Manager. This is followed by enterprise-grade considerations including security, guardrails, compliance, deployment architecture (using AWS Bedrock AgentCore, ECR, API Gateway), and a compelling ROI analysis.

### Key Takeaways

#### Mindset for working with AI
- The mindset for working with AI is crucial; one must grasp the process and provide accurate information to achieve effective results.
- High-quality input—rather than simply a large volume of data—is what ensures the output aligns with expectations.

#### Friendly AI assistant 
- Traditional workflows are inefficient.
- Amazon Quick Suite enables friendly AI agents to accelerate insight-to-action in a secure enterprise environment.

#### LotusHacks
- Collaboration is everything — Team synchronization and alignment matter most for success.
- More ideas ≠ Better ideas — Beware of Scope Creep; adding too many features can overwhelm the project and reduce quality.
#### Enterprise-Grade Multi-Agent System
- Traditional single-agent systems fail on complex, multi-domain problems like startup credit assessment.
- Multi-agent systems provide specialized expertise, better reasoning, auditability, and fault tolerance.
- Moving from POC to production requires strong focus on security, guardrails, governance, and operational readiness.
- Enterprise Al is not about making things work. It's about making them work securely, reliably, and at scale.

### Event Experience

Participating in the FCAJ Community Day was an incredibly valuable and insightful experience. It provided me with a comprehensive view of modern approaches in Agentic AI, Cloud Architecture, and Enterprise-Grade System Design.

#### Learning from highly skilled speakers
- Gained deep understanding of Amazon Quick Suite — a new agentic AI platform that enables seamless collaboration between humans and AI agents for enterprise workflows.
- Explored Amazon CloudFront as a foundational service for secure, high-performance, and cost-optimized content delivery with strong global edge capabilities.
- Discovered the surprising non-determinism in LLMs even with “deterministic” settings (temp=0), along with practical mitigation strategies.
- Learned how to build Enterprise-Grade Multi-Agent Systems, demonstrated through a real-world Startup Credit Scoring use case.

#### Hands-on technical exposure
- Understood when to use single-agent vs multi-agent architectures and how a “Virtual Credit Committee” with specialized agents delivers superior reasoning and auditability.
- Recognized the importance of guardrails, security, compliance, and observability when moving AI solutions from POC to production.
- Learned practical deployment patterns using AWS Bedrock AgentCore, ECR, Lambda, and API Gateway.    

#### Lessons learned
- Modern enterprise AI is not just about making things work — it’s about making them work securely, reliably, and at scale.
- Multi-agent systems are powerful for complex, multi-domain problems, but require strong architecture and enterprise thinking.
- Always design with variance, security, and auditability in mind, especially in high-stakes applications. 

#### Some event photos
![Nguyen Thi Van Anh](https://anhnguyen040.github.io/nguyenthivananh_2280600090/images/event1.jpg)