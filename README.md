# Deploying and Scaling Containerized Workloads in a Kubernetes Service Cluster (AKS)

Along this incredible Azure cloud journey, I decided to take a brief foray into learning Azure Kubernetes Service (AKS) Clusters and how to configure and manage the containerized workloads within. Here is a step-by-step guide to how I built out my first Kubernetes cluster.

<br>
<br>


_Project Overview:_

Task 1: Register the Microsoft.Kubernetes and Microsoft.KubernetesConfiguration resource providers.
Task 2: Deploy an Azure Kubernetes Service cluster
Task 3: Deploy pods into the Azure Kubernetes Service cluster
Task 4: Scale containerized workloads in the Azure Kubernetes service cluster

<br>
<br>

**Task 1: Registering the Microsoft.Kubernetes and Microsoft.KubernetesConfiguration resource providers**

First, I'll need to register the following two resource providers to deploy an AKS Cluster:

* Microsoft.Kubernetes 
* Microsoft.KubernetesConfiguration resource providers

This can be done by opening the Cloud Shell and inputting the following commands:

![image](https://user-images.githubusercontent.com/105020710/178777072-26152b3e-6698-467a-859e-f0dff2da277a.png)

<br>
<br>

**Task 2: Deploy the Azure Kubernetes Service Cluster**

Time to navigate to Kubernetes services in the Azure portal and create the AKS Cluster.

![image](https://user-images.githubusercontent.com/105020710/178778227-9a4c9572-3959-4916-8d62-b89625c4607c.png)

<br>

After a few more steps, the AKS Cluster is ready to create!

![image](https://user-images.githubusercontent.com/105020710/178778550-d7d2e78d-8edd-4773-9f60-bda6ea3a63fb.png)

<br>
<br>

**Task 3: Deploying pods into the Azure Kubernetes Service cluster**

Time to create the node pools into the AKS Cluster. So far there's one node pool.

![image](https://user-images.githubusercontent.com/105020710/178800538-73fc22ce-5273-4b39-86c9-e5bb2a1ee27e.png)

Switching to the Bash Cloud Shell this time, I'm going to run these commands to retrieve the AKS cluster credentials and verify connectivity to the AKS cluster:

RESOURCE_GROUP='AKS-rg1'

AKS_CLUSTER='AKS1'

az aks get-credentials --resource-group $RESOURCE_GROUP --name $AKS_CLUSTER

To verify connectivity I'll run: kubectl get nodes

<br>

![image](https://user-images.githubusercontent.com/105020710/178801691-84d5c14c-ae21-47d8-86e9-1359eff8f6d2.png)

Now I'll deploy the nginx image from Docker using:

kubectl create deployment nginx-deployment --image=nginx

<br>

![image](https://user-images.githubusercontent.com/105020710/178802193-f3ca70b2-9dca-434c-b42b-ff856be296bb.png)

<br>

Now I need to make the pod available from the internet by running:

kubectl expose deployment nginx-deployment --port=80 --type=LoadBalancer

<br>

![image](https://user-images.githubusercontent.com/105020710/178802560-4a188b55-41f1-4d9a-85f9-597d56ca5c8b.png)

<br>
<br>

**Task 4: Finally we'll need to scsale the containerized workloads to ensure they can handle increasing demand.**

Here I'm going to horizontally scale the number of pods and the number of cluster nodes using the following command:

kubectl scale --replicas=2 deployment/nginx-deployment

<br>

![image](https://user-images.githubusercontent.com/105020710/178803475-04dfde44-36d3-47d5-bf0b-47d2468c3803.png)

<br>

What if I want to scale out the cluster by increasing the number of nodes to 2? Easy!

RESOURCE_GROUP='AKS-RG1'

AKS_CLUSTER='AKS1'

az aks scale --resource-group $RESOURCE_GROUP --name $AKS_CLUSTER --node-count 2

The output from this command will be a JSON style template detailing all of the attributes of the nodes.

<br>

![image](https://user-images.githubusercontent.com/105020710/178805256-e9c28889-099e-4bc3-990e-5ddf28ecc463.png)

<br>

Verify the scaling completed:

<br>

![image](https://user-images.githubusercontent.com/105020710/178805387-3432419a-f230-40bf-b2fa-5150350c36d0.png)


<br>

Let's add 10 replicas:

kubectl scale --replicas=10 deployment/nginx-deployment

<br>

![image](https://user-images.githubusercontent.com/105020710/178805545-4ada7bd3-fa78-47cf-b46b-88dc6c430516.png)

<br>

Finally, we're going to get a bit fancy here and review the pods distribution across the cluster nodes by running:

kubectl get pod -o=custom-columns=NODE:.spec.nodeName,POD:.metadata.name

<br>

![image](https://user-images.githubusercontent.com/105020710/178805822-24e4cbce-907b-4765-8ae5-9bfbafb93a65.png)

<br>
<br>

**Summary**

Wow, so here I was able get an Azure Kubernetes Service Cluster up quickly by registering the providers, deploying the AKS Cluster, deploying pods into the cluster, and horizontally scaling the containerized workloads by adding 10 replicas spread across the two nodes! 

Kubernetes is a truly unique and highly important technology in today's application production environments and I'm glad I was able to share these tasks with you today to get started on my AKS journey!
