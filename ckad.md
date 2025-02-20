kubectl exec -i​t <Pod-Name> -- /bin/bash


1. Traditional vs. Kubernetes Architecture
- Traditional: Single monolithic app on dedicated server
- Kubernetes: Multiple small microservices

2. Kubernetes Key Features
- Uses replicas (multiple small web servers)
- Transient server deployment
- Independent nginx servers vs. traditional Apache setup
- Decoupled services

3. Microservices Implementation
- Connected via API calls
- Services link different components (frontend to backend)
- Easy replacement if services fail
- Independent operation

4. Important Notes
- NOT a traditional VM manager
- Requires different development approach
- Legacy apps need rewriting for cloud
- Decoupled, transient architecture

5. Benefits
- Better scalability
- Easier maintenance
- Independent service management
- More flexible deployment options

Key Takeaway: Kubernetes represents a fundamental shift from monolithic to microservice-based architecture, requiring new development approaches and application designs.


Here's a summary of the Legacy vs Cloud Architecture diagram and notes:

Architecture Comparison:
1. Legacy System
- Linear, vertical architecture
- Single monolithic application
- Direct, simple communication path
- Three-tier structure (app, server, database)

2. Cloud/Kubernetes System
- Distributed architecture
- Multiple interconnected components
- Complex communication patterns
- Multiple microservices (marked with 'N')
- Parallel processing capabilities

Technical Details:
- Communication: API call-driven
- Configuration: 
  * Stored in JSON format
  * Written in YAML
  * Agents convert YAML to JSON for database storage

Technology Note:
- Kubernetes uses Go Language
- Go characteristics:
  * Portable
  * Hybrid of C++, Python, and Java
  * Debated efficiency (mixed opinions on features)

Key Difference: Shows evolution from simple, linear legacy systems to complex, interconnected cloud architecture with distributed responsibilities and multiple service points.


<img width="878" alt="image" src="https://github.com/user-attachments/assets/1b696b85-425a-4e03-935d-84912cd56d26" />


Here's a summary of the Kubernetes terminology section:

1. Basic Concepts:
- Kubernetes orchestrates and manages containers
- Pods are the basic unit, containing one or more containers
- Containers in a Pod share IP address, storage, and namespace

2. Orchestration Management:
- Uses watch-loops (operators/controllers)
- Deployment manages ReplicaSets
- ReplicaSets maintain multiple pods with identical specs
- Jobs and CronJobs handle single/recurring tasks

3. API Objects:
- DaemonSet: Ensures one pod per node
- StatefulSet: Deploys pods in order, useful for non-cloud-friendly apps

4. Management Features:
- Labels: Help manage multiple pods across nodes
- Taints: Control pod scheduling on nodes
- Annotations: Store metadata that can't be used as selectors

5. Multi-tenancy:
- Allows multiple users/teams to share cluster resources
- Requires specific isolation mechanisms (detailed later)

The text provides a foundational overview of key Kubernetes concepts and components, focusing on how they work together in container orchestration.


Here's a summary of the Kubernetes security and resource management concepts:

1. Namespace
- Used for resource segregation
- Enables resource quotas and permissions
- Uses LimitRange for resource constraints
- Enforces unique Name values within namespace

2. Context
- Combines user, cluster name, and namespace
- Allows switching between different permission sets
- Configuration stored in ~/.kube/config
- Useful for managing dev/prod environments

3. Resource Limits
- Controls resource consumption by pods
- Can set minimum reserved resources
- Namespace-level limits override PodSpec limits

4. Pod Security Admission
- Beta feature for pod behavior control
- Applied at namespace level
- Three policy profiles:
  * Privileged
  * Baseline
  * Restricted

5. Network Policies
- Functions as cluster-internal firewall
- Controls Ingress and Egress traffic
- Can be configured based on:
  * Namespaces
  * Labels
  * Network traffic characteristics

These components form the core of Kubernetes' security and resource management framework, enabling fine-grained control over cluster resources and access.


Here's a complete summary of all Kubernetes master node components:

1. Kube-apiserver:
- Central control point for cluster operations
- Handles all traffic and validates actions
- Only component that connects to etcd
- Manages authentication and authorization

2. Kube-scheduler:
- Assigns Pods to nodes based on resources
- Uses pod-count algorithm by default
- Considers quotas, taints, and tolerations
- Allows custom scheduling configurations

3. Etcd Database:
- Stores cluster state and networking info
- Uses b+tree key-value store
- Appends values rather than modifying
- Handles concurrent updates via kube-apiserver
- Supports multi-master setup with failover
- Works with kubeadm for cluster deployment

4. Other Agents:
- Kube-controller-manager:
  * Core control loop daemon
  * Monitors cluster state
  * Manages multiple controllers (endpoints, namespace, replication)
  * Ensures desired state matches actual state

- Cloud-controller-manager:
  * Handles external cloud interactions
  * Manages cloud-specific operations
  * Requires --cloud-provider-external settings
  * Enables faster changes without affecting core processes

This forms the complete control plane architecture of a Kubernetes master node.


