### System Analysis and Design

#### 0. Table of Contents  
- [0. Table of Contents](#0-table-of-contents)
- [1. Overview](#1-overview)
  - [1.1 Overview of Design Progress](#11-overview-of-design-progress)
  - [1.2 Implementation Platforms and Frameworks](#12-implementation-platforms-and-frameworks)
- [2. Architecture Refinement:](#2-architecture-refinement)
- [3. Select any two analysis mechanisms identified in your analysis model, find suitable solutions in your implementation platform, and then provide a detailed description of the corresponding design mechanisms](#3-select-any-two-analysis-mechanisms-identified-in-your-analysis-model-find-suitable-solutions-in-your-implementation-platform-and-then-provide-a-detailed-description-of-the-corresponding-design-mechanisms)
- [4. Design two use case realizations by incorporating the design mechanisms and the refined architecture](#4-design-two-use-case-realizations-by-incorporating-the-design-mechanisms-and-the-refined-architecture)
- [5. Architectural styles used and critical design decisions made in your solution](#5-architectural-styles-used-and-critical-design-decisions-made-in-your-solution)
- [6. Non-Functional Requirements have been addressed in your design: Why and How?](#6-non-functional-requirements-have-been-addressed-in-your-design-why-and-how)
- [7. Progress on prototyping](#7-progress-on-prototyping)
- [8. Open issues in your design model](#8-open-issues-in-your-design-model)
- [9. If you have used an AI tool or technology to generate an output that you either paraphrase or direct quote in your writing, you must cite and reference this output as a source in your reference list. If you have used an AI tool or technology in the process of completing the above tasks (for example, generating technical solutions, improving your architectural decisions, creating software prototypes, implementing the PoC, and enhancing the contents of your report), an acknowledgment of how you have used AI tools or technologies is required](#9-if-you-have-used-an-ai-tool-or-technology-to-generate-an-output-that-you-either-paraphrase-or-direct-quote-in-your-writing-you-must-cite-and-reference-this-output-as-a-source-in-your-reference-list-if-you-have-used-an-ai-tool-or-technology-in-the-process-of-completing-the-above-tasks-for-example-generating-technical-solutions-improving-your-architectural-decisions-creating-software-prototypes-implementing-the-poc-and-enhancing-the-contents-of-your-report-an-acknowledgment-of-how-you-have-used-ai-tools-or-technologies-is-required)
- [10. Project self-reflection](#10-project-self-reflection)
- [11. Contributions of team members](#11-contributions-of-team-members)



#### 1. Overview  

##### 1.1 Overview of Design Progress
Building upon the solid foundation laid during the requirements analysis and initial modeling phases, the SmartCampus project has now advanced into the detailed System Design stage. Our primary focus has shifted from defining the functional requirements—what the system should do—to specifying the technical implementation details—how the system will be built.

In this phase, we have successfully transformed the logical analysis model into a platform-specific design architecture. This involved refining the system boundaries and defining the specific RESTful API contracts that facilitate communication between our mobile clients and the backend services. In parallel, we have moved from conceptual data modeling to concrete database schema design, ensuring that our data structures in MySQL and MongoDB are optimized for performance and integrity. Furthermore, our prototyping efforts have been updated to reflect the finalized business flows, with a specific emphasis on refining the user experience for the high-frequency Meal Ordering and Campus Card Recharge modules.

##### 1.2 Implementation Platforms and Frameworks
To align with our user-centric strategy, the SmartCampus system is engineered as a "Mobile-First" application supported by a robust, cloud-native backend infrastructure.

Frontend Strategy Given that our primary user base consists of students who rely heavily on mobile devices, the system’s presentation layer prioritizes mobile accessibility. We have selected the WeChat Mini Program as our primary client platform due to its instant accessibility and high penetration rate among the student demographic. This is complemented by native mobile applications (iOS/Android) to leverage system-level capabilities where necessary. For administrative purposes, a web-based management dashboard built with Vue.js provides merchants and staff with a comprehensive interface for data management and operational monitoring.

Backend and Infrastructure Supporting the mobile frontend is a scalable microservices architecture built on the Spring Boot 3.x framework. This choice ensures rapid development cycles while maintaining the stability required for a campus-wide system. Security is managed through Spring Security integrated with OAuth 2.0, providing a secure and seamless authentication experience for mobile users.

Data Storage and Management Our data strategy employs a hybrid approach to optimize performance for different types of information. We utilize MySQL 8.0 as the primary relational database to handle transactional data requiring strict consistency, such as user accounts and financial records. For unstructured data, such as user reviews and system logs, MongoDB 6.0 offers the necessary flexibility. To ensure a smooth and responsive user experience on mobile devices, Redis 7.0 is implemented as a high-speed caching layer for frequently accessed data, such as daily menus and session tokens. The entire backend ecosystem is containerized using Docker and orchestrated via Kubernetes, ensuring consistent deployment across development and production environments.

#### 2. Architecture Refinement:  
  1. Present a platform-dependent architecture with a refined overall structure  
  2. Provide a list of subsystems and interfaces  
  3. Demonstrate interface specification in detail with one or several samples between your system and external systems, such as the public information website, third-party messaging system, map services, and so on  
  4. Select one subsystem as an example to specify its interfaces in detail  

#### 3. Select any two analysis mechanisms identified in your analysis model, find suitable solutions in your implementation platform, and then provide a detailed description of the corresponding design mechanisms  

#### 4. Design two use case realizations by incorporating the design mechanisms and the refined architecture  

#### 5. Architectural styles used and critical design decisions made in your solution  

#### 6. Non-Functional Requirements have been addressed in your design: Why and How?  

#### 7. Progress on prototyping  

#### 8. Open issues in your design model  

#### 9. If you have used an AI tool or technology to generate an output that you either paraphrase or direct quote in your writing, you must cite and reference this output as a source in your reference list. If you have used an AI tool or technology in the process of completing the above tasks (for example, generating technical solutions, improving your architectural decisions, creating software prototypes, implementing the PoC, and enhancing the contents of your report), an acknowledgment of how you have used AI tools or technologies is required 

#### 10. Project self-reflection  

#### 11. Contributions of team members
