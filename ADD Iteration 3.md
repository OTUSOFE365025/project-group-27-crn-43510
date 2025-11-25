## Step 1. Review Inputs

| **Category** | **Details** |
|--------------|-------------|
| **Design purpose** | Greenfield system in a mature domain. Produce a detailed architecture to support construction of AIDAP that integrates with university systems and improves stakeholder communication and efficiency. |
| **Primary functional requirements** | **UC-1:** Because it enables core interactions by allowing users to query institutional information.<br> **UC-2:** Because it keeps users informed through timely updates and announcements.<br> **UC-3:** Because it supports essential academic content management for lecturers and students.<br> **UC-6:** Because secure authentication and personalization are vital for user trust and data protection.<br> **UC-8:** Because synchronized data ensures accurate and consistent system responses. |
| **Quality attributes** | <table><thead><tr><th>Scenario ID</th><th>Importance to the Customer</th><th>Difficulty of Implementation (According to Architect)</th></tr></thead><tbody><tr><td>QA-1</td><td>High</td><td>Medium</td></tr><tr><td>QA-2</td><td>High</td><td>Medium</td></tr><tr><td>QA-3</td><td>Medium</td><td>Medium</td></tr><tr><td>QA-4</td><td>High</td><td>High</td></tr><tr><td>QA-5</td><td>High</td><td>High</td></tr><tr><td>QA-6</td><td>Medium</td><td>High</td></tr><tr><td>QA-7</td><td>Medium</td><td>Medium</td></tr><tr><td>QA-8</td><td>High</td><td>High</td></tr></tbody></table> From this list, **QA-1 (Performance)**, **QA-2 (Usability)**, **QA-4 (Security)**, **QA-5 (Availability)**, and **QA-8 (Interoperability)** are selected as the **architectural drivers** for Iteration 1, since they directly support the main user interactions and system dependability for AIDAP’s core functions.|
| **Constraints** | All six constraints (CON-1 to CON-6) mentioned in **Constraints.md** are considered for AIDAP since they collectively ensure security, integration, performance, accessibility, and maintainability for the selected use cases.|
| **Architectural concerns** | All six architectural concerns (CRN-1 to CRN-6) mentioned in **Concerns.md** are considered for AIDAP as they collectively ensure data privacy, system scalability, team efficiency, collaboration, consistent development practices, and seamless integration across all components.|

## Step 2: Establish Iteration Goal by Selecting Drivers

For this iteration, the architect focuses on the QA-5 (Availability) quality attribute scenario, while also addressing QA-1 (Performance) through scalability:
> A critical backend service (e.g. Course Material Service) fails during peak usage (e.g. start of semester). The system detects the failure and resumes operation using a redundant instance in less than 30 seconds, ensuring no downtime for the end-user.

## Step 3: Choose One or More Elements of the System to Refine

Since the goal of this iteration is to ensure high **Availability (QA-5)** and **Scalability (QA-1)** with a recovery time of less than 30 seconds, we must look beyond the purely logical services defined in Iteration 2 (like CourseMaterialService) and refine the **physical deployment structure** that hosts them.

The elements of the system that need refinement to achieve redundancy and fast failover are the following:

* **Application Server Nodes:** These are the physical or virtual hosts that run the backend microservices (like the CourseMaterialService). They must be replicated to enable failover.
* **API Gateway / Load Balancer:** The entry point to the system must be refined to include a **Load Balancer** capability. This new element is critical for distributing traffic across the application servers and detecting when one fails.
* **Database Server:** The central repository for all critical data (CourseDatabase) must be able to survive a server failure without data loss, thus requiring a **replication mechanism** meaning creating a replica configuration.
* **Integration Connectors:** While the connectors themselves were defined in Iteration 2, they must be refined to incorporate **Fault Tolerance** logic (like the Circuit Breaker pattern) to ensure that a failure in an external system doesn't affect our internal redundant servers.

## Step 4: Choose One or More Design Concepts That Satisfy the Selected Drivers

This step selects detailed design concepts and patterns that will ensure the reliable implementation of **QA-5 (Availability)** and **QA-1 (Performance)** by defining how the chosen elements from Step 3 will be structured and operate.

| Design Decisions and Location | Rationale and Assumptions |
| :--- | :--- |
| **Apply the Active Redundancy Tactic** | By replicating the application server nodes (Service Cluster) and running all instances simultaneously, the system can instantly switch traffic to a healthy instance upon failure. This directly achieves the **"resumes operation in less than 30 seconds"** requirement of **QA-5 (Availability)**. |
| **Introduce a Load-Balanced Cluster Pattern** | This pattern is necessary to manage the active redundant servers. The **Load Balancer** component which is a part of the refined API Gateway will continuously distribute traffic (**QA-1 Performance**) and perform health monitoring (**QA-5 Availability**) across the identical server nodes. |
| **Adopt the Primary-Replica Database Replication Tactic** | To ensure the **CourseDatabase** can survive a server failure without data loss and allow quick recovery, we must replicate the data layer. Writes will go to the Primary, and if the Primary fails, a Replica will be promoted. This directly supports **QA-5 (Availability)** for the data tier. |
| **Implement the Circuit Breaker Fault Tolerance Pattern** | This software tactic is applied to the **Integration Connectors**. If an external system (like the LMS) is slow or down, the Circuit Breaker prevents the connection attempts from blocking our internal redundant services, thereby preserving the **QA-5 (Availability)** of AIDAP itself. |
| **Employ a Health Monitor Pattern** | This mechanism is essential for the Load Balancer to meet the **30 second wimdow**. The Load Balancer will constantly ping the Application Server nodes (just like a heartbeat). If a node misses the heartbeat, it is immediately marked unhealthy and removed from the rotation. |
