### **Section C: Core Kubernetes Concepts**

---

#### **7. Replication Controllers and ReplicaSets**

Replication Controllers and ReplicaSets are crucial for maintaining the desired number of Pod replicas in a Kubernetes cluster. They ensure the availability and scalability of applications by managing the lifecycle of Pods.

##### **7.a. Ensuring Pod Availability and Scaling**

- **Objective:**
  - The primary role of Replication Controllers and ReplicaSets is to ensure that a specified number of Pod replicas are running at any given time.
  - They help in maintaining the desired state by automatically replacing Pods that fail, are deleted, or are otherwise unavailable.
  
- **Availability:**
  - **Ensuring Minimum Replicas:** These controllers make sure that a minimum number of Pod replicas are running. If a Pod fails or is deleted, the controller automatically creates a new Pod to replace it.
  - **Self-Healing:** They continuously monitor Pods and take corrective actions if Pods are not in the desired state.

- **Scaling:**
  - **Manual Scaling:** Users can manually scale the number of replicas by updating the configuration.
  - **Automatic Scaling:** Kubernetes can use Horizontal Pod Autoscalers to adjust the number of replicas based on CPU utilization or other metrics.
  - **Example Command for Manual Scaling:**
    ```sh
    kubectl scale deployment my-deployment --replicas=5
    ```

##### **7.b. ReplicationController vs. ReplicaSet**

- **ReplicationController:**
  - **Overview:**
    - ReplicationController is an older Kubernetes object responsible for ensuring that a specified number of Pod replicas are running.
  - **Key Features:**
    - **Selector:** Uses a label selector to identify Pods it manages.
    - **Scaling:** Allows scaling the number of Pod replicas up or down.
    - **Use Case:** Historically used for managing replication, but has been largely replaced by ReplicaSets in modern Kubernetes deployments.
  - **Configuration Example:**
    ```yaml
    apiVersion: v1
    kind: ReplicationController
    metadata:
      name: my-replicationcontroller
    spec:
      replicas: 3
      selector:
        app: my-app
      template:
        metadata:
          labels:
            app: my-app
        spec:
          containers:
          - name: my-container
            image: nginx
    ```

- **ReplicaSet:**
  - **Overview:**
    - ReplicaSet is the next-generation controller used to ensure that a specified number of Pod replicas are running. It is the successor to ReplicationController and provides more flexibility and enhanced functionality.
  - **Key Features:**
    - **Label Selector:** Supports more advanced label selectors compared to ReplicationController.
    - **Integration with Deployments:** ReplicaSets are typically managed by Deployments, which handle rolling updates and rollbacks.
    - **Use Case:** Preferred for new applications and deployments due to its advanced features and integration with Deployments.
  - **Configuration Example:**
    ```yaml
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: my-replicaset
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: my-app
      template:
        metadata:
          labels:
            app: my-app
        spec:
          containers:
          - name: my-container
            image: nginx
    ```

##### **7.c. Code Sample: ReplicaSet Configuration**

- **Creating a ReplicaSet:**
  - To create a ReplicaSet, you define a YAML manifest that specifies the desired state of the ReplicaSet, including the number of replicas and the Pod template.
  - **Example YAML Configuration:**
    ```yaml
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: example-replicaset
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: example-app
      template:
        metadata:
          labels:
            app: example-app
        spec:
          containers:
          - name: example-container
            image: nginx:latest
            ports:
            - containerPort: 80
    ```
  - **Deploying the ReplicaSet:**
    - Save the configuration in a file named `replicaset.yaml`.
    - Apply the configuration using `kubectl`:
      ```sh
      kubectl apply -f replicaset.yaml
      ```
    - Verify the ReplicaSet and Pods:
      ```sh
      kubectl get replicasets
      kubectl get pods
      ```

- **Scaling the ReplicaSet:**
  - **Manual Scaling:** To change the number of replicas, update the `replicas` field in the ReplicaSet configuration and apply the changes.
    ```sh
    kubectl scale replicaset example-replicaset --replicas=5
    ```
  - **Verifying Scaling:**
    ```sh
    kubectl get pods
    ```

- **Deleting a ReplicaSet:**
  - To delete a ReplicaSet and its Pods:
    ```sh
    kubectl delete replicaset example-replicaset
    ```

---

These notes provide a thorough understanding of Replication Controllers and ReplicaSets, including their roles in ensuring Pod availability and scaling, their differences, and practical examples for configuring and managing ReplicaSets.
