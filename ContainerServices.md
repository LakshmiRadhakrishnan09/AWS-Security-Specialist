https://www.youtube.com/watch?v=AYAh6YDXuho

# Container Services in AWS

Container Services are suitable for microservice application ( Many services) . Not for a monolith.

Why container service?

Containers deployed on EC2 which have docker installed( 10 services on 3 EC2). Management becomes difficult. 
How many resources stll available? Where to schedule next container? Are docker containers still running? How to manage scaling(manually??)
To help managing containers - Container Orchestration tools( Docker Swarm, K8s, Mesos, Nomad, ECS)

## ECS

ECS: Container Orchestration service in AWS.
- Manage lifecycle of containers
- Load balace

ECS Cluster managed by AWS. 

Where are these containers running?\
On EC2 instances. These EC2 instances will have Docker Agent and ECS Agent. You have **delegated management of containers to ECS**. \
You still have to manage the actual Ec2 instances.\
You have to create Ec2. You have to join then to ECS cluster. When u schedule a new container you have to make sure there are enough EC2 instances available.
You need to manage OS, Docker agent, ECS agent.\
You have full access to Infrastructure. \
Infrastrure is managed by you.\
Pay for all Ec2 instances irrespective of their usage.

If you want to delegate infrastructure management also to AWS : AWS Fargate.\
Alternate to EC2 instances. You dont need to provision any infrastructure.Fargate will take care of spinning up VMs.\
Serveless way to launch containers.\
Fargate analyse container for CPU and RAM and storage required. Fargate will provision a server resource. It will run the container.\
You pay only for the resource ur containers are actually consuming.\
Easily scaling\
You need to manage only application\
You cannot access underlying infrastructure.

ECS with EC2:Container management by AWS. Infrastructure management by you. \
ECS with Fargate: Both container management and Infra management by AWS.

