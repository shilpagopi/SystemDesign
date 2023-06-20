# Google Cloud Platform (GCP)

### Components
##### Compute
* Compute Engine (IaaS): service to run VMs.
* App Engine(PaaS):host web and mobile applications
storage
* Containers: self-contained software environments without including OS. Containers run on VMs.
* Cloud Run Service: to run containers
* Google Kubernetes Engine (GKE): container orchestrator for multi-container apps
* Cloud Functions: deploy individual event-driven functions

##### Storage

* Cloud Storage: flat, object storage typically for images, videos, and log files. Offers multiple storage classes with immediate access, though differenlty priced based on frequency of access: Standard, Nearline, Coldline, and Archive.
* Filestore: NFS-compatible, hierarchical structure
* Cloud SQL : fully-managed service for 3 relational databases(MySQL, PostgreSQL, or Microsoft SQL Server) apt for online transaction processing. Difficult to scale them to handle high-volume, high-speed data
* NoSQL databases: scalable storing key/value pairs. Bigtable, Firestore, Firebase Realtime Database, and Memorystore
  * Bigtable is best for running large analytical workloads. 
  * Firestore is ideal for building client-side mobile and web applications. 
  * Firebase Realtime Database is best for syncing data between users in real time, such as for collaboration apps.
  * Memorystore is an in-memory datastore that’s typically used to speed up applications by caching frequently requested data.
* Cloud Spanner: relational database that’s massively scalable, but expensive
* Data warehouse: BigQuery - suited for data aggregation, analytical processing and business intelligence reporting.

##### Networking
* Virtual Private Cloud, or VPC: VPC can contain your VMs. Each VM in a VPC gets an IP address. You can also divide a VPC into subnets and define routes to specify how traffic should flow between them.
* VPC Network Peering : Connection from VPC to VPC
* Cloud VPN, Cloud Interconnect, or Peering: connections between a VPC and an on-premises network. A VPN sends encrypted traffic over the public Internet, 
whereas Cloud Interconnect and Peering communicate over a private, dedicated connection between your site and Google’s network. Cloud Interconnect is much more expensive than a VPN, 
but it provides higher speed and reliability since it’s a dedicated connection. Peering is free.
