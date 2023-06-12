# KUBERNETES_VETTEC_20230313

## Module 1: Introduction and Core Concepts

---

## Introduction to Kubernetes

[Kubernetes](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/), also known as K8s, is an open-source system designed to automate deploying, scaling, and managing containerized applications. It groups containers that make up an application into logical units for easy management and discovery. Kubernetes is designed on principles that have enabled massive scalability, providing a platform to run distributed systems resiliently, scaling and healing your applications as needed.

## Understanding Kubernetes Architecture

[Understanding Kubernetes architecture](https://kubernetes.io/docs/concepts/architecture/): The architecture of Kubernetes is built around the concept of distributed computing. The two main components of this architecture are the control plane and the data plane.

- The Control Plane: This is made up of the Kubernetes master node which contains components like the kube-apiserver, etcd, kube-scheduler, and kube-controller-manager.

  - The kube-apiserver is the front-end of the control plane and the only component in the control plane that we interact with directly. It exposes the Kubernetes API, which is used by external users to perform operations on the Kubernetes cluster.

  - etcd is the consistent and highly-available key-value store used as Kubernetes' backing store for all cluster data.

  - The kube-scheduler watches for newly created Pods with no assigned node, and selects a node for them to run on based on a variety of factors such as resource availability, policy constraints, affinity and anti-affinity specifications, and more.

  - The kube-controller-manager runs controllers, which are the background threads that handle routine tasks in the cluster. Some of these tasks include Node controller, Replication controller, Endpoints controller, and Service Account & Token controllers.

- The Data Plane: The Data Plane consists of all the worker nodes in a Kubernetes cluster. Each node contains a kubelet, a tiny application that communicates with the control plane, and a kube-proxy.

  - The kubelet ensures that containers are running in a pod. It takes a set of PodSpecs that are provided and ensures that the containers described in those PodSpecs are running and healthy.

  - The kube-proxy maintains network rules on nodes. These network rules allow network communication to your Pods from network sessions inside or outside of your cluster.

## Nodes and Pods: The Fundamental Units of Kubernetes

[Nodes and Pods](https://kubernetes.io/docs/concepts/architecture/nodes/):

- A Node is a worker machine and could be either a virtual or a physical machine, depending on the cluster. Each node contains the services necessary to run Pods, including the container runtime, kubelet and kube-proxy.

- A Pod is the smallest and simplest unit in the Kubernetes object model that you create or deploy. A Pod represents processes running on your cluster and encapsulates an application's container (or a group of tightly-coupled containers), storage resources, a unique network IP, and options that govern how the container(s) should run.

Here's an example of a simple Pod YAML file:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  containers:
  - name: nginx-container
    image: nginx
    resources:
      requests:
        memory: "128Mi"
        cpu: "500m"
      limits:
        memory: "256Mi"
        cpu: "1000m"
    ports:
    - containerPort: 8080
```

- This example YAML creates a Pod with one container running the `nginx-image`. The `nginx` label makes it easy to locate this particular Pod among others. The container exposes port `8080` to allow for network communication.

  - You can create a Pod by saving this YAML to a file (e.g., nginx-pod.yaml) and then using the `kubectl apply -f nginx-pod.yaml` command.

## Services and Ingress: Exposing applications

[Services and Ingress](https://kubernetes.io/docs/concepts/services-networking/service/):

- A Service in Kubernetes is an abstraction which defines a logical set of Pods and a policy by which to access them, sometimes called a micro-service. The set of Pods targeted by a Service is usually determined by a Label Selector. While Pods have a lifecycle, the Service, acting as an abstraction, allows for a continuous and reliable way to route traffic to the application.

- Ingress, on the other hand, is a collection of routing rules that govern how external users access services running in a Kubernetes cluster.

Here's an example of a LoadBalancer Service YAML file:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-nginx-service
spec:
  type: LoadBalancer
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```

- This Service is of type LoadBalancer, meaning it will expose the service onto an IP address that's reachable externally. It routes traffic to Pods labeled with app: nginx, and it maps incoming traffic on port 80 to the target Pod's port 8080.

  - You can create this Service by saving the YAML to a file (e.g., my-nginx-service.yaml) and then using the kubectl apply -f my-nginx-service.yaml command.

  - This YAML creates a Service that exposes the Pods with the label `app: nginx` on the TCP port 80, and it will route the incoming traffic to the target port 8080 on the Pods.

## Labels and Selectors: Organizing resources

[Labels and Selectors](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/):

- Labels are key/value pairs that can be attached to Kubernetes objects such as Pods and Services to organize them into subsets. For example, you might label a set of Pods that belong to the same application using the `app` label. Here's how to add a label to a running Pod using the `kubectl label` command:

```bash
kubectl label pods nginx app=nginx
```

- In this command, nginx is the name of the Pod you're labeling, and app=nginx is the label you're adding.

- Selectors, on the other hand, allow users to filter resources based on labels. You can use selectors when creating a Service to specify which Pods the Service should route traffic to, or with kubectl commands to select a specific set of resources to operate on. Here's how to use a selector with a kubectl command:

```bash
kubectl get pods -l app=nginx
```

In this command, -l app=nginx is the selector, which tells kubectl to only get Pods that have the app=nginx label.

- Labels and Selectors in a real world scenario: Consider you have an application deployed in a Kubernetes cluster with three different environments: development, staging, and production. You could use labels to differentiate between the environments, for example by labeling all Pods in the development environment with env=dev. When you need to update the application in the development environment, you could use a selector to only select the Pods with the env=dev label for the update, ensuring that the application in the staging and production environments are not affected by the update.

- [Namespaces: Partitioning clusters](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/):
  - Namespaces are an excellent way to divide cluster resources between multiple users or teams. In a multi-tenant environment, they provide a level of isolation: users can only access resources in the namespaces they have access to. Furthermore, the resource names must be unique within a namespace, but not across namespaces.

### Module 1 Tasks

- [Set up Kubernetes with Docker Desktop](https://docs.docker.com/desktop/kubernetes/):
  - Docker Desktop's Kubernetes features are an accessible way to get started with a Kubernetes development environment. By enabling Kubernetes in the Docker Desktop settings, you can use Docker Desktop to set up a single-node Kubernetes cluster on your local machine.

- [Create and manage your first Pod and Service](https://kubernetes.io/docs/tutorials/kubernetes-basics/deploy-app/deploy-intro/):

## Create and manage your first Pod and Service

### Step 1: Install kubectl

To interact with Kubernetes, you'll need to install `kubectl`, the Kubernetes command-line tool. If it's not already installed, you can install it by following the instructions on the [official Kubernetes documentation](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

### Step 2: Check Kubernetes Cluster

Ensure that your Kubernetes cluster is up and running. If you are using Docker Desktop, you can check the status of Kubernetes in the Docker Desktop menu. Also, you can use the following `kubectl` command to get the status of your Kubernetes nodes:

```bash
kubectl get nodes
```

### Step 3: Create a Pod

A Pod is the smallest deployable unit of computing in Kubernetes. You can create a Pod by writing a Pod configuration file in YAML or JSON format, and then using `kubectl apply` to create the Pod based on that file.

Here's an example of a simple Pod configuration file (`pod.yaml`):

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx-container
    image: nginx
    ports:
    - containerPort: 80
```

This configuration file describes a Pod that runs one container. The container runs the `nginx` Docker image and listens on port 80.

You can create the Pod with this command:

```bash
kubectl apply -f pod.yaml
```

### Step 4: Verify the Pod is Running

You can use the following command to verify that your Pod is up and running:

```bash
kubectl get pods
```

### Step 5: Create a Service

A Service in Kubernetes is an abstraction that defines a set of Pods and a policy to access them. You can create a Service in a similar way to a Pod - by writing a Service configuration file and then using `kubectl apply` to create the Service.

Here's an example of a simple Service configuration file (`service.yaml`):

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: LoadBalancer
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

This configuration file describes a Service that routes traffic to any Pods with the label `app=nginx` on port 80.

You can create the Service with this command:

```bash
kubectl apply -f service.yaml
```

### Step 6: Verify the Service is Running

You can use the following command to verify that your Service is up and running:

```bash
kubectl get services
```

That's it! You've now created your first Pod and Service in Kubernetes.

## Module 2: Deployments and StatefulSets

---

## Replication and Deployment: Maintaining availability

- [Replication and Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) in Kubernetes are managed by Deployment objects. Deployments are designed to ensure that a specified number of identical Pods - which can be thought of as instances of an application - are always running. They can create new Pods, remove old Pods, and adopt existing Pods to maintain the desired state. A Deployment ensures availability of your application by automatically replacing instances that go down or are deleted. For example, if your web application's Deployment consists of four replicas, then Kubernetes will start up another instance as soon as it notices that the count has fallen below four.

Here's an example of a Deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        resources:
          requests:
            memory: "128Mi"
            cpu: "500m"
          limits:
            memory: "256Mi"
            cpu: "1000m"
        ports:
        - containerPort: 80
```

- In a real-world scenario, imagine you have a web application that serves users globally. Deployments can ensure that there are always enough instances of your application available to serve your users, even if some instances go down due to hardware failure or for maintenance.

## Rolling updates and Rollbacks: Versioning and recovering

- [Rolling updates and Rollbacks](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-back-a-deployment) are crucial aspects of versioning and recovering in Kubernetes.

### Rolling Updates

- A Rolling update is a process that allows you to update the version of an application without downtime. Imagine your application as a row of dominoes. Rather than knocking them all down at once (which would mean a period of downtime), Kubernetes "knocks them down" one at a time - meaning it updates each instance of your application individually.

  - This is done by bringing up a new instance with the updated version of your application, then shutting down an old instance, and repeating this process until all instances are updated. This way, your application remains available during the entire update process.

  - In a more technical term, when you update the Docker image in the Pod template of a Deployment, Kubernetes notices the change and begins the rolling update process.

  - Here's how you might perform a rolling update using kubectl: `kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v2`

  - This command updates the Docker image of your Deployment to a new version (`v2`).

### Rollbacks

- A Rollback is essentially an "undo" operation. Just like you might undo a typo in a text document, you can undo a Deployment in Kubernetes. This is particularly useful if you discover that your new application version has a bug and you want to go back to the previous version.

  - For instance, if you notice an issue after updating the Docker image in your Deployment, you can revert to the older version using a Rollback. Here's how you might perform a rollback using kubectl: `kubectl rollout undo deployment.v1.apps/nginx-deployment`

  - This command reverts your Deployment to the state it was in before the latest update.

  - The combination of Rolling updates and Rollbacks provides you with a powerful tool for versioning and recovering. You can update your applications confidently, knowing that if something goes wrong, you can easily revert back to the previous version

Here's an example of a Deployment YAML file with a Rolling update strategy:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3  # Initial number of instances
  selector:
    matchLabels:
      app: nginx  # This deployment applies to Pods with label "app=nginx"
  strategy:
    type: RollingUpdate  # The strategy used to replace old Pods by new ones
    rollingUpdate:
      maxUnavailable: 1  # Max number of pods that can be unavailable during the update
      maxSurge: 1  # Max number of pods that can be scheduled above the original number of pods
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2  # Use version 1.14.2 of nginx for the initial deployment
        resources:
          requests:
            memory: "128Mi"
            cpu: "500m"
          limits:
            memory: "256Mi"
            cpu: "1000m"
        ports:
        - containerPort: 80
```
  
- This example creates a Deployment with three replicas of nginx version 1.14.2.

  - To update the nginx version, change the image field to the new version (e.g., nginx:1.16.1) and apply the updated configuration with kubectl apply. Kubernetes will start a rolling update.

  - If you discover an issue with the new version, you can rollback the Deployment using: `kubectl rollout undo deployment nginx-deployment`

  - This command undoes the last Deployment update, returning the nginx Pods to the previous version.
