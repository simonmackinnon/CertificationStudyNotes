# AWS SysOps Associate

- [Exam preparation:](#exam-preparation-)
- [Monitoring and Reporting](#monitoring-and-reporting)
  * [Create and maintain metrics and alarms utilizing AWS monitoring services](#create-and-maintain-metrics-and-alarms-utilizing-aws-monitoring-services)
    + [CloudWatch](#cloudwatch)
    + [CloudTrail](#cloudtrail)
    + [Cloudwatch vs. CloudTrail](#cloudwatch-vs-cloudtrail)
    + [Cost Expolorer](#cost-expolorer)
    + [Cost Allocation Tags](#cost-allocation-tags)
  * [Recognize and differentiate performance and availability metrics](#recognize-and-differentiate-performance-and-availability-metrics)
    + [EC2 Metrics](#ec2-metrics)
    + [EBS Metrics](#ebs-metrics)
    + [ELB Monitoring](#elb-monitoring)
    + [Monitoring Elasticache](#monitoring-elasticache)
  * [Multiple AWS Accounts](#multiple-aws-accounts)
    + [Organizations](#organizations)
  * [Tags & Resource Groups](#tags---resource-groups)
  * [AWS Config](#aws-config)
  * [Health Dashboards](#health-dashboards)
  * [Perform the steps necessary to remediate based on performance and availability metrics](#perform-the-steps-necessary-to-remediate-based-on-performance-and-availability-metrics)
- [Deployment and Provisioning](#deployment-and-provisioning)
  * [Identify and execute steps required to provision cloud resources](#identify-and-execute-steps-required-to-provision-cloud-resources)
    + [EC2](#ec2)
    + [Lifecycle](#lifecycle)
    + [Deploy EBS Volumes](#deploy-ebs-volumes)
    + [Elastic Load Balancers](#elastic-load-balancers)
    + [Deploy an ALB](#deploy-an-alb)
    + [Bastion Hosts (Jumpbox)](#bastion-hosts--jumpbox-)
  * [Identify and remediate deployment issues](#identify-and-remediate-deployment-issues)
    + [Potential EC2 Launch Issues](#potential-ec2-launch-issues)
    + [Containers and Serverless](#containers-and-serverless)
- [High Availability](#high-availability)
  * [Recognize and differentiate highly available and resilient environments on AWS](#recognize-and-differentiate-highly-available-and-resilient-environments-on-aws)
  * [Implement scalability and elasticity based on use case](#implement-scalability-and-elasticity-based-on-use-case)
    + [Elasticity & Scalability](#elasticity---scalability)
    + [Autoscaling](#autoscaling)
    + [Route 53](#route-53)
- [Storage and Data Management](#storage-and-data-management)
  * [Create and manage data retention](#create-and-manage-data-retention)
  * [Identify and implement data protection, encryption, and capacity planning needs](#identify-and-implement-data-protection--encryption--and-capacity-planning-needs)
    + [Relational Database Service](#relational-database-service)
    + [Aurora](#aurora)
    + [DynamoDB](#dynamodb)
    + [AWS Database Migration Service](#aws-database-migration-service)
- [Security and Compliance](#security-and-compliance)
  * [Artifact](#artifact)
  * [Implement and manage security policies on AWS](#implement-and-manage-security-policies-on-aws)
  * [Implement access controls when using AWS](#implement-access-controls-when-using-aws)
  * [Differentiate between the roles and responsibility within the shared responsibility model](#differentiate-between-the-roles-and-responsibility-within-the-shared-responsibility-model)
- [Networking](#networking)
    + [Global Infra](#global-infra)
  * [Apply AWS networking features](#apply-aws-networking-features)
  * [Implement connectivity services of AWS](#implement-connectivity-services-of-aws)
  * [Gather and interpret relevant information for network troubleshooting](#gather-and-interpret-relevant-information-for-network-troubleshooting)
- [Automation and Optimization](#automation-and-optimization)
  * [Use AWS services and features to manage and assess resource utilization](#use-aws-services-and-features-to-manage-and-assess-resource-utilization)
    + [CLI](#cli)
    + [Systems Manager (SSM)](#systems-manager--ssm-)
    + [Configuration Management](#configuration-management)
    + [AMIs](#amis)
    + [Cloudformation](#cloudformation)
    + [SDK](#sdk)
    + [Powershell](#powershell)
    + [OpsWorks](#opsworks)
  * [Employ cost-optimization strategies for efficient resource utilization](#employ-cost-optimization-strategies-for-efficient-resource-utilization)
  * [Automate manual or repeatable process to minimize management overhead](#automate-manual-or-repeatable-process-to-minimize-management-overhead)

## Exam preparation:
* https://aws.amazon.com/certification/
* https://www.aws.training/ 
* Video: Exam Readiness: AWS Certified SysOps Administrator - Associate
* https://acloud.guru/learn/aws-certified-sysops-administrator-associate-2019
* https://www.udemy.com/course/aws-certified-sysops-administrator-associate-practice-exams-2/
* https://aws.psiexams.com/#/dashboard AWS Certified SysOps Administrator - Associate - Practice
* https://app.linuxacademy.com/challenges/98f5c55c-edaf-441f-8f4f-4093646cce5b?redirect_uri=https:%2F%2Fapp.linuxacademy.com%2Fsearch%3Fquery%3DAWS%2520SysOps AWS Certified SysOps Administrator - Associate - Practice Exam
* Systems Operations on AWS - Classroom Live (3 Days)

## Monitoring and Reporting
### Create and maintain metrics and alarms utilizing AWS monitoring services
#### CloudWatch
- Service used for Monitoring AWS resources as well as the applications you run on AWS
- Stored indefinitely by default
- Retrieved using GetMetricStatistics API
- Most default metric granularity is 1, 3 or 5 minutes
- Detailed monitoring minimum granularity is 1sec (previously 1 minute) - needs high resolution monitoring enabled
    - for metric with 1 minute granularity, data stored for 15 days
- AWS Metrics prefixed with "AWS" this is reserved namespace
- Alarms
    - For any Service
    - Thresholds
    - Actions
    - Billing and Alarm data can be accessed only from the us-east-1 region.
        - Should use AWS Budgets instead
- Can be used on-premise, just need to download and configure SSM agent and Cloudwatch agent
- CloudWatch dashboards
    - International
    - Can display any widget (metric graph/number/text, etc) to any region
    - Must be in the region of the metric to add it to the board
- Logs can be dumped into S3 for longterm usage
- Log Streams
    - individual data from instances
- Log Groups
    - aggregated data based on similar systems
- Log events
    - individual data points from the streams
- CloudWatch rules can be used to trigger based on events, targets Lambda, Step-functions, or other account event buses
#### CloudTrail
- Logs for your AWS environment
- All API calls within the account
- For auditing
- Encrypted by Default with SSE
- Enabled by default (for 7 days)
    - can extend this manually (Create Trail)
- Can use "digest files" to validate logfile integrity
#### Cloudwatch vs. CloudTrail
- Cloudwatch is for metrics, alarms, rules, performance monitoring and notifications
- Cloudtrail is for auditing. Is a record of all API calls, detailed info, etc.
#### Cost Expolorer
- Tool to view and analyse costs, based on resource type, region, tag etc. 
- 13 months of data retained
- can generate reports (csv)
- can use to predict future costs 
- When enabled at organizations level, will show combined totals for all accounts
#### Cost Allocation Tags
- Use tags to tag resources
- Need to activate particular tags to be able to use them within cost explorer
- Can now track costs based on these activated tags
- If you see an error when doing this, probably not in root account
    
### Recognize and differentiate performance and availability metrics
#### EC2 Metrics
- Default (host level)
    - CPU
    - Network
    - Disk
    - Status Check
- Custom (virtual layer)
    - Memory (RAM utilisation)
    - Application Errors, messages, etc.
    - Any other virtual disk layer metrics
    - Is required to monitor disk usage activity of the ephemeral volumes of an Amazon EC2 instance
    - https://docs.aws.amazon.com/en_pv/AmazonCloudWatch/latest/monitoring/install-CloudWatch-Agent-on-EC2-Instance.html
        - Create the IAM user necessary for the CloudWatch agent to write data to CloudWatch
        - Create the IAM user necessary to store the configuration file in Parameter Store and send information to CloudWatch
        - Use CLI or SSM or CloudFormation to download and install the agent packages on the server (debian, rpm, msi)
            - SSM does this all automatically for the specific OS type
        - Create and modify an agent config file to specify what and frequencty of reporting (manual file edit, or use Cloudwatch Agent File wizard)
    - Manual (Console) Steps to set up custom metrics (old scripts)
        - Create IAM role for EC2 to access CloudWatch
        - Provision Ec2 Instance, assign created role
        - Add Bootstrap script 
            ```
            #!/bin/bash
            yum update -y
            sudo yum install -y perl-Switch perl-DateTime perl-Sys-Syslog perl-LWP-Protocol-https perl-Digest-SHA.x86_64
            cd /home/ec2-user/
            curl https://aws-cloudwatch.s3.amazonaws.com/downloads/CloudWatchMonitoringScripts-1.2.2.zip -O
            unzip CloudWatchMonitoringScripts-1.2.2.zip
            rm -rf CloudWatchMonitoringScripts-1.2.2.zip
            ```
         - Assign security group, allow ssh and http access
         - Launch with default settings for all else
         - run the following to test the custom metrics are working (should complete with "Verification completed successfully. No actual metrics sent to CloudWatch.")
             ```
             /home/ec2-user/aws-scripts-mon/mon-put-instance-data.pl --mem-util --verify --verbose
             ```
         - run the following to send data to CloudWatch (Successfully reported metrics to CloudWatch. Reference Id: 2b5ec5d7-ae62-11e9-b319-1148eff87fbf)
             ```
             /home/ec2-user/aws-scripts-mon/mon-put-instance-data.pl --mem-util --mem-used --mem-avail
             ```
            Can see the metric in the console now.
        - Set this up to run on Crontab (scheduled task)
             ```
             */1 * * * * root /home/ec2-user/aws-scripts-mon/mon-put-instance-data.pl --mem-util --mem-used --mem-avail
             ```
             this will now add new entry points to CloudWatch on a minute by minute basis, sent at 5 minute aggregated values (can make this more frequent with detailed monitoring)
#### EBS Metrics
- EBS Types
    - See https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumeTypes.html for latest stats
    - General Purpose SSD (gp2)*
        - Recommended for most workloads
        - System boot volumes
        - Virtual Desktops
        - Low-latency interactive apps
        - Dev and Test environments
        - Up to 16,000 IOPS
        - 3 IOPS/GiB
    - Provisioned IOPS SSD (io1)
        - critical business apps that need sustained IOPS perf
        - requires more than 16,000 IOPS or 250MiB/s throughput per volume 
        - For big database workloads (SQL Server, Oracle, MongoDB, etc.
        - up to 64,000 IOPS for Nitro-based instances (32,000 for others)
        - up to 1,000 MiB/s for Nitro-based instances (500 for others)
    - Throughput Optimised HDD (st1)
        - Steaming workloads
        - Big data, log processing
        - can't be boot volume
    - Cold HDD (sc1)
        - throughput optimised
        - large amounts of data
        - infrequently accessed
        - cheap storage
        - can't be boot volume
    - No need to pre-warm new EBS volumes
    - Volumes restored from snaps have latency on I/O ops when first accessed, you don't have to, but can pre-warm this.
        - to do this, just read all the blocks on volume so that latencty reduced.
- Modifying
    - Can do on-the-fly
    - size, type or IOPS perf
    - without detaching
    - can do via console or api/cli
- Metrics
    - VolumeReadBytes, VolumeWriteBytes (num bytes transferred per time period, avg, sum, min, max)
    - VolumeReadOps, VolumeWriteOps (num IO ops per time perios, total)
    - VolumeTotalReadTime, VolumeTotalWriteTime (num time spent on IO ops per time period, total)
    - VolumeIdleTime (time when no R/W ops performed per time period, total)
    - VolumeQueueLength (no. of waiting ops per time period, total)
    - VolumeThroughputPercentange (for prov. IOPs only, % of IOPS delivered vs. total IOPS for that volume, percent)
    - VolumeConsumedReadWriteOps (for prov. IOPS only, no. of R/W ops consumed per time period, total)
    - Volume Status Checks 
        - ok: normal
        - warning: degraded, severely degraded
        - impaired: stalled, not available
        - insufficient-data         
#### ELB Monitoring
- CloudWatch metrics (perf. metrics)
- Access logs
    - can store data where the EC2 instance has been deleted
    - on NLB, only captures TLS, not HTTP
- Request tracing (application LB only) - monitoring HTTP requests from users to your application **TIP: ALB ONLY, NOT NLB**
- CloudTrail logs (api call logs)

#### Monitoring Elasticache
- **_Recommended metrics to monitor_**:
    - CPUUtilization
        - **TIP**: if Memcached, > 90%, add more nodes 
    - EngineCPUUtilization
    - SwapUsage
        - **TIP**: if Memcached, > 50Mb, + memcached_connections_overhead
    - Evictions
        - no recomended settings
        - **TIP**: if Memcached, can scale up OR out
        - **TIP**: if Redis, can ONLY scale out
    - CurrConnections
        - set alarm on no. of concurrent connections, if large, configure app, or could be spike in traffic        
- Host level metrics
    - CPUUtilization (% CPU utilization for entire host. Because Redis is single-threaded, we recommend you monitor EngineCPUUtilization metric for nodes with 4 or more vCPUs, %)
    - FreeableMemory (amount of free memory available on host. derived from RAM, buffers & cache that OS reports as freeable, bytes)
    - NetworkBytesIn (no. bytes host has read from the network, total).	
    - NetworkBytesOut (no. bytes s sent out on all network interfaces by the instance.	Bytes
    - NetworkPacketsIn (no. packets received on all network interfaces by instance. Identifies volume of incoming traffic - no. of packets on single instance, total)
    - NetworkPacketsOut (no. packets sent on all network interfaces by instance. Identifies volume of outgoting traffic - no. of packets on single instance, total)
- Redis specific metrics
    - ActiveDefragHits
    - BytesUsedForCache
    - CacheHits
    - CacheMisses
    - CurrConnections
    - EngineCPUUtilization
    - Evictions
    - NewConnections
    - Reclaimed
    - ReplicationBytes
    - ReplicationLag
    - SaveInProgress

### Multiple AWS Accounts
- Strategy for isolating applications and workloads
#### Organizations
- Allows central policy admin / billing etc.
- SCPs - service control policies
    - centrally control service access from a single policy to child accounts
    - Overrides the child account's IAM
    - Can be applied down to account level granularity
- Can help automation of new accounts
    - Can create new accounts via Organizations, or invite existing accounts
- Consolidated billing
- Organization divided into Organizational Units, (can have multiple layers) 
- SCPs are applied at an OU or Org level

### Tags & Resource Groups
- Tags are Key-Value pair metadata for AWS resources
    - Some can be inherited (where resources are provisioned via another service (ASG, Beanstalk, CFN)
    - IAM Policies can now use tags as a condition
    - Tag Editor can bulk-edit or bulk-apply tags to resources
- Resource groups are a way of grouping resources by tag or stack basis
    - e.g. all EC2 instance with tag "stack:MyApp"
- Systems Manager - useful to manage resources in RGs
    - e.g. take a snapshot of all EC2 in a group

### AWS Config
- AWS has built some standard rules that cover common compliance config and remediation stuff
- Evaluates the state of out AWS environment at any point in time
    - Configuration Items
        - Point in time attribute of a resource
    - Configuration Snapshots
        - Collection of Config Items at a particular time
    - Configuration Stream
        - Stream of changed Config Items
    - Configuration History
        - Collection of Config Items for a resource over time
        - shows all the changes on a per action basis
    - Configuration Recorder 
        - what records and stores config items
        - logs config for account (per region)
        - stored in s3
        - notifications in SNS
- Can perform remediation actions
- Checks can be triggered on schedule or event driven
- Can trigger notifications for real-time alerting

### Health Dashboards
- Service Health
    - Status of Services on a per region basis
- Personal Health
    - alerts and remediation guidance for AWS services that you are using

### Perform the steps necessary to remediate based on performance and availability metrics

## Deployment and Provisioning
### Identify and execute steps required to provision cloud resources
#### EC2
- Pricing
    - On demand - pay per fixed rate, no commitment, good for spiky that can't be interupted
    - Reserved - capcity reservation - 1 or 3 years, discounted, good for predictable workloads
    - Scheduled - capacity reservation - 1 year RI for recurring period of time
    - Spot - bid for instance capacity - cheap, but can be kicked off if price rises above bid, so needs flexible start/end times
    - Dedicated hosts - physical dedicated servers, where multi tenancy can be done. can be good for per server software, i.e. VMWare, or regulatory needs
    - Dedicated instances - single account runs on server
    - i3.metal - physical host, can run own OS, etc.
- Virtualization
    - VMs run on servers with Hypervisor (e.g. AWS Nitro) running on it
    - CPUs, RAM, Instance store
- Tenancy
    - Shared - multiple tenants use the server
    - Dedicated - part of server, but no one else can use other parts of server
    - Dedicated Host - whole server
- Placement Groups
    - No cost for creating PGs
    - Name of PG must be account level unique
    - Spread PGs
        - Can only have 7 running instances per AZ
    - Cluster PGs
        - Must be in same AZ
- Volumes
    - Root Volume - where OS is installed
    - Additional Volumes - data
        - Not deleted when root volume is deleted
    - Instance store vs. EBS
        - Instance store
            - High performance - physically attached to host computer
            - Max size 10GB
            - Terminate instance by default terminates this device (**can't** be disabled)
            - Instance stored backed instances can't be stopped
            - Reboot commands do not effect instance store backed EC2 instances
        - EBS root device 
            - Max size 1-2TB (OS dependant)
            - Terminate instance by default terminates this device (can be manually disabled)
            - can be stopped
        - ![EBS vs. Instance Store Backed Instances](http://secureservercdn.net/160.153.137.15/3d9.249.myftpupload.com/wp-content/uploads/2016/04/screen-shot-2016-04-06-at-6-36-02-am.png)
    - Upgrading EC2 Volume Types
- Can store copies of on-prem VMs in EC2 by launching instances and using VM import/export
- Instance unlimited
    - A burstable performance instance configured as unlimited can sustain high CPU performance for any period of time whenever required
- Deployment Steps
    - Pick AMI
    - Pick Type (Size, etc.)
    - Configure (LC/ASG, Spot?, User Data, Monitoring, VPC, Subnet, Placement Group, IAM role, etc.)
    - Add Storage
    - Setup Security Groups
    - Add SSH Key Pair
    - e.g.
        ```
        aws ec2 run-instances \
            --image-id ami-abc1234 \
            --count 1 \
            --instance-type m4.large \
            --key-name keypair \
            --user-data file://my_script.txt \
            --subnet-id subnet-abcd1234 \
            --security-group-ids sg-abcd1234
        ```
- Debugging web servers
    - Resolution - DIG
    - Response - NC
    - Routing

#### Lifecycle
[LifeCycle](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-lifecycle.html)
![EC2 Lifecycle](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/images/instance_lifecycle.png)
- Security Groups
    - default nothing inbound
    - everything outbound
    - can allow access based on source Secrurity Group

#### Deploy EBS Volumes
- Elastic Block Store
    - Storage volume attached to EC2
    - File system, DB or OS
    - SSD backed storage
        - gp2: 
            - General Purpose 
            - boot volume 
        - io1: 
            - Provisioned IOPs (PIOPS)
            - I/O intensive, NoSQL, Relational DBs, latency sensitive workloads 
            - 50 IOPS/GB to a max of 64,000 IOPS
    - IOPS - input/output operations per second, performance benchmark for SSD volumes
    - If you hit IOPS limit exceeded, IO requests start queuing, may slow down your app
        - increase size of volume
        - switch to io1 from gp2 if need more than 16K IOPS
- If volumes are impaired
    - perform consistency check
        - Stop applications using it
        - Enable I/O on the volume
        - (can also have auto-enabled I/O on the volume, which makes volume immediately available, even if impaired)
- Volumes exist on EBS
- Snapshots exist in S3 (not accessible directly)
- Snaps are point-in-time copies of volumes, incremental
- Stop instance before snapping a EBS volume that is the root device for EC2 (but can snap while running)
- can create AMI from snaps and images
- can change EBS volume sizes on the fly including changing size and storage type
- Instance and volumes **(EC2 and EBS) always in the same availability zone**
- can copy to another AZ or region:
    - take snap or AMI, and copy to new AZ/region

#### Elastic Load Balancers
- Can only direct load within a single region
- Targets
    - where traffic gets routed, EC2, lambda, IP address
- Listeners
    - check for incoming client traffic
- Rule
    - applied to listeners to ensure incoming traffic matches, and specifies the target group, condition, and priority
- Behind the scenes:
    - operates in separate ELB-managed VPC
    - Connects to our VPCs using ENIs
- Application Load Balancer 
    - Work at app layer (layer 7)
    - Can route traffic based on Application contents, http headers, etc.
- Network Load Balancer
    - Works at transport layer (layer 4)
    - High performance, low latency
    - Most expensive
- Classic Load Balancer
    - works at both layer 4 and 7, but layer 7 capability is limited (x-forwarded for and sticky sessions)
- Pre-warm ELBs
    - If sudden spikes in traffic expected, use ELB pre-warming (need to contact AWS)
    - Provide
        - start/end
        - expected traffic
        - typical request size
- Static IPs
    - ALBs scale automatically, but result is new IP address for connections
    - NLBs have a static IP in each subnet, so only one IP address needed for connections
    - To solve ALB issue, can place ALB behind NLB
    - **TIP:** If exam question asks for LB with static/elastic IP, need to use Network LB
- Cross-Zone routing
    - disabled by default, but turn on to ensure even load on servers
- ELB Errors
    - 4XX error - client
        - 400 bad/malformed request
        - 401 unauthorized user
        - 403 forbidden - blocked by WAF
        - 460 client connection closed - maybe not enough time configured for client timeout
        - 463 LB got x-forwarded for header w/ > 30 IP addresses - can't do this
    - 5XX error - server
        - 500 internal server error, could be at LB
        - 502 bad gateway - app closed connection, or response malformed
        - 503 service available - no registered targets
        - 504 gateway timeout - app not responsing, could be an web server issue, app or DB not working
        - 562 unauthorised - got error code from ID provider when trying to auth a user
- ELB Cloudwatch Metrics
    - ELBs publish to CW for LB and instances
    - Metrics gather at 60 sec intervals
    - BackendConnectionErrors - how many unsuccessful connections to ec2 instances
    - Healthy/UnHealthyHostCount - number of instances registered or marked as unhealthy
    - HTTPCode_Backend_2XX,3XX,4XX,5XX
    - Latency - how long for instance to respond/connect
    - RequestCount - how many transactions per period (1 or 5 min)
    - SurgeQueueLength - no. of pending req, queue size is 1024 - more than that, rejected (CLB only)
    - SpilloverCount - no. or rejected req due to full queue (CLB only)

#### Deploy an ALB
    - spin up ec2 instance(s)
    - create ALB 
    - configure SG to allow incoming traffic
    - Add listener on required ports
    - register instance(s) to group
    - access via created DNS for ALB
    - can see ELB specific metrics in CloudWatch

#### Bastion Hosts (Jumpbox)
- host located in public subnet that allows access to instances in private subnet (via SSH or RDP)
- allows access to instances without having to grant them internet access
- Carefully limit port access to reduce attack surface (if Bastion Host compromised)

#### LDAP
- Launching a second LDAP server that replicates the on-premises LDAP server in your VPC will give your applications on AWS the desired low latency while retaining low latency for your on-premises applications.

### Identify and remediate deployment issues
#### Potential EC2 Launch Issues
    - InstanceLimitExceeded error - too many instances in the region (soft limit is 20 instances per region, can request to get this raised)
    - InsufficientInstanceCapacity error - AWS doesn't have enough hardware to meet request (wait some time and try again, or request less instances, different instance type, or reserved instances, non specific AZ request)

#### Containers and Serverless
- Containers
    - Consistent environment, portable
    - Operational efficiency - easy to manage environments
    - Dev productivity - tooling is good, Dockerfiles are (basically) like scripts
    - Version control - deployment defined in text file - Dockerfile
    - Docker 'shares' the host OS and kernal, saves the container needing it
    - EKS and ECS exist to address the "large fleet of containers on large fleet of VMs" operational issue, DNS is too slow
        - Deploying, scheduling, scaling & management of containerised applications
    - Hosting
        - EC2 (control of underlying VM, instance types, etc.)
        - Fargate - serverless version of managing EC2 (mainly only m-type instances, GPU)
    - Image registry
        - repo for container images
    - Problems:
        - sprawl, versioning, ownershop, bin packing, zombie containers

- Serverless
    - Lambda
        - Functions as a Service (BYO Code)
        - uses micro-VMs (micro-kernal) technology not Containers
        - Charged per resource reserved (CPU and Memory are allocated proportionally)
    - API Gateway
        - Managed service for Web-Proxy
        - Concurrancy limit (6000)

- AWS Batch
    - Job Scheduler - AWS Managed

## High Availability
### Recognize and differentiate highly available and resilient environments on AWS
### Implement scalability and elasticity based on use case

#### Elasticity & Scalability
- EC2, S: + instance size E: + no. of instances
- DynamoDB, S: unlimited amount of storage E: +/- IOPS for spikey traffic
- RDS, S: + instance size E: not very elastic (can do it with Aurora Serverless)
#### Autoscaling
- Healthchecks are used to figure out the whether to do scaling events (i.e. terminate and create a new instance)
- Can use CloudWatch to trigger ASG events
- Schedules
- Programmatically
- Manually set limits
- Launch Configurations/Templates
    - Configs for ASG only
    - Templates for EC2 more generally
- Auto Scaling Groups
    - Scale between min, max and aim at desired (desired should be the long term nominal requirement)
- Set CloudWatch alarms for desired +/- some value or nominal + some value
- Scaling Policy
    - Determines when an ASG scaling event should occur
    - Can be triggered via:
        - CloudWatch metric
        - Manually
        - Instance health check
        - Scheduled
    - Need to take into consideration Boot-time-to-service-time (server start-up/healthy time) to ensure no. of instances can meet traffic demand
        - Baking own AMI with application installed reduces BTTST, but decreases flexibility in what is installed/configured
    - Network / Latency a better metric for scaling than CPU utilisation, better reflection of client experience
- Autoscaling Lifecycle
    - ![Autoscaling Lifecycle](https://docs.aws.amazon.com/autoscaling/ec2/userguide/images/auto_scaling_lifecycle.png)
- Testing scalability
    - TheGrinder
    - Apache Jmeter
    - Bees with Machine Guns
    - use bash 'stress' command to simulate CPU spike
- RDS
    - Use Multi-AZ to make HA
    - All RDS database engines support Multi-AZ database deployment.
    - Must enable automated backups of RDS before enabling Read Replicas
    - Using provisioned IOPS for RDS - 	100GB to 64TB size, 1000 to 64-80K IOPS
    - Can only create Read-Replicas if backups are enabled
    - Reboot with Failover, can take some time to update UI (and API) for the AZ
    - Automated backups can be optionally retained
        - up to 35 days.
    - Automated snapshots are deleted when the database is terminated by default.
    - Manual snapshots persist even after a database is terminated. 
        - No expiration period for manual snapshots.
    - Versions
        - To find RDS version, go to DB, check Engine parameter (included version)
            - can use **aws rds describe-db-instances --region** command to find DB metadata
    - Encrypting    
        - Using Snapshots
            - Take a snap
            - make copy, during copy - do encryption
            - restore the snap (is encrypted DB if snap is encrypted)
        - Can't encrypt from free-tier instance, need to do during copy, but can do for prod/dev/test instances
        - restoring from encrypted snap, can only create encrypted instance
    - Sharing Encrypted snaps
        - Create custom KMS encryption key
        - Create RDS snap using custom key
        - Share key with other account
        - Use Console, API or CLI to shate the encrypted snapshot with other account
        - Can't share encrypted snaps as public
        - Can't share Oracle or MS SQL Server snaps encrypted using TDE
        - Can't share encrypted snaps encrypted with default KMS key (needs to be custom)
- Maintenance Windows (which services have one)
    - RDS
        - If not specified, a 30 min window for RDS in that region is selected, and on a randomly selected day of the week
    - Elasticache
    - Redshift
    - DynamoDB DAX
    - Neptune
    - Amazon DocumentDB
    - (Storage Garteway)
- Elasticache
    - Web service, in-memory cache in the cloud
    - used to cache commonly retrieved info from data-store
    - improve latency for read-heavy workloads
    - Memcached
        - No multi AZ
        - simple solution
    - Redis
        - supports Multi AZ 
        - Use Cases:
            - session management
            - gaming
            - leaderboards
            - real-time analytics
            - geospatial
            - chat/messaging
            - media streaming
            - pub/sub apps
    - See Monitoring section for which metrics to monitor
    - Use Redshift instead if OLAP traffic is heavy
- Aurora
    - MySQL and Postgres compatible
    - Encryption at rest by default
    - Cross region replicas - new cluster in replica region
    - Stores 6 copies of data across 3 AZs (2 per each AZ)
    - starts with 10GB and scales out by 10GB blocks up to 64TB
    - Compute scales to 64vCPUs and 488 GiB
    - Storage is self-healing
        - data blocks continuosly scanned and fixed (if needed)
    ![Amazon Aurora DB Clusters](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/images/AuroraArch001.png)
    - Can have up to 15 read replicas
    - If CPU maxed out
        - Scale Up (increase instance size)
    - if reads number too high
        - Scale out (add read replicas)
    - Failover of primary instance:
    - If you have replicas, flips CNAME record to the replica
    - if no replica attempts new DB in same AZ
    - then attempts to create new DB in different AZ
    - Aurora Serverless
        - pay per request
    - Describe using **aws rds describe-db-clusters**
    - Read replicas have failover priority, Tier 0 > teir 1 ... > tier 15
- Troubleshooting Autoscaling Groups
    - Key pair absent
    - Security Group absent
    - ASG config not correct
    - ASG not fount
    - instance type not supported
    - AZ no longer supported (this would be very rare!)
    - EBS volume mapping invalid
    - ASG service not enabled in account
    - attempting to attach EBS block device to instance-store AMI
- Cloud Front and Cache Hit Ratios
    - Cloudfront is AWS's CDN, distibuted server network that serves cached content based on fastest connection (content cached at 'Edge Locations') - reduces load on orgin server, and reduces latency for end user for cached content
    - Origin is original content
    - Distribution is name given to the particular CDN
    - Cache Hit Ratio is ratio of requests served from Edge Location vs. Origin
        - Higher the better
        - Can view it in CloudFront
        - **Maximise** it by:
            - Set TTL higher and Cache-Control max-age to ensure cached for as long as possible
            - Cache based on Query String
            - Cache based on Cookies
            - Cache based on Request Headers
            - Remove Accept-Encoding Header when compression not needed
            - Serve media content using HTTP

#### Route 53
- DNS
    - To use your own DNS server
        - configure a DHCP option set with DNS Servers 
        - replace the DHCP option set in the VPC with the new option set
- DNS Resoltion occurs at edge locations
- DNS Record Types:
    -A (address record) - resolves IPV4 address to domain name
    -AAAA (IPv6 address record)
    -CNAME (canonical name record)
    -CAA (certification authority authorization)
    -MX (mail exchange record)
    -NAPTR (name authority pointer record)
    -NS (name server record)
    -PTR (pointer record) (reverse DNS, resolves IP to FQDN)
    -SOA (start of authority record)
    -SPF (sender policy framework)
    -SRV (service locator)
    -TXT (text record)

- Routing Policies available:
    - Simple
        - Default - no complex smarts, just send requests to a endpoint
    - Weighted
        - splits traffic across multiple endpoints (e.g. 20% US-East-1, 80% US-West-1
    - Latency
        - Routes traffic based on end-user netowrk latency 
        - Can use service like cloudping.info
    - Failover
        - For Active/Passive setup, monitors health of site. sends traffic to passive site if active fails (is down).
    - Geolocation
        - Route traffic based on the user location (IP Address)
    - Geoproximity
        - Based on distance to location
    - Multivalue

## Storage and Data Management
### Create and manage data retention
- EBS - Block store, hard drives, attach to VMS
    - Network attached
    - Tied to Availability Zones (replicated within AZ)
    - Stop disk writes before snapshot
- EFS - File System, share across EC2 instances
    - Elastic
    - can be shared across systems
    - NFS based
- FSx - Windows File Server
- S3 
    - object based, KEY-VALUE store (key object name, value data, version id, metadata, sub-recs - ACLs - Torrent)
        - Files, etc.
        - Not suitable for OS installs (use EBS instead)
    - 0B - 5TB
    - unlimited storage
    - stored in buckets - basically a cloud based folder
    - Each account can have up to 100 buckets by default
    - S3 names are globally unique, urls are: https://s3-<region>.amazonaws.com/<bucketName>
    - interaction is API based, normal http response codes
    - Can get faster uploads by enabling multi-part upload
        - Also needed if file size > 5GB (max single PUT operation upload size)
    - new objects = read after write consistent
    - old objects = eventual consistent
    - If data is changing rapidly, consider EFS instead, for better performance
    - S3 Standard: SLA 99.9 available, 99.999999999 durable
    - S3 Infrequently Accessed: slightly less cost
    - S3 One zone IA - lower cost, not replicated across AZs
    - Glacier 
        - Expidited (1-10 min, etc.), Standard, Bulk. Standard tier takes about 3-5 hours
        - Encrypted at Rest by default
        - Can't upload to Glacier via console directly, can via CLI or SDK, or via LifeCycle Policy
    - versioning
        - pay for each version
        - each new version needs to be made public individually
        - deleting a versioned object will create a following version which is the delete marker
            - this can be used to undelete th object, by restoring the version marker to the previous version
        - can enable MFA delete, extra delete security
    - MFA Delete
        - Force users to use MFA to delete an object
        - Force users to use MFA to change bucket versioning on bucket
    - Access Control Lists => regulate access to individual objects
    - Bucket Policies => way of regulating access to entire buckets
        - Policies written in JSON
        - Can be used to make entire buckets public
        - Statement
            Sid - Statement Id
            Effect - Allow or Deny
            Principle - Who/what is attempting access
            Action - What actions are being effected (i.e. denied or allowed)
            Resource - What is being accessed (ARN - can be wildcard)
            Condition - Can be used to specifically block/allow based on conditions such as source IP address
        - **Exam TIP: Explicit Deny statements overrides Explicit Allow statements**
    - IAM - used to restrict user/role/group access to S3 (or other services/resources for that matter)
    - Default all buckets and objects are private
    - Successful upload gets HTTP 200 response
    - Server Access Logging - Detailed logging of access (i.e. requester, bucket name, request time, action, status, error codes) can be obtained using S3 Server Access Logging 
    - 3 types: S3, S3-IA and S3-resource
    - Can be encrypted - either client or server side (own or managed)
        - In transit (encrypting data to/from buckets)
            - SSL/TLS
        - At Rest (encrytping the stored data)
            - Server Side
                - S3 Managed Keys - __SSE S3__ (AES256, managed by AWS)
                - AWS KMS - __SSE-KMS__ (envelope key, audit trail when keys used and by who, and can manage own keys)
                - Cutomer Provided Keys - __SSE-C__ - Keys managed by customer
                - need to set x-amz-server-side-encryption header to AES256 or ams:kms for SSE-S3 or SSE-KMS, respectively
                    - use Bucket Policy to deny requests without encryption header
            - Client Side
                - Encrypt data on client side before uploading, decrypt client side after downloading
                - Uses a Client-Side Master Key (never sent to AWS) so only encrypted data sent
    - Cross Region Replication
        - Must be in a different region
        - Cross Region replication requires versioning to be enbabled on both source and destination buckets
        - Existing files will not be automatically replicated, need to be manually copied in via cli
        - New files and changes replicated automatically
        - Deleting an object (object marker) or object version in primary will not be automatically replicated into the replication bucket
    - LifeCycle Rules
        - To be used in conjunction with versioning
        - can have rules for current and previous versions
        - can use it to delete permanently as well
        - transition to standard IA class, Glacier, or delete
        - archive
    - Glue - used to perform Extract, Transform and Load (ETL) operations on S3 data
    - CloudFront
        - is a Content Delivery Network (CDN)
        - requests for any (not just static) content delivered to Edge Locations with lowest latency
        - __Edge location__ is a location where content will be cached. Different than Region or AZ
        - Origin is where the files will come from - S3, EC2, ELB or Route 53, or non-AWS source
            - Can configure multople origins, the second one will be used if the primary returns and error
        - Distribution is the collection of edge locations
        - Can actually PUT to edge locations, not just Read Only
        - Files chached for the Time-To-Live value (TTL), but can be invalidated with an account change
        - Should be secured using pre-signed URLs or cookies
        - Can force users to not be able to access content from origin directly
        - **exam tip** Common Errors:
            - 400 - bad request
            - 403 - object in s3 must be publicly accessible
            - 404 - object doesn't exist
            - 502 - cloudfront can't connect ot origin
            - 503 - EC2 server perf issue
            - 504 - EC2 server perf issue 
            - 5XX normall due to high traffic
    - Can be configured to create acccess logs
    - Snowball -  Physical disk connected to DC, files uploaded, then sent to AWS to be uploaded on network internally
        - replaced Import Export, different disks sent to AWS, difficult to manage
        - Encrypted
        - Types
            - Snowball - just basic storage
            - Edge - has compute capabilites on it, i.e. can run Lambdas on uploaded data
            - Snowmobile - Truck version of Snowball, Exabyte scale data transfer 100PB per truck
    - Transfer Acceleration
        - Upload files to an edge location, makes uploads faster when latency would be otherwise higher
        - Optimised over Amazone backbone technology
        - Cost extra
    - Static website hosting
        - Serverless site hosting
        - Endpoint format - <BucketName>.s3-website-<Region>.amazonaws.com
        - Scales automatically
    - File restrictions
        - Pre-signed URLs to restrict individual files
            - Uses SDKs primarily
            - Create via CLI:
                - Create role for access to S3
                - Attach to EC2 instance, sign into instance
                - use **aws s3 presign s3://<BucketName>/<filename>** to pre-sign
                    - default 60 mins (or use --expires value (seconds))
                - returns the pre-signed URL (can access private objects via this URL)
        - Signed Cookies to restrict multiple files
- Glacier - Archival S3
- AWS Storage Gateway 
    - Storage Gateway - connect on-prem DC to AWS Data Store (S3 or Glacier). VM installed in customer DC
        - Used for secondary purposes (normally)
        - Encrypted by default
        - File Gateway (NFS) - flat files
        - Volume Gateways (iSCSI) - block storage - incremental volume backups
            - Stored Volumes -  has point-in-time snapshots of on-prem local data, stored in S3. Asynch backups
            - Cached Voumes - store data in s3 and keep frequently accessed data locally in cache
        - Tape Gateway (VTL) - Archive data
- DataSync - managed service to sync data between on-prem and AWS

### Identify and implement data protection, encryption, and capacity planning needs
- Encryption:
    - for the following, can only be done at creation time:
        - EFS
        - EBS
        - RDS
- NoSQL vs SQL
    - NoSQL came about largely because of scale requirements
KMS & CloudHSM
- Place to generate, store and manage cryptographic keys for AWS
- HSMs (Hardware security modules) protect confidentiatily of keys
- very secure
- KMS:
    - **Shared hardware**
    - multi tentant
    - Free tier
    - ebs, s3, rds, dynamodb
    - Symmetric (same for encr and decr)
- CloudHSM
    - **dedicated instances** (not shared with other tenants)
    - no free tier
    - FIPS 140-2 lvl 3 compliance
    - useful for strong regulatory reqs (where dedicated h/w needed)
    - Asymmetric (can be different or same for encr and decr)
#### Relational Database Services
- Pick instance class: CPU, Memory and Network Performance
- Pick instance storage: Magnetic, GPU (SSD) or Provisioned IOPS
- Six different types of DB engines
    - Sql server, oracle, mysql, postgresql, aurora, mariadb
    - Multi AZ - DR
    - Read Replica - Performance
    - OLTP
- Automated backups
- RDS and Multi-AZ Failover
    - Multi-AZ is for DR, not for performance, exact copy of DB in another AZ, fails over (automatically) to that DB if the primary isn't usable
    - takes about 1 minute to fail over RDS
    - backups/restored taken from secondary, no I/O suspensions of primary
    - NOT a scaling solution
- RDS Read Replicas
    - read-only copy of DB to channel read-throughput traffic away from primary DB
    - really good for business reporting (BI) queries rather than primary DB
    - supported: (native async) MySQL, PosgreSQL, MariaDB, (SSD backed virtualised storage layer) Aurora
- Regional DR
    - DR for RDS should be to create Multi-AZ read replica in a separate region. For regional outage, promote to primary and update DNS to new primary endpoint
#### Aurora
- In-house build Relational Database
- You can create custom endpoints (load balanced) to handle criteria other and RO or RW
- Failover of primary instance:
    - If you have replicas, flips CNAME record to the replica
    - if no replica attempts new DB in same AZ
    - then attempts to create new DB in different AZ
- Primary/Replica pattern
- Aurora Serverless
    - Pay per request
#### DynamoDB
- Managed NoSQL database
- Supports:
    - Scalar Types (Number, Strings, Boolean, and Binary)
    - Document Types (lists and maps)
    - Set Types (String Set, Number Set, and Binary Set)
- Provide TPS and AWS scales out for you
- Fine-grained control to data
#### AWS Database Migration Service
- Mainly used when DB size if > 5TB
- Easy to set up -> see schema conversion tool for how to convert current to AWS
#### Athena
    - interactive service (serverless) to query S# using SQL
    - Pay per query or TB scanned
    - use for querying logs, clickstream data, etc.
    - can generate reports
## Security and Compliance
### Artifact
- AWS Artifact is service where compliance documentation for the relevant industries
- Compliance Frameworks
    - https://aws.amazon.com/compliance
    - PCI Secrity Standards Council
        - PCI DSS - most important for anything that takes CC payments
    - ISO
        - ISO/IEC 27001:2005 requirements for a Information Security Management System
    - HIPAA
        - Health Insurance (USA) requirements
    - NIST
        - cybersecurity risk framework (USA)
### Implement and manage security policies on AWS
- Certificate Manager
    - Can create and deploy certificates from this service
- DDoS
    - Distributed Denial of Service Attacks
    - Amplification/Reflection attacks
        - NTP Amplification - spoof source IP in request (victim's address) and this will make server send amplified NTP response to victim
        - Application Attack - send heaps of Get requests to a web server
        - Sloloris - send connection requests to server, try to hold them open as long as possible
    - Minimise attack surface area - use ALBs with WAF
    - WAF
        - Monitor HTTP/HTTPS requests
        - Cloudfront, ALB or API Gateway
        - **NOT** CLB or NLB
        - Anything in the http request, headers, body, ect.
        - Can: whitelist, blacklist, or count
        - 
    - Scale to absorb - Use ASGs 
    - Learn normal behaviour, plan for abnormal behaviour
    - Whitepaper: https://d1.awsstatic.com/whitepapers/Security/DDoS_White_Paper.pdf
- AWS Shield
    - Below layer 4
    - Protects against SYN/UDP Floods, Reflection Attacks, and other layer 3/4 attacks
    - DoS attack protection
    - Protects on ELB, CloudFront and Route 53 for all AWS customers
    - 
- Shield+ 
    - Dedicated 24/7 team of engineers who help fight DDoS attacks
- HSM 
    - Manage Keys
- KMS
    - Symmetric Keys only
    - Manage Keys (multi-tenanted)
    - Features:
        Create Keys
        Import Keys
        Use IAM to manage access to keys
        Rotate your keys
        Disable and re-enable keys
        Delete keys
        View use of keys in Cloudtrail
        Use custom Key Stores (The use of custom key stores requires CloudHSM resources to be available in your account.)
- Guard Duty
    - Person looking at logs (CloudTrail, VPC Flow, DNS)
    - Looks for suspicious activity
    - Subscription service (pay per month per million events, or per GB of data)
    - VPC Flow Log and DNS Log Analysis	
        - First 500 GB / month	$1.15 per GB
        - Next 2000 GB / month	$0.58 per GB
        - Over 2500 GB / month	$0.29 per GB
    - AWS CloudTrail Event Analysis	
        - Per 1 million events / month	$4.60 per 1 million events
- Macie
    - Scans objects in S3, finds PID or CC info and assesses risk
- Inspector 
    - Agent runs on instances to evaluate bad configurations
    - Steps to install and use:
        - Tag instance(s) with the assessment target tag(s)
        - download (wget/curl) and install (sudo bash install) the agent
        - Define the assessment target
            - select the tag(s) key(s)/value(s) to assess
        - Define the assessment template:
            - Name
            - Rules package (what kind of tests to run)
            - duration (longer is going to produce more complete results)
        - Run the assessment
        - Review the report
- AWS Marketplace - Security Products
    - Can purchase heaps of different services
    - Penetration testing servers - Kali Linux
    - Need to request authorization for penetration testing to occur
    - Just because we've used a product from Marketplace, still need to ask permission to do pen testing to/from AWS
- Hypervisors (or Virtual Machine Montitor)
    - Software/Firmware/Hardware that creates and runs VMs.
    - Hypervisors run on "host" machines
    - VMs are called "guest" machines
    - Xen - EC2 runs on this (c5 on KVMs)
    - Hardware Virutual Machine, fully virtualised, VMs on top of hypervisors not aware of multi-tenancy with other VMs
    - Paravirtualisation, lighter version (used to be quicker, but not so much anymore)
        - Choose HVM over PV
    - Only AWS have access to Hypervisors
    - Windows EC2 can only be HVM, Linux can be both
    - Hypervisor "scrubs" memory/RAM before it's available to the customer

- IAM
    - Credentials
        - email/Password - root account access
        - Username/Password - Console access
        - Secret Key/Access Key - Programatic Access
        - SSH Key - Service Access
        - MFA access
    - User: any entity that can have access to the system
    - Role: **Temporary** credentials used to allow access to system
        - for federated access
        - access via saml or oauth (SSO)
    - Group: Collection of users, makes managing large collection easier
    - Policies: JSON Document which defines the scope of access to the system which the entity/role has
        - Order:
            - collect policy rules for entity 
            - is there an explicit deny? if so, not authorised (http 403 error)
            - is there an explicit allow? if so, authorised, otherwised not denied
        - AWS Managed - aws created policies, commonly used permissions
        - Job function - based on what job function a user performs
        - Customer managed - custome policies that allows uncommon access
        - Identity based: attatched to IAM identities (users, groups, roles)
        - Resource based: attache to resources
    - If account compromised, detatch all policies from U/R/G 
    - Authorisation
        - Changes to policies is **immediately** applied to entities
    - IAM entity names must be alpha-numeric, plus these characters: + = , . @ _ -
    - MFA & Reporting
        - Can set up MFA for individual users
            - CLI: **aws iam create-virtual-mfa-device**, **aws iam enable-mfa-device** commands
        - Can get report in console under IAM "Credential Report" - field that shows this status for each user is **mfa_active**
        - Can enforce the use of MFA
    - Security Token Service
        - Federated (normally AD)
            - SSO - SAML-based authentication
            - Can sign into AWS using AD credentials (no need to create new user)
            ![Federated AWS Access](https://docs.aws.amazon.com/IAM/latest/UserGuide/images/saml-based-federation.diagram.png)
        - Federated with Mobile
            - Web authentication (Google, Facebook, etc.)
        - Cross Account access
            - Users from one account accessing resources in another
### Implement access controls when using AWS
### Differentiate between the roles and responsibility within the shared responsibility model
![Shared Responsibility Model](https://d1.awsstatic.com/security-center/Shared_Responsibility_Model_V2.59d1eccec334b366627e9295b304202faf7b899b.jpg)
- Who is responsible for what when something happens
- AWS (Security of the Cloud)
    - Physical - Data Centres
    - Network - Custom networking tech, etc. - (see [Day in the Life of a Billion Packets](https://www.youtube.com/results?search_query=A+Day+in+the+Life+of+a+Billion+Packets))
                   
                                        EC2 (i3.metal)
    - Hypervisor
- Customer (Security in the Cloud)
                   
                                        EC2
    - Guest OS
                   
                                        Lambda
    - Application
                   
                                        S3, RDS
    - Data
- Infrastructure Services (EC2, EBS, Auto-scaling, VPC)
    - Can apply security patches
- Container Services (RDS, EMR, Beanstalk)
    - AWS applies security patches, but customer needs to apply measures
- Abstracted Services
    - Customer applies access control only
- Vulnerability scanning and pen testing
    - Need to let AWS know

## Networking
- Mainly working Layer 4 and above
- IP Addressing
    - To send, need to know destination IP, routing IP prefix, broadcast address, and subnect mask
    - this is defined by CIDR e.g. 192.168.1.0/24
        - Fixed/Flexible address parts. /24 means first 24 bits of address fixed, last 8 are flexible (2^8 addresses available)
- Amazon VPC
    - created inside region, spans AZs
    - Router is included in the VPC creation, for internal communication
        - Represented by a Route Table
    - we create subnets -> traditionally create Public and Private subnets in each AZ
        - 5 IPs are reserved
    - internet access created by adding a router gateway device (Internet Gateway - IGW)
        - Add route in Route Table to the internet, associate this with public subnets
    - All instances are assigned a private IP address
        - instances in a public subnet will have a public IP and private IP
    - To connect two locations within AWS, use a VPN (encrypted internet tunnel)
        - To connect to customer data centre, use Customer Gateway (CGW) on DC side, Virtual Private Gateway (VGW) on VPC side
    - To connect two VPCs, can use VPC peering, still need to add entry into route table in each VPC to route traffic
    - NACLs 
        - default NACL allows all inbound and outbound ipv4 and ipv6 traffic
        - custom NACL denies all traffic by default, allows ipv4 by rule and ipv6 by rule (if VPC is configured with ipv6 cidr)
        - ordered rules, allows all by default, need to add explicit deny all after all the allow rules to make a whitelist
        - default NACL allows all traffic by default
        - custom NACLs denay all traffic by default
    - Security Groups 
        - Changes to SGs are applied immediately
        - Can have multiple SGs per instance
        - all inbound traffic blocked by default - all outbound allowed
        - SGs are **stateful**, allowed inbound traffic is allowed back out, without needing to specify the outbound rule (ACLs work Stateless)
        - Can't block specific traffic with SGs, only allow
    - Use Bastion Hosts (jumpbox) to ensure secure connections to instances
    - NAT instance (or prefer NAT gateway)
        - allows safe connections for instances in private subnet to the internet without exposing them to the incoming traffic
        - require disabled Source/Destination check
        - must be in public subnet
        - create route out from private subnet to the NAT
        - Bottleneck traffic at NAT? Throughput limited by instance size, so increase type/size
    - NAT Gateway
        - Managed device
        - Scales to 45Gbps (was 10Gbps)
        - has public IP address automatically
        - more secure (can't SSH, etc, patched automatically)
    - VPC endpoints
        - enables private connection from VPC to supported AWS services (no internet or DX required)
        - interface endpoints
            - ENI with private IP address
            - serves as entry point to supported AWS service
            - Can secure enpoint using Security group
        - gateway endpoints
            - gateway used as a target to supported AWS service (for use in Route Table)
            - S3, DynamoDB only
            - difficult to secure
    - No Transative peering - can't connect from one VPC to another via a middle one, need direct peering between each VPC that need to connect
    - No Edge to Edge routing, can't use resources from VPC-A in VPC-B, even when peered, need direct connection from VPC-B to resources
    - Debugging
        - Is it instance issue?
        - Is there a NACL issue
        - View VPC Flow Logs
            - Can use to inspect traffic to/from a NLB
    - VPC Flow logs
        - Send logs to CLoudwatch, or S3 bucket. 
        - Can stream logs to ElasticSearch or Lambda
        - can only enable flow logs for peered VPCs if they're in the same account
        - tags not available for Flow Logs
        - Can't change flow log configuration after creation
    - CIDR Calculations
        - Need to be able to calculate number of addresses in CIDR for /XX ranges
        - /24 - 256, etc.
        - /28 is smallest = 16, -5 reserved, only 11 ip addresses availble.
        - /16 is largest
    - Direct Connect
        - Connection between AWS region/VPC and customer private network.
        - To connect each region/VPC together, use the Direct Connect gateway
        - All over AWS's backbone network, much faster, not over internet
    
    - Cornell Standard VPC 1.0 (VPN)
    ![Cornell Standard VPC 1.0](https://blogs.cornell.edu/cloudification/files/2016/04/Typical-Cornell-VPC-Configuration-1f8gtsn.png)
    - Cornell Standard VPC 2.0 (Direct Connect)
    ![Cornell Standard VPC 2.0 (Direct Connect)](https://blogs.cornell.edu/cloudification/files/2017/05/Cornell-Standard-VPC-Configuration-version-y4gnnx.png)
    - A Cloud Guru VPC overview
    ![ Cloud Guru VPC overview](http://neonta.com/wp-content/uploads/2017/01/vpc-diagram.png)

    ### Steps to provision create a custom VPC 
    (ClickOps but...)
    - Select region
    - Go to ‘VPC’ Dashboard
    - Click ‘Create VPC’
        - Give it a name
        - Pick the cidr block (use cidr.xyz to check values) - largest is 10.0.0.0/16
        - Use "Amazon provided IPv6 CIDR block and Default Tenancy
        - Once created, along with the VPC, this creates the following by default:
            - Route Table
            - NACL
            - Security Group
    - Create subnet(s), we want 1 private and 1 public
        - CIDR block is sub of CIDR for VPC - e.g. 10.0.1.0/24 and 10.0.2.0/24
        - Give name that references AZ for readability
        - Make one subnet public (i.e. 10.0.1.0)
            - Select subnet - Actions - “Modify auto-assign IP settings”
            - Check “Enable auto-assign public IPv4 address” - Save
    - Add an internet gateway - IGW
        - Click on “internet gateways”
        - Click “Create Internet Gateway”
        - Provide name
        - Click “Create”
        - Select IGW - click Actions - click Attach to VPC
        - Select the VPC, click attach
    - Configure main Route
        - Click on “Route Tables”
        - Click on “Create Route Table”
        - Give name (publicRoute, etc.) and select VPC
        - Create Route out to internet
            - Click “Edit Routes”
            - Click “Add Route” - 0.0.0.0/0 - select the internet gateway
            - Click “Add Route” - ::/0 - select the internet gateway
            - Click “Save Routes”
        - Click on subnet associations tab - Edit subnet associations
        - Add public subnet (10.0.1.0) to route table
    - Create EC2 instance in public subnet
        - Click on Ec2 Service
        - Launch instance
        - Select ami
        - Select VPC
        - Select Public subnet
        - Give name 
        - Add WebDMZ security group, SSH and HTTP access
    - Create EC2 instance in private subnet
        - Click on Ec2 Service
        - Launch instance
        - Select ami
        - Select VPC
        - Select private subnet
        - Give name 
        - Add default security group
    - Create private security group
        - “Security Groups” under EC2
        - Click “Create Security Group”
        - Add the following Inbound
            - All ICMP - Source 10.0.1.0/24
            - Http  - Source 10.0.1.0/24
            - Https - Source 10.0.1.0/24
            - MySQL/Aurora  - Source 10.0.1.0/24 (or whatever service you need to access)
            - SSH - Source 10.0.1.0/24
        - Go to private EC2 instance - change the security group to the new Private SG (Actions, Networking, Change Security Groups)
    - Add NAT instance (should use NAT Gateway instead)
        - Launch EC2 instance, Community AMIs, Search “nat”, select 1st instance in list, use t2.micro
        - Launch in public subnet in custom VPC
        - Add to WebDMZ SG
        - Select “Make General Purpose (SSD) the boot volume for this instance.”
        - Launch
        - Disable Source/Destination checks
            - Select instance, actions, networking, change Source/Dest. Check
            - Select “Yes, Disable”
        - Create Route from private EC2 instance to NAT Instance
            - Click on VPC Dashboard, Route Tables, Select the main route
            - Go to Routes tab, edit routes, add route
            - Add internet traffic 0.0.0.0/0 targeting the NAT instance (Instance, select the NAT instance)
            - Private instance can now access the internet
        - Terminate the instance
            - Private instance can NOT access the internet
        - Remove the route on the main route table (should now be a blackhole status)
    - Add NAT Gateway
        - VPC Dashboard - NAT Gateways - Create NAT Gateway
        - Select public subnet
        - Create new Elastic IP address
        - Click “Create a NAT Gateway”
        - Click “Edit Route Table”
        - Select Main route table - Routes tab - Edit routes - Add route - 0.0.0.0/0, target NAT Gateway -> select the NAT Gateway - Save, Close
            - Once NAT GW available, Private instance can access the internet
    - Configure NACLs (evaluated before SGs)
        - VPC Dashboard - Network ACLs
        - Create network ACL - give name and assign to VPC
        - Edit subnet associations, assign to public subnet - web content not accessible
        - Select the new NACL, edit inbound rules, add rules for port 80, 443, 22 on all sources (0.0.0.0/0)
        - Select the new NACL, edit outbound rules, add rules for port 80, 443 and 1024 - 65535 (ephemeral ports) on all sources (0.0.0.0/0)

    
#### Global Infra
- No. edge locations > no. of AZ > no. of Regions

### Apply AWS networking features

### Implement connectivity services of AWS
### Gather and interpret relevant information for network troubleshooting

## Automation and Optimization
- Use RI for base workload and On-Demand for variable workload
- Use TrustedAdviser to check real-time optimization guidance (security, cost, etc.)
- AWS Budgets for custom budget reports and alerts
### Use AWS services and features to manage and assess resource utilization
#### CLI
- use _aws configure_ to set up
- Secret Access Key/Access Key, Region and default output format
- use _--filter_ to filter the command for elements with particular value
- use _--query_ to limit results
- use _--dry-run_ to perform non-committal execution (see expected output)
#### Systems Manager (SSM)
- Replaces traditional management tools, e.g. Chef, Puppet, Ansible
- tool to manage all infra
- integrates with CW dashboards to view data and detect issues
- Can logically group resources
- has RunCommand which allows centralised running of ops tasks (pre-defined commands) on resources
    - stop/start/terminate/re-size ec2
    - attach/detatch EBS volumes
    - create snaps/ backup DynamoDB
    - apply patches
    - run an Ansible playbook
    - run a shell script
- can use it to manage on-prem instances (it's agent based)
- Session Manager
    - allow instance access via the console - browser (feature of SSM)
    - SSH or RDP
- Parameter store 
    - Collection of account data
    - shouldn't store secret info, instead use reference to Secrets Manager entry
    - can store confidential info such as passwords/connection strings/licence codes
    - can store as plain text or encrypted
    - referred to by name
    - pass to EC2, CloudFormation, Lambda, Ec2 Run Command etc.
#### Configuration Management
- Template for the root volume - OS, Apps
- Launch permissions, public, private, specific
- Block device mappings, what volumes should be attached at launch time
- AMIs
- User Data
- Deployment Frameworks
- OpsWorks
- Cloudformation
#### AMIs
- If AMI is created programatically, need to register before it can be used
    - registered on regional basis
    - can share/copy an AMI to another AMI or account
        - Need to allow Read access to storage of AMI to allow sharing
        - If encrypted, need to share encryption key and snap for the AMI
        - If there's a __billingProducts__ code, can't directly copy the AMI
- Foundational components
- Company-wide tools/standards - e.g. domain join and Active Directory
- Create
    - Launch instance
    - Install apps
    - create AMI from this
- Can create AMI from EBS snapshot (Linux only)
- What should we configure
    - Agents
    - Logging/Monitoring/Security (can be dynamic at launch - Infra team)
    - Application Runtime (Frameworks, Apache, Tomcat, J2EE) (can be dynamic at launch, Hybrid AMI - App team)
    - Application (can be dynamic at launch, Hybrid AMI - developers)
- Use "Sysprep" to remove instance details for Windows AMIs
- Region specific
    - Can copy to another region
- Can share with another account

#### Cloudformation
- Infrastructure as Code
- JSON or YAML
- Declaritive language (resources, not functions)
- Engine determines order based on dependicies
- Can take in parameters, and output values based on deployed resources
- template creation of resources as a "single" unit
- has Drift Detection - shows differences from the template
- deployed resources from template is a Stack
    - changes required can be previewed
- Can operate on collection of resources (Stack)
- Stack Sets are useful for standard stacks that should be deployed across many accounts
- Sections:
    - **Parameters** Input parameters to be used by the template during deployments, e.g. environment, static IPs, application version, etc.
    - **Mappings** Dictionary to map to important stuff like latest AMI IDs or instances, etc. (use Fn::FindInMap)
    - **Resources** the resources to deploy
    - **Conditions** conditionally create resources, i.e. based on environment
    - **Init** use this to deploy some stuff on EC2 instances during CFN deployments
    - **WaitCondition** Conditions that define when the resource actually is created fully
    - **Outputs** values of outputs specified by the script, normally for alerting and for other resources to consume
- Can use Macros to speed up CFN template creation for common resources, etc.
- Can use CloudFormation Development Kit to use commong programming languages (TypeScript, JavaScript, and Python, preview support for C#/.NET, & Java) to "code" CloudFormation templates
- AWS Architecture Center has CFN templates for common architectures
- Template Designer shows visual editor of the template
- Has "Drift Detect" support to show differences in resources vs. stack
- User --on-failure DO_NOTHING to keep resources (and logs) for investigation

#### ElasticBeanstalk
- Service to deploy and scale web applications / web servers
- Just upload code, it does deployments, load-balancing, and auto-scaling
- service is free, but still pay for provisioned resources

#### OpsWorks
- Managed instances of Chef or Puppet
- Coded in Ruby

#### Elastic MapReduce (Amazon EMR)
- managed cluster/platform for big data analysis
- Apache Hadoop, etc.
- Persistent 
    - stays alice after job complete
- Transient 
    - is shut down automatically after job complete

#### SQS
- Max SQS long pooling timeout is 20 seconds


#### Service Catalog
- Catalog of approved products (products defined by CloudFormation)
- Portfolio is collection of products, could be entire app stack
- Control access by IAM
- Products can be 'ordered' via a portal
- Can share portfolios with other accounts or Organizations node

#### SDK
- Heaps of language support
#### Powershell
- runs on all OSs
#### OpsWorks
- Fully managed service for Chef/Puppet
- Has AWS proprietary OpsWorks Stacks - own version of Chef - layered architecture
### Employ cost-optimization strategies for efficient resource utilization
### Automate manual or repeatable process to minimize management overhead

