# Infrastructure

## AWS Zones
us-east-1

us-east-2

us-west-1

## Servers and Clusters

### Table 1.1 Summary
| Asset      | Purpose           | Size                                                                   | Qty                                                             | DR                                                                                                           |
|------------|-------------------|------------------------------------------------------------------------|-----------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
| server clusters | server clusters with 3 nodes for an application/service  | t3.micro | 2 replicas with 3 nodes each | replicated in another region |
| load balancer clusters | load balancer clusters to prevent pointing directly at the web server application  |  | 2 replicas with 3 nodes each | replicated in another region |
| database clusters | database clusters with 3 nodes to store the data  | t3.micro | 2 replicas with 3 nodes each | replicated in another region |
| github repo | Used to store the terraform code |  |  |  |
| kubernetes monitoring stack | For monitoring the application |  |  |  |
| Monitoring platform | Prometheus and Grafana  |  |  |  |
| ssh keys | for interacting with EC2 instances |  |  |  |
| AMI image | custom AMI image created by us |  |  |  |

### Descriptions
The Server cluster with 3 nodes hosts a service and 3 nodes helps in HA.
The load balancer clusters with 3 load balancers helps in preventing pointing directly at the web server application.
The database clusters with 3 nodes reposible to store the user data, application data etc.

## DR Plan
### Pre-Steps:
Ensure the infrastructure is set up and working in the DR site.

## Steps:

Increase replica set to 7 nodes with an Arbiter in a third DataCenter.

Replace one of the nodes with an Arbiter node on a low-end host in a separate DC with visibility to the other two DCs.

Reconfigure the replica set to remove all unavailable nodes and introduce an arbiter in the remaining DC at the time of failure.

 Assume DC1 is lost. Reconfigure DC2 to remove all nodes from DC1 and add an arbiter node to DC2 so a new primary can be elected.

Create a cloud load balancer and point DNS to the load balancer. This way you can have multiple instances behind 1 IP in a region. During a failover scenario, you would fail over the single DNS entry at your DNS provider to point to the DR site. This is much more intelligent than pointing to a single instance of a web server.

Have a replicated database and perform a failover on the database. While a backup is good and necessary, it is time-consuming to restore from backup. In this DR step, we would have already configured replication and would perform the database failover. Ideally, our application would be using a generic CNAME DNS record and would just connect to the DR instance of the database.