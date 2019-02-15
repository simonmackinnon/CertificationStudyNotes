##IAM
- Users - individual access to the console
- Groups - collection of users
- Roles - assigned to resources to grant access to other resources
- Policies - documents that define resource access
- Root account = god mode
- Sign in url is customisable, (globally unique)
- Always enable MFA
- IAM is universal
- New users = no permissions, get access key and Secret access key, password (can only view this once)

##S3
- object based, Key-value store (key object name, value data, version id, metadata, sub-recs - ACLs - Torrent)
- 0B - 5TB
- unlimited storage
- stored in buckets
- S3 names are globally unique, urls are: https://<region>.amazonaws.com/<bucket>
- interaction is API based, normal http response codes
- new objects = read after write consistent
- old objects = eventual consistent
- SLA 99.9 available, 99.999999999 durable
- versioning
    - each new version needs to be made public individually
    - deleting a versioned object will create a following version which is the delete marker
    - can enable MFA delete, extra delete security
- Access Control Lists => regulate access to individual objects
- Bucket Policies => way of regulating access to entire buckets
- Default all buckets and objects are private
- Successful upload gets HTTP 200 response
- 3 types: S3, S3-IA and S3-resource
- Can be encrypted - either client or server side (own or managed)

##EC2
- As you increaset CPU / ram you also increase network throughput/bandwith
- AMI - Amazon Machine Image
- An AMI is required for Auto scaling
- AMI nneds to determin the root device for boot disk EBS or Instance store volume
- Virtulisation type ie what hypervisor - xen ( highly customised ) or nitro
- HVM or PV ( Hardware Virtulisation ) ( Paravirtualisation )
- HVM provides extentions to be "closer" to the hardware, ie access GPU
- AWS runs all interl CPU's
- CPU Types are Letter then Number Ie m - Gernal and 5 which is the 5th generation
- Local disks are called "Instance store" Ephemeral Storage
- Dedicated Tenancy a VPC and EC2 where you are the only customer on that hardware
- Spot instance, heavy discounts for unused compute but can be terminated at any time
- EBS - Persistant storage
