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

#### 8. Open issues in your design model  

#### 9. If you have used an AI tool or technology to generate an output that you either paraphrase or direct quote in your writing, you must cite and reference this output as a source in your reference list. If you have used an AI tool or technology in the process of completing the above tasks (for example, generating technical solutions, improving your architectural decisions, creating software prototypes, implementing the PoC, and enhancing the contents of your report), an acknowledgment of how you have used AI tools or technologies is required 

#### 10. Project self-reflection  

#### 11. Contributions of team members
