<h1>Imperative commands</h1>

Imperative commands in Kubernetes are direct commands used to perform operations without needing to create and apply configuration files. Here is a list of commonly used imperative commands for managing services, pods, replicasets, and deployments:

### Services

1. **Create a Service**
   ```bash
   kubectl create service clusterip <service-name> --tcp=<port>:<target-port>
   ```

2. **Expose a Pod or Deployment as a Service**
   ```bash
   kubectl expose pod <pod-name> --port=<port> --target-port=<target-port> --name=<service-name>
   kubectl expose deployment <deployment-name> --port=<port> --target-port=<target-port> --name=<service-name>
   ```

3. **Delete a Service**
   ```bash
   kubectl delete service <service-name>
   ```

### Pods

1. **Run a Pod**
   ```bash
   kubectl run <pod-name> --image=<image> --port=<port>
   ```

2. **Delete a Pod**
   ```bash
   kubectl delete pod <pod-name>
   ```

### ReplicaSets

1. **Create a ReplicaSet**
   ```bash
   kubectl create rs <replicaset-name> --image=<image> --replicas=<number>
   ```

2. **Scale a ReplicaSet**
   ```bash
   kubectl scale rs <replicaset-name> --replicas=<number>
   ```

3. **Delete a ReplicaSet**
   ```bash
   kubectl delete rs <replicaset-name>
   ```

### Deployments

1. **Create a Deployment**
   ```bash
   kubectl create deployment <deployment-name> --image=<image>
   ```

2. **Scale a Deployment**
   ```bash
   kubectl scale deployment <deployment-name> --replicas=<number>
   ```

3. **Update a Deployment Image**
   ```bash
   kubectl set image deployment/<deployment-name> <container-name>=<new-image>
   ```

4. **Rollout Undo (Rollback) a Deployment**
   ```bash
   kubectl rollout undo deployment/<deployment-name>
   ```

5. **Delete a Deployment**
   ```bash
   kubectl delete deployment <deployment-name>
   ```

### General Management

1. **Apply Configuration Changes**
   ```bash
   kubectl apply -f <configuration-file.yaml>
   ```

2. **Edit a Resource Configuration**
   ```bash
   kubectl edit <resource-type> <resource-name>
   ```

3. **Get Resource Information**
   ```bash
   kubectl get <resource-type>
   kubectl get <resource-type> <resource-name>
   ```

4. **Describe a Resource**
   ```bash
   kubectl describe <resource-type> <resource-name>
   ```

5. **Delete a Resource**
   ```bash
   kubectl delete <resource-type> <resource-name>
   ```

### Examples

1. **Run an Nginx Pod**
   ```bash
   kubectl run nginx --image=nginx --port=80
   ```

2. **Expose the Nginx Pod as a Service**
   ```bash
   kubectl expose pod nginx --port=80 --target-port=80 --name=nginx-service
   ```

3. **Create a Deployment with 3 Replicas**
   ```bash
   kubectl create deployment nginx-deployment --image=nginx
   kubectl scale deployment nginx-deployment --replicas=3
   ```

These imperative commands allow you to quickly create, update, and manage Kubernetes resources directly from the command line without needing to define YAML or JSON configuration files.
