### System Analysis and Design

**Team Name**: CampusCode  
**Team Members**:
- 2353924 Feng Juncai (冯俊财)
- 2351869 Ji Peng (纪鹏)  
- 2353240 Zhang Shikou (张诗蔻)
- 2352993 Yu Yilian (于伊莲)

#### 0. Table of Contents

- [0. Table of Contents](#0-table-of-contents)
- [1. Overview](#1-overview)
  - [1.1 Overview of Design Progress](#11-overview-of-design-progress)
  - [1.2 Implementation Platforms and Frameworks](#12-implementation-platforms-and-frameworks)
- [2. Architecture Refinement](#2-architecture-refinement)
  - [2.1. Platform-dependent architecture with a refined overall structure](#21-platform-dependent-architecture-with-a-refined-overall-structure)
  - [2.2. List of subsystems and interfaces](#22-list-of-subsystems-and-interfaces)
  - [2.3. Demonstrate interface specification in detail with one or several samples between your system and external systems](#23-demonstrate-interface-specification-in-detail-with-one-or-several-samples-between-your-system-and-external-systems)
  - [2.4. Meal Ordering Subsystem - Interface Specification](#24-meal-ordering-subsystem---interface-specification)
- [3. Two Selected Analysis Mechanisms and Their Design Mechanisms](#3-two-selected-analysis-mechanisms-and-their-design-mechanisms)
  - [3.1 Data Persistence Mechanism](#31-data-persistence-mechanism)
  - [3.1.3 Persistence Layer Design and Framework Integration](#313-persistence-layer-design-and-framework-integration)
  - [3.2 Security Mechanism](#32-security-mechanism)
  - [3.2.2 Security Architecture and Multi-Layer Protection](#322-security-architecture-and-multi-layer-protection)
- [4. Two Use Case Realizations](#4-two-use-case-realizations)
  - [4.1 Meal Ordering Use Case](#41-meal-ordering-use-case)
- [5. Architectural Styles and Design Decisions](#5-architectural-styles-and-design-decisions)
  - [5.1 Architectural Styles](#51-architectural-styles)
  - [5.2 Critical Design Decisions](#52-critical-design-decisions)
- [6. Non-Functional Requirements](#6-non-functional-requirements)
  - [6.1 Security Requirements](#61-security-requirements)
  - [6.2 Usability Requirements](#62-usability-requirements)
  - [6.2 Usability Requirements](#62-usability-requirements-1)
  - [6.3 Performance and Scalability Requirements](#63-performance-and-scalability-requirements)
  - [6.3 Performance and Scalability Requirements](#63-performance-and-scalability-requirements-1)
  - [6.4 Maintainability and Extensibility Requirements](#64-maintainability-and-extensibility-requirements)
- [7. Progress on prototyping](#7-progress-on-prototyping)
  - [7.1. Front-end Prototyping](#71-front-end-prototyping)
  - [7.2. Back-end Prototyping](#72-back-end-prototyping)
- [8. Open Issues in the Design Model](#8-open-issues-in-the-design-model)
  - [8.1. Data Consistency Assurance Mechanism Across Distributed Services](#81-data-consistency-assurance-mechanism-across-distributed-services)
  - [8.2. System Performance Optimization Under Real-Time High-Concurrency Scenarios](#82-system-performance-optimization-under-real-time-high-concurrency-scenarios)
- [9. If you have used an AI tool or technology to generate an output that you either paraphrase or direct quote in your writing, you must cite and reference this output as a source in your reference list. If you have used an AI tool or technology in the process of completing the above tasks (for example, generating technical solutions, improving your architectural decisions, creating software prototypes, implementing the PoC, and enhancing the contents of your report), an acknowledgment of how you have used AI tools or technologies is required](#9-if-you-have-used-an-ai-tool-or-technology-to-generate-an-output-that-you-either-paraphrase-or-direct-quote-in-your-writing-you-must-cite-and-reference-this-output-as-a-source-in-your-reference-list-if-you-have-used-an-ai-tool-or-technology-in-the-process-of-completing-the-above-tasks-for-example-generating-technical-solutions-improving-your-architectural-decisions-creating-software-prototypes-implementing-the-poc-and-enhancing-the-contents-of-your-report-an-acknowledgment-of-how-you-have-used-ai-tools-or-technologies-is-required)
- [10. Project self-reflection](#10-project-self-reflection)
- [11. Contributions of team members](#11-contributions-of-team-members)

#### 1. Overview  

##### 1.1 Overview of Design Progress

Building upon the solid foundation laid during the requirements analysis and initial modeling phases, the SmartCampus project has now advanced into the detailed System Design stage. Our primary focus has shifted from defining the functional requirements—what the system should do—to specifying the technical implementation details—how the system will be built.

In this phase, we have successfully transformed the logical analysis model into a platform-specific design architecture. This involved refining the system boundaries and defining the specific RESTful API contracts that facilitate communication between our mobile clients and the backend services. In parallel, we have moved from conceptual data modeling to concrete database schema design, ensuring that our data structures in MySQL and MongoDB are optimized for performance and integrity. 

##### 1.2 Implementation Platforms and Frameworks

To align with our user-centric strategy, the SmartCampus system is engineered as a "Mobile-First" application supported by a robust, cloud-native backend infrastructure.

Frontend Strategy Given that our primary user base consists of students who rely heavily on mobile devices, the system’s presentation layer prioritizes mobile accessibility. We have selected the WeChat Mini Program as our primary client platform due to its instant accessibility and high penetration rate among the student demographic. This is complemented by native mobile applications (iOS/Android) to leverage system-level capabilities where necessary. For administrative purposes, a web-based management dashboard built with Vue.js provides merchants and staff with a comprehensive interface for data management and operational monitoring.

Backend and Infrastructure Supporting the mobile frontend is a scalable microservices architecture built on the Spring Boot 3.x framework. This choice ensures rapid development cycles while maintaining the stability required for a campus-wide system. Security is managed through JWT, providing a secure and seamless authentication experience for mobile users.

Data Storage and Management Our data strategy employs a hybrid approach to optimize performance for different types of information. We utilize MySQL 8.0 as the primary relational database to handle transactional data requiring strict consistency, such as user accounts and financial records. For unstructured data, such as user reviews and system logs, MongoDB 6.0 offers the necessary flexibility. To ensure a smooth and responsive user experience on mobile devices, Redis 7.0 is implemented as a high-speed caching layer for frequently accessed data, such as daily menus and session tokens. The entire backend ecosystem is containerized using Docker and orchestrated via Kubernetes, ensuring consistent deployment across development and production environments.

#### 2. Architecture Refinement  

##### 2.1. Platform-dependent architecture with a refined overall structure

This section refines the logical layered architecture from Assignment 2 into a **platform-specific implementation**, mapping abstract components to concrete technologies. The key evolution is transforming generic architectural layers into executable deployment configurations with specific frameworks, versions, and integration strategies.

<div align="center">
  <img src="diagrams/platform_architecture.png" alt="SmartCampus Platform Architecture" style="width:90%; max-width:900px;"/>

**Figure 2.1: Platform-Dependent Architecture (5-Layer Deployment Model)**

</div>

###### 2.1.1 Architectural Refinement Overview

**From Assignment 2 (Logical) to Assignment 3 (Physical):**

| Aspect                       | Assignment 2 (Logical Architecture) | Assignment 3 (Platform-Dependent Architecture)       |
| ---------------------------- | ----------------------------------- | ---------------------------------------------------- |
| **Presentation Layer** | Abstract "mobile client"            | WeChat Mini Program (HBuilderX + uni-app)            |
| **Business Logic**     | Generic "service layer"             | 4 Spring Boot microservices (ports 8081-8084)        |
| **Data Access**        | "Database layer"                    | MySQL 8.0 + MongoDB 6.0 + Redis 7.0 (hybrid storage) |
| **Authentication**     | OAuth 2.0 (conceptual)              | JWT (JSON Web Token) with HS256 signing              |
| **Deployment**         | Not specified                       | Docker containers orchestrated by Kubernetes         |
| **Communication**      | Abstract API calls                  | RESTful HTTP + JWT authentication via Nginx gateway  |

**Microservices Scope Refinement:**

Building upon Assignment 2's four business subsystems (Library, Academic, Life Services, Logistics), this assignment focuses detailed architectural refinement on the Life Services domain (particularly meal ordering, campus card, and feedback functionalities) as a representative case study. This targeted approach allows in-depth exploration of microservices decomposition, hybrid data storage strategies, and external API integrations without redundantly documenting similar patterns across all subsystems. The four core services (`auth-service`, `meal-service`, `card-service`, `feedback-service`) demonstrate the complete architecture blueprint applicable to other domains.

**Authentication Refinement Rationale:**

The evolution from OAuth 2.0 to JWT reflects practical mobile-first optimization: JWT's stateless, self-contained tokens eliminate server-side session storage (essential for Kubernetes horizontal scaling), reduce bandwidth for WeChat Mini Program requests, and enable independent signature validation across microservices without centralized authentication queries. This shift ensures that our mobile-first architecture remains lightweight, secure, and scalable.

###### 2.1.2 Technology Stack Mapping

**Core Platform Technologies:**

| Layer                    | Technology                     | Version         | Key Capabilities                                             |
| ------------------------ | ------------------------------ | --------------- | ------------------------------------------------------------ |
| **Client**         | WeChat Mini Program (uni-app)  | Vue 3           | HBuilderX IDE, one-click deployment, 90%+ campus penetration |
| **Gateway**        | Nginx + Spring Security        | 1.24+ / 6.2+    | SSL termination, JWT authentication, rate limiting           |
| **Services**       | Spring Boot + Spring MVC       | 3.3+ (JDK 17+)  | Microservices, RESTful APIs, embedded Tomcat                 |
| **Data Access**    | Spring Data JPA + MyBatis-Plus | 3.2+ / 3.5.x    | CRUD abstraction, dynamic SQL, MongoDB driver                |
| **Database**       | MySQL + MongoDB + Redis        | 8.0 / 6.0 / 7.0 | ACID transactions, flexible documents, sub-ms caching        |
| **Infrastructure** | Docker + Kubernetes            | 24.x / 1.28+    | Containerization, orchestration, auto-scaling                |

###### 2.1.3 Microservices Decomposition

The four business subsystems identified in Assignment 2 are decomposed into independently deployable microservices:

**Core Business Services:**

| Service Name         | Port | Responsibilities                                                | Database                     | External Dependencies |
| -------------------- | ---- | --------------------------------------------------------------- | ---------------------------- | --------------------- |
| `auth-service`     | 8081 | User authentication, JWT issuance, session management           | MySQL (`users`, `roles`) | WeChat Login API      |
| `meal-service`     | 8082 | Restaurant/menu management, order processing, ratings & ranking | MySQL + MongoDB + Redis      | Payment gateway API   |
| `card-service`     | 8083 | Campus card balance query, recharge processing                  | MySQL + Redis                | Alipay/WeChat Pay API |
| `feedback-service` | 8084 | User feedback submission, admin review workflow                 | MySQL + MongoDB              | -                     |

###### 2.1.4 Hybrid Data Storage Strategy

The hybrid multi-modal storage architecture refines Assignment 2's generic "data layer" into three specialized databases:

| Data Type                                                     | Storage          | Schema Design                       | Access Pattern                 |
| ------------------------------------------------------------- | ---------------- | ----------------------------------- | ------------------------------ |
| **Transactional** (user accounts, card balance, orders) | MySQL 8.0 InnoDB | Normalized tables with foreign keys | Spring Data JPA + MyBatis-Plus |
| **Semi-structured** (reviews, feedback, logs)           | MongoDB 6.0      | Flexible BSON documents             | Spring Data MongoDB            |
| **High-frequency** (menu cache, rankings, sessions)     | Redis 7.0        | Key-value + Sorted Sets; TTL 1-2h   | Spring Cache annotations       |

###### 2.1.5 Deployment & Security

| Environment           | Deployment            | Configuration                                          |
| --------------------- | --------------------- | ------------------------------------------------------ |
| **Development** | Docker Compose        | Hot-reload, local DBs                                  |
| **Production**  | Kubernetes (3+ nodes) | 2-3 replicas/service, auto-scaling, Persistent Volumes |

**Security:** HTTPS enforcement, JWT (HS256, 2h expiration), BCrypt hashing, parameterized queries, daily backups

##### 2.2. List of subsystems and interfaces

###### 2.2.1 Meal Ordering Subsystem

Based on Assignment 2's analysis model, the Meal Ordering subsystem comprises 9 core classes: 5 entity classes (Student, Order, Dish, Restaurant, Reservation) for business data modeling, 2 boundary classes (SystemInterface, OrderingInterface) for user interaction handling, and 2 control classes (OrderController, MenuController) for business logic coordination. Inter-subsystem interactions include: Meal Service invoking Auth Service for user identity verification (JWT token → User_id), calling Payment Service for campus card deduction (Order_id + Amount → Payment result), utilizing Notification Service for order status updates (Order_id + Status → Push message), and receiving payment callbacks (Transaction_id + Status → Order update).

**API Interface:**

| API Interface | Method | Parameters | Description |
|--------------|--------|------------|-------------|
| /api/meal/restaurants | GET | token, location | Get restaurant list by location |
| /api/meal/menu | GET | token, restaurant_id | Get menu items for specific restaurant |
| /api/meal/dish/detail | GET | token, dish_id | Get detailed dish information |
| /api/meal/order/create | POST | token, dish_ids, quantities, restaurant_id | Create new meal order |
| /api/meal/order/status | GET | token, order_id | Query order status |
| /api/meal/order/cancel | POST | token, order_id | Cancel pending order |
| /api/meal/order/history | GET | token, page, size | Get user's order history |
| /api/meal/review/submit | POST | token, order_id, rating, comment, images | Submit dish review |
| /api/meal/ranking/daily | GET | token, date | Get daily popular dishes ranking |


###### 2.2.2 Dishes recommendation and ranking Subsystem

Here is the fully translated version of your table in English:

| API Interface                      | Method | Parameters                                           | Description                                                                                                                                                  |
| ---------------------------------- | ------ | ---------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| /api/dishes/rankings               | GET    | sortType, category, restaurantId                     | Retrieves a ranked list of dishes, supporting sorting by popularity or rating.                                                                               |
| /api/dishes/{dishId}/vote          | POST   | userId, rating, comment                              | Allows a student to rate and comment on a specific dish. If the student has already voted, the existing record is updated; otherwise, a new vote is created. |
| /api/dishes/{dishId}               | GET    | -                                                    | Retrieves detailed information about a specific dish, including name, price, description, associated restaurant, average rating, and total number of votes.  |
| /api/dishes/search                 | GET    | keyword                                              | Searches for dishes based on a keyword (supports fuzzy matching).                                                                                            |
| /api/dishes/new                    | GET    | month                                                | Retrieves a list of newly recommended dishes for the specified month (defaults to the current month if not provided).                                        |
| /api/merchants/{merchantId}/dishes | POST   | name, price, description, category, allergens, image | Allows a merchant to submit a new dish. The dish status defaults to "pending review".                                                                        |
| /api/admin/dishes/pending`         | GET    | -                                                    | Retrieves a list of all dishes with "pending review" status for administrator approval.                                                                      |
| /api/admin/dishes/{dishId}/review  | PUT    | status, rejectionReason                              | Enables an administrator to review a dish: approve it for publication or reject it (with an optional rejection reason).                                      |
| /api/dishes/{dishId}/comments      | GET    | page, size                                           | Retrieves a paginated list of user comments for a specific dish.                                                                                             |
| /api/dishes/{dishId}/comments      | POST   | userId, content, images                              | Allows a user to submit a text-and-image comment for a specific dish.                                                                                        |

###### 2.2.3 Feedback Service Subsystem

The Feedback Service Subsystem is responsible for collecting, managing, and processing feedback related to campus dining services. It provides a standardized and traceable mechanism for students to submit dining-related feedback and for administrators to review, process, and respond to these submissions. This subsystem plays a critical role in improving food quality, service efficiency, and management transparency.

| API Interface                   | Method | Parameters                                           | Description                                                                    |
| ------------------------------- | ------ | ---------------------------------------------------- | ------------------------------------------------------------------------------ |
| `/api/feedback/submit`        | POST   | `token`, `content`, `category`                 | Submit a new dining feedback record by a student.                              |
| `/api/feedback/list`          | GET    | `token`, `status`                                | Retrieve a list of feedback records based on status (e.g., pending, reviewed). |
| `/api/feedback/detail`        | GET    | `token`, `feedbackId`                            | Query detailed information of a specific feedback record.                      |
| `/api/feedback/review`        | POST   | `token`, `feedbackId`, `decision`, `comment` | Administrator reviews feedback and submits a decision.                         |
| `/api/feedback/status/update` | POST   | `token`, `feedbackId`, `status`                | Update the status of a feedback record after review.                           |
| `/api/feedback/notify`        | POST   | `feedbackId`                                       | Notify the student of the feedback review result.                              |

##### 2.3. Demonstrate interface specification in detail with one or several samples between your system and external systems

##### 2.4. Meal Ordering Subsystem - Interface Specification

This section provides detailed specifications for the Meal Ordering subsystem's core interfaces, including request/response formats and authentication requirements.

###### 2.4.1. Order Creation Interface

| API Interface          | Method | Parameters                                 | Description           |
| ---------------------- | ------ | ------------------------------------------ | --------------------- |
| /api/meal/order/create | POST   | token, dish_ids, quantities, restaurant_id | Create new meal order |

**Request Parameters:**

- `token` (String, Header): JWT authentication token
- `restaurant_id` (Integer, Body): Target restaurant identifier
- `items` (Array, Body): Order items list
  - `dish_id` (Integer): Dish identifier
  - `quantity` (Integer): Order quantity
- `delivery_time` (String, Body, Optional): Expected pickup time (format: "HH:mm")
- `note` (String, Body, Optional): Special instructions (max 200 chars)

**Response:**

- Success (200): Returns `order_id`, `total_price`, `estimated_time`, `payment_url`
- Error (400): Invalid dish_id or insufficient stock
- Error (401): Token expired or invalid
- Error (403): Restaurant closed or user blacklisted

**Request Example:**

```json
POST /api/meal/order/create
Headers: {
  "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
Body: {
  "restaurant_id": 101,
  "items": [
    {"dish_id": 2001, "quantity": 2},
    {"dish_id": 2015, "quantity": 1}
  ],
  "delivery_time": "12:30",
  "note": "Less spicy please"
}
```

**Response Example:**

```json
{
  "code": 200,
  "data": {
    "order_id": 87654,
    "total_price": 45.50,
    "estimated_time": "12:45",
    "payment_url": "https://pay.smartcampus.com/order/87654"
  },
  "message": "Order created successfully"
}
```

###### 2.4.2. Order Status Query Interface

| API Interface          | Method | Parameters      | Description        |
| ---------------------- | ------ | --------------- | ------------------ |
| /api/meal/order/status | GET    | token, order_id | Query order status |

**Request Parameters:**

- `token` (String, Header): JWT authentication token
- `order_id` (Integer, Query): Order identifier

**Response:**

- Success (200): Returns `order_id`, `status` (PENDING/PREPARING/READY/COMPLETED/CANCELLED), `items`, `total_price`, `restaurant_info`, `create_time`, `update_time`
- Error (404): Order not found
- Error (403): Order belongs to different user

**Request Example:**

```http
GET /api/meal/order/status?order_id=87654
Headers: {
  "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

**Response Example:**

```json
{
  "code": 200,
  "data": {
    "order_id": 87654,
    "status": "PREPARING",
    "items": [
      {"dish_name": "Kung Pao Chicken", "quantity": 2, "price": 18.00},
      {"dish_name": "Fried Rice", "quantity": 1, "price": 9.50}
    ],
    "total_price": 45.50,
    "restaurant_info": {
      "name": "East Canteen",
      "location": "Building 12, 1st Floor"
    },
    "create_time": "2024-12-13 11:45:30",
    "update_time": "2024-12-13 12:10:15"
  }
}
```

###### 2.4.3. Review Submission Interface

| API Interface           | Method | Parameters                               | Description        |
| ----------------------- | ------ | ---------------------------------------- | ------------------ |
| /api/meal/review/submit | POST   | token, order_id, rating, comment, images | Submit dish review |

**Request Parameters:**

- `token` (String, Header): JWT authentication token
- `order_id` (Integer, Body): Completed order identifier
- `rating` (Integer, Body): Rating score (1-5)
- `comment` (String, Body): Review text (max 500 chars)
- `images` (Array, Body, Optional): Image URLs (max 3 images)
- `dish_ratings` (Array, Body, Optional): Individual dish ratings
  - `dish_id` (Integer): Dish identifier
  - `rating` (Integer): Dish-specific rating (1-5)

**Response:**

- Success (201): Returns `review_id`, `points_earned` (reward points for review)
- Error (400): Order not completed or already reviewed
- Error (403): Review content violates policy

**Request Example:**

```json
POST /api/meal/review/submit
Headers: {
  "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
Body: {
  "order_id": 87654,
  "rating": 5,
  "comment": "Delicious food and fast service!",
  "images": [
    "https://cdn.smartcampus.com/reviews/img_001.jpg"
  ],
  "dish_ratings": [
    {"dish_id": 2001, "rating": 5},
    {"dish_id": 2015, "rating": 4}
  ]
}
```

**Response Example:**

```json
{
  "code": 201,
  "data": {
    "review_id": 45621,
    "points_earned": 10
  },
  "message": "Review submitted successfully"
}
```

###### 2.4.4. Menu Retrieval Interface

| API Interface  | Method | Parameters           | Description                            |
| -------------- | ------ | -------------------- | -------------------------------------- |
| /api/meal/menu | GET    | token, restaurant_id | Get menu items for specific restaurant |

**Request Parameters:**

- `token` (String, Header): JWT authentication token
- `restaurant_id` (Integer, Query): Restaurant identifier
- `category` (String, Query, Optional): Filter by category (e.g., "staple", "beverage")
- `sort_by` (String, Query, Optional): Sort criteria ("price", "sales", "rating")

**Response:**

- Success (200): Returns array of dishes with `dish_id`, `name`, `price`, `description`, `category`, `stock`, `image_url`, `rating`, `sales_count`, `is_available`
- Success (304): Not Modified (if cache valid via ETag)
- Error (404): Restaurant not found

**Request Example:**

```http
GET /api/meal/menu?restaurant_id=101&category=staple&sort_by=sales
Headers: {
  "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

**Response Example:**

```json
{
  "code": 200,
  "data": [
    {
      "dish_id": 2001,
      "name": "Kung Pao Chicken",
      "price": 18.00,
      "description": "Classic Sichuan dish with peanuts",
      "category": "staple",
      "stock": 25,
      "image_url": "https://cdn.smartcampus.com/dishes/2001.jpg",
      "rating": 4.8,
      "sales_count": 156,
      "is_available": true
    },
    {
      "dish_id": 2015,
      "name": "Fried Rice",
      "price": 9.50,
      "description": "Egg fried rice with vegetables",
      "category": "staple",
      "stock": 30,
      "image_url": "https://cdn.smartcampus.com/dishes/2015.jpg",
      "rating": 4.5,
      "sales_count": 203,
      "is_available": true
    }
  ],
  "cache_time": "2024-12-13 12:00:00"
}
```



#### 3. Two Selected Analysis Mechanisms and Their Design Mechanisms 

##### 3.1 Data Persistence Mechanism

In the SmartCampus platform, data persistence serves as the core infrastructure that enables efficient and reliable campus lifestyle services. As the platform integrates multiple services—including dining, feedback submission, and campus card top-ups—it must handle highly heterogeneous data types: ranging from strongly consistent transactional data (e.g., user identities, order records, and top-up transactions) to highly flexible unstructured content (e.g., dish comments, feedback messages, and system logs). To meet diverse requirements for performance, consistency, and scalability across different scenarios, we adopt a hybrid persistence strategy that combines relational databases, document stores, and in-memory caching to build a layered and high-performance data storage architecture.

###### 3.1.1 Data Persistence Requirements

Data in SmartCampus exhibits significant diversity:

- **Structured transactional data**: such as student accounts, campus card balances, order statuses, and payment records-requiring ACID properties and strong consistency;
- **Semi-structured/unstructured data**: such as rich-media comments on dishes (text + images), user feedback content, and system operation logs-characterized by flexible schemas and high write frequency;
- **High-frequency read hotspots**: such as daily menus, restaurant information, session tokens, and dish ranking lists-demanding ultra-low latency responses.

To manage this data efficiently and securely, we abandon a single-database approach in favor of a multi-engine, collaborative hybrid persistence architecture, ensuring each data type is handled by the storage technology best suited to its characteristics.

###### 3.1.2 Persistence Architecture and Multi-Modal Storage Technologies

Our persistence architecture is built upon a three-tier data storage model:

1. **MySQL 8.0 (Relational Core Database)**Serves as the primary transactional database, storing all business entities requiring strong consistency, including `Student`, `CampusCard`, `Order`, and `TopUpTransaction`. The InnoDB engine guarantees ACID compliance and supports complex joins and foreign key constraints, making it ideal for critical operations such as order creation, balance deduction, and top-up confirmation.
2. **MongoDB 6.0 (Document-Oriented Extension Store)**Handles schema-flexible, write-intensive unstructured data, such as `Comment` documents (containing text and image URLs), `Feedback` submissions, and system audit logs. Its dynamic schema greatly simplifies the storage and evolution of rich-media comments and variable feedback forms.
3. **Redis 7.0 (In-Memory Caching Layer)**Deployed at the front of the data access path to cache frequently accessed data, including:

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

###### 3.1.4 Typical Data Persistence Scenarios

1. **Campus Card Top-Up and Balance Management**A user's top-up request triggers the creation of a `TopUpTransaction` entity, which is persisted to MySQL to ensure an immutable financial audit trail. Simultaneously, the updated `CampusCard.balance` value is refreshed in Redis, allowing subsequent balance queries to bypass the database entirely.
2. **Dish Comments and Feedback Submission**User-submitted rich-media comments are stored as JSON documents in MongoDB, preserving their original structure (including embedded images). Meanwhile, associated metadata-such as dish ID and user ID-remains in MySQL to facilitate cross-system relational analysis.
3. **Real-Time Dish Rankings**
   Based on voting statistics stored in MySQL, a background scheduled task (or event-driven processor) computes the latest rankings and writes the result set (including `dishId`, `score`, and `rank`) into a Redis Sorted Set. The frontend retrieves the full leaderboard with a single `ZRANGE` operation, achieving response times under 10ms.

##### 3.2 Security Mechanism

In the SmartCampus platform, security serves as the critical foundation that enables trustworthy and reliable campus lifestyle services. As the platform integrates multiple sensitive functions—including financial transactions, personal data management, and multi-role interactions—it must handle diverse security requirements: ranging from user authentication and authorization to data protection and attack prevention. To meet these requirements across different scenarios, we adopt a comprehensive security strategy that combines authentication, authorization, encryption, and monitoring to build a multi-layered and robust security architecture.

###### 3.2.1 Security Requirements

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

1. **Infrastructure Security Layer**Serves as the foundation, implementing security configurations, vulnerability scanning, and the principle of least privilege for all system components. Container security hardening and network segmentation ensure basic protection at the infrastructure level.
2. **Network Security Layer**Provides transport-level protection with mandatory HTTPS/TLS encryption, Web Application Firewall (WAF) deployment, and proper network isolation between different service tiers. This layer prevents eavesdropping and unauthorized network access.
3. **Session and Access Control Layer**Manages user authentication, authorization checks, and session security. JWT-based stateless authentication supports mobile clients while maintaining security. Role-based and attribute-based access control ensures proper permission management across the platform.
4. **Application Protection Layer**Implements input validation, output encoding, and business logic security checks. This layer prevents application-level attacks such as injection and cross-site scripting while ensuring data integrity throughout business processes.
5. **Monitoring and Response Layer**
   Provides real-time security monitoring, anomaly detection, and emergency response procedures. Security event correlation and automated alerting enable rapid response to potential threats.

###### 3.2.3 Security Component Design and Framework Integration

At the implementation level, security components are integrated throughout the application stack using Spring Security and complementary technologies:

- **Authentication implementation**: JWT-based stateless authentication using Spring Security filters and custom token services, supporting multiple client types including WeChat Mini Programs;
- **Authorization implementation**: Three-tier authorization combining URL-level security configuration, method-level `@PreAuthorize` annotations, and custom permission evaluators for data-level access control;
- **Data protection**: Integration of BCrypt for password hashing, AES encryption for sensitive data, and TLS for secure communications;
- **Attack prevention**: Built-in protections against common web vulnerabilities through Spring Security configurations and custom security filters.

###### 3.2.4 Typical Security Application Scenarios

1. **Student Payment Transaction Security**A student's payment request triggers JWT token validation, role-based permission checking, and payment signature verification. The transaction is recorded with complete audit information including timestamp, IP address, and operator identity, ensuring non-repudiation and traceability.
2. **Multi-Role Platform Access Control**Different user roles (student, merchant, administrator) receive distinct permission sets through JWT claims. The authorization layer enforces access restrictions based on both URL patterns and business method annotations, preventing privilege escalation and unauthorized data access.
3. **Sensitive Data Protection**
   User credentials are hashed using BCrypt before storage, while payment information is encrypted using AES-GCM. All sensitive data transmissions employ TLS 1.2+ encryption, and database connections use SSL protection to prevent data interception.

This security mechanism provides comprehensive protection while maintaining performance and usability, forming an essential foundation for the trusted operation of the SmartCampus platform.
 
#### 4. Two Use Case Realizations
##### 4.1 Meal Ordering Use Case

###### 4.1.1 Use Case Selection

We selected **Meal Ordering** because it: (1) represents the highest-frequency student activity (60-70% daily usage), (2) encompasses hybrid storage, JWT security, microservices coordination, and high concurrency, and (3) naturally demonstrates multiple design patterns addressing real architectural challenges.

###### 4.1.2 Design Patterns Applied

| Pattern | Problem | Solution | Benefit |
|---------|---------|----------|---------|
| **Adapter** | Multiple payment APIs with inconsistent interfaces | Unified `PaymentAdapter` interface with concrete adapters | Easy to add new payment methods; simplifies testing |
| **Repository** | Hybrid storage (MySQL/MongoDB/Redis) creates scattered data access logic | Domain-oriented repositories abstract database operations | Technology independence; centralized caching |
| **Strategy** | Dynamic pricing rules (discounts, membership, promotions) | `PricingStrategy` interface with runtime-selectable implementations | New promotions without modifying core logic |
| **Observer** | Order status changes require multiple reactions (notifications, updates, tasks) | `OrderStatusSubject` notifies `OrderObserver` implementations asynchronously | Decoupled notification logic; fault-tolerant |

###### 4.1.3 Architecture Integration

| Design Aspect | Architecture (Sec 2) | Design Mechanism (Sec 3) | Pattern |
|---------------|----------------------|--------------------------|---------|
| Authentication | API Gateway + JWT Filter | JWT Security | Security Filter |
| Menu Access | Menu Service | Redis + MySQL | Repository (cache-aside) |
| Order Storage | Order Service | MySQL Transactions | Repository (JPA) |
| Reviews | Feedback Service | MongoDB | Repository (aggregations) |
| Payment | Payment Gateway | HTTPS + AES | Adapter |
| Notifications | RabbitMQ | Async Messaging | Observer |

###### 4.1.4 Class Diagram

The following class diagram illustrates the complete design of the Meal Ordering use case, showing all entities, controllers, and design pattern implementations:
 
<p align="center">
  <img src="diagrams/cd_ordermeal_patterns.svg" alt="Meal Ordering Use Case - Class Diagram" title="Meal Ordering Use Case - Class Diagram" style="display:block; margin:0 auto; width:100%; max-width:800px; height:auto;">
</p> 

The diagram demonstrates:
- **Core Entities Layer**: Student, Order, Dish, and Restaurant classes with their relationships
- **Controllers Layer**: OrderController and MenuController managing business logic
- **Design Patterns Layer**: 
  - Adapter Pattern for payment method abstraction
  - Repository Pattern for data persistence abstraction  
  - Strategy Pattern for dynamic pricing strategies
  - Observer Pattern for order status notifications
- **Security Layer**: SecurityFilter and JWTTokenService for authentication and authorization 

###### 4.1.5 Sequence Diagrams

**Sequence Diagram 1: Order Creation Flow**

This sequence diagram illustrates the complete flow of a student placing a meal order, demonstrating the integration of security mechanisms, caching strategies, and design patterns:

<p align="center">
  <img src="diagrams/id_ordermeal.svg" alt="Order Creation Sequence Diagram" title="Order Creation - Interaction Diagram" style="display:block; margin:0 auto; width:100%; max-width:1000px; height:auto;">
</p>

**Key interactions demonstrated:**
- **JWT Authentication**: SecurityFilter validates the JWT token before allowing order creation
- **Cache-First Strategy**: MenuCacheService checks Redis for dish availability before querying MySQL
- **Repository Pattern**: OrderRepository abstracts persistence logic, coordinating MySQL and Redis operations
- **Adapter Pattern**: PaymentAdapter provides a unified interface for different payment methods (WeChat Pay, Alipay, Campus Card)
- **Observer Pattern**: NotificationService is triggered upon successful payment to send order confirmation

**Sequence Diagram 2: Order Query and Review Submission Flow**

This sequence diagram shows how students query their order history and submit reviews, highlighting the hybrid storage strategy and cache optimization:

<p align="center">
  <img src="diagrams/id_trackorder.svg" alt="Order Query and Review Sequence Diagram" title="Order Tracking and Review - Interaction Diagram" style="display:block; margin:0 auto; width:100%; max-width:1000px; height:auto;">
</p>

**Key interactions demonstrated:**
- **Cache-First Query**: System first attempts to retrieve order list from Redis cache
- **Cache Miss Handling**: On cache miss, data is fetched from MySQL and cached for subsequent requests
- **Ownership Validation**: JWTTokenService validates that the user owns the order before allowing review submission
- **Hybrid Storage**: Review content (rich text + images) is stored in MongoDB for flexibility, while order metadata is updated in MySQL
- **Ranking Update**: RankingService updates dish rankings in Redis Sorted Set for real-time leaderboard updates

Both diagrams demonstrate the seamless integration of Section 3's design mechanisms (persistence and security) with Section 2's architectural decisions (microservices, JWT authentication, hybrid storage).
 
#### 5. Architectural Styles and Design Decisions
##### 5.1 Architectural Styles

The solution adopts a hybrid style that integrates Microservices within a foundational Layered Architecture.

Layered Backbone with Embedded Microservices The system follows a strict Layered Architecture (Presentation, Gateway, Business Logic, Data Persistence) to ensure clear separation of concerns. Within the Business Logic layer, we integrated Microservices, decomposing core functionalities into independent units (e.g., Auth, Meal Services) to achieve high availability and scalability.

Polyglot Persistence Complementing the layered design, the data layer employs a Polyglot Persistence strategy. It utilizes MySQL for transactional consistency, MongoDB for unstructured feedback, and Redis for hotspot caching to precisely meet diverse storage requirements.

##### 5.2 Critical Design Decisions

Shift to Stateless JWT Authentication We replaced the initial OAuth 2.0 model with JWT (JSON Web Tokens). This eliminates the need for server-side session storage, allowing the system to scale effectively in a Kubernetes environment while reducing network overhead.

"Mobile-First" Strategy via WeChat Mini Program Prioritizing usability, we selected the WeChat Mini Program as the primary interface. Its "no-install" nature and high campus penetration (90%+) significantly lower adoption barriers compared to native mobile apps.

Asynchronous Processing for High Concurrency To handle peak dining traffic (11:30–13:00), we implemented an asynchronous messaging strategy using RabbitMQ. Non-critical tasks are offloaded from the main thread, preventing system blocking and ensuring sub-second response times.

#### 6. Non-Functional Requirements

##### 6.1 Security Requirements
The campus digital platform handles a large volume of sensitive data, including student identity information, campus card balances, payment records, dining preferences, and user feedback. Any data breach, identity impersonation, or payment fraud could severely damage user trust and potentially lead to legal and compliance risks. Given the platform's support for online payments, unified identity authentication, and integration with multiple systems, security must serve as the cornerstone of the system design.

###### 6.1.1 Layered Security Architecture and Evolution of Authentication Mechanism  
In the a2 phase, the system was initially designed with `OAuth 2.0` and SSO for unified authentication. However, we later recognized that `OAuth 2.0` introduces unnecessary complexity-such as authorization server management-for a closed-campus environment serving only internal users. It is also better suited for third-party integrations, whereas SmartCampus primarily uses its own front-end clients that benefit from a lighter, more integrated solution.

Accordingly, we replaced `OAuth 2.0` with a stateless authentication mechanism based on `Spring Security` and `JWT`. After login, the system issues a signed JWT containing the user's identity, roles, and permissions. Clients include this token in the Authorization header, and the API gateway or microservice interceptors validate its signature, expiration, and claims.

Because `JWT` is self-contained and verifiable via public-key cryptography, the system no longer requires centralized session storage or an authorization server-improving scalability, fault tolerance, and deployment simplicity. This approach better meets the platform's needs for high concurrency, low latency, and autonomous control, while fully satisfying assignment 1's security goal: "ensuring secure data transmission and access".

###### 6.1.2 End-to-End Encryption of Sensitive Data  
In response to assignment 1's requirement for "AES-256 encryption during transmission and storage", our technical stack explicitly employs `Spring Security with HTTPS (TLS 1.3)` to secure data in transit. At the database layer, `MySQL 8.0` enables Transparent Data Encryption (TDE), and `Redis 7.0` is configured with access controls and TLS-encrypted connections. Critical fields-such as campus card top-up amounts and payment credentials-are encrypted before persistence, ensuring that even in the event of unauthorized database access, plaintext data remains inaccessible.

###### 6.1.3 Secure Payment and Transaction Workflow  
In the campus card top-up subsystem, interactions between `TopUpController` and the external Payment Gateway are encapsulated using the Adapter pattern. Every payment request includes a cryptographic signature and a timestamp to prevent replay attacks. Only after receiving a confirmed success response from the payment provider does the system update the card balance and log the transaction via `AuditLogger`. This "deduct-first, credit-later" atomic workflow prevents financial inconsistencies caused by network failures or service interruptions.

###### 6.1.4 Fine-Grained Permission Control and Operational Auditing  
During dish publishing and review workflows, the system explicitly validates user permissions through use cases such as "Verify Permission" and "View User Authority". All critical operations-including publishing dishes, reviewing feedback, and modifying orders-are recorded in audit logs, associated with user IDs and timestamps. This fulfills Assignment 1's governance requirements for "data traceability" and "accountability".

##### 6.2 Usability Requirements

##### 6.2 Usability Requirements  
SmartCampus primarily serves on-campus students whose usage scenarios are highly fragmented-for example, placing meal orders between classes or checking card balances while queuing. If the interface is complex, the learning curve steep, or the operation flow lengthy, user adoption will suffer significantly. The "90%+ student adoption rate" target outlined in Assignment 1 fundamentally depends on an exceptional user experience. Therefore, usability is not merely a supplementary feature-it is a core driver of business success.

###### 6.2.1 User Journey-Centric Interface Redesign  
Section 5 of Assignment 2 illustrates optimizations across key interfaces, directly implementing assignment 1's goals of "reducing cognitive load" and enabling "first-time users to operate independently without training":  
- The **meal ordering page** now features a persistent shopping cart area with real-time price calculation, minimizing navigation jumps.  
- The **top-up page** offers both preset quick-amount buttons and a custom input field, balancing efficiency with flexibility.  
- The **voting page** auto-fills known context to eliminate redundant input.

###### 6.2.2 Multimodal Interaction and Context-Aware Help  
The system embeds a "?" help icon next to complex features (e.g. on the feedback submission page or new dish publishing page); clicking it instantly displays text-and-image-based guidance closely relevant to the current task. Additionally, as described in Section 5.3 of Assignment 2, the feedback subsystem allows users to submit input using a combination of text and images, lowering the barrier to expression-this is a concrete realization of assignment1's principle that "the help system should be closely aligned with the user's task context".

###### 6.2.3 Seamless Language Switching and Accessibility Support  
The frontend supports real-time switching between Simplified Chinese and English without requiring a page refresh, and the user's language preference is persistently stored in local cache. Additionally, all icons are accompanied by text labels, fully satisfying Assignment 1's requirement for "multilingual support and accessibility."

###### 6.2.4 Transparent State Visibility and Immediate Feedback  
Clear status indicators are provided throughout key user flows such as ordering, top-ups, and voting:  
- After a successful top-up, a notification appears and the card balance updates automatically.  
- Upon voting, a "Vote Successful" message is shown immediately, and rankings are refreshed in real time.  
- After submitting feedback, the system displays a "Pending Review" status along with an estimated processing time.  

This "action-feedback" loop significantly enhances users' sense of control and reduces anxiety, aligning with the usability principles of "clear operational logic" and "minimizing uncertainty".

##### 6.3 Performance and Scalability Requirements

##### 6.3 Performance and Scalability Requirements  
SmartCampus must serve a campus population of thousands, with usage patterns characterized by sharp peaks—particularly during meal times (11:30–13:00 and 17:00–18:30), when hundreds of concurrent meal orders, real-time balance queries, and ranking updates must be handled without degradation. The target of "reducing service delivery time by 40%" can only be achieved through a high-performance, horizontally scalable architecture.

###### 6.3.1 Microservices-Based Horizontal Scaling  
As detailed in Sections 2.1 and 2.2 of Assignment 2, we adopted a microservices architecture that decouples the system into independently deployable services (e.g., order service, top‑up service, voting service). Each service can be scaled horizontally based on load, using Kubernetes to manage automatic pod replication and service discovery via Spring Cloud Netflix Eureka. This design directly addresses Assignment 2’s requirement that “the system must be capable of scaling to support growth in user numbers and transaction volume.”

###### 6.3.2 Multi-Level Caching Strategy  
To satisfy the “low-latency access” objective, we implemented a two-tier caching layer:  
- **In‑memory hot‑data cache**: Frequently accessed data such as today’s menu, dish rankings, and campus‑card balances are stored in Redis, reducing database hits and delivering response times under 50 ms.  
- **Static resource CDN**: Dish images, help‑page illustrations, and other static assets are distributed via a content‑delivery network, lowering server load and improving page‑load speed for users across different campus locations.  
These measures collectively ensure that even during peak hours, the system maintains the sub‑second response times required.

###### 6.3.3 Asynchronous Processing and Message Queues  
Time‑insensitive operations—such as generating weekly dining reports, sending batch notifications, and updating aggregated rankings—are offloaded to background jobs powered by RabbitMQ. This prevents long‑running tasks from blocking critical user‑facing flows and enables the system to handle sudden traffic surges gracefully, thereby meeting the availability target of “99.9% uptime” stipulated.

###### 6.3.4 Database Read‑Write Separation and Connection Pooling  
MySQL 8.0 is configured with a master‑slave replication setup: write operations (e.g., orders, top‑ups) go to the master, while read‑intensive queries (e.g., menu browsing, ranking displays) are routed to read replicas. Combined with HikariCP connection pooling, this architecture sustains high throughput and avoids database bottlenecks, directly supporting the goal of “improving resource utilization to 85%+.”

##### 6.4 Maintainability and Extensibility Requirements  
SmartCampus is envisioned as a long‑term platform that will evolve with the campus’s needs. Adding new services (e.g., library‑seat booking, sports‑facility reservation) or modifying existing ones must not require extensive re‑engineering or introduce system‑wide instability. The architectural decisions described in Assignment 2 are explicitly guided by Assignment 1’s emphasis on “building scalable architecture supporting 50,000+ concurrent users” and “enabling data‑driven decision making for institutional planning.”

###### 6.4.1 Clear Separation of Concerns and Layered Design  
The layered architecture (presentation, security/gateway, business logic, data access, infrastructure, and storage) enforces strict boundaries between components. Developers working on the feedback subsystem, for example, need not understand the internals of the payment gateway; they interact only with well‑defined service interfaces. This modularity fulfills the demand for "reducing administrative overhead by 30% through system integration."

###### 6.4.2 API‑First Development and Contract‑Based Integration  
All service‑to‑service communication employs RESTful APIs with OpenAPI (Swagger) documentation, and external integrations (e.g., with the campus‑card system or third‑party payment providers) are wrapped behind adapters. This contract‑driven approach ensures that changes in one service do not cascade failures to others, and new features can be added by implementing new APIs without disrupting existing workflows—aligning with the principle of “establishing SmartCampus as the market leader in university digital transformation.”

###### 6.4.3 Centralized Configuration and Versioned Deployments  
Using Spring Cloud Config, environment‑specific settings (database URLs, feature toggles, external service endpoints) are stored in a version‑controlled repository (Git) and injected at runtime. Combined with Docker‑based containerization and Kubernetes deployment manifests, this allows the same service image to be promoted from development to production with minimal manual intervention, thereby supporting the goal of “enabling rapid, reliable iteration and feature rollout.”

###### 6.4.4 Comprehensive Monitoring and Automated Diagnostics  
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

For administrative users, we have independently developed a responsive web management dashboard based on Vue.js 3 + Vite. This backend system uses npm as the package manager and starts the development server on local localhost:5173 through the npm run dev command, supporting hot reload and modular development.

<p align="center">
  <img src="source/vue.png" alt="allscopes" title="scope" style="display:block; margin:0 auto; max-width:300px; height:auto;"/>
</p>

Taking the login page as an example, our front-end code is as follows:


```vue
<template>
<view class="container">
		<view class="circle-bg top-left"></view>
		<view class="circle-bg bottom-right"></view>
<template>
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
</template>
```

This page is designed based on system UI snapshots, offering a simple and efficient user authentication entry point. It includes fields for entering student ID and password, with secure password masking display and a responsive login button. The interface is optimized for mobile screens, supporting the WeChat Mini Program platform's instant access and use experience. The page effect is shown in the figure below:

<p align="center">
  <img src="source/Login_page.png" alt="allscopes" title="scope" style="display:block; margin:0 auto; max-width:700px; height:auto;"/>
</p>

##### 7.2. Back-end Prototyping

The backend system is built using the **Spring Boot 3.x** framework, following microservices architecture principles. The overall functionality is decomposed into multiple highly cohesive and loosely coupled service modules (such as meal service, feedback service, and campus card service). In the current prototyping phase, we have not yet integrated the full suite of Spring Cloud distributed governance components—such as a service registry or configuration center—and are instead focusing on implementing core business logic and validating API contracts for each microservice. Inter-service communication is currently handled via hardcoded URLs or local hosts file configurations for testing purposes. In subsequent iterations, we plan to introduce Spring Cloud Netflix Eureka as the service registry and discovery mechanism, complemented by Spring Cloud Config for centralized configuration management, thereby enabling comprehensive microservices governance in the production environment.

The frontend and backend communicate through **RESTful APIs**, with interface design following unified specifications, supporting JSON data format, and integrating global exception handling and standardized response structures.

We begin our work by creating a Spring Boot microservice called `auth-service` :

<p align="center">
  <img src="source/spring_initializr.png" alt="allscopes" title="scope" style="display:block; margin:0 auto; max-width:700px; height:auto;"/>
</p>

which is structured as follows:

Taking the **User Authentication Microservice** (auth-service) as an example, this service is responsible for handling security-related functions such as student and administrator login and session management. We have defined the following key interfaces for it:

- `POST /api/auth/login`: User password login
- `POST /api/auth/logout`: User logout
- `GET /api/auth/userinfo`: Get current user information

Core controller code snippet:

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
      
        // Validate username and password
        UserInfoBO user = authService.authenticate(request.getUsername(), request.getPassword());
        if (user == null) {
            return ResponseEntity.badRequest().body(ApiResponse.error("Invalid username or password"));
        }

        // Generate JWT access token
        String accessToken = tokenService.generateToken(user);
        // Return standardized response
        TokenInfoVO tokenVO = new TokenInfoVO(accessToken, user.getUserId(), user.getRole());
        return ResponseEntity.ok(ApiResponse.success(tokenVO));
    }
}
```

**Security Implementation:**

We have deeply integrated **Spring Security + JWT** to implement a stateless token-based authentication mechanism. All protected APIs require a valid JWT Access Token, which is uniformly validated at the gateway layer, ensuring clear system security boundaries and precise permission control.

**Current Development Status:**

Currently, each microservice has initially completed the development and integration testing of core interfaces. Testing can be performed through Postman or Swagger UI, laying a solid foundation for subsequent integration with the frontend mini-program and management backend.

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
