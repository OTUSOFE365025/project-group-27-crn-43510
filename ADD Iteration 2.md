# ADD Iteration 2
**Project:** AIDAP — AI-Powered Digital Assistant Platform  
**Iteration:** 2 — Identifying Structures to Support Primary Functionality

## Step 1. Review Inputs (Same as Iteration 1)

| **Category** | **Details** |
|--------------|-------------|
| **Design purpose** | Greenfield system in a mature domain. Produce a detailed architecture to support construction of AIDAP that integrates with university systems and improves stakeholder communication and efficiency. |
| **Primary functional requirements** | UC-1:Because it enables core interactions by allowing users to query institutional information.<br>UC-2:Because it keeps users informed through timely updates and announcements.<br> UC-3: Because it supports essential academic content management for lecturers and students.<br>UC-6:Because secure authentication and personalization are vital for user trust and data protection.<br>UC-8:Because synchronized data ensures accurate and consistent system responses. |
| **Quality attributes** | <table><thead><tr><th>Scenario ID</th><th>Importance to the Customer</th><th>Difficulty of Implementation (According to Architect)</th></tr></thead><tbody><tr><td>QA-1</td><td>High</td><td>Medium</td></tr><tr><td>QA-2</td><td>High</td><td>Medium</td></tr><tr><td>QA-3</td><td>Medium</td><td>Medium</td></tr><tr><td>QA-4</td><td>High</td><td>High</td></tr><tr><td>QA-5</td><td>High</td><td>High</td></tr><tr><td>QA-6</td><td>Medium</td><td>High</td></tr><tr><td>QA-7</td><td>Medium</td><td>Medium</td></tr><tr><td>QA-8</td><td>High</td><td>High</td></tr></tbody></table> From this list, **QA-1 (Performance)**, **QA-2 (Usability)**, **QA-4 (Security)**, **QA-5 (Availability)**, and **QA-8 (Interoperability)** are selected as the **architectural drivers** for Iteration 1, since they directly support the main user interactions and system dependability for AIDAP’s core functions.|
| **Constraints** | All six constraints (CON-1 to CON-6) mentioned in **Constraints.md** are considered for AIDAP since they collectively ensure security, integration, performance, accessibility, and maintainability for the selected use cases.|
| **Architectural concerns** | All six architectural concerns (CRN-1 to CRN-6) mentioned in **Concerns.md** are considered for AIDAP as they collectively ensure data privacy, system scalability, team efficiency, collaboration, consistent development practices, and seamless integration across all components.|

---

## Step 2: Establish Iteration Goal by Selecting Drivers

The goal of this iteration is to address the general architectural concern of identifying and refining the internal structures and detailed interfaces required to support the primary functional requirements of the AIDAP system. Identifying these elements is crucial for understanding how functionality is realized across the service modules and for enabling effective work allocation to development teams, addressing the concerns of **CRN-3 (Workload Balancing and Efficiency)** and **CRN-4 (Collaboration and Communication)**.

In this second iteration, besides the functional implementation concern, the architect considers the system's primary use cases that were partially addressed in Iteration 1:

- **UC-3: Manage and Publish Course Materials**  
- **UC-8: Synchronize Institutional Data**

---

## Step 3: Choose One or More Elements of the System to Refine

In this iteration, we focus on refining the specific parts of AIDAP that directly support UC-3 and UC-8. This includes the backend services for course materials, the connectors that sync data with university systems, and the interfaces between the AI middleware and these services. These components are refined because they need to work together smoothly across different layers to handle academic content and keep institutional data updated.

---

## Step 4: Choose One or More Design Concepts That Satisfy the Selected Drivers

This step selects detailed design concepts and patterns that will ensure the reliable implementation of UC-3 (Manage Course Materials) and UC-8 (Synchronize Institutional Data) within the Task-Specific Services, Integration Connectors, and Data Layer being refined.

| Design Decisions and Location | Rationale and Assumptions |
|------------------------------|----------------------------|
| Use Service Decomposition for Course Material and Data-Sync Services | Breaking UC-3 and UC-8 into smaller backend services ensures each function (course materials, LMS sync, resource updates) is easier to maintain and can be assigned to different team members. This supports CRN-3 and CRN-4 by improving team productivity and reducing coupling thus improving efficiency .<br><br>**Discarded Alternatives:** Keep all features inside one large service, but this was rejected because it increases complexity, creates merge conflicts, and slows down development. |
| Adopt a Clear API Interface Pattern Between AI Middleware, Backend Services, and Connectors | A structured API design ensures smooth communication between the AI layer, backend services, and university systems. This helps reduce integration errors and supports cross-layer consistency for UC-3 and UC-8.<br><br>**Discarded Alternatives:** Allow each developer to design APIs independently, but this risks inconsistent endpoints, duplicates logic, and slows integration. |
| Apply a Data Synchronization Pattern for Institutional Systems (LMS, Course Updates, Resources) | Ensures data exchanged between external institutional systems and AIDAP stays accurate and up-to-date. This pattern supports reliability and reduces the chance of stale or mismatched academic data.<br><br>**Alternative Considered:** Perform manual or ad-hoc synchronization, but this can lead to data drift, increased maintenance work, and unreliable system behavior. |

---

## Step 5: Instantiate Architectural Elements, Allocate Responsibilities, and Define Interfaces

| Design Decisions and Location | Rationale |
|------------------------------|-----------|
| Instantiate separate backend services for Course Material Management and Data Synchronization | The system now needs real backend components & not just concepts. Creating two concrete services (CourseMaterialService and DataSyncService) makes it clear which team member handles UC-3 and which handles UC-8, supporting CRN-3 (workload balancing). It also ensures each service can be tested individually, supporting CRN-4. |
| Define explicit API interfaces between AI Middleware , Backend Services & Integration Connectors | Since Step 4 selected a structured API pattern, this step instantiates the actual API endpoints and request structures. Clear interfaces prevent integration mistakes and ensure each module knows exactly how to communicate. This also prepares the modules for unit testing and mock testing. |
| Instantiate Connector Modules for LMS, Course Database, and Institutional Systems | To support UC-8, the connectors described earlier must now be created as real modules with clear responsibilities: fetch, update, and validate data. Instantiating these connectors ensures reliable syncing and avoids ambiguity about where data transformations occur. |
| Allocate responsibilities across layers (Task-Specific Services → Connectors → Data Layer) | Responsibilities are now divided: services handle logic, connectors handle communication, and the data layer handles storage. This reduces coupling and clarifies which component owns which part of the workflow, preventing overlap or duplication in work. This also makes the management easier. |
| Define data exchange structures for synchronization | Since data-sync reliability is a driver in this iteration, the system must define the exact data formats and mapping rules used between systems. This prevents inconsistent data during syncing and reduces errors caused by unclear formats. |
