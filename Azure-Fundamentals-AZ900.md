# AZ900 Azure Fundamentals

## Cloud Concepts
- Econonmy of Scale
    - Since Azure owns and runs so much hardware, they can do so more efficiently, and pass on the savings to the customer

## Azure Architecture
- Regions
    - Region Pairs 
        - A region that is linked with another region in the same geography (at least 300 miles apart)
        - Some services can replicate
        - Patch/update one region at a time
        - Can failover from one region to the other when there's an outage

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
- Containers
- Kubernetes Service
- Functions

## Networking
- Load Balancers
    - Network LB
        - Can be used for any traffic (internal or external)
        


## Storage
- Blob
    - used for storing large amount of unstructured data
- Disk
    - allows data to be stored persistently and attached as a virtual hard disk
- File
    - uses SMB protocol
    - allows multiple VMs to R/W the files
- Archive
    - Cold 

## Database
- CosmosDB
    - PaaS DB offering
- Azure SQL DB
    - PaaS DB offering

## Authentication and Authorization

## Azure Solutions
- IoT Central
    - IoT Central is a Software-as-a-Service solution that can help ease the costs and effort of developing a whole IoT solution from scratch.
- IoT Hub
    - IoT Hub provides a managed and secure backend for millions of IoT devices.
- Artificial Intelligence
    - an ML model defines what you want your machine learning implementation to learn

## Security

## Privacy, Compliance and Trust

## Pricing

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

## Exam Preparation