**Resources**:
  * **Source code**: [https://github.com/luksa/kubernetes-in-action](https://github.com/luksa/kubernetes-in-action)

# Chap 2. First steps with Docker and K8s
## 2.2. Setting up a K8s cluster
* [Optional] Run the cluster in **KinD**:
  ```bash
  kind create cluster --name k8s-playground --config resources/me/config.yaml

  # verify the installation succeeded
  docker container ls

  minikube start --nodes 2 -p multinode-demo
  ```
### 2.2.1. Running a local single-node K8s cluster with Minikube
* Installing Minikube using the below command:
  ```bash
  curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/

  # verify the installation succeeded
  minikube version
  ```

* Starting a K8s cluster with Minikube:
  ```bash
  minikube start
  ```
  ![](./img/chap02/01.png)

* Installing Kubectl using the below command:
  ```bash
  curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" && chmod +x kubectl && sudo mv kubectl /usr/local/bin/

  # verify the installation succeeded
  kubectl version --short
  kubectl cluster-info
  ```
  ![](./img/chap02/02.png)
  ![](./img/chap02/03.png)

### 2.2.2. Using a hosted K8s cluster with Google K8s Engine
* Setting up a Google Cloud Project and downloading the necessary client binaries:
  * Following this instruction to get started: [https://cloud.google.com/container-engine/docs/before-you-begin](https://cloud.google.com/container-engine/docs/before-you-begin).
  * After that, you can install `kubectl` tool in `gcloud` by the below command:
    ```bash
    gcloud components install kubectl
    ```
* Creating a K8s cluster with 3-nodes:
  ```bash
  gcloud container clusters create kubia --num-nodes 3 --machine-type f1-micro --zone asia-northeast1-a
  ```

## 2.3. Running our first app on K8s
### 2.3.1. Deplying your Node.js app
* Prepare `manhcuong8499/kubia` image:
  ```bash
  cd resources/me/chap02/kubia
  docker build -t kubia .
  docker tag kubia manhcuong8499/kubia
  docker push manhcuong8499/kubia
  ```

* Deloy kubia app in K8s:
  ```bash
  kubectl run kubia --image=manhcuong8499/kubia --port=8080
  ```

#### 2.3.1.1. Introducing Pods
* Listing all pods in the cluster:
  ```bash
  kubectl get pods
  ```
  ![](./img/chap02/04.png)

* To see more information about the pods, use the `describe` command:
  ```bash
  kubectl describe pod kubia
  ```
  ![](./img/chap02/05.png)

### 2.3.2. Accessing your web application
* Tell K8s expose the `kubia` app to the outside world by creating a service object:
  ```bash
  kubectl expose pod kubia --type=NodePort --name kubia-http
  ```
  ![](./img/chap02/06.png)

* Listing all services in the cluster:
  ```bash
  kubectl get services
  ```
  ![](./img/chap02/07.png)

* Get IP-Address:
  ```bash
  minikube service kubia-http --url
  ```
  ![](./img/chap02/08.png)

* Testing the app:
  ```bash
  curl http://192.168.49.2:30884
  ```
  ![](./img/chap02/09.png)

### 2.3.3. Horizontally scaling the application
* Create kubia service:
  ```bash
  kubectl create deployment kubia --image=manhcuong8499/kubia
  ```
  ![](./img/chap02/10.png)

* Get replication controller:
  ```bash
  kubectl get replicasets
  ```
  ![](./img/chap02/11.png)
  * The `DISIRED` column shows the number of replicas that we want to have.

* To scale up the number of replicas of your pod, you need to change the desired replica count on the replication controller:
  ```bash
  kubectl scale deployment kubia --replicas=3
  ```
  ![](./img/chap02/12.png)

* Verify the number of pods:
  ```bash
  kubectl get all
  ```
  ![](./img/chap02/13.png)

* Get the IP-Address of the service:
  ```bash
  minikube service -p multinode-demo --all
  ```
  ![](./img/chap02/14.png)

* Testing:
  ```bash
  curl <url>
  ```
  ![](./img/chap02/15.png)

# Chapter 3. Pods: running containers in K8s
## 3.1. Creating pods from YAML or JSON descriptors
### 3.1.1. Examining a YAML descriptor of an existing pod
* Three important sections are found in almost all K8s resources:
  * `metadata`: Includes the name, namespace, labels, and other information about the pod.
  * `spec`: Contains the actual description of the pod's contents, such as the pod's containers, volumes, and other data.
  * `status`: Contains the current information about the running pod, such as what condition the pod is in, the description and status of each container, and the pod's internal IP and other basic info.

### 3.1.2. Creating a simple YAML descriptor for a pod
* When you implement a descriptor, you can use the `kubectl explain` command to get more information about the fields that you can use in the descriptor:
  ```bash
  kubectl explain pod.spec
  kubectl explain pods
  ```
### 3.1.3. Using `kubectl create` to create the pod
* To create the pod from your YAML file:
  ```bash
  kubectl create -f resources/me/chap03/kubia-manual.yaml
  ```
  ![](./img/chap03/01.png)

* Verity:
  ```bash
  kubectl get all
  ```
  ![](./img/chap03/02.png)

### 3.1.4. View application logs
* To view the logs of a container in a pod, use the `kubectl logs` command:
  ```bash
  kubectl logs kubia-manual
  ```
  ![](./img/chap03/03.png)

* If you have multiple pods running on the same app, use this command to view the logs of a specific container in a specific pod:
  ```bash
  kubectl logs kubia-manual -c kubia
  ```
  ![](./img/chap03/04.png)

### 3.1.5. Sending requests to the pod
* To forward your machine's local port 8888 to port 8080 of your `kubia-manual` pod:
  ```bash
  kubectl port-forward kubia-manual 8888:8080
  ```
  ![](./img/chap03/05.png)
  * The port forwarder is running and you can connect to your pod through the local port.

* Try sending requests to the forwarded port:
  ```bash
  curl localhost:8888
  ```
  ![](./img/chap03/06.png)

## 3.2. Organizing pods with labels
### 3.2.1. Specifying labels when creating a pod
* See the file [kubia-manual-with-labels.yaml](./resources/me/chap03/kubia-manual-with-labels.yaml) to understand. Now run this app.
  ```bash
  kubectl create -f resources/me/chap03/kubia-manual-with-labels.yaml
  ```
  ![](./img/chap03/07.png)

* List all available labels:
  ```bash
  kubectl get pods --show-labels
  ```
  ![](./img/chap03/08.png)

* List all pods with a specific label:
  ```bash
  kubectl get pods -l creation_method=manual --show-labels
  kubectl get pods -L creation_method,env --show-labels  # create a new column following the labels
  ```
  ![](./img/chap03/09.png)

### 3.2.2. Modifying labels on existing pods
* Assign label `creation_method=manual` to the `kubia-manual` pod:
  ```bash
  kubectl label pod kubia-manual creation_method=manual
  kubectl get pods --show-labels
  ```
  ![](./img/chap03/10.png)