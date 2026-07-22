---
title: "Blog 2"
date: 2026-07-08
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Deploying a Modern Data Platform in Minutes with Modern Data Architecture Accelerator (MDAA)

Hello everyone in the AWS Study Group VN community.

While exploring AWS Big Data, I came across a very interesting article about the Modern Data Architecture Accelerator (MDAA) — an open-source framework developed by AWS. MDAA helps businesses deploy a Modern Data Platform faster, simpler, and still compliant with AWS Best Practices for security, data governance, and scalability.

If you are learning about Data Lake, Data Platform, or AI Platform on AWS, this is definitely a solution worth considering.

---

## Why did AWS develop MDAA?

Today, a Modern Data Platform is no longer just creating an S3 Bucket to store data. A complete system typically requires combining many AWS services such as:

- Amazon S3 for data storage
- AWS Glue for ETL and metadata management
- AWS Lake Formation for access governance
- AWS IAM for permissions
- AWS KMS for data encryption
- AWS CloudTrail for activity logging
- AWS CloudWatch for system monitoring

With so many services, building the infrastructure becomes complex. The deployment team must ensure every resource is configured correctly for Security, Governance, and Compliance. AWS believes this process often takes a lot of time and is prone to errors when done manually.

---

## How does MDAA work?

The standout feature of MDAA is that it changes the way infrastructure is built. Instead of writing thousands of lines of IaC for each AWS resource, users only need to declare their requirements in a YAML file. MDAA will automatically turn these declarations into a complete AWS architecture using AWS CDK and CloudFormation.

According to AWS, in an example deployment of a governed Data Lake, the infrastructure code can be reduced from about 1,800 lines of CloudFormation to just around 45 lines of YAML. This significantly simplifies the deployment process.

---

## MDAA Module Architecture

MDAA is not a fixed product, but built with a modular architecture. AWS provides over 40 different modules, each handling a specific function such as:

- data storage
- ETL
- data governance
- Machine Learning
- Generative AI

The modules can be combined like LEGO blocks. When a business needs to expand the system, you only need to add modules without redesigning the entire architecture.

To enable modules to communicate with each other, MDAA uses AWS Systems Manager Parameter Store to share configuration information between components. This way, you don’t have to manually declare ARNs or IDs of each resource.

---

## Governance Built-in from the Start

One of MDAA’s strengths is that it integrates Governance right from the deployment stage. In many real-world projects, Data Governance is only added after the system is already built. MDAA does it differently.

During deployment, MDAA already integrates data governance services such as:

- AWS Lake Formation
- AWS Glue Data Catalog
- AWS KMS
- AWS CloudTrail

and many other security mechanisms. As a result, the data platform is built with governance capabilities from day one, rather than having to retrofit the architecture later.

---

## Four Reference Deployment Models

AWS has prepared four reference architectures to suit different needs:

1. **Basic Data Lake**: Suitable for businesses that want to centralize data from multiple sources into one unified storage location.
2. **Data Science Platform**: Expanded to support analysis and Machine Learning model building.
3. **SageMaker Unified Studio**: Designed for environments where multiple teams of Data Engineers, Data Analysts, and Data Scientists work together.
4. **Generative AI Platform**: Supports Amazon Bedrock and Retrieval-Augmented Generation (RAG) architecture to build AI applications on internal data.

With MDAA, when adding new features or models, you don’t need to redesign the entire data platform.

---

## Real-world Example

The article mentions a real case at a university system with 17 campuses, managing about 7.2 TB of data and more than 8,000 dashboards.

After applying MDAA:

- Time to deploy new features and dashboards was reduced by approximately 95%
- Data governance was standardized across the entire system
- Dependency on third-party solutions was reduced

This is proof that MDAA is suitable for organizations with large-scale data, not just small applications.

---

## Is MDAA suitable for every project?

According to the blog, MDAA is aimed at organizations building Modern Data Platforms, Data Lakes, or large-scale data analytics systems. The strength of this framework is not creating more AWS services, but standardizing architecture according to AWS Best Practices.

MDAA helps:

- Reduce deployment time
- Standardize security and governance
- Allow technical teams to focus on data instead of building infrastructure

If your project is just a small system, MDAA may be overkill. But for large organizations with complex data and high governance requirements, MDAA is a very worthwhile option.

---

## Conclusion

MDAA is an interesting approach from AWS in building modern data platforms. Instead of letting engineers configure each service individually, MDAA helps automate nearly the entire deployment process and integrates governance, security, and scalability mechanisms from the start.

If you are researching Data Lake, Data Platform, or data architecture on AWS, this article is well worth reading and referencing how AWS standardizes large-scale data system deployments.

---

## References

- AWS Blog: Deploy Modern Data Platforms in Minutes with MDAA