# AWS SysOps Associate

- [Monitoring and Reporting](#monitoring-and-reporting)
  * [Create and maintain metrics and alarms utilizing AWS monitoring services](#create-and-maintain-metrics-and-alarms-utilizing-aws-monitoring-services)
    + [CloudWatch](#cloudwatch)
    + [Cloudwatch vs. CloudTrail](#cloudwatch-vs-cloudtrail)
  * [Recognize and differentiate performance and availability metrics](#recognize-and-differentiate-performance-and-availability-metrics)
    + [EC2 Metrics](#ec2-metrics)
    + [EBS Metrics](#ebs-metrics)
    + [ELB Monitoring](#elb-monitoring)
    + [Monitoring Elasticache](#monitoring-elasticache)
  * [Organizations](#organizations)
  * [Tags & Resource Groups](#tags---resource-groups)
  * [EC2 Pricing](#ec2-pricing)
  * [AWS Config](#aws-config)
  * [Perform the steps necessary to remediate based on performance and availability metrics](#perform-the-steps-necessary-to-remediate-based-on-performance-and-availability-metrics)
- [Deployment and Provisioning](#deployment-and-provisioning)
  * [Identify and execute steps required to provision cloud resources](#identify-and-execute-steps-required-to-provision-cloud-resources)
  * [Identify and remediate deployment issues](#identify-and-remediate-deployment-issues)
- [High Availability](#high-availability)
  * [Implement scalability and elasticity based on use case](#implement-scalability-and-elasticity-based-on-use-case)
  * [Recognize and differentiate highly available and resilient environments on AWS](#recognize-and-differentiate-highly-available-and-resilient-environments-on-aws)
- [Storage and Data Management](#storage-and-data-management)
  * [Create and manage data retention](#create-and-manage-data-retention)
  * [Identify and implement data protection, encryption, and capacity planning needs](#identify-and-implement-data-protection--encryption--and-capacity-planning-needs)
- [Security and Compliance](#security-and-compliance)
  * [Implement and manage security policies on AWS](#implement-and-manage-security-policies-on-aws)
  * [Implement access controls when using AWS](#implement-access-controls-when-using-aws)
  * [Differentiate between the roles and responsibility within the shared responsibility model](#differentiate-between-the-roles-and-responsibility-within-the-shared-responsibility-model)
- [Networking](#networking)
  * [Apply AWS networking features](#apply-aws-networking-features)
  * [Implement connectivity services of AWS](#implement-connectivity-services-of-aws)
  * [Gather and interpret relevant information for network troubleshooting](#gather-and-interpret-relevant-information-for-network-troubleshooting)
- [Automation and Optimization](#automation-and-optimization)
  * [Use AWS services and features to manage and assess resource utilization](#use-aws-services-and-features-to-manage-and-assess-resource-utilization)
  * [Employ cost-optimization strategies for efficient resource utilization](#employ-cost-optimization-strategies-for-efficient-resource-utilization)
  * [Automate manual or repeatable process to minimize management overhead](#automate-manual-or-repeatable-process-to-minimize-management-overhead)

## Monitoring and Reporting
### Create and maintain metrics and alarms utilizing AWS monitoring services
#### CloudWatch
- Service used for Monitoring AWS resources as well as the applications you run on AWS
- Stored indefinitely by default
- Retrieved using GetMetricStatistics API
- Most default metric granularity is 1, 3 or 5 minutes
- Detailed monitoring minimum granularity is 1sec (previously 1 minute) - needs high resolution monitoring enabled
- Alarms
    - For any Service
    - Thresholds
    - Actions
- Can be used on-premise, just need to download and configure SSM agent and Cloudwatch agent
- CloudWatch dashboards
    - International
    - Can display any widget (metric graph/number/text, etc) to any region
    - Must be in the region of the metric to add it to the board
#### Cloudwatch vs. CloudTrail
- Cloudwatch is for metrics, alarms, rules, performance monitoring and notifications
- Cloudtrail is for auditing. Is a record of all API calls, detailed info, etc.
    
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
    - Manual (Console) Steps to set up custom metrics
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
- Request tracing (application LB only) - monitoring HTTP requests from users to your application
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
### Organizations
- Allows central policy admin / billing etc.
- SCPs - service control policies
    - centrally control service access from a single policy to child accounts
    - Overrides the child account's IAM
- Can help automation of new accounts
    - Can create new accounts via Organizations, or invite existing accounts
- Consolidated billing

### Tags & Resource Groups
- Tags are Key-Value pair metadata for AWS resources
    - Some can be inherited (where resources are provisioned via another service (ASG, Beanstalk, CFN)
- Resource groups are a way of grouping resources by tag or stack basis
- Systems Manager - useful to manage resources in RGs
    - e.g. take a snapshot of all EC2 in a group
    
### EC2 Pricing
- On demand - pay per fixed rate, no commitment, good for spiky that can't be interupted
- Reserved - capcity reservation - 1 or 3 years, discounted, good for predictable workloads
- Spot - bid for instance capacity - cheap, but can be kicked off if price rises above bid, so needs flexible start/end times
- Dedicated hosts - physical dedicated servers, where multi tenancy can be done. can be good for per server software, i.e. VMWare, or regulatory needs

### AWS Config
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

### Health Dashboards
- Service Health
    - Status of Services on a per region basis
- Personal Health
    - alerts and remediation guidance for AWS services that you are using

### Perform the steps necessary to remediate based on performance and availability metrics

## Deployment and Provisioning
### Identify and execute steps required to provision cloud resources
#### Deploying EC2
- Pick AMI
- Pick Type (Size, etc.)
- Configure (LC/ASG, Spot?, User Data, VPC, Subnet, Placement Group, IAM role, etc.)
- Setup Security Groups, etc.
- 
### Identify and remediate deployment issues

## High Availability
### Implement scalability and elasticity based on use case
### Recognize and differentiate highly available and resilient environments on AWS

## Storage and Data Management
### Create and manage data retention
### Identify and implement data protection, encryption, and capacity planning needs

## Security and Compliance
### Implement and manage security policies on AWS
### Implement access controls when using AWS
### Differentiate between the roles and responsibility within the shared responsibility model

## Networking
### Apply AWS networking features
### Implement connectivity services of AWS
### Gather and interpret relevant information for network troubleshooting

## Automation and Optimization
### Use AWS services and features to manage and assess resource utilization
### Employ cost-optimization strategies for efficient resource utilization
### Automate manual or repeatable process to minimize management overhead
