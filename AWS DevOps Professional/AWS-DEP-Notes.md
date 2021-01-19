# Domain 1: SDLC Automation
## Apply concepts required to automate a CI/CD pipeline
- Continuous Integration 
	- constantly merging code to ensure all new software is added
	- all new code built/tested on build server (devs notified about failures)
	- allows bugs to be found/fixed early
	- deliver code faster
	- can deploy quicker
	- CodeCommit
		- Git-based version control system.
		- Private repositories
		- No size limit on repos
		- Fully managed, HA
		- In AWS Account - security and compliance posture increase
		- Integrated with Jenkins/CodeBuild/Other CI tools
	- CodeBuild
		- Fully managed build service in AWS
		- Basically an alternative to other build tools like Jenkins
		- Elastic Scaling, no servers needed, no queue
		- Pay for usage, time to complete builds
		- Uses Docker under the hood, very reproducable
		- Can use own Docker images to extend capabilities
		- Security - Natively integrates with KMS for artifacts, IAM for build permissions, VPC for network security, CloudTrail for logging API calls
		- Gets code from version control repo
		- build instructions defined in code (buildspec.yml)
			- Build projects are split into separate phases  https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html
				- install
				- post_build
			- can specify artifacts to store/use after job finishes (and container is destroyed)
		- logs output to S3 and CloudWatch
		- Metrics monitor in CodeBuild statistics
		- CloudWatch events to detect failed builds/tests - trigger notifications -  can go to Lambda
		- CloudWatch Alarms for failures threshold nofifications
		- Can output to SNS
		- Docker, ECR & buildspec.yml https://docs.aws.amazon.com/codebuild/latest/userguide/sample-docker.html
		
- Continuous Delivery
	- Deliver software reliably when needed
	- Deployments are easy/quick
	- Can deploy many times per day vs. long release cycles (months, etc.)
	- Automated Deployments - CodeDeploy, Jenkins CD, Spinnaker
- Continuous Deployment
- Tech stack for CI/CD
	- Stages
		- Code
			- AWS CodeCommit
			- GitHub
			- Bitbucket
		- Build
			- AWS CodeBuild
			- Jenkins CI
		- Test
			- AWS CodeBuild
			- Jenkins CI
		- Deploy
			- AWS Elastic Beanstalk
			- AWS CodeDeploy
		- Provision
			- AWS Elastic Beanstalk
			- User managed EC2 Instances fleet (CloudFormation)
	- All can be managed using AWS CodePipeline
	
	- CodeDeploy
	- CodePipeline
	- CodeStar
	- Jenkins on EC2
- Continuous Delivery vs. Continuous Deployment
	- Continuous Delivery:
		- Ability to deploy often using automation
		- May involve a manual step to "approve" a deployment
		- Deployments still automated
	- Continuous Deployment
		- Full automation. All code changes deployed to production
		- No manual intervention of approvals
		- Bugs are automatically detected
	
## Determine source control strategies and how to implement them
- Branches useful to allow developers to divert from a main branch of code. 
- Allows development of code without impacting master code
- Merge code when it's ready (use Pull Request)
	- Pull Requests allow team to review code before it's integrated
## Apply concepts required to automate and integrate testing
- Use CodeCommit Nofifications and Triggers or CloudWatch events/rules to automate actions from events in our repositories; Triggers like SNS or SQS, Lambda, etc. https://docs.aws.amazon.com/codecommit/latest/userguide/how-to-notify-lambda.html
## Apply concepts required to build and manage artifacts securely
- Use IAM policies to restict pushes to master (or other production branches). This helps protect master branches from unwanted commits. IAM policy should have explicit deny on GitPush, etc. See https://docs.aws.amazon.com/codecommit/latest/userguide/how-to-conditional-branch.html
## Determine deployment/delivery strategies (e.g., A/B, Blue/green, Canary, Red/black) and how to implement them using AWS Services

# Domain 2: Configuration Management and Infrastructure as Code
## Determine deployment services based on deployment needs
## Determine application and infrastructure deployment models based on business needs
## Apply security concepts in the automation of resource provisioning
## Determine how to implement lifecycle hooks on a deployment
## Apply concepts required to manage systems using AWS configuration management tools and services

# Domain 3: Monitoring and Logging
## Determine how to set up the aggregation, storage, and analysis of logs and metrics
## Apply concepts required to automate monitoring and event management of an environment
## Apply concepts required to audit, log, and monitor operating systems, infrastructures, and applications
## Determine how to implement tagging and other metadata strategies

# Domain 4: Policies and Standards Automation
## Apply concepts required to enforce standards for logging, metrics, monitoring, testing, and security
## Determine how to optimize cost through automation
## Apply concepts required to implement governance strategies

# Domain 5: Incident and Event Response
## Troubleshoot issues and determine how to restore operations
## Determine how to automate event management and alerting
## Apply concepts required to implement automated healing
## Apply concepts required to set up event-driven automated actions

# Domain 6: High Availability, Fault Tolerance, and Disaster Recovery
## Determine appropriate use of multi-AZ versus multi-region architectures
## Determine how to implement high availability, scalability, and fault tolerance
## Determine the right services based on business needs (e.g., RTO/RPO, cost)
## Determine how to design and automate disaster recovery strategies
## Evaluate a deployment for points of failure

