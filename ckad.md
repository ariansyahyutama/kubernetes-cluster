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
