## ATAM Analysis (Risks, Non-Risks, Sensitivity, Trade-off)

### **QA-5 Scenario**  
#### A critical backend service (e.g., Course Material Service) fails during peak usage (e.g. start of semester). The system detects the failure and resumes operation using a redundant instance in less than 30 seconds, ensuring no downtime for the end-user.

## Architectural Decisions

| **ID** | **Architectural Decision** | Rationale for <30 Second Recovery |
| :--- | :--- | :--- |
| **AD1** | **Redundant Application Server Cluster** | Multiple active servers (Active Redundancy) means the backup is running instantly, achieving near-zero recovery latency. |
| **AD2** | **Asynchronous Primary-Replica Database Replication** | The Replica is ready for fast promotion, avoiding lengthy restores. Asynchronous writes minimize delay and recovery time (RTO). |
| **AD3** | **Load Balancer with Health Monitor** | The load balancer quickly detects failure and immediately redirects traffic to a healthy server, ensuring fast failover ($\le 30$ seconds). |
| **AD4** | **Circuit Breaker Fault Tolerance Pattern** | Instantly stops calls to failing external systems, preventing resource depletion and preserving the internal availability time window. |

## Risks

| **ID** | **Architectural Decision** | Explanation |
| :--- | :--- | :--- |
| **R1** | **AD1 / AD2** | **CRN-5 State Loss:** User session data is lost when a server fails, forcing the user to re-authenticate, which compromises the "no downtime" goal. |
| **R2** | **AD2** | **Data Inconsistency:** Using Asynchronous Replication risks losing recent database transactions that weren't copied to the Replica before the Primary failed. |
| **R3** | **AD3 / AD4** | **CRN-3 Undesigned Monitoring:** Operations staff lack a logging and alerting system to detect when a failover or Circuit Breaker trip occurs, making failure management impossible. |

## Non-Risks

| **ID** | **Architectural Decision** | **Explanation** |
| :--- | :--- | :--- |
| **N1** | **AD1** | Active Redundancy fundamentally ensures that the system will not completely stop, mitigating the core risk of single-point-of-failure causing a total outage. |
| **N2** | **AD3** | The Load Balancer effectively manages increased user demand (QA-1), preventing service overload and eliminating the risk of performance-related failure. |
| **N3** | **AD4** | The Circuit Breaker ensures that an unstable external system (QA-8 failure) cannot cause a cascading failure or resource exhaustion inside AIDAP. |

## Sensitivities

| **ID** | **Architectural Decision** | Explanation |
| :--- | :--- | :--- |
| **S1** | **AD3** | The actual recovery time is highly sensitive to the Load Balancer's health check interval. A longer interval directly increases the overall RTO. |
| **S2** | **AD2** | The potential data loss window is sensitive to the replication lag between the Primary and Replica database nodes at the time of failure. |
| **S3** | **AD4** | The effectiveness of failure isolation (QA-8) is sensitive to the trip threshold settings on the Circuit Breaker (e.g. how many failures over what time period ruins the circuit). |

## Trade-offs

| **ID** | **Architectural Decision** | Explanation |
| :--- | :--- | :--- |
| **T1** | **AD2** | **Trade-off: Availability (RTO $\le 30$ seconds) vs. Data Consistency:** Prioritizing fast recovery via Asynchronous Replication comes at the cost of accepting the risk of minor data loss (R2). |
| **T2** | **AD1 / AD3** | **Trade-off: Availability vs. Complexity/Cost:** Adding redundant App Servers and Load Balancer clusters significantly increases hardware, maintenance, and operational complexity. |
| **T3** | **AD1 / AD3** | **Trade-off: Availability vs. Effort:** The need for fast failover forces the use of complex, automated DevOps tools (CRN-3), increasing the initial development effort. |

## ATAM Scenario Analysis

<table style="width: 100%; border-collapse: collapse; border: 1px solid black;">
    <thead>
        <tr>
            <th colspan="9" style="border: 1px solid black; padding: 8px; background-color: #f2f2f2;"><b>Scenario Analysis</b></th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="border: 1px solid black; padding: 8px; font-weight: bold;"><b>Analysing Scenario</b></td>
            <td colspan="8" style="border: 1px solid black; padding: 8px;"><b>S1.1</b></td>
        </tr>
        <tr>
            <td style="border: 1px solid black; padding: 8px; font-weight: bold;"><b>Scenario</b></td>
            <td colspan="8" style="border: 1px solid black; padding: 8px;">A critical backend service (e.g., Course Material Service) fails during peak usage (e.g. start of semester). The system detects the failure and resumes operation using a redundant instance in less than 30 seconds, ensuring no downtime for the end-user.</td>
        </tr>
        <tr>
            <td style="border: 1px solid black; padding: 8px; font-weight: bold;"><b>Attributes</b></td>
            <td colspan="8" style="border: 1px solid black; padding: 8px;">(QA-1)Availability, (QA-5)Performance, (QA-8)Interoperability</td>
        </tr>
        <tr>
            <td style="border: 1px solid black; padding: 8px; font-weight: bold;"><b>Stimulus</b></td>
            <td colspan="8" style="border: 1px solid black; padding: 8px;">Internal service failure/ external system failure (due to Circuit Breaker).</td>
        </tr>
        <tr>
            <td style="border: 1px solid black; padding: 8px; font-weight: bold;"><b>Environment</b></td>
            <td colspan="8" style="border: 1px solid black; padding: 8px;">Peak Usage</td>
        </tr>
        <tr>
            <td style="border: 1px solid black; padding: 8px; font-weight: bold;"><b>Response</b></td>
            <td colspan="8" style="border: 1px solid black; padding: 8px;">System resumes operation in &le; 30 seconds.</td>
        </tr>
        <tr>
            <td style="border: 1px solid black; padding: 8px; font-weight: bold;"><b>Architecture Decision</b></td>
            <td style="border: 1px solid black; padding: 8px; font-weight: bold;"><b>Risk</b></td>
            <td style="border: 1px solid black; padding: 8px; font-weight: bold;"><b>Non-Risk</b></td>
            <td style="border: 1px solid black; padding: 8px; font-weight: bold;"><b>Sensitivity</b></td>
            <td style="border: 1px solid black; padding: 8px; font-weight: bold;"><b>Trade-off</b></td>
        </tr>
        <tr>
            <td style="border: 1px solid black; padding: 8px;"><b>AD1: Redundant App Server Cluster</b></td>
            <td style="border: 1px solid black; padding: 8px;">R1</td>
            <td style="border: 1px solid black; padding: 8px;">N1</td>
            <td style="border: 1px solid black; padding: 8px;"></td>
            <td style="border: 1px solid black; padding: 8px;">T2</td>
        </tr>
        <tr>
            <td style="border: 1px solid black; padding: 8px;"><b>AD2: Asynchronous DB Replication</b></td>
            <td style="border: 1px solid black; padding: 8px;">R2</td>
            <td style="border: 1px solid black; padding: 8px;"></td>
            <td style="border: 1px solid black; padding: 8px;">S2</td>
            <td style="border: 1px solid black; padding: 8px;">T1</td>
        </tr>
        <tr>
            <td style="border: 1px solid black; padding: 8px;"><b>AD3: Load Balancer with Health Monitor</b></td>
            <td style="border: 1px solid black; padding: 8px;">R3</td>
            <td style="border: 1px solid black; padding: 8px;">N2</td>
            <td style="border: 1px solid black; padding: 8px;">S1</td>
            <td style="border: 1px solid black; padding: 8px;">T2</td>
        </tr>
        <tr>
            <td style="border: 1px solid black; padding: 8px;"><b>AD4: Circuit Breaker Pattern</b></td>
            <td style="border: 1px solid black; padding: 8px;">R3</td>
            <td style="border: 1px solid black; padding: 8px;">N3</td>
            <td style="border: 1px solid black; padding: 8px;">S3</td>
            <td style="border: 1px solid black; padding: 8px;"></td>
        </tr>
    </tbody>
</table>

## ATAM Utility Tree

<div style="text-align: center;">
  <img src="Diagrams/Utility Tree.png" alt="Utility Tree" width="1000">
</div>
