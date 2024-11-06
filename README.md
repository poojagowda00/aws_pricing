Tenancy Type:
Shared Tenancy: This is the default option where EC2 instances run on physical hardware that is shared with other AWS customers.

Dedicated Host: You can run your EC2 instances on physical servers that are dedicated to your use only.

Dedicated Instances: These instances run on hardware that is physically isolated from other customers, but AWS manages the server.


 Instance Type:Instance pricing varies widely based on the instance type you select. Some common EC2 instance families include
t3.micro (low-cost, general-purpose)
m5.large (balanced compute, memory, and network)
c5.large (compute-optimized)
r5.large (memory-optimized)
p3.2xlarge (GPU instances, for ML or high-performance tasks)

Conclusion (Best EC2 Setup for a General Application)
Tenancy: Shared Tenancy
Instances: Use m5.large or t3.medium based on expected traffic and load.
Containers: If you use containers, choose Amazon ECS with EC2 or AWS Fargate for simplicity and cost-efficiency.

EBS Pricing for General Purpose SSD (gp3)
For EBS gp3 volumes, you will be choosing both the IOPS (Input/Output Operations Per Second) and Throughput (data transfer rate), along with the storage amount you need. Here's the breakdown:

1. EBS Volume Type:
General Purpose SSD (gp3) is the default and cost-effective SSD option for most workloads.
2. IOPS (Input/Output Operations Per Second):
gp3 allows you to provision IOPS independently from the storage size, with a maximum of 16,000 IOPS per volume.
The default IOPS for a gp3 volume is 3,000 IOPS, but you can increase it as needed.
3. Throughput (MBps):
gp3 volumes allow you to provision up to 1,000 MBps throughput.
The default throughput for a gp3 volume is 125 MBps, but you can scale it up.
4. Storage Amount:
gp3 storage pricing is based on the amount of provisioned storage (GB), and the cost scales with how much storage you provision.
The minimum size for a gp3 volume is 1 GB, and you can provision up to 16 TB (16,000 GB).
Typical Configuration Example
Storage Size: 500 GB
Provisioned IOPS: 5,000 IOPS (you need higher performance for your application)
Provisioned Throughput: 250 MBps (for high throughput)
S3 Standard:
Best for: Frequently accessed data with low latency and high throughput.
Pricing: Higher storage cost compared to infrequent access and archive options, but very fast access.
Use case: Web hosting, content delivery, big data analytics.
S3 Intelligent-Tiering:
Best for: Data with unknown or changing access patterns. Automatically moves data between frequent and infrequent access tiers.
Pricing: You pay for the monitoring and automation charge, along with the storage in each tier (frequent access and infrequent access).
Use case: Data that could be accessed frequently at times, but you aren’t sure about the access pattern.
S3 Standard - Infrequent Access (IA):
Best for: Data that is infrequently accessed but needs to be retrieved quickly when needed.
Pricing: Lower storage cost than S3 Standard, but higher retrieval costs. You are charged per GB for both storage and access.
Use case: Backup, disaster recovery, long-term storage.
S3 One Zone - Infrequent Access (One Zone-IA):
Best for: Data that is infrequently accessed and can be recreated if lost. This storage class stores data in a single availability zone, reducing cost.
Pricing: Lower cost than standard IA, but lower durability because it's stored in a single zone.
Use case: Secondary backups, or for less critical data.
S3 Glacier Flexible Retrieval:
Best for: Archival storage with retrieval times ranging from minutes to hours.
Pricing: Very low storage cost, but retrieval has varying costs depending on how fast you need to access the data (Expedited, Standard, or Bulk).
Use case: Long-term data archiving, backup, compliance data.
S3 Glacier Deep Archive:
Best for: Extremely low-cost archival storage with retrieval times in hours (typically 12 hours or more).
Pricing: The most cost-effective storage option, but retrieval time is slower.
Use case: Data that rarely needs to be accessed, but must be retained for compliance or archival purposes.

Recommendation Based on Typical Use Cases:
For most application deployments, a common combination might be:
S3 Standard: For frequently accessed data like logs, user uploads, and web content.
S3 Standard-IA or S3 One Zone-IA: For backups and infrequently accessed data where retrieval time is not critical.
S3 Glacier Flexible Retrieval: For archival storage with less frequent access, but needing data within hours.
S3 Glacier Deep Archive: For long-term archiving where the data needs to be kept for compliance, but not accessed often.
S3 Intelligent-Tiering: If you’re unsure about the access patterns for your data or if it changes over time.
Management Features like S3 Object Lambda and S3 Glacier Instant Retrieval are useful if you have specific requirements, such as transforming data on retrieval or needing fast access to archived data.

AWS Application Load Balancer (ALB) Pricing Breakdown
When calculating the cost of an Application Load Balancer (ALB), several factors come into play, such as Load Balancer Capacity Units (LCUs), processed bytes, new connections, connection duration, requests per second, and rule evaluations. Below is a detailed breakdown of the options you provided.
    Number of Application Load Balancers (ALBs):
ALBs are priced based on the number of load balancers you deploy. For each ALB, AWS charges a fixed rate per hour (this is typically the primary cost for ALBs).
Number of ALBs: 2
   Load Balancer Capacity Units (LCUs):
LCUs are used to measure the consumption of the resources by your ALB and are based on a few key metrics, such as processed bytes, connections, request rates, and rule evaluations.
   
   Processed Bytes
This refers to the amount of data processed by the ALB for different targets such as Lambda functions, EC2 Instances, or IP addresses
Processed Bytes (Lambda functions as targets): If your ALB forwards traffic to Lambda functions, AWS will calculate the total amount of data processed by the ALB in bytes.
Example:
Total processed bytes for Lambda functions: 5 GB

   Processed Bytes for EC2 Instances and IP Addresses:
Processed Bytes (EC2 Instances and IP addresses as targets): For EC2 instances or IP addresses, the data processed by the ALB will be charged as per the total number of bytes the ALB forwards.
Example:
Total processed bytes for EC2 targets: 100 GB
 
   Number of New Connections:
This refers to the number of new TCP connections initiated to the load balancer. The more new connections the ALB handles, the higher the cost.

   Average Number of New Connections:
The number of new connections that the ALB establishes per second or per minute will impact the LCU pricing.
Example:
New connections per ALB: 1,000 new connections per minute

    Connection Duration:
Connection duration is the average time that each new connection remains open. Shorter connection durations result in more connections being handled within a given time, which can affect costs.
Average Connection Duration:
If the average connection lasts less than 1 second, you will be charged for at least 1 second of connection duration.
Example:
Average connection duration: 5 seconds

    Number of Requests per Second:
This dimension tracks the number of requests processed by the ALB per second.
   Average Number of Requests per Second:
The ALB counts the number of HTTP(S) requests per second that it handles. More requests will increase LCU usage and, consequently, the cost.
Example:
Average requests per second per ALB: 500 requests/sec

    Rule Evaluations per Request:
Rule Evaluations are the number of rules the ALB evaluates for each request before routing the traffic to the appropriate target.
Average Number of Rule Evaluations per Request:
Each request can trigger one or more rules to be evaluated by the ALB before traffic is forwarded to the target. More rule evaluations mean higher LCU usage.
Example:
Rule evaluations per request: 5 rule evaluations per request


AWS CodeBuild Pricing Breakdown
Amazon CodeBuild pricing is based on several factors, including the compute type (instance type), the number of builds per month, and the average build duration. Here’s a breakdown of the options available for you
 Amazon CodeBuild Compute Type:
CodeBuild offers three compute types to choose from, each with different performance levels and associated costs:

Build General1 (Small):

vCPUs: 2
Memory: 3.75 GB
Pricing: Cheapest option, typically used for smaller builds.
Build General2 (Medium):

vCPUs: 4
Memory: 7.5 GB
Pricing: Ideal for medium-sized builds and more complex projects.
Build General3 (Large):

vCPUs: 8
Memory: 16 GB
Pricing: The most powerful instance type, used for large or resource-intensive builds.

 Number of Builds per Month: example 100
 Average Build Duration (Minutes): 10 mins
 Compute Instance Type: General2 (Medium)

      AWS CodeDeploy Pricing Breakdown
Amazon CodeDeploy is a fully managed deployment service that automates software deployment to various compute services, such as EC2 instances, Lambda, and on-premises instances. The pricing is primarily based on two key factors:
Number of on-premise instances: The number of on-premise servers or virtual machines where you are deploying code.
Number of deployments: The number of code deployments you perform per month.

On-Premise Instances: These are the physical servers or virtual machines located outside AWS where the application will be deployed. CodeDeploy tracks the number of on-premise instances and charges accordingly.
Example:
Number of on-premise instances: 5 instances

Deployments: A deployment is a single operation where a new version of your application is deployed to one or more instances.
Number of deployments per month: 10 deployments


  AWS CodePipeline Pricing Breakdown
Amazon CodePipeline is a continuous integration and continuous delivery (CI/CD) service for automating the build, test, and deploy phases of your release process. The pricing for CodePipeline is based on two key metrics:
Number of Active Pipelines (V1): This applies to pipelines of type V1 that have been active for more than 30 days and have at least one code change run through them during the month.
Number of Action Execution Minutes (V2): This applies to pipelines of type V2 and is charged based on the total action execution time.
Active Pipelines (V1) are defined as pipelines that are used for more than 30 days and have at least one code change in the month.

Pricing: AWS charges a fixed fee for each active pipeline of type V1.
Example:
Number of active pipelines (V1): 2 pipelines

   Number of Action Execution Minutes (V2):
For V2 pipelines, you are charged based on the total action execution duration, which is calculated from when an action starts executing until it reaches a completion state. The total duration is rounded up to the nearest minute.
Free Tier: CodePipeline provides 100 free action execution minutes per month for pipelines of type V2.
Action Types Excluded: Manual approval actions and custom actions do not incur charges for action execution minutes.
Pricing: After using the free 100 minutes per month, you will be charged for each minute of execution.
Example:
Action execution minutes for V2 pipelines: 150 minutes in total for all V2 pipelines.





