https://www.youtube.com/watch?v=AYAh6YDXuho

# Container Services in AWS

Container Services are suitable for microservice application ( Many services) . Not for a monolith.

Why container service?

Containers deployed on EC2 which have docker installed( 10 services on 3 EC2). Management becomes difficult. \
How many resources stll available? Where to schedule next container? Are docker containers still running? How to manage scaling(manually??) \
To help managing containers - Container Orchestration tools( Docker Swarm, K8s, Mesos, Nomad, ECS)

## ECS

ECS: Container Orchestration service in AWS.
- Manage lifecycle of containers
- Load balance

ECS Cluster managed by AWS. \

**ECS with EC2:Container management by AWS. Infrastructure management by you. U pay for EC2 even if not used.** \
**ECS with Fargate: Both container management and Infra management by AWS.Pay as u go.**

### ECS with EC2

Where are these containers running?\
On EC2 instances. These EC2 instances will have Docker Agent and ECS Agent. You have **delegated management of containers to ECS**. \
You still have to manage the actual Ec2 instances.\
You have to create Ec2. You have to join then to ECS cluster. When u schedule a new container you have to make sure there are enough EC2 instances available.\
**You need to manage OS, Docker agent, ECS agent.**\
You have full access to Infrastructure. \
Infrastrure is managed by you.\
Pay for all Ec2 instances irrespective of their usage.

### ECS with Fargate

If you want to delegate infrastructure management also to AWS : AWS Fargate.\
Alternate to EC2 instances. You dont need to provision any infrastructure.Fargate will take care of spinning up VMs.\
Serveless way to launch containers.\
Fargate analyse container for CPU and RAM and storage required. Fargate will provision a server resource. It will run the container.\
You pay only for the resource ur containers are actually consuming.\
Easily scaling\
You need to manage only application\
You cannot access underlying infrastructure.


## EKS

To use k8s in AWS. \

U create a cluster. AWS will provision Master nodes with k8s services installed. **Cluster and master nodes are managed by AWS.** Master Nodes replicated in multiple AZ. \
Storage Etcd( all configuration and state) : AWS takes care. \
Worker Nodes: U create EC2 instances and connect then to EKS cluster. \
Connect using kubectl commands. \
Worker node has docker agent and k8s processes. U need to install.\
You need to manage EC2 instances.

EKS with node group: semi-managed. Easy to configure.  \
EKS with Fargate: Fully managed worker nodes.


When to use EKS: If u are using k8s already. \
ECS is specific to AWS. At later point if u want to migrate to another platfom is difficult. EKS can be migrated easily to on premise K8s cluster. \
EKS is much better choice than ECS generally from orchestration perspective.

When to use ECS: If u have a less complex system, then deploy to ECS.

Amazon ECR: Repository for Container images.

## ECS Task Definition

In ECS we define an application using one or more task definitions.

For a web application with web, app and data containers, if u create one task definition for all 3 containers, then they are started together and co-located together on same host. Not a good practice. Do not scale independently.

Define one container per task definition. Each layer can scale seperately.

Containers in a task are co-located in the same host. AWS recommends that each task definition contains one container image and an optional sidecar that enhances the primary container. Mixing different containers and business functions in the same task definition is not recommended

Tasks can be run as
- Batch : Scheduled tasks
- Service: Continuous tasks
- Run-Task: Manual

## ECS Cluster

EC2 based - Cost effective for workloads which consistantly need high CPU. Direct access to host. Can configure additional logging/control.

Fargate - If complete capacity is needed only during some time or based on events.No management

### Other compute options

Local Zones

Wavelength

AWS outpost

ECS Anywhere

### ECS Permissions

There are two types of role

**Task Execution Role**: Used by Fargate agent. To download images, retrieve credentials from secret manager, publish logs to cloudwatch \
**Container Instance Role(Similar to above role)**: Used by EC2 Container agent. To communicate with ECS Service. For registeration and removal of nodes. \
**Task Role**: Used by application to talk to other AWS Services like s3, dynamodb. 

### Logging

Container logs can be published to CloudWatch using  awslogs driver

### Deployment option

- Rolling: Desired 6. Minimum 100%, Maximum 200%. There should be 6 tasks always running. Can have 12 tasks maximum. Gradually replace existing containers with new containers.
Desired 6. Minimum 50%, Maximum 200%. Reduced num ber of cycles.
- Blue Green. Old and new tasks running side by side. More expensive. Able to respond to issues with new version. Use double the capacity.

### Networking

- Host : Container is conneced to host network. Container port 3000 uses Host port 3000. You can run one instance of an application in a host. Port already used. Security issues as container can access other ports running on host.
- Bridge: Virtual layer between host and container. Map host port to container port. Dynamic Port mapping : Docker assigns a random host port for container. Inside host, application runs on same port , say 3000, but different host ports are mapped to this containers. problem is caller need to know the random ports. ELB hides dynamic ports. Problem: SG and NACL need to allow traffic on dynamic ports.
- AWS VPC : Each task is assigned a network interface and private IP.  No need to use dynamic port. Can configure SG at task level. You can use network monotoring tools for task.Problems: There is a limit on number of tasks running in a EC2 instance. **Enable ENI trunking.** Start up delay.

Bridge mode can be configured to use dynamic port allocation. Docker will assign an unused host port and map it to the container port in this setup. This flexibility comes with a drawback. Since the port is assigned from a wide range of ephemeral ports, we need to open the Security group and NACL firewalls to allow traffic on those ports. Both Host networking and awsvpc use static port allocation, and this makes it easier to monitor traffic.

Use awsvpc mode networking and specify **task level security group**. With the awsvpc mode, each Task is assigned an elastic network interface (ENI) and receives a private IP address within the VPC. We can attach a security group to the task network interface and configure the inbound rule to allow access from an upstream service security group. This approach would ensure that calls are allowed only in a specific direction and from specific callers. Host and Bridge networking modes do not support task-level security groups

  
### Service to Service Communication

1. Internal Load Balancer for each service: Health checks handled by LB. Simple and easy. But cost is more
2. Shared Internal Load Balancer; Seperate Traget Groups and use path based routing
3. CloudMap. Service Domains mapped in CloudMap. Maintain end point information in cloudMap. Map domain names. Suitable in aws vpc mode. Can cause DNS cache/stale issues. No Health checks.Application need to handle connection failures. 
4. AppMesh. Dont use a LB. Each container use an envoy side-car container. So a container only talks to its envoy. Envoy fwds request to another service's envoy. Envoys are controlled by App Mesh. Service Discovery + Load Balancing. Extremely low latency. AppMesh use CloudMap and delivers mapping to services. SideCar perform health checks. Service Discovery + LB functionalities. Extremely low latency. Allows mTLS auth.
  
Maintaining Transport Layer Security All the Way to Your Container: Using the Network Load Balancer with Amazon ECS .

How to use certificates

1. Storing the certificate and private key in the Docker image
2. Storing the certificates in AWS Systems Manager Parameter Store and Amazon S3
3. Storing the certificates in AWS Secrets Manager
4. Using self-signed certificates, generated as the Docker container is created
5. Building and managing a private certificate authority
6. Using the **new ACM Private CA** to issue private certificates
    - The private key for any private CA that you create with ACM Private CA is created and stored in a FIPS 140-2 Level 3 Hardware Security Module (HSM) managed by AWS. 
    - Add OpenSSL to the Docker image (if it is not already included).
    - Generate a key-value pair (a cryptographically related private and public key).
    - Use that private key to make a CSR.
    - Call the ACM Private CA API or CLI **issue-certificate** operation, which issues a certificate based on the CSR.
    - Call the ACM Private CA API or CLI **get-certificate** operation, which returns an issued certificate.

Notes:

Why should we use EKS/ECS over EC2?

- Zero down time deployments are difficult with EC2. 
- If u run containers in EC2, management is difficult.
- If its a monolith, then EC2 is fine.
- Even if we run as containers in EC2, we cannot run multiple instace of same application in a Ec2 due to port issues. See networking section above for details.
