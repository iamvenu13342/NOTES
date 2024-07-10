<h1>COMMANDS IN KUBERNETES</h1>
In Kubernetes, there are a variety of commands available to manage pods, replicas, and deployments. Below is a comprehensive list of commonly used commands for each of these resources using `kubectl`, the command-line tool for interacting with Kubernetes clusters.

<H2>Pods</H2>

1. **List Pods**
   ```bash
   kubectl get pods
   ```

2. **Describe a Pod**
   ```bash
   kubectl describe pod <pod-name>
   ```

3. **Create a Pod**
   ```bash
   kubectl apply -f <pod-definition.yaml>
   ```

4. **Delete a Pod**
   ```bash
   kubectl delete pod <pod-name>
   ```

5. **Logs of a Pod**
   ```bash
   kubectl logs <pod-name>
   ```

6. **Exec into a Pod**
   ```bash
   kubectl exec -it <pod-name> -- /bin/bash
   ```

7. **Get Pod's YAML/JSON Configuration**
   ```bash
   kubectl get pod <pod-name> -o yaml
   kubectl get pod <pod-name> -o json
   ```

8. **Port Forwarding to a Pod**
   ```bash
   kubectl port-forward <pod-name> <local-port>:<pod-port>
   ```

9. **Attach to a Pod**
   ```bash
   kubectl attach <pod-name>
   ```

10. **Copy Files to/from a Pod**
    ```bash
    kubectl cp <source> <pod-name>:<destination>
    kubectl cp <pod-name>:<source> <destination>
    ```

<H2>ReplicationControllers (older method)</H2>

1. **List ReplicationControllers**
   ```bash
   kubectl get rc
   ```

2. **Describe a ReplicationController**
   ```bash
   kubectl describe rc <rc-name>
   ```

3. **Create a ReplicationController**
   ```bash
   kubectl apply -f <rc-definition.yaml>
   ```

4. **Delete a ReplicationController**
   ```bash
   kubectl delete rc <rc-name>
   ```

5. **Scale a ReplicationController**
   ```bash
   kubectl scale rc <rc-name> --replicas=<number>
   ```

<H2>ReplicaSets</H2>

1. **List ReplicaSets**
   ```bash
   kubectl get rs
   ```

2. **Describe a ReplicaSet**
   ```bash
   kubectl describe rs <rs-name>
   ```

3. **Create a ReplicaSet**
   ```bash
   kubectl apply -f <rs-definition.yaml>
   ```

4. **Delete a ReplicaSet**
   ```bash
   kubectl delete rs <rs-name>
   ```

5. **Scale a ReplicaSet**
   ```bash
   kubectl scale rs <rs-name> --replicas=<number>
   ```

<H2>Deployments</H2>

1. **List Deployments**
   ```bash
   kubectl get deployments
   ```

2. **Describe a Deployment**
   ```bash
   kubectl describe deployment <deployment-name>
   ```

3. **Create a Deployment**
   ```bash
   kubectl apply -f <deployment-definition.yaml>
   ```

4. **Delete a Deployment**
   ```bash
   kubectl delete deployment <deployment-name>
   ```

5. **Update a Deployment**
   ```bash
   kubectl apply -f <updated-deployment-definition.yaml>
   ```

6. **Scale a Deployment**
   ```bash
   kubectl scale deployment <deployment-name> --replicas=<number>
   ```

7. **Rollout Status of a Deployment**
   ```bash
   kubectl rollout status deployment <deployment-name>
   ```

8. **Rollout History of a Deployment**
   ```bash
   kubectl rollout history deployment <deployment-name>
   ```

9. **Pause a Deployment**
   ```bash
   kubectl rollout pause deployment <deployment-name>
   ```

10. **Resume a Deployment**
    ```bash
    kubectl rollout resume deployment <deployment-name>
    ```

11. **Rollback a Deployment**
    ```bash
    kubectl rollout undo deployment <deployment-name>
    ```

12. **Get Deployment's YAML/JSON Configuration**
    ```bash
    kubectl get deployment <deployment-name> -o yaml
    kubectl get deployment <deployment-name> -o json
    ```

### Additional Notes

- To specify a namespace for any of these commands, use the `-n <namespace>` flag.
- To filter and format output, you can use flags like `-o wide`, `-o json`, or `-o yaml`.
- Many commands have additional flags and options; use `kubectl <command> --help` for more details on each command.

This list covers the essential commands for managing pods, replication controllers, replica sets, and deployments in Kubernetes.

In Kubernetes, services are used to expose a set of pods as a network service. Below are the commonly used commands to manage services using `kubectl`.

<H2>Services</H2>

1. **List Services**
   ```bash
   kubectl get services
   ```

2. **Describe a Service**
   ```bash
   kubectl describe service <service-name>
   ```

3. **Create a Service**
   ```bash
   kubectl apply -f <service-definition.yaml>
   ```

4. **Delete a Service**
   ```bash
   kubectl delete service <service-name>
   ```

5. **Expose a Pod or Deployment as a Service**
   ```bash
   kubectl expose pod <pod-name> --port=<port> --target-port=<target-port> --name=<service-name>
   kubectl expose deployment <deployment-name> --port=<port> --target-port=<target-port> --name=<service-name>
   ```

6. **Get Service's YAML/JSON Configuration**
   ```bash
   kubectl get service <service-name> -o yaml
   kubectl get service <service-name> -o json
   ```

7. **Edit a Service**
   ```bash
   kubectl edit service <service-name>
   ```

8. **Port Forwarding to a Service**
   ```bash
   kubectl port-forward service/<service-name> <local-port>:<service-port>
   ```

### Additional Notes

- **Specify Namespace:** To specify a namespace for any of these commands, use the `-n <namespace>` flag.
- **Filter and Format Output:** Use flags like `-o wide`, `-o json`, or `-o yaml` to filter and format output.
- **Additional Flags:** Many commands have additional flags and options; use `kubectl <command> --help` for more details on each command.

These commands cover the essential operations for managing services in a Kubernetes cluster.


