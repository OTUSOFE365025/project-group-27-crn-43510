# ADD Iteration 1
**Project:** AIDAP — AI-Powered Digital Assistant Platform  
**Iteration:** 1 — Establishing an overall system structure

## Step 1. Review Inputs

| **Category** | **Details** |
|--------------|-------------|
| **Design purpose** | Greenfield system in a mature domain. Produce a detailed architecture to support construction of AIDAP that integrates with university systems and improves stakeholder communication and efficiency. |
| **Primary functional requirements** | UC-1:Because it enables core interactions by allowing users to query institutional information.<br>UC-2:Because it keeps users informed through timely updates and announcements.<br> UC-3: Because it supports essential academic content management for lecturers and students.<br>UC-6:Because secure authentication and personalization are vital for user trust and data protection.<br>UC-8:Because synchronized data ensures accurate and consistent system responses. |
| **Quality attributes** | <table><thead><tr><th>Scenario ID</th><th>Importance to the Customer</th><th>Difficulty of Implementation (According to Architect)</th></tr></thead><tbody><tr><td>QA-1</td><td>High</td><td>Medium</td></tr><tr><td>QA-2</td><td>High</td><td>Medium</td></tr><tr><td>QA-3</td><td>Medium</td><td>Medium</td></tr><tr><td>QA-4</td><td>High</td><td>High</td></tr><tr><td>QA-5</td><td>High</td><td>High</td></tr><tr><td>QA-6</td><td>Medium</td><td>High</td></tr><tr><td>QA-7</td><td>Medium</td><td>Medium</td></tr><tr><td>QA-8</td><td>High</td><td>High</td></tr></tbody></table> From this list, **QA-1 (Performance)**, **QA-2 (Usability)**, **QA-4 (Security)**, **QA-5 (Availability)**, and **QA-8 (Interoperability)** are selected as the **architectural drivers** for Iteration 1, since they directly support the main user interactions and system dependability for AIDAP’s core functions.|
| **Constraints (summary)** | CON-1: Data privacy & security (university policy)<br>CON-2: Integrate with approved university systems (LMS, registration, calendars, email)<br>CON-3: Cloud-based, support up to 5,000 concurrent users, 99.5% uptime<br>CON-4: Support web, mobile, voice; meet accessibility standards<br>CON-5: AI must be efficient, multi-language, and updatable/modular<br>CON-6: Monitoring, logging, and maintainability requirements |
| **Architectural concerns (summary)** | CRN-1: Data privacy & compliance<br>CRN-2: Scalability & maintainability<br>CRN-3: Team workload balance and planning<br>CRN-4: Team collaboration & communication<br>CRN-5: Coding standards, documentation, and repo structure<br>CRN-6: Frontend-backend integration and data flow |
| **Business context** | Reduce time-to-answer for stakeholders, centralize access to institutional data, reduce help-desk load, and provide analytics and notifications; MVP in ~3–4 sprints. |

---
