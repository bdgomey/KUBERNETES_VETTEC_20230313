# Kubernetes Learning Curriculum

## Module 1: Introduction and Core Concepts

---

## Introduction to Kubernetes

- [Kubernetes](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/), also known as K8s, is an open-source system designed to automate deploying, scaling, and managing containerized applications. It groups containers that make up an application into logical units for easy management and discovery. Kubernetes is designed on principles that have enabled massive scalability, providing a platform to run distributed systems resiliently, scaling and healing your applications as needed.

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

### _Here's an example of a LoadBalancer Service YAML file_

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

- [Labels and Selectors](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/): Labels are key/value pairs that can be attached to Kubernetes objects such as Pods and Services to organize them into subsets. For example, you might label a set of Pods that belong to the same application using the `app` label. Here's how to add a label to a running Pod using the `kubectl label` command:

```bash
kubectl label pods nginx app=nginx
```

- In this command, nginx is the name of the Pod you're labeling, and app=nginx is the label you're adding.

- Selectors, on the other hand, allow users to filter resources based on labels. You can use selectors when creating a Service to specify which Pods the Service should route traffic to, or with kubectl commands to select a specific set of resources to operate on. Here's how to use a selector with a kubectl command:

```bash
kubectl get pods -l app=nginx
```

- In this command, -l app=nginx is the selector, which tells kubectl to only get Pods that have the app=nginx label.

- Labels and Selectors in a real world scenario: Consider you have an application deployed in a Kubernetes cluster with three different environments: development, staging, and production. You could use labels to differentiate between the environments, for example by labeling all Pods in the development environment with env=dev. When you need to update the application in the development environment, you could use a selector to only select the Pods with the env=dev label for the update, ensuring that the application in the staging and production environments are not affected by the update.

- [Namespaces: Partitioning clusters](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/):
  - Namespaces are an excellent way to divide cluster resources between multiple users or teams. In a multi-tenant environment, they provide a level of isolation: users can only access resources in the namespaces they have access to. Furthermore, the resource names must be unique within a namespace, but not across namespaces.

### Module 1 Tasks

- [Set up Kubernetes with Docker Desktop](https://docs.docker.com/desktop/kubernetes/):
  - Docker Desktop's Kubernetes features are an accessible way to get started with a Kubernetes development environment. By enabling Kubernetes in the Docker Desktop settings, you can use Docker Desktop to set up a single-node Kubernetes cluster on your local machine.

- [Create and manage your first Pod and Service](https://kubernetes.io/docs/tutorials/kubernetes-basics/deploy-app/deploy-intro/):
## Here's how you can create and manage your first Pod and Service

### Create and manage your first Pod and Service

#### Step 1: Install kubectl

- To interact with Kubernetes, you'll need to install `kubectl`, the Kubernetes command-line tool. If it's not already installed, you can install it by following the instructions on the [official Kubernetes documentation](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

#### Step 2: Check Kubernetes Cluster

- Ensure that your Kubernetes cluster is up and running. If you are using Docker Desktop, you can check the status of Kubernetes in the Docker Desktop menu. Also, you can use the following `kubectl` command to get the status of your Kubernetes nodes:

```bash
kubectl get nodes
```

#### Step 3: Create a Pod

- A Pod is the smallest deployable unit of computing in Kubernetes. You can create a Pod by writing a Pod configuration file in YAML or JSON format, and then using `kubectl apply` to create the Pod based on that file.

### _Here's an example of a simple Pod configuration file (`pod.yaml`):_

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

- This configuration file describes a Pod that runs one container. The container runs the `nginx` Docker image and listens on port 80.

### _You can create the Pod with this command:_

```bash
kubectl apply -f pod.yaml
```

#### Step 4: Verify the Pod is Running

### _You can use the following command to verify that your Pod is up and running:_

```bash
kubectl get pods
```

#### Step 5: Create a Service

- A Service in Kubernetes is an abstraction that defines a set of Pods and a policy to access them. You can create a Service in a similar way to a Pod - by writing a Service configuration file and then using `kubectl apply` to create the Service.

### _Here's an example of a simple Service configuration file (`service.yaml`):_

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

- This configuration file describes a Service that routes traffic to any Pods with the label `app=nginx` on port 80.

### _You can create the Service with this command:_

```bash
kubectl apply -f service.yaml
```

#### Step 6: Verify the Service is Running

### _You can use the following command to verify that your Service is up and running:_

```bash
kubectl get services
```

That's it! You've now created your first Pod and Service in Kubernetes.

## Module 2: Deployments and StatefulSets

---

## Replication and Deployment: Maintaining availability

- [Replication and Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) in Kubernetes are managed by Deployment objects. Deployments are designed to ensure that a specified number of identical Pods - which can be thought of as instances of an application - are always running. They can create new Pods, remove old Pods, and adopt existing Pods to maintain the desired state. A Deployment ensures availability of your application by automatically replacing instances that go down or are deleted. For example, if your web application's Deployment consists of four replicas, then Kubernetes will start up another instance as soon as it notices that the count has fallen below four.

### _Here's an example of a Deployment:_

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

### _Here's an example of a Deployment YAML file with a Rolling update strategy:_

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


## Persistent Volumes (PV), Storage Classes, and Persistent Volume Claims (PVC)

- Persistent Volumes (PV), Storage Classes, and Persistent Volume Claims (PVC) are key components in Kubernetes for managing storage in a decoupled manner. PVs provide a way to provision and manage persistent storage resources, while PVCs are used by applications to request and use those resources. Storage Classes act as an abstraction layer between PVs and PVCs, allowing dynamic provisioning and customization of storage resources.

## Persistent Volumes (PV)

- A Persistent Volume (PV) is a piece of storage in the Kubernetes cluster that is provisioned by an administrator and made available for use by applications. PVs have a lifecycle independent of the Pods using them, meaning they can be dynamically provisioned, deleted, or reused. PVs provide a way to decouple storage management from application lifecycle management.

### _Here's an example of a PV YAML file:_

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: my-storage-class
  hostPath:
    path: /data/my-pv
```

- In this example, we create a PV with a storage capacity of 10Gi, accessible in a read-write mode by a single node at a time (`ReadWriteOnce`). The `persistentVolumeReclaimPolicy` determines what happens to the PV when it is released, and in this case, it is set to `Retain`, meaning the data will be retained even after the PV is released. The `storageClassName` specifies the name of the Storage Class associated with this PV, and `hostPath` defines the path on the host where the actual storage is located.

## Storage Classes

- A Storage Class is an abstraction layer that defines the properties and parameters for dynamically provisioning PVs. It allows administrators to define different classes of storage with various capabilities, such as performance, replication, or backup policies. PVCs can request storage from a specific Storage Class, and the cluster dynamically provisions a PV that matches the requirements defined in the Storage Class.

### _Here's an example of a Storage Class YAML file:_

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: my-storage-class
provisioner: example.com/my-provisioner
parameters:
  type: fast
```

- In this example, we define a Storage Class named `my-storage-class`. The `provisioner` field specifies the provisioner responsible for dynamically provisioning PVs. The `parameters` field allows additional customization, in this case specifying the storage type as `fast`.

## Persistent Volume Claims (PVC)

- A Persistent Volume Claim (PVC) is a request made by an application to use a certain amount of storage from a PV. PVCs are created by users and bound to PVs based on matching criteria defined in the PVC's specifications, such as storage size, access mode, and storage class.

### _Here's an example of a PVC YAML file:_

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
  storageClassName: my-storage-class
```

- In this example, we create a PVC named `my-pvc` requesting 5Gi of storage with a `ReadWriteOnce` access mode. The `storageClassName` field specifies the name of the Storage Class associated with this PVC, and the cluster will bind the PVC to a PV that matches the storage requirements defined in the Storage Class.

## StatefulSets: Managing stateful applications

- [StatefulSets](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/): StatefulSets are a bit like a class at school. Every student (Pod) in the class (StatefulSet) has their own unique roll number (unique identifier). This roll number doesn't change even if a student leaves and comes back to the class later. The teacher (Kubernetes) always knows which student is which by their roll number. The students also have their own personal lockers (persistent storage) where they can keep their things, and if they come back after a holiday, their lockers still contain all their stuff.
  - This is different from a gym class (Deployment), where it doesn't matter which specific students are in the class, just that there are enough students to play the game. There are no assigned lockers, so there's nowhere to keep personal items.
  - Now, imagine our school (Kubernetes cluster) has a computer lab (a database like MongoDB). Each computer (database instance) in the lab is assigned to a specific student. The student can leave notes on the computer and they'll still be there the next day because the computer is always assigned to the same student. That's how a StatefulSet works.

### _Here's an example of a StatefulSet YAML file for a MongoDB database:_

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mongo
spec:
  serviceName: "mongo"
  replicas: 3 # we want 3 students in our class
  selector:
    matchLabels:
      app: mongo # all the students in this class have the 'app=mongo' label
  template:
    metadata:
      labels:
        app: mongo # the label for our students
    spec:
      terminationGracePeriodSeconds: 10
      containers:
      - name: mongo
        image: mongo # the type of computer in our lab
        ports:
        - containerPort: 27017
        volumeMounts: # the lockers for our students
        - name: mongo-persistent-storage
          mountPath: /data/db
  volumeClaimTemplates: # the request for lockers in our school
  - metadata:
      name: mongo-persistent-storage
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi
```

- The `volumeClaimTemplates` section is like a request for lockers in our school. We're asking for storage that we can both read and write to (ReadWriteOnce), and we're asking for 1 gigabyte (1Gi) of storage for each locker.

- Persistent Volumes and Persistent Volume Claims: Managing storage

  - Persistent Volumes (PVs) and Persistent Volume Claims (PVCs) provide a method for you to provision storage in a Kubernetes cluster, decoupling the storage from the Pod lifecycle. A PV is a piece of storage in the cluster that's provisioned by an administrator, and a PVC is a request for storage by a user. PVCs can request specific sizes and access modes (like once read/write, many read-only).

  - For instance, if you're running a database in your Kubernetes cluster, you'd want to use a PV and PVC to ensure that your data persists even if your database Pod is deleted and replaced. The PVC ensures that the newly created Pod is still able to access the same volume of storage, and hence, the same data.

### Module 2 Tasks

## Task 1: Create Deployments and experiment with rolling updates and rollbacks

## Step 1: Create a Deployment

### _Let's start by creating a simple Deployment using a basic nginx image:_

```bash
kubectl create deployment my-nginx --image=nginx:1.19.1
```

### _In this command, `my-nginx` is the name of the Deployment and `nginx:1.19.1` is the Docker image it uses._

## Step 2: Check Your Deployment

### _Verify that your Deployment is working properly:_

```bash
kubectl get deployments
```

### _You should see output similar to the following:_

```bash
NAME       READY   UP-TO-DATE   AVAILABLE   AGE
my-nginx   1/1     1            1           2m
```

## Step 3: Perform a Rolling Update

### _Now, let's perform a rolling update to a newer version of nginx:_

```bash
kubectl set image deployment/my-nginx nginx=nginx:1.19.2
```

### _This command updates the Docker image used in your Deployment to version 1.19.2.

## Step 4: Verify the Rolling Update

### _You can see the progress of the rolling update like so:_

```bash
kubectl rollout status deployment/my-nginx
```

### _When the rollout has finished, you'll see a message like this:_

```bash
deployment "my-nginx" successfully rolled out
```

### _You can also use the `kubectl descibe deployment my-nginx` command again to see the updated image._

## Step 5: Perform a Rollback

### _If you find an issue with the new version of your application, you can roll back to the previous version like this:_

```bash
kubectl rollout undo deployment/my-nginx
```

## Step 6: Verify the Rollback

- Again, you can use the `kubectl rollout status deployment/my-nginx` command to see the progress of the rollback, and the `kubectl descibe deployment my-nginx` command to see the previous version.

- These steps demonstrate a simple use case of rolling updates and rollbacks with Deployments in Kubernetes. In a real-world scenario, you would probably have more complex configuration in your Deployments, and you'd use more advanced rollout and rollback strategies. But the basic principles remain the same.

## Task2: Set up a StatefulSet with a Persistent Volume with docker desktop

### _Create a Persistent Volume (PV) Manifest:_

   ```yaml
   apiVersion: v1
   kind: PersistentVolume
   metadata:
     name: my-pv
   spec:
     capacity:
       storage: 10Gi
     accessModes:
       - ReadWriteOnce
     persistentVolumeReclaimPolicy: Retain
     storageClassName: local
     hostPath:
       path: /data
   ```

### _Apply the Persistent Volume Manifest:_

   ```bash
   kubectl apply -f persistent-volume.yaml
   ```

- This will create the Persistent Volume in the Kubernetes cluster.

### _Create a Persistent Volume Claim (PVC) Manifest:_

   ```yaml
   apiVersion: v1
   kind: PersistentVolumeClaim
   metadata:
     name: my-pvc
   spec:
     accessModes:
       - ReadWriteOnce
     resources:
       requests:
         storage: 10Gi
     storageClassName: local
   ```

### _Apply the Persistent Volume Claim Manifest:_

   ```bash
   kubectl apply -f persistent-volume-claim.yaml
   ```

- This will create the Persistent Volume Claim in the Kubernetes cluster.

### _Create a StatefulSet Manifest:_

   ```yaml
   apiVersion: apps/v1
   kind: StatefulSet
   metadata:
     name: my-statefulset
   spec:
     replicas: 1
     serviceName: my-service
     selector:
       matchLabels:
         app: getting-started
     template:
       metadata:
         labels:
           app: getting-started
       spec:
         containers:
           - name: todo-list
             image: bjgomes/getting-started
             volumeMounts:
               - name: my-volume
                 mountPath: /data
     volumeClaimTemplates:
       - metadata:
           name: my-volume
         spec:
           volumeName: my-pvc
   ```

- Adjust the `replicas`, `image`, and other values according to your needs.

### _Apply the StatefulSet Manifest:_

   ```bash
   kubectl apply -f statefulset.yaml
   ```

- This will create the StatefulSet and associate it with the Persistent Volume Claim and Persistent Volume.

### _Create a Service Manifest for the StatefulSet:_

   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: my-service
   spec:
     selector:
       app: getting-started
     ports:
       - name: http
         protocol: TCP
         port: 80
         targetPort: 80
     clusterIP: None
   ```

### _Apply the Service Manifest:_

   ```bash
   kubectl apply -f service.yaml
   ```

- This will create a headless service for the StatefulSet.

### _Verify the StatefulSet, Persistent Volume, Persistent Volume Claim, and Service Status:_

   ```bash
   kubectl get statefulsets
   kubectl get pv
   kubectl get pvc
   kubectl get services
   ```

- Ensure that the StatefulSet is running, the Persistent Volume and Persistent Volume Claim are bound, and the Service is created for the StatefulSet.

- check to see if the pod is working `kubectl port-forward my-pod 8080:80`

- Please ensure proper indentation and spacing when creating and applying the YAML manifests.

## Module 3: Configuration and Secrets

- In this module, we will explore various techniques for managing configuration and secrets in Kubernetes. We will cover ConfigMaps, Secrets, Environment Variables, and Init Containers.

### ConfigMaps: Managing configuration data

- ConfigMaps are a way to manage and pass configuration data to your containers. They allow you to separate configuration from your application code and provide a flexible way to store and update configuration values. 

### _Here's an example of a ConfigMap in YAML format:_

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-configmap
data:
  database-url: mysql://my-database:3306
  api-key: abc123
```

### Secrets: Managing sensitive data

- Secrets are similar to ConfigMaps, but they are specifically designed to store sensitive data such as passwords, API keys, and certificates. They provide a secure way to manage and distribute this type of information to your containers. 

### _Here's an example of a Secret in YAML format:_

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
data:
  username: dXNlcm5hbWU=
  password: cGFzc3dvcmQ=
```

### Environment Variables: Passing configuration to containers

- Environment Variables are a commonly used method for passing configuration to containers. Kubernetes allows you to define environment variables at the pod or container level, and you can set their values using ConfigMaps or Secrets. 

### _Here's an example of defining environment variables in a pod specification:_

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: my-app-image
      env:
        - name: DATABASE_URL
          valueFrom:
            configMapKeyRef:
              name: my-configmap
              key: database-url
        - name: API_KEY
          valueFrom:
            secretKeyRef:
              name: my-secret
              key: api-key
```

### Init Containers: Initializing containers

- Init Containers are special containers that run and complete before the main containers in a pod start. They are useful for performing initialization tasks, such as fetching configuration data or setting up dependencies, before the main application container starts.

### _Here's an example of defining an init container in a pod specification:_

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  initContainers:
    - name: init-container
      image: init-image
      command: ["bash", "-c", "echo Initializing..."]
  containers:
    - name: main-container
      image: main-app-image
      # ...
```

- By using init containers, you can ensure that your main containers have all the necessary resources and dependencies available before they start running.

### Module 3 Tasks

## Demo 1: Creating and Consuming ConfigMap and Secret in a Pod

- In this demo, we will create a ConfigMap and a Secret, and then consume them in a Pod. This will demonstrate how to manage and pass configuration data and sensitive information to your containers.

### Step 1: Create a ConfigMap

- Create a file named `configmap.yaml` and open it in a text editor.
- Add the following YAML code to define a ConfigMap:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-configmap
data:
  database-url: mysql://my-database:3306
  api-key: abc123
```

- Save the file.

### Step 2

- Create a Secret

  - Create a file named `secret.yaml` and open it in a text editor.
  - Add the following YAML code to define a Secret:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
data:
  username: dXNlcm5hbWU=
  password: cGFzc3dvcmQ=
```

_Note: In this example, the values for `username` and `password` are base64-encoded. Make sure to encode your sensitive data accordingly._

- Save the file.

### Step 3: Create a Pod to Consume the ConfigMap and Secret

- Create a file named `pod.yaml` and open it in a text editor.
- Add the following YAML code to define a Pod that consumes the ConfigMap and Secret:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: my-app-image
      env:
        - name: DATABASE_URL
          valueFrom:
            configMapKeyRef:
              name: my-configmap
              key: database-url
        - name: API_KEY
          valueFrom:
            secretKeyRef:
              name: my-secret
              key: api-key
```

- Save the file.

### Step 4: Apply the ConfigMap, Secret, and Pod

- Open a terminal and navigate to the directory where you saved the YAML files.
- Run the following commands to apply the ConfigMap, Secret, and Pod:

```bash
kubectl apply -f configmap.yaml
kubectl apply -f secret.yaml
kubectl apply -f pod.yaml
```

### Step 5: Verify the Pod

- Run the following command to check the status of the Pod:

```bash
kubectl get pods
```

- Look for the Pod named `my-pod` and ensure its status is `Running`.

### Step 6: Access Configuration Data in the Pod

- Access the shell of the Pod by running the following command:

```bash
kubectl exec -it my-pod -- /bin/bash
```

- Once inside the Pod's shell, run the following command to print the environment variables:

```bash
env
```

- Check if the `DATABASE_URL` and `API_KEY` environment variables are present and have the correct values.

Congratulations! You have successfully created a ConfigMap and Secret and consumed them in a Pod. The environment variables in the Pod are now populated with the configuration data and sensitive information.

---

## Demo 2: Creating an Init Container for Pre-startup Configuration

In this demo, we will create an init container that performs pre-startup configuration tasks. This will help initialize dependencies or fetch configuration data before the main application container starts.

### Step 1: Create an Init Container

- Create a file named `pod.yaml` and open it in a text editor.
- Add the following YAML code to define a Pod with an init container:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  initContainers:
    - name: init-container
      image: init-image
      command: ["bash", "-c", "echo Initializing..."]
  containers:
    - name: main-container
      image: main-app-image
      # ...
```

- Save the file.

### Step 2: Apply the Pod

- Open a terminal and navigate to the directory where you saved the YAML file.
- Run the following command to apply the Pod:

```bash
kubectl apply -f pod

.yaml
```

### Step 3: Verify the Init Container

- Run the following command to check the status of the Pod:

```bash
kubectl get pods
```

- Look for the Pod named `my-pod` and ensure its status is `Running`.

### Step 4: Check the Logs of the Init Container

- Run the following command to view the logs of the init container:

```bash
kubectl logs my-pod -c init-container
```

- Verify that the log output contains the message "Initializing...".

Congratulations! You have successfully created an init container for pre-startup configuration. The init container runs and completes before the main application container starts, allowing you to perform any necessary initialization tasks.

---

_Note: Ensure that you have a Kubernetes cluster set up and configured before running these demos. Adjust the image names and other details in the YAML files according to your specific environment._