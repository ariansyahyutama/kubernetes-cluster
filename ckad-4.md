
# Summary: Traditional Applications - Considerations for Kubernetes Implementation

A major challenge in implementing Kubernetes in production is adapting traditional application designs. Key points:

- Proper Kubernetes deployment requires more than simple containerization
- Traditional applications were designed for long-running processes with tight interdependencies
- Example: Apache web servers with continuous configuration and scaling to larger servers
- Traditional apps assume uninterrupted operation with persistent connections to resources
- Early containerization often occurred without redesigning applications
- This approach led to problems during resource failures, upgrades, or configuration changes
- Organizations must consider the cost and effort of redesign when moving to Kubernetes

# Summary: Decoupled Resources in Kubernetes

Decoupling resources is fundamental to Kubernetes architecture:

- Components are designed to be independent rather than using dedicated connections for the life of an instance
- This separation enables individual components to be removed, replaced, or rebuilt independently
- Instead of hard-coding resource connections, services act as intermediaries that facilitate flexible connections
- Applications no longer rely on single machines - multiple systems can be orchestrated as needed
- As Kubernetes evolves, resources are increasingly divided, making deployment simpler
- Developers can focus on optimizing specific functions without extensive concern for other system components

- # Summary: Transience in Kubernetes

Transience is a core principle in Kubernetes architecture:

- All components should be built with the expectation that other components are temporary
- Applications must anticipate that related components will regularly die and be rebuilt
- Designing for transient relationships enables easier updates and scaling
- "Upgrades" in Kubernetes differ from traditional applications - instead of updating existing instances, containers are terminated and replaced with new ones
- This replacement approach contrasts with traditional applications that were designed for long-term, persistent relationships
- Traditional design prioritized efficiency and ease of use through stable connections, while Kubernetes embraces impermanence# Summary: Flexible Framework in Kubernetes

Kubernetes employs a flexible architecture that functions like a coordinated group of independent elements:

- Multiple decoupled resources work together without permanent relationships, similar to schools of fish or pods of whales
- This approach provides flexibility, higher availability, and easier scalability
- Instead of using monolithic servers (like Apache), Kubernetes can deploy multiple smaller servers (like nginx) to distribute workload
- While this decoupled, flexible, transient framework is not the most efficient design
- It requires controllers and watch-loops to continuously monitor cluster state and reconcile it with declared configurations
- This architecture leverages hardware commoditization, enabling numerous smaller servers to handle large workloads instead of single, massive systems


# Summary: Kubernetes Resource Management

Understanding resource usage is crucial for successful Kubernetes deployments:

- Kubernetes enables easy cluster scaling to meet changing demands
- The kube-scheduler (or custom schedulers) uses PodSpec to determine optimal node placement
- Autoscalers can automatically add/remove nodes or pods based on defined plans
- cgroups settings limit CPU and memory usage by individual containers
- By default, pods consume CPU and memory as needed, coexisting with other Linux processes
- Pods are only scheduled to nodes with sufficient resources to meet all requests
- Kubernetes itself doesn't include cluster-wide resource monitoring - external tools like Prometheus are needed
- For local resource consumption issues, use `kubectl describe pod` command
- Resource consumption problems may only become apparent after pod termination

# Summary: CPU Management in Kubernetes

Key points about CPU requests and limits in Kubernetes:

- CPU is measured in millicores (or millicpu) - 1/1000th of a CPU core (e.g., 700m = 0.7 CPU)
- Containers exceeding CPU limits aren't killed but throttled
- When limits are set at pod level rather than container level, all containers in the pod are throttled together
- CPU usage can temporarily exceed limits as millicores aren't continuously evaluated
- Resource specifications are defined in YAML using:
  - spec.containers[].resources.limits.cpu
  - spec.containers[].resources.requests.cpu
- CPU values are absolute, not relative to cluster capacity or other pods' requirements
- 1 CPU in Kubernetes equates to:
  - 1 AWS vCPU
  - 1 GCP Core
  - 1 Azure vCore
  - 1 Hyperthread on a bare-metal Intel processor with Hyperthreading
 
# Summary: CPU Management in Kubernetes

Key points about CPU requests and limits in Kubernetes:

- CPU is measured in millicores (or millicpu) - 1/1000th of a CPU core (e.g., 700m = 0.7 CPU)
- Containers exceeding CPU limits aren't killed but throttled
- When limits are set at pod level rather than container level, all containers in the pod are throttled together
- CPU usage can temporarily exceed limits as millicores aren't continuously evaluated
- Resource specifications are defined in YAML using:
  - spec.containers[].resources.limits.cpu
  - spec.containers[].resources.requests.cpu
- CPU values are absolute, not relative to cluster capacity or other pods' requirements
- 1 CPU in Kubernetes equates to:
  - 1 AWS vCPU
  - 1 GCP Core
  - 1 Azure vCore
  - 1 Hyperthread on a bare-metal Intel processor with Hyperthreading
 
# Summary: Memory (RAM) Management in Kubernetes

Key points about memory management in Kubernetes:

- With Docker engine, the memory limit value specified in Kubernetes is converted to an integer and passed to the `docker run --memory <value> <image>` command
- Memory limit enforcement is less predictable than CPU limits
- When a container exceeds its memory limit, it may be restarted
- If a container requests more memory than specified in its request setting, the entire Pod may be evicted from the node
- Memory resources are specified in YAML using:
  - spec.containers[].resources.limits.memory
  - spec.containers[].resources.requests.memory


# Summary: Ephemeral Storage in Kubernetes

Key points about ephemeral storage management:

- Container files, logs, EmptyDir storage, and Kubernetes cluster data are stored on the host node's root filesystem
- Ephemeral storage is a limited resource that requires management like CPU and memory
- The Kubernetes scheduler only selects nodes with sufficient space to accommodate the sum of all container requests
- If a container or the combined containers in a pod exceed their storage limit, the pod will be evicted
- Ephemeral storage resources are specified in YAML using:
  - spec.containers[].resources.limits.ephemeral-storage
  - spec.containers[].resources.requests.ephemeral-storage


# Summary: Using Label Selectors in Kubernetes

Labels are a powerful mechanism in Kubernetes for organizing and selecting objects:

- Labels enable selection of objects that may not share other characteristics
- Operators (watch-loops) track and manage objects using labels
- Hard-coding the same label for different objects can cause conflicts when managed by different operators
- Consider using different labels to distinguish environments (e.g., development/production), departments, teams, and developers
- Selectors are namespace-scoped by default but can be expanded with the `--all-namespaces` argument
- Object metadata, including labels, can be viewed with `kubectl get object-name -o yaml`

Example:
```
apiVersion: v1
kind: Pod
metadata:
  annotations:
    cni.projectcalico.org/podIP: 192.168.113.220/32
  creationTimestamp: "2024-06-11T15:13:082"
  generateName: examplepod-1234-
  labels:
    app: examplepod
    pod-template-hash: 1234
  name: examplepod-1234-vtlzd
```

Key usage hint: When selecting pods by label, use equals sign (=) instead of colon (:) in command line, with no spaces, and apply `-l` or `--selector` options with kubectl.

# Summary: Using Built-in Labels and Node Selectors in Kubernetes

Kubernetes provides built-in labels and selector functionality:

- Labels can be used to query specific resources with the `--selector` flag
- Example command to select pods by app label:
```
ckad1$ kubectl -n test2 get --selector app=examplepod pod
```

- Nodes have built-in labels including `arch`, `hostname`, and `os`
- These built-in labels can be used to assign pods to specific node types
- Viewing node labels:
```
ckad1$ kubectl get node worker

...
creationTimestamp: "2024-06-10T14:23:042"
labels:
  beta.kubernetes.io/arch: amd64
  beta.kubernetes.io/os: linux
  kubernetes.io/arch: amd64
  kubernetes.io/hostname: worker
  kubernetes.io/os: linux
managedFields:
...
```

- The `nodeSelector` field in a pod specification can target specific nodes:
```
spec:
  nodeSelector:
    kubernetes.io/hostname: worker
  containers:
```

# Summary: Multi-Container Pods

While multi-container pods may seem to contradict Kubernetes' decoupling philosophy, they serve important purposes:

- Running an entire OS in a single container would sacrifice Kubernetes' granular scalability benefits
- Adding additional co-located containers in a pod can make sense for certain use cases
- Multiple containers in a pod can each be optimized and developed independently
- This approach maintains scalability while allowing containers to be repurposed to meet workload needs
- The multi-container model balances the benefits of co-location with the advantages of Kubernetes' flexible architecture

# Summary: Sidecar, Adapter, Ambassador and initContainers

Multi-container pod patterns serve specific purposes in Kubernetes:

**Sidecar Container:**
- Adds functionality missing from main container
- Keeps core application decoupled and scalable
- Handles auxiliary functions like logging (e.g., Fluentd) or monitoring (e.g., Prometheus)

**Adapter Container:**
- Modifies data on ingress or egress to match specific needs
- Standardizes output format between applications and monitoring tools
- Transforms varied application outputs into a unified format without modifying either side

**Ambassador Container:**
- Functions as a Kubernetes-native API gateway built on Envoy
- Provides external access without implementing custom services or ingress controllers
- Handles local connection proxying, request limiting, and routing from main container to outside world

**initContainer:**
- Runs and completes before main containers start
- Used for prerequisites, verification, or setup tasks
- Example: Running checksum verification and security scans before starting main containers
- Only after successful completion of initContainers will application containers start

spec:
  containers:
  - name: intended
    image: workload
  initContainers:
  - name: scanner
    image: scanapp

# Summary: Custom Resource Definitions in Kubernetes

Custom Resource Definitions (CRDs) extend Kubernetes' functionality beyond built-in resources:

- CRDs allow dynamic addition of new resources to Kubernetes clusters
- Once added, custom resources can be managed using standard tools like `kubectl`
- Custom resources are stored as structured data in the etcd database and accessed via kube-apiserver
- A controller/operator is needed to continuously monitor and reconcile the desired state for custom resources
- Custom operators automate tasks that would otherwise require manual intervention outside Kubernetes

Two methods to add custom resources:
1. Custom Resource Definitions - simpler but less flexible
2. Aggregated APIs - more flexible but requires writing a new API server

The Kubernetes reconciliation workflow for custom resources follows the same pattern as built-in resources:
- Operators use watch loops to monitor the kube-apiserver
- When actual state doesn't match declared state, the controller makes API calls to resolve the difference
- CRDs are added to the API path under `apiextensions.k8s.io/v1`

Resources for finding existing operators:
- Clusters using Calico already employ several CRDs
- GitHub has more information on the operator framework and SDK
- OperatorHub website catalogs existing operators that may be useful

# Understanding Custom Resource Definitions (CRDs) in Kubernetes

## What are CRDs?

A Custom Resource Definition (CRD) is a way to extend Kubernetes' API by creating your own custom resources. Think of it as adding a new type of object to Kubernetes beyond the built-in ones like Pods, Deployments, or Services.

## How CRDs Work - A Simplified Explanation

1. **Standard Kubernetes Resources**
   - By default, Kubernetes knows about certain objects like Pods, Services, etc.
   - These are part of the core API (like `apps/v1`, `core/v1`)

2. **Creating Your Own Custom Resources**
   - Let's say you want Kubernetes to understand a new type of object - like a "Database" or "MessageQueue"
   - CRDs let you teach Kubernetes about these new objects

3. **The Components**:
   - **Custom Resource Definition (CRD)**: The schema that defines your new resource type
   - **Custom Resource (CR)**: An instance of your custom type
   - **Custom Controller/Operator**: The software that knows how to manage your custom resources

## Real-World Example

Imagine you want to manage PostgreSQL databases in Kubernetes:

1. You create a CRD called `PostgreSQL` that specifies:
   - What properties a PostgreSQL resource should have (storage size, version, replicas)
   - What API group it belongs to (e.g., `databases.example.com/v1`)

2. Now users can create PostgreSQL resources:
```yaml
apiVersion: databases.example.com/v1
kind: PostgreSQL
metadata:
  name: my-production-db
spec:
  version: "13.1"
  storage: 100Gi
  replicas: 3
```

3. A PostgreSQL operator (controller) watches for these resources and:
   - Detects when a new PostgreSQL resource is created
   - Sets up actual PostgreSQL instances in the cluster
   - Configures backups, replication, etc.
   - Maintains the database according to the specification

## Benefits of CRDs

1. **Domain-Specific Abstractions**: Create objects that match your business domain
2. **Declarative Management**: Manage complex applications through simple YAML
3. **Kubernetes-Native Experience**: Use `kubectl` and other familiar tools
4. **GitOps Compatible**: Store your custom resources in Git like other Kubernetes resources
5. **Automation**: Controllers automate complex operational tasks

CRDs allow you to extend Kubernetes to handle specialized workloads while maintaining the same consistent user experience and workflow that makes Kubernetes powerful.


# Summary: Kubernetes Jobs and CronJobs

Kubernetes Jobs and CronJobs provide mechanisms for running transient microservices:

- **Jobs** (part of the `batch` API group):
  - Run pods to completion rather than continuously
  - Automatically restart failed pods until completion targets are met
  - Support both batch processing and one-off tasks
  - Can be configured with:
    - `parallelism`: Number of pods that can run concurrently
    - `completion`: Number of successful pod completions required for job success

- **CronJobs**:
  - Schedule Jobs using familiar Linux cron time syntax
  - Create Job resources at specified intervals
  - Pods should be idempotent as timing may result in skipped or duplicate executions

- **Concurrency Policy** options:
  - `Allow` (default): Runs new jobs alongside existing ones
  - `Forbid`: Skips new job if previous is still running
  - `Replace`: Cancels current job and starts a new one

Jobs and CronJobs enable efficient resource use by supporting decoupled, time-bound microservices that don't need to run continuously.
