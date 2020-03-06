# AZ900 Azure Fundamentals

## Cloud Concepts
- Benefits if using Cloud Services
    - High Availability
        - ability of system to respond to users (% of time)
    - Scalability
        - abilty of system/website to handle grown of users/demand/requests
    - Elasticity
        - ability to automatically grow/shrink resources based on demand
    - Agility
        - ability to respond to changes in the market or environment
        - can test things cheaply/experiment
    - Fault Tolerance
        - abiltiy of a system to handle faults like power, networking or hardware failures
    - Disaster Recovery
        - ability of a system toi recover from a failure within a period of time, and how much data is lost
    - Econonmy of Scale
        - Since Azure owns and runs so much hardware, they can do so more efficiently, and pass on the savings to the customer
    - CapEx vs. OpEx
        - CapEx is money invested (upfront) in assets (like computers) that return investment over time
        - OpEx is the cost of doing business on daily bases, ongoing expenses
    - Consumption-based model
        - Pay per time used (minute, hour)
        - Pay for resources used (size, number)

- Paradigms for hosting apps
    - IaaS
        - Infrastructure such as VMs, networking, LBs, firewalls that you can get in a Cloud environment
    - PaaS
        - Provider runs your application, don't have access to hardware, but you provide the software/code
    - SaaS
        - Software available to use within the cloud, only configuration access only
![Compare paradigms](https://stack247.files.wordpress.com/2015/05/azure-on-premises-vs-iaas-vs-paas-vs-saas.png)
    
- Cloud Types
    - Public
        - Computing services offered over the public internet
        - anyone can sign up
    - Private
        - Computing services offered to only select users
        - internal or corporate cloud
        - can be hosted on own or leased hardware
    - Hybrid
        - Combination of public and private clouds
        - scale private infrastructure to the cloud

## Azure Architecture
- Regions
    - 58 regions
![Azure Regions](https://azurecomcdn.azureedge.net/cvt-92799112b92ce1432ee2e938fe2233012be6a469ece7c4743b538ebd039395e4/images/shared/regions-map-large.svg)
    - Region Pairs 
        - A region that is linked with another region in the same geography (at least 300 miles apart)
        - Some services can replicate
        - Patch/update one region at a time
        - Can failover from one region to the other when there's an outage
- Availability Zones
- Resource Groups
- Azure Resource Manager
- Benefits / Usage of core Azure architectural components

## Compute
- Virtual Machines
    - Is your machine to install/control
    - IaaS - including networking
    - Tooling to manage VMs
    - Bluepirnts
        - Used to configure the entire stack of resources to be used
    - Azure recommentations
    - Many different OS types
    - Pricing
        - hourly
        - More compute/memory, more cost
- Scale Sets
    - Group of identical, load balanced VMs
    - Controlled by a scaling policy, e.g. when CPU is > X%, add another VMs or scheduled
    - Way of ensuring High Availability, if a machine stops, app keeps working
    - Can manage multiple VMs easily w/ the same config
    - Auto scaling, run what you need, up or down
- App Services
- Azure Container Instances 
- Azure Kubernetes Service
- Functions

## Networking
- Virtual Network
- Load Balancers
    - Network LB
        - Can be used for any traffic (internal or external)
- VPN Gateway
- Application Gateway
- Content Delivery Network

## Storage
- Blob
    - used for storing large amount of unstructured data
- Disk
    - allows data to be stored persistently and attached as a virtual hard disk
- File
    - uses SMB protocol
    - allows multiple VMs to R/W the files
- Archive
    - Cold, infrequently accessed data, cheap storage, slow/expensive access

## Database
- CosmosDB
    - PaaS DB offering
- Azure SQL DB
    - PaaS DB offering
- Azure DB for MySQL
- Azure DB for PostgreSQL
- Azure DB Migration service

## Marketplace
- Service where external vendors can sell their Azure products and solutions

## Azure Solutions
- IoT
    - IoT Central is a Software-as-a-Service solution that can help ease the costs and effort of developing a whole IoT solution from scratch.
    - IoT Hub provides a managed and secure backend for millions of IoT devices.
- Big Data / Analytics
    - SQL Data Warehouse
    - HDInsight
    - Azure Databricks
- Artificial Intelligence
    - Machine Learning service
        - an ML model defines what you want your machine learning implementation to learn
    - Machine Learning Studio
- Serverless
    - Azure Functions
    - Logic Apps
    - Event Grid
- DevOps
    - Azure Devops
    - Azure DevTest Labs
- Benefits/Outcomes of using Azure solutions
- Tools
    - Azure Portal
    - Azure PowerShell
    - Azure CLI
    - Cloud Shell
- Azure Adviser

## Security
- Network Security Groups
- Application Security Groups
- User Defined Rules
- Azure Firewall
- Azure DDoD Protection
- security solution appropriateness
- Security Tools
    - Azure Security Center
    - Azure Security Center usage scenarios
    - Key Vault
    - Azure Information Protection (AIP)
    - Azure Advanced Threat Protection (ATP)

## Identity Services
- Authentication and Authorization
- Azure Active Directory
- Azure Multi-Factor Authentication

## Governance
- policies and initiatives with Azure Policy
- Role-Based Access Control (RBAC)
- Locks
- Azure Advisor security assistance
- Azure Blueprints

## Monitoring & Reporting
- Azure Monitor
- Azure Service Health
- Use cases and benefits of Azure Monitor and Azure Service Health

## Privacy, Compliance and Trust
- Compliance 
    - GDPR
    - ISO
    - NIST
- Microsoft Privacy Statement
- Trust center
- Service Trust Portal
- Compliance Manager
- Azure Compliance
- Azure Government
- Azure China

## Pricing
- Subscriptions
    - access control
    - offer types
    - Management groups
- Cost management
    - Purchase options
    - Azure Free account
    - resource types, services, locations, ingress/egress traffic
    - Zones for billing purposes
    - Pricing calculator
    - Total Cost of Ownership (TCO) calculator
    - Cost Minimisation best practices
        - performing cost analysis
        - spending limits 
        - quotas
        - using tags to identify cost owners, 
        - Azure reservations 
        - Azure Advisor
    - Azure Cost Management

## Support
- Levels
    - Basic
    - Developer
        - For trial and non-production environments
    - Standard
    - Professional Direct 
        - Max 2 hour response time for Cat B incidents
    - Premier
        - Fastest response time
- Support Tickets
- Support Channels
- Knowledge Centre
- Service Level Agreements (SLA)
    - Composite SLAs
    - determin appropriate SLA for an app
- Service Lifecycle
    - Previews
        - Private
        - Public
    - General Availability
    - Monitor feature/service updates 

## Exam Preparation
- https://learn.acloud.guru/course/az-900-microsoft-azure-fundamentals
- https://www.udemy.com/course/az900-azure/
- https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3VwUY
