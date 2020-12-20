# AWS Solutions Architect Professional

## Exam Breakdown
#### Design for Organizational Complexity (12.5%)
#### Design for New Solutions (31%)
#### Migration Planning (15%)
#### Cost Control (12.5%)
#### Continuous Improvement for Existing Solutions (29%)

## DataStores
- Consistency Models
	- ACID (when strong consistency needed)
		- Atomic - Transactions are all or nothing
		- Consistent - Transactions must be valid
		- Isolated - Transactions can't mess with one another
		- Durable - Completed Transactions must stick around
	- BASE (when scaled data stores needed, S3, DynamoDB)
		- Basic Availability - values available, even if stale
		- Soft-state - might not be instantly consistent across stores
		- Eventual Consistency - will achieve consistency at some point
- S3
	- Object Store (max size 5TB, largest PUT 5GB)
	- For large object uploads, use multi-part uploads (more efficient)
	- S3 path is not filepath, is a key (think DB not filesystem)
	- Consistency
		- New objects (which haven't also been read previously) can be read immediately
		- If a HEAD/GET request is issued for a non-existent object, then reads after initial put requests will result in the initial GET/HEAD response until the new object is fully replicated
		- If an object is updated/deleted, the updating/deletion needs to be fully replicated to other places before that will be served up - eventually (i.e. the current file will be served until then)
		- only one person can update an object at a time, multiple requests processed in timestamp order
	- 

## Networking

## Security

## Migrations

## Architecting to Scale

## Business Continuity

## Deployment and Operations Management

## Cost Management




Domain 1: Design for Organizational Complexity
1.1 Determine cross-account authentication and access strategy for complex organizations (for example, an
organization with varying compliance requirements, multiple business units, and varying scalability
requirements)
1.2 Determine how to design networks for complex organizations (for example, an organization with varying
compliance requirements, multiple business units, and varying scalability requirements)
1.3 Determine how to design a multi-account AWS environment for complex organizations (for example, an
organization with varying compliance requirements, multiple business units, and varying scalability
requirements)
Domain 2: Design for New Solutions
2.1 Determine security requirements and controls when designing and implementing a solution
2.2 Determine a solution design and implementation strategy to meet reliability requirements
2.3 Determine a solution design to ensure business continuity
2.4 Determine a solution design to meet performance objectives
2.5 Determine a deployment strategy to meet business requirements when designing and implementing a
solution
Domain 3: Migration Planning
3.1 Select existing workloads and processes for potential migration to the cloud
3.2 Select migration tools and/or services for new and migrated solutions based on detailed AWS knowledge
3.3 Determine a new cloud architecture for an existing solution
3.4 Determine a strategy for migrating existing on-premises workloads to the cloud
Version 1.3 SAP-C01 3 | P a g e
Domain 4: Cost Control
4.1 Select a cost-effective pricing model for a solution
4.2 Determine which controls to design and implement that will ensure cost optimization
4.3 Identify opportunities to reduce cost in an existing solution
Domain 5: Continuous Improvement for Existing Solutions
5.1 Troubleshoot solution architectures
5.2 Determine a strategy to improve an existing solution for operational excellence
5.3 Determine a strategy to improve the reliability of an existing solution
5.4 Determine a strategy to improve the performance of an existing solution
5.5 Determine a strategy to improve the security of an existing solution
5.6 Determine how to improve the deployment of an existing solution




