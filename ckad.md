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
