# AWS Solution Architect Associate

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
- EC2 Metrics
    - Default (host level)
        - CPU
        - Network
        - Disk
        - Status Check
    - Custom (virtual layer)
        - Memory (RAM utilisation)
        - Application Errors, messages, etc.
        - Any other virtual disk layer metrics
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
