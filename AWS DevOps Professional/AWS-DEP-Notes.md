- [Domain 1: SDLC Automation](#domain-1--sdlc-automation)
  * [Apply concepts required to automate a CI/CD pipeline](#apply-concepts-required-to-automate-a-ci-cd-pipeline)
    + [Continuous Integration](#continuous-integration)
      - [CodeCommit](#codecommit)
      - [CodeBuild](#codebuild)
    + [Continuous Delivery](#continuous-delivery)
      - [CodeDeploy](#codedeploy)
      - [CodePipeline](#codepipeline)
      - [CodeStar](#codestar)
      - [Continuous Delivery vs. Continuous Deployment](#continuous-delivery-vs-continuous-deployment)
  * [Determine source control strategies and how to implement them](#determine-source-control-strategies-and-how-to-implement-them)
  * [Apply concepts required to automate and integrate testing](#apply-concepts-required-to-automate-and-integrate-testing)
  * [Apply concepts required to build and manage artifacts securely](#apply-concepts-required-to-build-and-manage-artifacts-securely)
  * [Determine deployment/delivery strategies (e.g., A/B, Blue/green, Canary, Red/black) and how to implement them using AWS Services](#determine-deployment-delivery-strategies--eg--a-b--blue-green--canary--red-black--and-how-to-implement-them-using-aws-services)
- [Domain 2: Configuration Management and Infrastructure as Code](#domain-2--configuration-management-and-infrastructure-as-code)
    + [CloudFormation](#cloudformation)
    + [Elastic Beanstalk](#elastic-beanstalk)
    + [Lambda](#lambda)
  * [Determine deployment services based on deployment needs](#determine-deployment-services-based-on-deployment-needs)
  * [Determine application and infrastructure deployment models based on business needs](#determine-application-and-infrastructure-deployment-models-based-on-business-needs)
  * [Apply security concepts in the automation of resource provisioning](#apply-security-concepts-in-the-automation-of-resource-provisioning)
  * [Determine how to implement lifecycle hooks on a deployment](#determine-how-to-implement-lifecycle-hooks-on-a-deployment)
  * [Apply concepts required to manage systems using AWS configuration management tools and services](#apply-concepts-required-to-manage-systems-using-aws-configuration-management-tools-and-services)
- [Domain 3: Monitoring and Logging](#domain-3--monitoring-and-logging)
  * [Determine how to set up the aggregation, storage, and analysis of logs and metrics](#determine-how-to-set-up-the-aggregation--storage--and-analysis-of-logs-and-metrics)
  * [Apply concepts required to automate monitoring and event management of an environment](#apply-concepts-required-to-automate-monitoring-and-event-management-of-an-environment)
  * [Apply concepts required to audit, log, and monitor operating systems, infrastructures, and applications](#apply-concepts-required-to-audit--log--and-monitor-operating-systems--infrastructures--and-applications)
  * [Determine how to implement tagging and other metadata strategies](#determine-how-to-implement-tagging-and-other-metadata-strategies)
- [Domain 4: Policies and Standards Automation](#domain-4--policies-and-standards-automation)
  * [Apply concepts required to enforce standards for logging, metrics, monitoring, testing, and security](#apply-concepts-required-to-enforce-standards-for-logging--metrics--monitoring--testing--and-security)
  * [Determine how to optimize cost through automation](#determine-how-to-optimize-cost-through-automation)
  * [Apply concepts required to implement governance strategies](#apply-concepts-required-to-implement-governance-strategies)
- [Domain 5: Incident and Event Response](#domain-5--incident-and-event-response)
  * [Troubleshoot issues and determine how to restore operations](#troubleshoot-issues-and-determine-how-to-restore-operations)
  * [Determine how to automate event management and alerting](#determine-how-to-automate-event-management-and-alerting)
  * [Apply concepts required to implement automated healing](#apply-concepts-required-to-implement-automated-healing)
  * [Apply concepts required to set up event-driven automated actions](#apply-concepts-required-to-set-up-event-driven-automated-actions)
- [Domain 6: High Availability, Fault Tolerance, and Disaster Recovery](#domain-6--high-availability--fault-tolerance--and-disaster-recovery)
  * [Determine appropriate use of multi-AZ versus multi-region architectures](#determine-appropriate-use-of-multi-az-versus-multi-region-architectures)
  * [Determine how to implement high availability, scalability, and fault tolerance](#determine-how-to-implement-high-availability--scalability--and-fault-tolerance)
  * [Determine the right services based on business needs (e.g., RTO/RPO, cost)](#determine-the-right-services-based-on-business-needs--eg--rto-rpo--cost-)
  * [Determine how to design and automate disaster recovery strategies](#determine-how-to-design-and-automate-disaster-recovery-strategies)
  * [Evaluate a deployment for points of failure](#evaluate-a-deployment-for-points-of-failure)

# Domain 1: SDLC Automation
## Apply concepts required to automate a CI/CD pipeline
### Continuous Integration 
- constantly merging code to ensure all new software is added
- all new code built/tested on build server (devs notified about failures)
- allows bugs to be found/fixed early
- deliver code faster
- can deploy quicker
#### CodeCommit
- Git-based version control system.
- Private repositories
- No size limit on repos
- Fully managed, HA
- In AWS Account - security and compliance posture increase
- Integrated with Jenkins/CodeBuild/Other CI tools
#### CodeBuild
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
		
### Continuous Delivery
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
	
#### CodeDeploy
- agent-based deployments
- polls for "work to do"
- sends the appspec.yml to the instance
- app is pulled from source (s3 or github) (can use `aws deploy push` command to create deployment artifact)
- instance then runs the instructions
- reports back to CodeDeploy on the status of the deployment (success/failure)
- Can group instances by deployment group (dev/test/prod) - Use Tags for this
- can integrate with CodePipeline, use artifacts from CodeBuild or source from CodeCommit
- Can use existing OS config tools, applications, integrates with ASG
- Blue/Green deployments with only with Ec2 instances (not on-premises)
- Can do without ASG (i.e. manually provision instances, not good devops practice), but is much easier with ASG (copies existing ASG to new group)
- Requires a Load Balancer (can be classic or ALB)
- Deployment Configs 
- Replace all at once okay for non-production, service has unavailability
- Can do 1 at a time, half at a time, or custom (number of healthy hosts, % of healthy hosts, % setting not good for small number of hosts)
- Support for AWS Lambda deployments as well as EC2
- Only in charge of deployment (won't do infra creation)
- appspec.yml: Configuration of how CodeDeploy works for an app, defined in appspec.yml
- files: 
	- source: where a file is coming from in repository 
	- destination: where it should go on server
	- The copying part is performed during the Install lifecycle event (can't specify the scripts to run in Install event)
- hooks: 
	- ![LifeCycle](https://k2y3h8q6.stackpathcdn.com/wp-content/uploads/2020/08/Lifecycle-Event-Hooks-in-CodeDeploy1.png):
	- Scripts that will be run during the deployment (different stages)
	- Can specify the timeout for different step (max 3600 sec, https://docs.aws.amazon.com/codedeploy/latest/userguide/limits.html)
		- ApplicationStop: define how to stop the service (e.g stop the httpd service)
		- BeforeInstall: install dependecies (e.g. install httpd service)
		- AfterInstall: script to run after install, potentially do some application re-configuration (e.g. update a config with instance meta-data values, etc.)
		- ApplicationStart: define how to start the service (e.g. start the httpd service)
		- ValidateService: run a healthcheck (i.e. run unit tests, UI tests). if this fails the deployment fails
	- View the lifecycle events in "View Events" link in deployment record (in console) - details not available via CLI/API
	- Different hooks for Lambda vs. ECS vs. EC2/On-Prem (different hooks available if using Load Balancer, or not) (https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file-structure-hooks.html)
	- Some Deployment specific environment variables available to Hooks, Can be useful to have branched code for Production vs. Non-Production code in appspec.yml
	```
	if [ "$DEPLOYMENT_GROUP_NAME" == "Staging" ]
	then
	    sed -i -e 's/LogLevel warn/LogLevel debug/g' /etc/httpd/conf/httpd.conf
	fi
	```
- CloudWatch events to detect failed deployments 
    - can go to Lambda, Kinesis, SNS or any other eventbridge target	
    - Can also trigger instance reboot after a successful deployment (if the application requires that)
- trigger notifications - Directly send messages to SNS from CodeDeploy
- View logs in CloudWatch (need CodeDeploy and CloudWatch Logs agent)
- Rollbacks (what to do if deployment fails)
    - Manual Rollback (if the deployment fails, just run the deployment again with a previously working version of the app)
    - Automated Rollback 
	- Roll back when deployment fails (if any lifecycle event has failure, roll back)
	- Roll back when alarm thresholds are met:
		- E.g. Deployment succeeds but CPU alarm spikes, causing a Cloudwatch alarm to trigger, we want to roll-back
		- E.g. Deployment succeeds but application errors encountered is above acceptable value, we can define custom logs metric/alarm, trigger this when above the value and roll-back
- On Premises Instance registration
- Needs an IAM identity to authenticate requests (user/role)
- Role for lots of instances
- User(s) for few instances
- about 11 steps to configure for user, 8 for role
- Deploying to Lambda
- Configuration can be linear, canary or all-at-once:
	- linear: traffic hits new version at specified percentage for specified time, growing at linear rate, after 10 increments all traffic hits new function
	- canary: traffic hits new version at specified percentage for specified time. if all is okay, then the rest of traffic hits new version of function 
	- all-at-once: all traffic hits new version of function immediately
- Hooks available:
    - BeforeAllowTraffic: Run *another* Lambda Function. E.g. to make sure everything is fine (potentially database is available, etc.)
    - AfterAllowTraffic: Run *another* Lambda Function. useful to see if the new version of the function is working correctly (healthchecks & other monitoring)
#### CodePipeline
- Continuous Delivery and Deployment Visualisation tool
- Can orchestrate the entire CI/CD flow
- Source can be in Github / CodeCommit / S3
- Build using AWS native (CodeBuild) or other services (e.g. Jenkins)
- Can then run 3rd party tools for testing
- Can Deploy using CodeDeploy, ECS, Beanstalk, CloudFormation ...
- Made up of stages
    - Stages can have sequencial or parallel steps
	- where a stage runs is defined (in code) by the 'runOrder' parameter. 
	- the higher a runOrder value, the later the action occurs. 
	- Actions with the same runOrder value are in the same Action Group, i.e. they will run in parallel
    - Stages define the steps to perform, like build, test, deploy
    - Can build in manual approval to any stage
    - Each stage is integrated via *artifacts*
	- Artifacts are the output of each stage, stored in S3, the subsequent steps have access to these
	- e.g. codecommit puts source into S3, used by build step to build package, package deployed by deploy step, to server, test step validate the deployment  
	- Can retry failed stages in deployments, although this uses the existing artifact from the previous stage. (i.e. if you need the source to be up to date, need to do whole deployment, not just stage).
- Artifacts
    - Role used in each stage needs IAM access to the relevant artifacts bucket   
    - Can store in default S3 location. This make setup simpler, but a new bucket is created for each new pipeline
    - Can use a custom bucket (useful when you have many pipelines
    - Each pipeline has a folder in the bucket
    - Each Stage has a folder under that
    - Each deployment of each stage has an artifact (if specified)
    - Encryption Key
	- Can use default AWS KMS Key
	- Can use Customer Managed Key
- If you want to output to a different bucket within a stage, or at end of deployment, Can add an additional action to upload to another S3 bucket 
- Note, CodeBuild artifacts are different than CodePipeline artifacts (although the former can be used for the latter)
- Manual Approval Steps:
    - Needs to be configured sequencial order (i.e. before the step that needs approval)
    - Max time out for approval action is 7 days (https://docs.aws.amazon.com/codepipeline/latest/userguide/limits.html)
- All Integrations (https://docs.aws.amazon.com/codepipeline/latest/userguide/best-practices.html#use-cases)
    - Heaps of different services to integrate with natively. Noteworthy ones:
    	  - Lambda
    	      - Useful for full control over action steps, success/faulure state (use codepipeline.putJobSuccessResult etc. to indicate job success, etc.)
    	      - Really useful to connect to 3rd-party applications in Pipeline (i.e. Slack, or other email client, write to enterprise system)
    	  - Cloudformation (deploy a new stack when code changes)
    	      - Can deploy a stack for testing then delete it once done, can be repeated in different stages (like test, prod, etc.)
    	      - Can also create CodePipelines using CloudFormation, which can in turn create CloudFormation stacks (https://github.com/aws-samples/codepipeline-nested-cfn)
          - CodeBuild/Deploy
          - Jenkins
          - But many other, including test platforms/services    
#### CodeStar
- Abstraction / Quickstart service for settng up CodeCommit/Deploy/Pipeline, as well us compute/hosting infrastructure (i.e. Lambda/EC2/Beanstalk) for a project using templates (i.e. Express.js app or plain HTML app)
- Select Project Type, how it will be deployed/run, and the infrastructure is provisioned for you automatically
- Integrtes with Github/Jira
- Gives you a Wiki for the project/team
- What is the difference between CodeStar and Proton? (https://acloudguru.com/blog/engineering/what-you-need-to-know-about-the-new-aws-proton-service)

#### Continuous Delivery vs. Continuous Deployment
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
### CloudFormation
- Can use cfn-init instead of EC2 user-data for setting up instances. Cfn-init is desired state, rather than script
- Use cfn-signal to make CloudFormation service wait for EC2 service to be set up before completing
- Can turn off rollbacks on failure if you need to debug an issue in a stack
- Nested stacks useful for code-reuse, especially across teams or stacks
- Can create a Change-set separate from stack creation, this allows for a manual review of what will change prior to applying the changes
- Deletion policy: Default is to delete (except DB instance, DB cluster - snapshot). To retain or create snapshot, change the DeletionPolicy attribute on the resource.
- Can stop stacks from being terminated termination protection. To delete a stack, need to turn this off first
- Using params from SSM
	- Advantage is that updating stacks after param value updates means the inputs are update automatically
	- Need to trigger stack update to do this
	- Public parameters available
		- can reference things like the latest AMI for a linux distro automatically, i.e. no need to add an SSM Param Store parameter and manually update when new images released, this is available through a puplic param value 
- DependsOn
	- Ensures dependecy of resource creation order, e.g. ensure a DB is created before creating the web-server that will connect to it
- Lambda Function deployments
	- Can be inline, or reference an S3 object (or object version)
	- Inline limited to about 4KB characters, no packaging (reference libraries, etc.)
- Custom Resources - Usefule for AWS services not covered by CFN, or non-AWS resource, empty an S3 bucket prior to deleting, fetch an AMI ID, or anything really that you'd like (super powerful)
	- Create / Update / Delete event type and event properties, passed to lambda, as well as response URL (presigned URL). Lambda repsonds back to service via the response URL a success/failed status after running whatever code is needed, plus any additional resource properties (which as suggested, are then properties of the custom resource)
- Drift Detection
	- Shows modified/deleted/in-sync resources within a stack. To fix, need to rebuild the stack, or manually fix the resources where you don't want to replace the resources -> remove drifted resources from stack (with retain deletion policy) then import the resources back into the stack with the desired state
- Update\_rollback\_failed status: 
	- need to either continue the rollback (manually fix the resources that caused the failure) or skip the resources (status of those will be Update_complete)
	- Potentially need to contact AWS support, if cause was service limitation, etc.
- InsufficientCapabilitiesException - Error occurs when you don't acknowledge that CFN needs to create IAM resources. You need to allow cloudformation the capability to create IAM resources
- cfn-hup
	- default recheck timeout is 15min, can be modified using "interval" config
	- put change hooks configs in hooks.d directory, these specify what should run if the metadata Init section has changed (provide path, action, resource, etc.)
	- essentially, can update EC2 instances when the init section changes (e.g. an input param changes) without needing to replace the instance
- Stack Policies 
	- Used to lock down specific resources within a stack (resource rather than IAM principal). Uses allow/deny statements. Can deny updates on particular resources/patterns. 
	- from CLI, add inline using `--stack-policy-body` or reusable using `--stack-policy-url` (s3 object URL)
	- To update a resource that's blocked by a policy, need to update the policy during the update command to ensure the action can be carried out (it's automatically reverted once the update is complete)
### Elastic Beanstalk
- Use Elastic Beanstalk to bootstrap your application. Create Environments using console/cli, this creates all the infra needed for a scalable app (ASGs/LCs,ELBs, etc.) `eb create <env-name>`
- Deploy changes using eb cli command `eb deploy`
- Save EB configs as code (native to Elastic Beanstalk) 
	- `eb config save <env-name> -cfg <config-name>` 
	- this stores the non-default settings in a yaml file
	- Set environment variables using `eb setenv` command
	- Upload saved configs using `eb config put <config-name>` (i.e. saves the local config file to eb environment remote)
	- Update env to use saved config using `eb config <env-name> --cfg prod` (uses remote config, not local)
	- Saved configs can be loaded in other environments, not just the one it was created in. Can be loaded into another region, which can be good useful for multi-region DR strategy
- .ebextensions for configs
	- define options settings for the environment
	- General Options for all environments here: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/command-options-general.html
	- See the format of the options settings here: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/ebextensions-optionsettings.html
	- sits at project root level, so is automatically included when running an `eb deploy` command
	- Settings Precendence: https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/command-options.html#configuration-options-precedence
		- Client, then
		- Saved Configurations, then
		- .ebextensions, executed in filename alphabetical order, then
		- default Elastic Beanstalk values 
- .ebextensions for resources
	- Can add CloudFormation resources into a .ebextensions template using the Resources block 
	- Can also add in Outputs, but these are outputs of the CloudFormation stack, not the EB environment
	- Can create environment variables from CloudFormation instrinsic functions (e.g. ref a resource) in the options_settings section
		- unresolved when viewing in the EB environment configs, but are resolved at runtime on instance(s)
		- check the values by running `/opt/elasticbeanstalk/bin/get-config environment` (pipe to jq for pretty view)
		- view the whole options settings using `/opt/elasticbeanstalk/bin/get-config optionsettings` (useful for debugging)
	- For resources that must persist outside of the Elastic Beanstalk environment (e.g. an RDS DB that will be shared between environments):
		- Create resource as part of separate app (manually or using CloudFormation)
		- Reference the resource using environment variables in the options settings 
	- Commands (server commands):
		- Commands are executed on the E2 instance, before the application is set up
		- Useful to install applications needed for the app to run, etc.
	- Container Commands (application pre-deployment config commands):
		- Commands are run after the application is set up, but before the app is deployed  
		- Useful for replacing config values, set connection strings, etc.
		- `leader_only` parameter ensures the command is only executed once (i.e. on the master node)
			- useful for one-off commands (some non-idempotent database commands, notification commands)
	- Good features to know:
		- Configuration presets: set up different level of HA infra based on cost/environment requirements: single instance in dev, LB and multiple instances in prod, etc.
		- Application version lifecycle settings
			- Each deployment means a new application version is stored
			- Elastic Beanstalk only allows 1000 versions to be stored, need to clean up versions after many deployments
			- Lifecycle policy can be enabled
				- keeps based on number of versions, or version age.  
				- Sets if version bundle should be retained or deleted
				- Which IAM role to use to perform version lifecycle actions
			- Won't delete currently deployed versions
		- Clone environment
			- Creates a copy of an existing environment (can change some basic settings like IAM role, env name/url)
		- Terminate environment
			- destroys all the resources/configs 
		- Rebuild an environment
			- destroys the environment, then builds it from scratch using the configs/extensions
			- Need to be careful of datastores (back up DBs or file systems if data required after rebuild)
		- Managed Updates:
			- Set an update window for updates to be applied automatically on a weekly basis
			- can perform instance replacement 
			- Using rolling deployment to do it
	- Rolling Updates stratagies:
		- If using an HA configuration (ELB, ASG, etc.) how to deploy new versions?
			- All at once 
				- fast
				- but downtime
			- Rolling 
				- updates a few at a time
				- no downtime
				- app runs below max capacity
				- number of instances to update is determined by batch size
				- once batch is deployed/healthy, moves onto next batch
				- runs both version of application at the same time
				- no additional cost
				- slow if bucket size is small
			- Rolling with additional batches
				- creates instances to match batch size
				- spin up new instances to move the batch
				- old application still available
				- small additional cost
				- app runs above max capacity temporarily
				- slow if bucket size is small
			- Immutable
				- spin up new temporary ASG/instances
				- deploy to instances
				- swaps to the new instances once healthy
					- merges instances to existing ASG
					- terminates the old instances
					- removes the temporary ASG 
				- expensive (double capacity)
				- very quick rollback in case of failures
			- Blue/Green
				- Separate environment for new version of app
				- Validation can be done over time
				- Rollback is simple
				- use DNS (e.g. Route53) to set the traffic flow (can use weighted traffic policies to test little traffic to new app first) 
		- Can use the Swap Environment URL feature to perform Blue/Green DNS Switch. Takes some time to propogate DNS / Browser cache TTL to expire
		- Perform CPU intensive / Scheduled / Long running tasks
			- work to be performed sent from web server to SQS queue, the queue to use is an environment configuration
			- scheduled tasks are performed as per the cron.yaml 
		- Multi-Docker Integration
			- Uses ECS cluster under the hood as tasks on EC2 
			- Dockerrun.aws.json is an Elastic Beanstalk specific JSON file that describes how to deploy a set of Docker containers as an Elastic Beanstalk application (used if there is a complex Muli-container Docker application)
### Lambda
- For purposes of DevOps pro exam, service is used for operations / glue services together
- Triggers:
	- API Gateway, external API can invoke function (serverless API, http interface)
	- ALB - can put a load balancer in front of function (lose native security integrations, but can route based on path or other layer 7 attributes)
	- CloudWatch events - glue of all of our DevOps automation
		- react to any event in our infrastructure
		- Cron event can regularly schedule the run of lambda
	- Cloudwatch logs - alerting based on log patterns in cloudwatch logs  (abstracts creating custom log metric / alarm / CW Event)
	- CodeCommit - react to CC hooks, if code is committed, notify, or monitor changes
	- DynamoDB - streams, can react to new data in table (i.e. run lambda to sign up a new user if a record added to user table, etc.)
	- Kinesis - RT processing of data, incoming data, lambda can process the data
	- S3 
		- trigger lambda when new object added to bucket 
		- can use prefix patterns
		- need to be careful about recursion (don't have a lambda write to a bucket that triggers the lambda)
	- SNS - lambda can react to notifications, lambda is the subscriber to a topic. if it can't handle a notification 3x, message goes to a DLQ
	- SQS - like normal queue message processing, marks message processed, or puts back on the queue
-  Environment variables
	- Encryption in transit, 
		- use "Enable helpers for encryption in transit" option in the console. (Supply KmsKeyArn to encrypt with KMS key in CloudFormation)
		- Code needs to decrypt using KMS key (execution role needs access to KMS key and decrypt calls)
		```
		{
		    "Version": "2012-10-17",
		    "Statement": [
			{
			    "Effect": "Allow",
			    "Action": "kms:Decrypt",
			    "Resource": "arn:aws:kms:ap-southeast-2:955966247963:key/30b5b72f-9a13-499c-a184-d899a5999c83"
			}
		    ]
		}
		```
		- (KMS Key Policy needs to allow function to use it too)
	- Can also use secure parameters via SSM Parameter store (access via API calls in code, no via Env variables) needs IAM permissions
	- Can alsom use secrets manager (access via API calls in code, no via Env variables) needs IAM permissions
- Versions
	- default is $LATEST version (which is pointer to the most recent version, version returned if no version specified)
	- each deployment makes a new version (we can alias versions, blue, green, live, new-feature, etc.) 
	- Access to aliases is better, as we can switch where the alias points without backend/front-end change (eventBridge rule might target a particualr function alias)
	- Can create weighted alias 
		- (% of traffic to two versions, e.g. 5% to v2, 95% to v1, useful for Blue/Green deployments)
		- Logs indicate which 
		- CodeDeploy Linear and Canary Deployments uses this under the hood
### AWS Serverless Application Model (SAM Framework)
- The AWS Serverless Application Model (AWS SAM) is an open-source framework that you can use to build serverless applications on AWS.
- Transforms a SAM template to CloudFormation via the SAM Framework (i.e. it is an abstraction of CloudFormation specific to serverless resources)
- Simplified application packaging - `sam build`
	- Package is built in the `app\.aws-sam\build` directory
	- Contains all the dependencies that need to be installed for the app (note: don't check this into source control, default sam init adds build folder to .gitignore) 
- Can test function invocation locally (via Docker) `sam local invoke "HelloWorldFunction" -e events/event.json`
- Can run the API locally (via Docker) and expose on localhost:port `sam local start-api` & `curl http://127.0.0.1:3000/hello`
- Package with `sam package` command (specify s3 bucket or ECR repo for docker images)
- SAM and CodeDeploy
	- Can use CodeDeploy to perform function updates (e.g. do Canary or Linear Deployments)
	- When `sam deploy` runs, it triggers a new Code Deploy deployment (which utilises an alias with weighted routing etc.)
	- Can run pre and post traffic commands to validate the function using PreTraffic/PostTraffic Hooks (references other Lambda functions)
- Can't delete a SAM deployment using the CLI, delete the CloudFormation stack instead
### Step Functions
- Workflow orchestration of separate Lambda/Batches. Can have wait steps defined. Runs up to 1 year for each execution. State Machine definition 
- JSON document defines the state-machine. This can be viewed in Step Functions console (or with IDE plugins, e.g. VSCode)
- Useful for separating RT functionality (i.e. Website request responses) from Non-RT functionality (i.e sign up messaging, other back-end reporting, etc.)
- Useful for initiating retries when a function fails (i.e. retry a deployment, etc. X no. of times before ending)
- Useful to run parallel tasks (e.g. send notification and store message data, etc.)
- Useful to run DevOps tasks (notifications, post-deploy test tasks, etc.)
- Can view execution history, which provides details of success/failure, as well as the reason/state/function it failed in
- CloudWatch can monitor for status-changes natively (especially for failed status), target a specific function or SNS topic, etc.
- CloudWatch can natively target StepFunctions, i.e. schedule a step-function to run daily, or trigger step-functions in event-based manner
- API Gateway can now directly invoke Step-Functions https://aws.amazon.com/blogs/compute/introducing-amazon-api-gateway-service-integration-for-aws-step-functions/

### API Gateway
- Can integrate with Lambda Functions, or other AWS services
- Functions need to be in the same region as th API
- API Keys - Can provide them to clients to connect (easy to track, but not best solution for security - use Authorizers instead)
- Can log API calls to CloudWatch, need to assign IAM role that has permissions to write to CW
- Private Endpoints - deploy API internal to a VPC - Available internally to other services, API can only use functions/endpoints in the VPC
- LAMBDA_PROXY: Entire request is proxied over to the function
- LAMBDA: use request/response mapping template 
	- modify the event that passed to the function (requires scripting in Velocity Template Language - VTL)
	- Can use API Gateway to modify the output with a mapping template at the response layer (e.g. sort json object before serving response)
- Lambda endpoint
	- API method can point to Lambda, Lambda alias, or Lambda version 
	- Can use weighted routing at Lambda alias
- Deployments are done to Stages
	- Each stage can be rolled-back to previous version
	- Each stage has it's own URL, and can point to specific version/alias of function
	- Stage variables used to change often changing config values e.g. lambda ARN, http endpoint, mapping templates
		- passed to the "context" object in Lambda, so using the variables is done at runtime (not API/Lambda config level)
		- Can map to an event attribute, or transform using mapping template, e.g. { "name" : "$stageVariables.<variable_name>"}
		- we can also map directly to a function name/alias/version, by declaring it as a stage variable (e.g. "myfunction", then specifying ${stageVariables.myfunction} in the "Lambda Function" parameter in the Integration Request section (need to ensure the API has permissions to invoke all values of the lambda)
- Canary Deployment
	- Adding a Canary to a stage allows a variable amount of requests to be directed to a canary. New deployments are automatically directed to the canary, and canary % is whatever it was set to. Can alter the value incrementally, or promote the canary to move the deployment to the stage 
	- Other way of doing this is to use an alias with weighted routing
	- Which to use? Former means doing API deployment, and altering the traffic direction at API layer, latter deploys to Lambda and directs traffic at Lambda layer. If we use SAM, we can use CodeDeploy to automate this in Linear fashion at Lambda layer, but it would need to be manaully scripted to do this at API layer
- Throttling
	- API calls limted on account level to 10,000 per second (irrespective of region, VPC, etc.)
	- Can create "Usage Plan" which defines usage of an API stage and/or method (Rate/Burst), and this is associated with an API key (which is given to users).
	- Throttling at Lambda level (so how many can be concurrently executed, if this happens: increase function's reserved capacity)
- Step Function integration
	- Can trigger an execution using "AWS Service" as Integration Type for an API method
		- Define Service as "Step Functions"
		- Define the Action (i.e. "StartExecution")
		- Assign an execution role that has permission to perform the chosen action against the specified Step Function
	- Note: result of the Step Function Execution is not returned the the API (only whether the exection start invocation succeeded)
		- makes sense: Step function can run for a year
		- To get the result, need to poll the step function DescribeExecution action until it's not "RUNNING" 

### ECS
- Docker 
 	- Run "Containers" (app + runtime + dependencies packaged together) - Runs the same on any OS
	- Containers created from Images (can find many public images on dockerhub.com, can store private images on ECR, etc.)
	- Can run many containers on a machine
	- VMs run on Guest OS on Hypervisor on Host OS, Containers run on Daemon on Host OS directly
- ECS Clusters 
	- Grouping of EC2 instances running the ECS agent
	- Run special ECS AMI
	-  

### OpsWorks



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
