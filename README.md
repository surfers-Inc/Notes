# AWS Certified Cloud Practitioner (CLF-C02) Notes

Short, exam-focused notes for the CLF-C02 exam. Each service note explains what it is, the keywords the exam uses, the common trap, and a link to the official AWS documentation for deeper reading.


## Study order

1. [Exam overview](00-exam-overview.md): exam format, domains, strategy, and a keyword-to-service lookup
2. [Cloud concepts](01-cloud-concepts.md): cloud value, CAF, Well-Architected, economics, pricing, support plans, migration
3. [Compute and networking](02-compute-and-networking.md): EC2, Lambda, containers, VPC, DNS, CDN, and connectivity
4. [Storage and databases](03-storage-and-databases.md): S3, EBS, EFS, FSx, RDS, DynamoDB, and more
5. [Security and compliance](04-security-and-compliance.md): shared responsibility, IAM, encryption, threat detection, compliance
6. [Management and analytics](05-management-and-analytics.md): monitoring, governance, cost tools, analytics, integration, AI/ML, and other service categories
7. [Cheat sheet and exam highlights](06-cheat-sheet-exam-highlights.md): final review, comparisons, and renamed services

## How to read each service note

| Marker | Meaning |
|---|---|
| 📌 **What is it?** | Plain-language definition with an analogy |
| 🎯 **Need to Know for the Exam** | Keywords and facts the exam tests, including pricing basics |
| ⚠️ **Exam Trap / Common Distractor** | The look-alike answer to avoid |
| 📖 **AWS docs** | Official AWS page for reading beyond the exam-level summary and deeper understanding to the service |

## Official sources

- [AWS CLF-C02 exam guide](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/cloud-practitioner-02.html)
- Domain task statements: [Domain 1](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/cloud-practitioner-02-domain1.html) · [Domain 2](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/cloud-practitioner-02-domain2.html) · [Domain 3](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/cloud-practitioner-02-domain3.html) · [Domain 4](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/cloud-practitioner-02-domain4.html)
- [AWS CLF-C02 in-scope services](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/clf-02-in-scope-services.html) and [out-of-scope services](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/clf-02-out-of-scope-services.html)
- [In-scope technologies and concepts](https://docs.aws.amazon.com/aws-certification/latest/cloud-practitioner-02/clf-technologies-concepts.html)
- [AWS Certified Cloud Practitioner exam page](https://aws.amazon.com/certification/certified-cloud-practitioner/) and [exam policies](https://aws.amazon.com/certification/policies/)
- [AWS Skill Builder: CLF-C02 exam preparation](https://skillbuilder.aws/category/exam-prep/cloud-practitioner-foundational-CLF-C02)
- [Overview of Amazon Web Services (whitepaper)](https://docs.aws.amazon.com/whitepapers/latest/aws-overview/introduction.html)
- [AWS Documentation home](https://docs.aws.amazon.com/)

## Further study and practice

Community material

### Labs and knowledge checks

- [AWS re/Start knowledge checks](https://github.com/cypherpulse/AWS-reStart-KCs/tree/master/KC): community repository; use for catch-up and review.
- [AWS re/Start Linux labs](https://github.com/cbecerrae/aws-restart-labs/tree/main/Linux): community Linux exercises.

### Community exam preparation

- [CLF-C02 short notes and practice exams](https://github.com/kananinirav/AWS-Certified-Cloud-Practitioner-Notes): check answers against current AWS documentation.
- [Practice exam 1](https://github.com/kananinirav/AWS-Certified-Cloud-Practitioner-Notes/blob/master/practice-exam/practice-exam-1.md): community questions.
- [Cloud Practitioner video](https://www.youtube.com/watch?v=dsoefw1UlRI): third-party video supplied by the cohort.
- [AWSBoy practice exams](https://www.awsboy.com/aws-practice-exams/): older community material; verify against CLF-C02.

## Important notes on changing content

- AWS renames services and changes support plans, prices, and availability regularly. Older practice questions may use outdated names. See **Names you may see (old and new)** in the [cheat sheet](06-cheat-sheet-exam-highlights.md#names-you-may-see-old-and-new).
- The exam's in-scope service list is non-exhaustive and can change. Use these notes to learn each service's purpose and the differences between services, then confirm changing details on the official AWS documentation.