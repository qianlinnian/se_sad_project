### System Analysis and Design

#### 0. Table of Contents  
- [System Analysis and Design](#system-analysis-and-design)
  - [0. Table of Contents](#0-table-of-contents)
  - [1. Overview](#1-overview)
    - [1.1 Overview of Design Progress](#11-overview-of-design-progress)
    - [1.2 Implementation Platforms and Frameworks](#12-implementation-platforms-and-frameworks)
  - [2. Architecture Refinement:](#2-architecture-refinement)
    - [2.1. Platform-dependent architecture with a refined overall structure](#21-platform-dependent-architecture-with-a-refined-overall-structure)
      - [2.1.1 Architectural Refinement Overview](#211-architectural-refinement-overview)
      - [2.1.2 Technology Stack Mapping](#212-technology-stack-mapping)
      - [2.1.3 Microservices Decomposition](#213-microservices-decomposition)
      - [2.1.4 Hybrid Data Storage Strategy](#214-hybrid-data-storage-strategy)
      - [2.1.5 Deployment \& Security](#215-deployment--security)
    - [2.2. Provide a list of subsystems and interfaces](#22-provide-a-list-of-subsystems-and-interfaces)
    - [2.3. Demonstrate interface specification in detail with one or several samples between your system and external systems](#23-demonstrate-interface-specification-in-detail-with-one-or-several-samples-between-your-system-and-external-systems)
    - [2.4. Select one subsystem as an example to specify its interfaces in detail](#24-select-one-subsystem-as-an-example-to-specify-its-interfaces-in-detail)
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

##### 2.1. Platform-dependent architecture with a refined overall structure  

This section refines the logical layered architecture from Assignment 2 into a **platform-specific implementation**, mapping abstract components to concrete technologies. The key evolution is transforming generic architectural layers into executable deployment configurations with specific frameworks, versions, and integration strategies.

<div align="center">
  <img src="diagrams/platform_architecture.png" alt="SmartCampus Platform Architecture" style="width:90%; max-width:900px;"/>
  
**Figure 2.1: Platform-Dependent Architecture (5-Layer Deployment Model)**
</div>

###### 2.1.1 Architectural Refinement Overview

**From Assignment 2 (Logical) to Assignment 3 (Physical):**

| Aspect | Assignment 2 (Logical Architecture) | Assignment 3 (Platform-Dependent Architecture) |
|--------|-------------------------------------|-----------------------------------------------|
| **Presentation Layer** | Abstract "mobile client" | WeChat Mini Program (HBuilderX + uni-app) |
| **Business Logic** | Generic "service layer" | 4 Spring Boot microservices (ports 8081-8084) |
| **Data Access** | "Database layer" | MySQL 8.0 + MongoDB 6.0 + Redis 7.0 (hybrid storage) |
| **Authentication** | OAuth 2.0 (conceptual) | JWT (JSON Web Token) with HS256 signing |
| **Deployment** | Not specified | Docker containers orchestrated by Kubernetes |
| **Communication** | Abstract API calls | RESTful HTTP + JWT authentication via Nginx gateway |

**Microservices Scope Refinement:**

Building upon Assignment 2's four business subsystems (Library, Academic, Life Services, Logistics), this assignment focuses detailed architectural refinement on the Life Services domain (particularly meal ordering, campus card, and feedback functionalities) as a representative case study. This targeted approach allows in-depth exploration of microservices decomposition, hybrid data storage strategies, and external API integrations without redundantly documenting similar patterns across all subsystems. The four core services (`auth-service`, `meal-service`, `card-service`, `feedback-service`) demonstrate the complete architecture blueprint applicable to other domains.

**Authentication Refinement Rationale:**

The evolution from OAuth 2.0 to JWT reflects practical mobile-first optimization: JWT's stateless, self-contained tokens eliminate server-side session storage (essential for Kubernetes horizontal scaling), reduce bandwidth for WeChat Mini Program requests, and enable independent signature validation across microservices without centralized authentication queries. This shift ensures that our mobile-first architecture remains lightweight, secure, and scalable.

###### 2.1.2 Technology Stack Mapping

**Core Platform Technologies:**

| Layer | Technology | Version | Key Capabilities |
|-------|-----------|---------|------------------|
| **Client** | WeChat Mini Program (uni-app) | Vue 3 | HBuilderX IDE, one-click deployment, 90%+ campus penetration |
| **Gateway** | Nginx + Spring Security | 1.24+ / 6.2+ | SSL termination, JWT authentication, rate limiting |
| **Services** | Spring Boot + Spring MVC | 3.3+ (JDK 17+) | Microservices, RESTful APIs, embedded Tomcat |
| **Data Access** | Spring Data JPA + MyBatis-Plus | 3.2+ / 3.5.x | CRUD abstraction, dynamic SQL, MongoDB driver |
| **Database** | MySQL + MongoDB + Redis | 8.0 / 6.0 / 7.0 | ACID transactions, flexible documents, sub-ms caching |
| **Infrastructure** | Docker + Kubernetes | 24.x / 1.28+ | Containerization, orchestration, auto-scaling |

###### 2.1.3 Microservices Decomposition

The four business subsystems identified in Assignment 2 are decomposed into independently deployable microservices:

**Core Business Services:**

| Service Name | Port | Responsibilities | Database | External Dependencies |
|--------------|------|-----------------|----------|----------------------|
| `auth-service` | 8081 | User authentication, JWT issuance, session management | MySQL (`users`, `roles`) | WeChat Login API |
| `meal-service` | 8082 | Restaurant/menu management, order processing, ratings & ranking | MySQL + MongoDB + Redis | Payment gateway API |
| `card-service` | 8083 | Campus card balance query, recharge processing | MySQL + Redis | Alipay/WeChat Pay API |
| `feedback-service` | 8084 | User feedback submission, admin review workflow | MySQL + MongoDB | - |

###### 2.1.4 Hybrid Data Storage Strategy

The hybrid multi-modal storage architecture refines Assignment 2's generic "data layer" into three specialized databases:

| Data Type | Storage | Schema Design | Access Pattern |
|-----------|---------|---------------|----------------|
| **Transactional** (user accounts, card balance, orders) | MySQL 8.0 InnoDB | Normalized tables with foreign keys | Spring Data JPA + MyBatis-Plus |
| **Semi-structured** (reviews, feedback, logs) | MongoDB 6.0 | Flexible BSON documents | Spring Data MongoDB |
| **High-frequency** (menu cache, rankings, sessions) | Redis 7.0 | Key-value + Sorted Sets; TTL 1-2h | Spring Cache annotations |

###### 2.1.5 Deployment & Security

| Environment | Deployment | Configuration |
|-------------|------------|---------------|
| **Development** | Docker Compose | Hot-reload, local DBs |
| **Production** | Kubernetes (3+ nodes) | 2-3 replicas/service, auto-scaling, Persistent Volumes |

**Security:** HTTPS enforcement, JWT (HS256, 2h expiration), BCrypt hashing, parameterized queries, daily backups

##### 2.2. Provide a list of subsystems and interfaces  

##### 2.3. Demonstrate interface specification in detail with one or several samples between your system and external systems

##### 2.4. Select one subsystem as an example to specify its interfaces in detail  


#### 3. Select any two analysis mechanisms identified in your analysis model, find suitable solutions in your implementation platform, and then provide a detailed description of the corresponding design mechanisms  

#### 4. Design two use case realizations by incorporating the design mechanisms and the refined architecture  

#### 5. Architectural styles used and critical design decisions made in your solution  

#### 6. Non-Functional Requirements have been addressed in your design: Why and How?  

#### 7. Progress on prototyping  

#### 8. Open issues in your design model  

#### 9. If you have used an AI tool or technology to generate an output that you either paraphrase or direct quote in your writing, you must cite and reference this output as a source in your reference list. If you have used an AI tool or technology in the process of completing the above tasks (for example, generating technical solutions, improving your architectural decisions, creating software prototypes, implementing the PoC, and enhancing the contents of your report), an acknowledgment of how you have used AI tools or technologies is required 

#### 10. Project self-reflection  

#### 11. Contributions of team members
