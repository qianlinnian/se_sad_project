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
##### 2.2.2 Dishes recommendation and ranking Subsystem

Here is the fully translated version of your table in English:

| API Interface | Method | Parameters | Description |
|---------------|--------|------------|-------------|
|/api/dishes/rankings | GET | sortType, category, restaurantId | Retrieves a ranked list of dishes, supporting sorting by popularity or rating. |
| /api/dishes/{dishId}/vote | POST | userId, rating, comment | Allows a student to rate and comment on a specific dish. If the student has already voted, the existing record is updated; otherwise, a new vote is created. |
| /api/dishes/{dishId} | GET | - | Retrieves detailed information about a specific dish, including name, price, description, associated restaurant, average rating, and total number of votes. |
| /api/dishes/search | GET | keyword | Searches for dishes based on a keyword (supports fuzzy matching). |
| /api/dishes/new | GET | month | Retrieves a list of newly recommended dishes for the specified month (defaults to the current month if not provided). |
| /api/merchants/{merchantId}/dishes | POST | name, price, description, category, allergens, image | Allows a merchant to submit a new dish. The dish status defaults to "pending review". |
| /api/admin/dishes/pending` | GET | - | Retrieves a list of all dishes with "pending review" status for administrator approval. |
| /api/admin/dishes/{dishId}/review | PUT | status, rejectionReason | Enables an administrator to review a dish: approve it for publication or reject it (with an optional rejection reason). |
| /api/dishes/{dishId}/comments | GET | page, size | Retrieves a paginated list of user comments for a specific dish. |
| /api/dishes/{dishId}/comments | POST | userId, content, images | Allows a user to submit a text-and-image comment for a specific dish. |

##### 2.2.3 Feedback Service Subsystem

The Feedback Service Subsystem is responsible for collecting, managing, and processing feedback related to campus dining services. It provides a standardized and traceable mechanism for students to submit dining-related feedback and for administrators to review, process, and respond to these submissions. This subsystem plays a critical role in improving food quality, service efficiency, and management transparency.

| API Interface | Method | Parameters | Description |
|--------------|--------|------------|-------------|
| `/api/feedback/submit` | POST | `token`, `content`, `category` | Submit a new dining feedback record by a student. |
| `/api/feedback/list` | GET | `token`, `status` | Retrieve a list of feedback records based on status (e.g., pending, reviewed). |
| `/api/feedback/detail` | GET | `token`, `feedbackId` | Query detailed information of a specific feedback record. |
| `/api/feedback/review` | POST | `token`, `feedbackId`, `decision`, `comment` | Administrator reviews feedback and submits a decision. |
| `/api/feedback/status/update` | POST | `token`, `feedbackId`, `status` | Update the status of a feedback record after review. |
| `/api/feedback/notify` | POST | `feedbackId` | Notify the student of the feedback review result. |

#### 3. analysis mechanisms
##### 3.1 Data Persistence Mechanism

In the SmartCampus platform, data persistence serves as the core infrastructure that enables efficient and reliable campus lifestyle services. As the platform integrates multiple services—including dining, feedback submission, and campus card top-ups—it must handle highly heterogeneous data types: ranging from strongly consistent transactional data (e.g., user identities, order records, and top-up transactions) to highly flexible unstructured content (e.g., dish comments, feedback messages, and system logs). To meet diverse requirements for performance, consistency, and scalability across different scenarios, we adopt a hybrid persistence strategy that combines relational databases, document stores, and in-memory caching to build a layered and high-performance data storage architecture.

##### 3.1.1 Data Persistence Requirements

Data in SmartCampus exhibits significant diversity:

- **Structured transactional data**: such as student accounts, campus card balances, order statuses, and payment records-requiring ACID properties and strong consistency;
- **Semi-structured/unstructured data**: such as rich-media comments on dishes (text + images), user feedback content, and system operation logs-characterized by flexible schemas and high write frequency;
- **High-frequency read hotspots**: such as daily menus, restaurant information, session tokens, and dish ranking lists-demanding ultra-low latency responses.

To manage this data efficiently and securely, we abandon a single-database approach in favor of a multi-engine, collaborative hybrid persistence architecture, ensuring each data type is handled by the storage technology best suited to its characteristics.

##### 3.1.2 Persistence Architecture and Multi-Modal Storage Technologies

Our persistence architecture is built upon a three-tier data storage model:

1. **MySQL 8.0 (Relational Core Database)**  
   Serves as the primary transactional database, storing all business entities requiring strong consistency, including `Student`, `CampusCard`, `Order`, and `TopUpTransaction`. The InnoDB engine guarantees ACID compliance and supports complex joins and foreign key constraints, making it ideal for critical operations such as order creation, balance deduction, and top-up confirmation.

2. **MongoDB 6.0 (Document-Oriented Extension Store)**  
   Handles schema-flexible, write-intensive unstructured data, such as `Comment` documents (containing text and image URLs), `Feedback` submissions, and system audit logs. Its dynamic schema greatly simplifies the storage and evolution of rich-media comments and variable feedback forms.

3. **Redis 7.0 (In-Memory Caching Layer)**  
   Deployed at the front of the data access path to cache frequently accessed data, including:  
   - Today's dish menu (`Dish` list)  
   - Real-time dish rankings and aggregated ratings  
   - User session tokens  
   - Low-balance alert thresholds  
   With automatic TTL expiration and protection against cache penetration, Redis significantly reduces backend database load and delivers millisecond-level response times for mobile clients.

The entire backend data service stack is containerized using Docker and orchestrated by Kubernetes, ensuring consistent deployment and elastic scalability across development, testing, and production environments.

##### 3.1.3 Persistence Layer Design and Framework Integration
![alt text](source/image.png)

At the software architecture level, the persistence layer sits between the business logic layer and physical storage, providing a unified data access abstraction. We implement this layer using the Spring Boot 3.x + Spring Data ecosystem:

- **Relational data access**: Spring Data JPA is used to interact with MySQL, automatically generating CRUD methods via Repository interfaces; for complex queries (e.g., multi-condition order filtering), **MyBatis-Plus** is employed to provide fine-grained control over native SQL.
- **Document data access**: Spring Data MongoDB enables automatic mapping between POJOs and BSON documents, supporting annotation-based index definitions and aggregation pipelines.
- **Caching integration**: Leveraging Spring Cache Abstraction together with the Redisson client, caching logic is declaratively managed at the Service layer using annotations such as `@Cacheable` and `@CacheEvict`, effectively decoupling business logic from caching strategies.

##### 3.1.4 Typical Data Persistence Scenarios

1. **Campus Card Top-Up and Balance Management**  
   A user's top-up request triggers the creation of a `TopUpTransaction` entity, which is persisted to MySQL to ensure an immutable financial audit trail. Simultaneously, the updated `CampusCard.balance` value is refreshed in Redis, allowing subsequent balance queries to bypass the database entirely.

2. **Dish Comments and Feedback Submission**  
   User-submitted rich-media comments are stored as JSON documents in MongoDB, preserving their original structure (including embedded images). Meanwhile, associated metadata-such as dish ID and user ID-remains in MySQL to facilitate cross-system relational analysis.

3. **Real-Time Dish Rankings**  
   Based on voting statistics stored in MySQL, a background scheduled task (or event-driven processor) computes the latest rankings and writes the result set (including `dishId`, `score`, and `rank`) into a Redis Sorted Set. The frontend retrieves the full leaderboard with a single `ZRANGE` operation, achieving response times under 10ms.

##### 3.2 Security Mechanism

In the SmartCampus platform, security serves as the critical foundation that enables trustworthy and reliable campus lifestyle services. As the platform integrates multiple sensitive functions—including financial transactions, personal data management, and multi-role interactions—it must handle diverse security requirements: ranging from user authentication and authorization to data protection and attack prevention. To meet these requirements across different scenarios, we adopt a comprehensive security strategy that combines authentication, authorization, encryption, and monitoring to build a multi-layered and robust security architecture.

##### 3.2.1 Security Requirements

Security in SmartCampus encompasses multiple dimensions:

- **Authentication requirements**: Student, merchant, and administrator identities require secure verification with support for mobile access and stateless sessions;
- **Authorization requirements**: Fine-grained access control across different roles and data isolation between entities;
- **Data protection requirements**: Sensitive information such as passwords, payment details, and personal data needing encryption at rest and in transit;
- **Attack prevention requirements**: Protection against common threats including SQL injection, XSS, CSRF, and replay attacks;
- **Audit and compliance requirements**: Complete logging of sensitive operations and adherence to data protection regulations.

To address these diverse requirements effectively, we implement a defense-in-depth security architecture with multiple protective layers and specialized security components.

##### 3.2.2 Security Architecture and Multi-Layer Protection
![alt text](source/SmartCampusSecurityArchitecture.png)

Our security architecture is built upon a five-layer defense model:

1. **Infrastructure Security Layer**  
   Serves as the foundation, implementing security configurations, vulnerability scanning, and the principle of least privilege for all system components. Container security hardening and network segmentation ensure basic protection at the infrastructure level.

2. **Network Security Layer**  
   Provides transport-level protection with mandatory HTTPS/TLS encryption, Web Application Firewall (WAF) deployment, and proper network isolation between different service tiers. This layer prevents eavesdropping and unauthorized network access.

3. **Session and Access Control Layer**  
   Manages user authentication, authorization checks, and session security. JWT-based stateless authentication supports mobile clients while maintaining security. Role-based and attribute-based access control ensures proper permission management across the platform.

4. **Application Protection Layer**  
   Implements input validation, output encoding, and business logic security checks. This layer prevents application-level attacks such as injection and cross-site scripting while ensuring data integrity throughout business processes.

5. **Monitoring and Response Layer**  
   Provides real-time security monitoring, anomaly detection, and emergency response procedures. Security event correlation and automated alerting enable rapid response to potential threats.

##### 3.2.3 Security Component Design and Framework Integration

At the implementation level, security components are integrated throughout the application stack using Spring Security and complementary technologies:

- **Authentication implementation**: JWT-based stateless authentication using Spring Security filters and custom token services, supporting multiple client types including WeChat Mini Programs;
- **Authorization implementation**: Three-tier authorization combining URL-level security configuration, method-level `@PreAuthorize` annotations, and custom permission evaluators for data-level access control;
- **Data protection**: Integration of BCrypt for password hashing, AES encryption for sensitive data, and TLS for secure communications;
- **Attack prevention**: Built-in protections against common web vulnerabilities through Spring Security configurations and custom security filters.

##### 3.2.4 Typical Security Application Scenarios

1. **Student Payment Transaction Security**  
   A student's payment request triggers JWT token validation, role-based permission checking, and payment signature verification. The transaction is recorded with complete audit information including timestamp, IP address, and operator identity, ensuring non-repudiation and traceability.

2. **Multi-Role Platform Access Control**  
   Different user roles (student, merchant, administrator) receive distinct permission sets through JWT claims. The authorization layer enforces access restrictions based on both URL patterns and business method annotations, preventing privilege escalation and unauthorized data access.

3. **Sensitive Data Protection**  
   User credentials are hashed using BCrypt before storage, while payment information is encrypted using AES-GCM. All sensitive data transmissions employ TLS 1.2+ encryption, and database connections use SSL protection to prevent data interception.

This security mechanism provides comprehensive protection while maintaining performance and usability, forming an essential foundation for the trusted operation of the SmartCampus platform.


#### 4. Design two use case realizations by incorporating the design mechanisms and the refined architecture  

#### 5. Architectural styles used and critical design decisions made in your solution  

Certainly! Below is the complete English translation of your provided Chinese content for **Section 6: Non-Functional Requirements**, including subsections on **Security** and **Usability**, tailored for an A3-level design document:

---

### 6. Non-Functional Requirements

### 6.1 Security Requirements
The campus digital platform handles a large volume of sensitive data, including student identity information, campus card balances, payment records, dining preferences, and user feedback. Any data breach, identity impersonation, or payment fraud could severely damage user trust and potentially lead to legal and compliance risks. Given the platform's support for online payments, unified identity authentication, and integration with multiple systems, security must serve as the cornerstone of the system design.

#### 6.1.1 Layered Security Architecture and Evolution of Authentication Mechanism  
In the a2 phase, the system was initially designed with `OAuth 2.0` and SSO for unified authentication. However, we later recognized that `OAuth 2.0` introduces unnecessary complexity-such as authorization server management-for a closed-campus environment serving only internal users. It is also better suited for third-party integrations, whereas SmartCampus primarily uses its own front-end clients that benefit from a lighter, more integrated solution.

Accordingly, we replaced `OAuth 2.0` with a stateless authentication mechanism based on `Spring Security` and `JWT`. After login, the system issues a signed JWT containing the user's identity, roles, and permissions. Clients include this token in the Authorization header, and the API gateway or microservice interceptors validate its signature, expiration, and claims.

Because `JWT` is self-contained and verifiable via public-key cryptography, the system no longer requires centralized session storage or an authorization server-improving scalability, fault tolerance, and deployment simplicity. This approach better meets the platform's needs for high concurrency, low latency, and autonomous control, while fully satisfying assignment 1's security goal: "ensuring secure data transmission and access".

#### 6.1.2 End-to-End Encryption of Sensitive Data  
In response to assignment 1's requirement for "AES-256 encryption during transmission and storage", our technical stack (Section 2.4 of A2) explicitly employs `Spring Security with HTTPS (TLS 1.3)` to secure data in transit. At the database layer, `MySQL 8.0` enables Transparent Data Encryption (TDE), and `Redis 7.0` is configured with access controls and TLS-encrypted connections. Critical fields-such as campus card top-up amounts and payment credentials-are encrypted before persistence, ensuring that even in the event of unauthorized database access, plaintext data remains inaccessible.

#### 6.1.3 Secure Payment and Transaction Workflow  
In the campus card top-up subsystem, interactions between `TopUpController` and the external Payment Gateway are encapsulated using the Adapter pattern. Every payment request includes a cryptographic signature and a timestamp to prevent replay attacks. Only after receiving a confirmed success response from the payment provider does the system update the card balance and log the transaction via `AuditLogger`. This "deduct-first, credit-later" atomic workflow prevents financial inconsistencies caused by network failures or service interruptions.

#### 6.1.4 Fine-Grained Permission Control and Operational Auditing  
During dish publishing and review workflows, the system explicitly validates user permissions through use cases such as "Verify Permission" and "View User Authority". All critical operations-including publishing dishes, reviewing feedback, and modifying orders-are recorded in audit logs, associated with user IDs and timestamps. This fulfills Assignment 1's governance requirements for "data traceability" and "accountability".


### 6.2 Usability Requirements  
SmartCampus primarily serves on-campus students whose usage scenarios are highly fragmented-for example, placing meal orders between classes or checking card balances while queuing. If the interface is complex, the learning curve steep, or the operation flow lengthy, user adoption will suffer significantly. The "90%+ student adoption rate" target outlined in Assignment 1 fundamentally depends on an exceptional user experience. Therefore, usability is not merely a supplementary feature-it is a core driver of business success.

#### 6.2.1 User Journey-Centric Interface Redesign  
Section 5 of the A2 document illustrates optimizations across key interfaces, directly implementing assignment 1's goals of "reducing cognitive load" and enabling "first-time users to operate independently without training":  
- The **meal ordering page** now features a persistent shopping cart area with real-time price calculation, minimizing navigation jumps.  
- The **top-up page** offers both preset quick-amount buttons and a custom input field, balancing efficiency with flexibility.  
- The **voting page** auto-fills known context to eliminate redundant input.

#### 6.2.2 Multimodal Interaction and Context-Aware Help  
The system embeds a "?" help icon next to complex features (e.g. on the feedback submission page or new dish publishing page); clicking it instantly displays text-and-image-based guidance closely relevant to the current task. Additionally, as described in Section 5.3 of a2, the feedback subsystem allows users to submit input using a combination of text and images, lowering the barrier to expression-this is a concrete realization of assignment1's principle that "the help system should be closely aligned with the user's task context".

#### 6.2.3 Seamless Language Switching and Accessibility Support  
The frontend supports real-time switching between Simplified Chinese and English without requiring a page refresh, and the user's language preference is persistently stored in local cache. Additionally, all icons are accompanied by text labels, fully satisfying Assignment 1's requirement for "multilingual support and accessibility."

#### 6.2.4 Transparent State Visibility and Immediate Feedback  
Clear status indicators are provided throughout key user flows such as ordering, top-ups, and voting:  
- After a successful top-up, a notification appears and the card balance updates automatically.  
- Upon voting, a "Vote Successful" message is shown immediately, and rankings are refreshed in real time.  
- After submitting feedback, the system displays a "Pending Review" status along with an estimated processing time.  

This "action-feedback" loop significantly enhances users' sense of control and reduces anxiety, aligning with the usability principles of "clear operational logic" and "minimizing uncertainty".


### 6.3 Performance and Scalability Requirements  
SmartCampus must serve a campus population of thousands, with usage patterns characterized by sharp peaks—particularly during meal times (11:30–13:00 and 17:00–18:30), when hundreds of concurrent meal orders, real-time balance queries, and ranking updates must be handled without degradation. The target of "reducing service delivery time by 40%" can only be achieved through a high-performance, horizontally scalable architecture.

#### 6.3.1 Microservices-Based Horizontal Scaling  
As detailed in Sections 2.1 and 2.2 of Assignment 2, we adopted a microservices architecture that decouples the system into independently deployable services (e.g., order service, top‑up service, voting service). Each service can be scaled horizontally based on load, using Kubernetes to manage automatic pod replication and service discovery via Spring Cloud Netflix Eureka. This design directly addresses Assignment 2’s requirement that “the system must be capable of scaling to support growth in user numbers and transaction volume.”

#### 6.3.2 Multi-Level Caching Strategy  
To satisfy the “low-latency access” objective, we implemented a two-tier caching layer:  
- **In‑memory hot‑data cache**: Frequently accessed data such as today’s menu, dish rankings, and campus‑card balances are stored in Redis, reducing database hits and delivering response times under 50 ms.  
- **Static resource CDN**: Dish images, help‑page illustrations, and other static assets are distributed via a content‑delivery network, lowering server load and improving page‑load speed for users across different campus locations.  
These measures collectively ensure that even during peak hours, the system maintains the sub‑second response times required.

#### 6.3.3 Asynchronous Processing and Message Queues  
Time‑insensitive operations—such as generating weekly dining reports, sending batch notifications, and updating aggregated rankings—are offloaded to background jobs powered by RabbitMQ. This prevents long‑running tasks from blocking critical user‑facing flows and enables the system to handle sudden traffic surges gracefully, thereby meeting the availability target of “99.9% uptime” stipulated.

#### 6.3.4 Database Read‑Write Separation and Connection Pooling  
MySQL 8.0 is configured with a master‑slave replication setup: write operations (e.g., orders, top‑ups) go to the master, while read‑intensive queries (e.g., menu browsing, ranking displays) are routed to read replicas. Combined with HikariCP connection pooling, this architecture sustains high throughput and avoids database bottlenecks, directly supporting the goal of “improving resource utilization to 85%+.”

### 6.4 Maintainability and Extensibility Requirements  
SmartCampus is envisioned as a long‑term platform that will evolve with the campus’s needs. Adding new services (e.g., library‑seat booking, sports‑facility reservation) or modifying existing ones must not require extensive re‑engineering or introduce system‑wide instability. The architectural decisions described in Assignment 2 are explicitly guided by Assignment 1’s emphasis on “building scalable architecture supporting 50,000+ concurrent users” and “enabling data‑driven decision making for institutional planning.”

#### 6.4.1 Clear Separation of Concerns and Layered Design  
The layered architecture (presentation, security/gateway, business logic, data access, infrastructure, and storage) enforces strict boundaries between components, as illustrated in Section 2.3 of Assignment 2. Developers working on the feedback subsystem, for example, need not understand the internals of the payment gateway; they interact only with well‑defined service interfaces. This modularity fulfills the demand for “reducing administrative overhead by 30% through system integration.”

#### 6.4.2 API‑First Development and Contract‑Based Integration  
All service‑to‑service communication employs RESTful APIs with OpenAPI (Swagger) documentation, and external integrations (e.g., with the campus‑card system or third‑party payment providers) are wrapped behind adapters. This contract‑driven approach ensures that changes in one service do not cascade failures to others, and new features can be added by implementing new APIs without disrupting existing workflows—aligning with the principle of “establishing SmartCampus as the market leader in university digital transformation.”

#### 6.4.3 Centralized Configuration and Versioned Deployments  
Using Spring Cloud Config, environment‑specific settings (database URLs, feature toggles, external service endpoints) are stored in a version‑controlled repository (Git) and injected at runtime. Combined with Docker‑based containerization and Kubernetes deployment manifests, this allows the same service image to be promoted from development to production with minimal manual intervention, thereby supporting the goal of “enabling rapid, reliable iteration and feature rollout.”

#### 6.4.4 Comprehensive Monitoring and Automated Diagnostics  
Each microservice exports health and performance metrics via Spring Boot Actuator, which are collected by Prometheus and visualized in Grafana dashboards. Logs from all services are aggregated in the ELK Stack (Elasticsearch, Logstash, Kibana). This observability stack enables engineers to quickly pinpoint the root cause of issues—whether a slow database query, a failing external API, or a memory leak—and proactively address them before they affect users, directly contributing to the target of “maintaining user satisfaction above 4.5/5.0.”

#### 7. Progress on prototyping  
##### 7.1. Front-end Prototyping

In this project, considering that the core user base consists of on-campus students who heavily rely on mobile devices, we prioritized mobile accessibility and user experience in the presentation layer design. Therefore, WeChat Mini Program was selected as the primary client platform-it requires no download or installation, offers an instant "use-and-go" experience, and enjoys extremely high adoption among university students, effectively lowering the barrier to entry for users.

The front-end client is developed using the uni-app cross-platform framework, with HBuilderX as the main development environment. uni-app enables us to write code once using Vue.js syntax and compile it to multiple platforms-including WeChat Mini Programs, H5 web apps, and native mobile apps-laying a solid technical foundation for future expansion to native iOS/Android applications or web interfaces.

During development, we adopted the "Run to Mini Program Simulator" mode, which automatically compiles the project in real time and pushes it directly into WeChat DevTools. This workflow enables efficient debugging, real-device previewing, and performance profiling. It fully leverages HBuilderX's strengths in intelligent code completion, rapid component generation, and project management, while also taking advantage of WeChat DevTools' deep support for Mini Program API debugging, network monitoring, and log inspection-significantly enhancing both development efficiency and consistency in user experience.

which is structured as follows:
<p align="center">
  <img src="source/structure.png" alt="allscopes" title="scope" style="display:block; margin:0 auto; width:20%; max-width:500px; height:auto;"/>
</p>

The way to run the front end:
<p align="center">
  <img src="source/way.png" alt="allscopes" title="scope" style="display:block; margin:0 auto; max-width:700px; height:auto;"/>
</p>


对于管理侧用户，我们独立开发了一套基于 Vue.js 3 + Vite 的响应式 Web 管理仪表盘。该后台系统采用 npm 作为包管理工具，通过 npm run dev 命令在本地 localhost:5173 启动开发服务器，支持热更新与模块化开发。

<p align="center">
  <img src="source/vue.png" alt="allscopes" title="scope" style="display:block; margin:0 auto; max-width:300px; height:auto;"/>
</p>

Taking the login page as an example, our front-end code is as follows:

```vue
<view class="container">
		<view class="circle-bg top-left"></view>
		<view class="circle-bg bottom-right"></view>

		<view class="content">
			<view class="header">
				<image class="logo" src="/static/logo.png" mode="aspectFill"></image>
				<view class="title-group">
					<text class="title">Welcome Back !</text>
				</view>
			</view>

			<view class="form-card">
				<view class="input-wrapper">
					<text class="input-label"> ID </text>
					<view class="input-box">
						<input
							class="input-field"
							v-model="userId"
							type="text"
							placeholder="please input your ID"
							placeholder-class="placeholder-style"
							confirm-type="next"
						/>
					</view>
				</view>

				<view class="input-wrapper">
					<text class="input-label"> password </text>
					<view class="input-box">
						<input
							class="input-field"
							v-model="userKey"
							password
							type="text"
							placeholder="please input your password"
							placeholder-class="placeholder-style"
							confirm-type="done"
							@confirm="handleLogin"
						/>
					</view>
				</view>

				<button 
					class="login-btn" 
					:loading="isLoading" 
					:disabled="isLoading" 
					@click="handleLogin"
				>
					{{ isLoading ? 'Logging in......' : 'Log In' }}
				</button>
				
				<view class="footer-link">
					<text>Forget password?</text>
				</view>
			</view>
		</view>
	</view>
```

This page is designed based on system UI snapshots, offering a simple and efficient user authentication entry point. It includes fields for entering student ID and password, with secure password masking display and a responsive login button. The interface is optimized for mobile screens, supporting the WeChat Mini Program platform's instant access and use experience. The page effect is shown in the figure below:

<p align="center">
  <img src="source/Login_page.png" alt="allscopes" title="scope" style="display:block; margin:0 auto; max-width:700px; height:auto;"/>
</p>


#### 7.2 Back-end Prototyping

后端系统采用 **Spring Boot 3.x** 框架构建，遵循微服务架构原则，将整体功能拆分为多个高内聚、低耦合的服务模块（如餐饮服务、反馈服务、校园卡服务等）。在当前原型阶段，我们暂不引入 Spring Cloud 的完整分布式治理组件（如注册中心、配置中心），而是聚焦于各微服务核心业务逻辑的实现与 API 接口的验证。

前后端通过 **RESTful API** 进行通信，接口设计遵循统一规范，支持 JSON 数据格式，并集成全局异常处理与标准化响应结构。

We begin our work by creating a Spring Boot microservice called `auth-service` : 

<p align="center">
  <img src="source/spring_initializr.png" alt="allscopes" title="scope" style="display:block; margin:0 auto; max-width:700px; height:auto;"/>
</p>

which is structured as follows:

以 **用户认证微服务**（auth-service） 为例，该服务负责处理学生和管理员的登录、会话管理等安全相关功能。我们为其定义了如下关键接口：

- `POST /api/auth/login`：用户密码登录
- `POST /api/auth/logout`：退出登录
- `GET /api/auth/userinfo`：获取当前用户信息

核心控制器代码片段如下：

```java
@RestController
public class AuthController {

    @Autowired
    private AuthService authService;

    @Autowired
    private TokenService tokenService;

    @PostMapping("/api/auth/login")
    public ResponseEntity<ApiResponse<TokenInfoVO>> login(
            @Valid @RequestBody LoginRequest request) {
        
        // 验证用户名密码
        UserInfoBO user = authService.authenticate(request.getUsername(), request.getPassword());
        if (user == null) {
            return ResponseEntity.badRequest().body(ApiResponse.error("用户名或密码错误"));
        }

        // 生成 OAuth 2.0 访问令牌
        String accessToken = tokenService.generateToken(user);
        
        // 返回标准化响应
        TokenInfoVO tokenVO = new TokenInfoVO(accessToken, user.getUserId(), user.getRole());
        return ResponseEntity.ok(ApiResponse.success(tokenVO));
    }
}
```

安全性方面，我们深度集成 **Spring Security + OAuth 2.0**，实现基于令牌（Token）的无状态认证机制。所有受保护的 API 均需携带有效 Access Token，由网关层统一校验，确保系统安全边界清晰、权限控制精准。

目前，各微服务已初步完成核心接口的开发与联调，可通过 Postman 或 Swagger UI 进行测试调用，为后续与前端小程序及管理后台的集成奠定了坚实基础。

--- 

此文档结构清晰体现了 SmartCampus 项目“**移动端优先、微服务驱动、安全可靠**”的技术路线，符合课程对系统分析与设计阶段原型实现的要求。

##### 7.2. Back-end Prototyping
 
#### 8. Open Issues in the Design Model

##### 8.1. Data Consistency Assurance Mechanism Across Distributed Services

**Problem Description**
In the current microservices architecture, how to ensure data consistency for business operations that span multiple services? For example, when a user completes a campus card top-up operation, this process involves the payment service, account balance service, transaction record service, and notification service. If one of these service operations fails or experiences a network timeout, how can we ensure that the data across the entire business chain remains in a consistent state?

**Challenge**
It is necessary to design and implement a reliable distributed transaction coordination mechanism, considering the adoption of patterns such as Saga, TCC (Try-Confirm-Cancel), or event sourcing architecture to ensure eventual data consistency between microservices. This requires balancing the complexity of the solution, its performance impact, and the costs of development and maintenance.

##### 8.2. System Performance Optimization Under Real-Time High-Concurrency Scenarios

**Problem Description**
How can the system handle sudden high-concurrency requests to services such as order processing, payment, and dish ranking during peak campus dining hours (e.g., 11:30 AM - 1:00 PM)? Can the current architectural design effectively manage peak traffic loads without service degradation or response timeouts?

**Challenge**
Detailed capacity planning strategies, multi-level caching solutions, and elastic scaling mechanisms need to be formulated. Considerations include setting appropriate rate limiting and circuit breaking rules, optimizing database connection pools and Redis cache configurations, and establishing an effective performance monitoring and early warning system.


#### 9. If you have used an AI tool or technology to generate an output that you either paraphrase or direct quote in your writing, you must cite and reference this output as a source in your reference list. If you have used an AI tool or technology in the process of completing the above tasks (for example, generating technical solutions, improving your architectural decisions, creating software prototypes, implementing the PoC, and enhancing the contents of your report), an acknowledgment of how you have used AI tools or technologies is required 

#### 10. Project self-reflection  

#### 11. Contributions of team members
