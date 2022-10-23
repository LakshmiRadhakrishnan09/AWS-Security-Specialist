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

ECS Cluster managed by AWS. 

### ECS with EC2

Where are these containers running?\
On EC2 instances. These EC2 instances will have Docker Agent and ECS Agent. You have **delegated management of containers to ECS**. \
You still have to manage the actual Ec2 instances.\
You have to create Ec2. You have to join then to ECS cluster. When u schedule a new container you have to make sure there are enough EC2 instances available.\
You need to manage OS, Docker agent, ECS agent.\
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

ECS with EC2:Container management by AWS. Infrastructure management by you. \
ECS with Fargate: Both container management and Infra management by AWS.

## EKS

To use k8s in AWS. \

U create a cluster. AWS will provision Master nodes with k8s services installed. Cluster and **master nodes** are managed by AWS. Master Nodes replicated in multiple AZ. \
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



Why should we use EKS/ECS over EC2?

- Zero down time deployments are difficult with EC2. 
- If u run containers in EC2, management is difficult.
- If its a monolith, then EC2 is fine.
