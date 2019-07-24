# AWS SysOps Associate

## Monitoring and Reporting
### Create and maintain metrics and alarms utilizing AWS monitoring services
#### CloudWatch
- Service to monitor all of your AWS resources and applications
- Stored indefinitely by default
- Retrieved using GetMetricStatistics API
- Most default metric granularity is 1, 3 or 5 minutes
- Detailed monitoring minimum granularity is 1sec (previously 1 minute) - needs high resolution monitoring enabled
- Alarms
    - For any Service
    - Thresholds
    - Actions
- Can be used on-premise, just need to download and configure SSM agent and Cloudwatch agent
    
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
#### EBS Monitoring
- EBS Types
    - See https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumeTypes.html for latest stats
    - General Purpose SSD (gp2)*
        - Recommended for most workloads
        - System boot volumes
        - Virtual Desktops
        - Low-latency interactive apps
        - Dev and Test environments
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
    
          
        
### Perform the steps necessary to remediate based on performance and availability metrics

## High Availability
### Implement scalability and elasticity based on use case
### Recognize and differentiate highly available and resilient environments on AWS

## Deployment and Provisioning
### Identify and execute steps required to provision cloud resources
### Identify and remediate deployment issues

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
