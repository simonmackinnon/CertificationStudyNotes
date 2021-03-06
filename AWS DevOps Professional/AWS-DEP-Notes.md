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
				- pre_build
				- build
				- post_build
			- specify the commands to run/install for each stage. each stage has the 
				- "run-as" - specify a different linux user to run the phase as
				- "commands" - run commands for the phase
				- "finally" - Use to perform phase clean-up actions
				- "runtime-versions" (install phase only) list of runtimes to install on the build image
			- Can set up environment variables and parameters
			- Can output objects as "artifacts" of the job
				- Specify what to output in the buildspec "artifacts" section
				- Specify where to store it in the build project artifacts settings (S3 Bucket)
					- Ensure the option to modify service role for s3 access is enabled, or modify IAM policy manually to allow access	 
			- subsequent stages to not run if previous stage doesn't succeed
				- make things that are likely to fail (authenticated actions, network actions) happen earlier (don't want to do expensive/time-consuming commands if the whole job will fail)
			- can specify artifacts to store/use after job finishes (and container is destroyed)
			    - these can be output to S3 (useful for pipeline jobs, i.e. deploy a built artifact)
		- Environement Variables 
			- available to each job (e.g. AWS_DEFAULT_REGION, CODEBUILD_BUILD_ID)
				- Full list here: https://docs.aws.amazon.com/codebuild/latest/userguide/build-env-ref-env-vars.html
				- Output them using the 'printenv' command
			- Can add to env variables list (via buildspec, or override during build)
			- For sensitive values, use SSM parameters (specify 'Parameter' for the codebuild Env variable Type)
				- Ensure SSM readonly access to the codebuild service role to use this 
				- Can also use Secrets Manager for more complex object (DB Credentials, API keys, etc.)
		- logs output to S3 and CloudWatch
		- Metrics monitor in CodeBuild statistics
		- CloudWatch events to detect failed builds/tests - trigger notifications 
		    - can go to Lambda, SNS or any other eventbridge target
		    - can create scheduled rules or triggers to start project build (i.e. when changes to repo occur, trigger the build: CloudWatch -> Rules)
		    - can use it to validate pull requests (i.e. build and test code on each commit)
		- CloudWatch Alarms for failures threshold nofifications
		- Can output to SNS
		- Docker, ECR & buildspec.yml (Likely exam question on this setup) https://docs.aws.amazon.com/codebuild/latest/userguide/sample-docker.html
		
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
	    - agent-based deployments
	        - polls for "work to do"
	        - sends the appspec.yml to the instance
	        - app is pulled from source (s3 or github) (can use `aws deploy push` command to create deployment artifact)
	        - instance then runs the instructions
	        - reports back to CodeDeploy on the status of the deployment (success/failure)
	    - Can group instances by deployment group (dev/test/prod)
	    - can integrate with CodePipeline, use artifacts from CodeBuild or source from CodeCommit
	    - Can use existing OS config tools, applications, integrates with ASG
	    - Blue/Green deployments woth only with Ec2 instances (not on-premises)
	    - Support for AWS Lambda deployments as well as EC2
	    - Only in charge of deployment (won't do infra creation)
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

