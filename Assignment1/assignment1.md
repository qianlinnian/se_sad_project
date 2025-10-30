# System Analysis and Design

- [System Analysis and Design](#system-analysis-and-design)
  - [1. Introduction](#1-introduction)
    - [1.1 Background](#11-background)
    - [1.2 Motivation](#12-motivation)
    - [1.3 Scope](#13-scope)
    - [1.4 Target Users](#14-target-users)
      - [Primary Users: Students](#primary-users-students)
      - [Secondary Users: Faculty \& Staff](#secondary-users-faculty--staff)
  - [2. Strategic Analysis](#2-strategic-analysis)
    - [2.1 SWOT](#21-swot)
    - [2.2 Goals](#22-goals)
    - [2.3 Initiatives](#23-initiatives)
  - [3. Roadmap](#3-roadmap)
  - [4. Use case modelling and Business Process Modelling](#4-use-case-modelling-and-business-process-modelling)
  - [5. Glossary of terms](#5-glossary-of-terms)
  - [6. Supplementary specification](#6-supplementary-specification)
  - [7. Initial snapshots of the system's user interface](#7-initial-snapshots-of-the-systems-user-interface)
  - [8. AI tool usage disclosure](#8-ai-tool-usage-disclosure)
  - [9. References](#9-references)
  - [10. Team member contributions](#10-team-member-contributions)
  - [11. Agile artifacts](#11-agile-artifacts)

## 1. Introduction

**SmartCampus** — Your Campus Life Helper
**Team Name**：CampusCode
**Team Members**：
2353924 Feng Juncai    冯俊财
2351869 Ji Peng        纪鹏
2353240 Zhang Shikou   张诗蔻
2352993 Yu Yilian      于伊莲

### 1.1 Background

Modern universities offer various digital services (library, academic portal, dining, facility management), but these operate independently with separate interfaces, authentication systems, and data structures. Students must switch between multiple platforms daily. While many universities have developed integrated platforms to consolidate digital services, current implementations have limitations. For example, existing platforms focus primarily on academic management, with minimal integration of daily life services.

### 1.2 Motivation

SmartCampus reimagines integrated campus platforms with a student-first approach, enhancing rather than replacing existing infrastructure. Our motivation stems from recognizing that students' campus life extends beyond academics—they need to reserve library seats, check package notifications, report maintenance issues, and order meals efficiently. We aim to provide true integration encompassing both academic and daily life services, delivering proactive personalized experiences through mobile-first design.

### 1.3 Scope

SmartCampus focuses specifically on enhancing students' campus life by intelligently managing diverse services through four integrated subsystems. 

<img src="diagrams/scopeall.svg" alt="allscopes" title="scope" width="auto" style="margin: 0;"/>

- The **Daily Life Service** Subsystem facilitates convenient canteen meal ordering with mobile payment integration, real-time package collection notifications linked to campus courier services, a comprehensive lost-and-found platform connecting the campus community, sports facility booking for gyms and courts, and campus shuttle schedule access with real-time location tracking.

- The **Library Service** Subsystem enables real-time seat reservation with availability tracking, book borrowing and renewal with automated due-date reminders, study space inquiry across campus locations, and personal reading analytics to help students track their academic progress. 

- The **Logistics Management** Subsystem streamlines dormitory repair requests with photo documentation and progress tracking, utility bill inquiry and convenient online payment options, campus card top-up services with transaction history, and comprehensive facility maintenance tracking across campus buildings.

- The **Academic Service** Subsystem provides intuitive online course selection, personalized schedule management with conflict detection, comprehensive grade inquiry with statistical analysis and trend visualization, exam schedule tracking with countdown reminders, and credit progress monitoring toward graduation requirements.

These four subsystems work together to create a unified, intelligent campus service ecosystem that addresses students' comprehensive needs throughout their daily campus life.

### 1.4 Target Users

#### Primary Users: Students

- **Population**: 15,000-30,000 per university
- **Needs**: Integrated access to library, academic, dining, and logistics services
- **Usage**: 80%+ mobile, high frequency during peak hours
- **Pain Points**: Multiple logins, scattered information, time-consuming tasks

#### Secondary Users: Faculty & Staff

- **Faculty**: Library access, course management, facility booking
- **Service Staff**: Administrators Staff
- **Needs**: Operational dashboards, real-time updates, reporting tools 

## 2. Strategic Analysis

Conduct a strategic analysis, such as a SWOT and TOWS analysis, for your team and the proposed product, and then clarify your business goals and initiatives. 

### 2.1 SWOT

Strengths:

- **Comprehensive Integration**: Unified platform for library, dining, logistics, and academic services
- **User-Centric Design**: Focus on reducing student task management time by 50%
- **Technical Expertise**: Team has strong background in system analysis and design
- **SSO Integration**: Seamless authentication across existing campus systems
- **Mobile-First Approach**: Responsive design for multi-device access

Weaknesses:

- **Resource Constraints**: Limited development team and budget
- **System Dependencies**: Relies on existing campus infrastructure and APIs
- **No Established Brand**: First-time product with no user base
- **Data Privacy Risks**: Handling sensitive student personal and academic information
- **Limited Testing Scope**: Cannot test with real users before deployment

Opportunities: 

- **High Market Demand**: 85% of surveyed students want unified campus services
- **Digital Transformation Trend**: Universities investing in smart campus initiatives
- **Market Gap**: Few comprehensive campus service platforms exist in China
- **Government Support**: National policies promoting smart education and digital campuses
- **Scalability Potential**: Can expand to other universities after successful pilot

Threats:

- **Existing Habits**: Students already use separate apps (WeChat, Alipay) for services
- **Competition**: Other universities developing similar platforms 
- **User Resistance**: Students may resist learning new platform
- **Budget Constraints**: Universities may have limited IT investment budgets
- **Technology Changes**: Rapid evolution of mobile technologies may require frequent updates

### 2.2 Goals

SmartCampus aims to create a comprehensive, user-friendly platform that integrates all essential campus services. Our primary objectives include implementing single sign-on authentication across all services, reducing students' daily routine task management time from 30-60 minutes to under 15 minutes through intelligent automation, and delivering personalized, proactive notifications based on individual user behavior patterns. We strive to seamlessly connect the four core subsystems while maintaining data security and user privacy.  

The platform maintains a clear focus on student-facing services to ensure optimal user experience and system efficiency. Future expansion possibilities include course evaluation systems, campus marketplace for student trading, study group matching based on courses and interests, and comprehensive event calendars, all contingent on user feedback and demonstrated demand.

### 2.3 Initiatives

To achieve our business objectives, we have identified four core strategic initiatives:

- Initiative 1: Unified Authentication & System Integration

Implement seamless single sign-on experience across all campus systems through OAuth 2.0 and a unified API gateway.

- Initiative 2: Intelligent Task Automation

Significantly reduce students' daily task management time through smart notifications, automatic renewals, and one-click workflows.

- Initiative 3: Mobile-First User Experience

Ensure optimal mobile performance and experience through responsive design and progressive web application technologies.

- Initiative 4: User Adoption & Continuous Improvement

Drive user behavior change and continuously optimize the product through interactive onboarding, campus-wide promotion, and rapid iteration.

## 3. Roadmap

Build an agile or MVP roadmap to provide a clear vision and timeline.  
| Phase | Objective | Key Features | Success Criteria |
|-------|-----------|--------------|------------------|
| **Phase 1: Foundation** | Establish core infrastructure and validate basic functionality | - Single Sign-On (SSO)<br>- Library seat reservation & book borrowing<br>- Basic canteen meal ordering<br>- Course schedule inquiry<br>- Mobile-responsive interface | - Core functionality operational<br>- Positive pilot user feedback<br>- System stability validated |
| **Phase 2: Expansion** | Extend service coverage based on user feedback | - Package notification system<br>- Dormitory repair requests<br>- Sports facility booking<br>- Intelligent notifications<br>- Campus shuttle tracking<br>- Lost & Found platform | - Full system integration<br>- Reduced task management time<br>- High feature adoption |
| **Phase 3: Intelligence** | Enhance experience through smart features | - Personalized recommendations<br>- Predictive notifications<br>- Analytics dashboard<br>- Auto-renewal for library books<br>- Smart conflict detection<br>- Admin management portal | - AI features operational<br>- High user retention<br>- Scalability demonstrated |

The development follows agile methodology with iterative sprints, continuous user feedback integration, and phased rollout to minimize risks and ensure quality delivery.

## 4. Use case modelling and Business Process Modelling

## 4.1. Use Cases Modeling

### 4.1.2. Academic Affairs Service Subsystem

##### Use Case Diagram
<img src="diagrams/AASS-UCD.svg" alt="allscopes" title="scope" width="auto" style="margin: 0;"/>

##### Detailed Specification for Use Case

Use Case: **Select Courses**
---

|Use Case| Select Courses |
| :---: | :--- | 
| ID | UC01 | 
| Specification | Students query course information through the system and register for courses of interest. The system automatically checks for time conflicts and updates the schedule. | 
| Actors | Student | 
| Pre-condition | - Student is logged into the system.<br>- The course registration period is open.<br>- Student has not reached the maximum course limit.| 
| Basic Path | 1. Student selects the "Select Courses" function.<br>2. System executes Search Courses.<br>3. Student browses the list of available courses.<br>4. Student selects one or more courses to register for.<br>5. System executes Register for Courses.<br>6. System invokes Check Conflicts.<br>7. If no conflicts, system executes Update Schedule.<br>8. System displays "Course selection successful." | 
| Alternative Path | 1. Course is full:<br>- System displays "This course is full," registration denied.<br>- Flow ends.<br>2. Time conflict exists:<br>- System displays "Selected course conflicts with existing schedule."<br>- User can choose to cancel or proceed.<br>- If proceeding, mark the conflict status (e.g., "Pending confirmation").<br>3. Exceeds maximum course limit:<br>- System displays "Maximum course limit reached."<br>- Prevents adding more courses.| 
| Exception Flows| 1. Network interruption:<br>- Display "Operation failed, please try again"<br>2. System timeout:<br>- Automatically return to the main menu  |
| Post condition | <br>- Successfully selected courses are added to the student's schedule.<br>- The system records the registration information.<br>- If conflicts exist, the user is notified | 

<br></br>

Use Case: **Drop Courses**
---

|Use Case| Drop Courses |
| :---: | :--- | 
| ID | UC02 | 
| Specification | Students remove selected courses from their current schedule. The system checks for any impact on other arrangements and updates the schedule. | 
| Actors | Student | 
| Pre-condition | - Student is logged into the system.<br>- Student has selected at least one course.<br>- The course drop period is open | 
| Basic Path | 1. Student selects the "Drop Courses" function.<br>2. System displays the list of currently selected courses.<br>3. Student selects a course to drop.<br>4. System executes Register for Courses (in reverse).<br>5. System invokes Check Conflicts (to check if other courses are affected).<br>6. If no conflicts, system executes Update Schedule.<br>7. System displays "Course drop successful." | 
| Alternative Path | 1. Course cannot be dropped:<br>- System displays "This course cannot be dropped."<br>- Flow ends.<br>2. Dropping causes credit deficit:<br>- System displays "Total credits fall below the minimum requirement after dropping."<br>- User can choose to proceed or cancel.<br>3.Past the drop deadline:<br>- System displays "The course drop period has ended."<br>- Prevents dropping the course.| 
| Exception Flows| 1. Database write failure : <br>Display "Course drop failed, please contact administrator".<br>2. Network interruption:<br>Return to error page.  |
| Post condition | - The selected course is removed from the student's schedule.<br>- The system updates course enrollment numbers and schedule information.<br>- May release a spot for another student | 

<br></br>

Use Case: **Courses Schedule Query**
---

|Use Case| Courses Schedule Query |
| :---: | :--- | 
| ID | UC03 | 
| Specification | Students, teachers, and administrators can query course schedule information, but their permissions and accessible content differ. Administrators have the highest level of authority and can perform management operations. | 
| Actors | Student , Teacher , Administrator | 
| Pre-condition | - The user (student, teacher, or administrator) has successfully logged into the system.<br>- Valid course schedule data has been entered into the system. | 
| Basic Path | 1.The user (student, teacher, or administrator) navigates to the "Course Schedule Query" interface.<br>2. The system displays relevant query options and interface elements based on the user's role.<br>3. The user enters query criteria.<br>4. The system retrieves and displays the course schedule information that matches the criteria. | 
| Alternative Path | 1. A student performs a query operation:<br>- The system displays the detailed schedule for the courses the student has selected .<br>2. A teacher performs a query operation:<br>- The system displays the detailed schedule for the courses the teacher is teaching.<br>3. An administrator performs a query operation:<br>- The system displays the complete schedule information for all courses.<br>- The administrator can perform management operations, including: Adding, editing, deleting course schedules or reviewing and approving requests to change course schedules. | 
| Post condition | - The course schedule information corresponding to the user's role has been successfully displayed. | 

<br></br>

#### concise text descriptions:<br>
Use Case : **Exam Arrangement Query**<br>
This use case involves students, teachers, and administrators in querying and updating exam arrangement information.

- **Students** can query the exam times, locations, and related details for specific courses; view their own exam schedules to plan their review times and arrange other activities.
- **Teachers** can view the exam arrangements for the courses they are responsible for, including proctoring information; update or modify exam arrangements, such as changing exam times or locations.
- **Administrators** can manage the exam arrangement information for the entire school; review and approve exam arrangement updates submitted by teachers; publish new exam arrangements or make global adjustments to existing arrangements.

<br>

Use Case : **Grade Query and Analysis**<br>
This use case involves students, teachers, and administrators in querying, analyzing, and managing grade information.

- **Students** can query their own grades, including course grades, exam scores, and overall performance; analyze their grade trends, comparing performance across different semesters or different courses; request grade re-evaluation if they have doubts about their grades.
- **Teachers** can view and analyze the grades of students in the courses they teach; manage the entry and updating of grades to ensure accuracy; handle students' grade query requests and grade re-evaluation requests.
- **Administrators** can oversee the entire school's grade management process, ensuring the fairness and transparency of grades, and deal with system-level issues related to grade queries and analysis, such as data inconsistencies or system errors.

## 4.2. Activity Diagrams

##### • Online Course Selection<br>
The process begins with "Login and Enter the System", indicating that the student logs in and accesses the interface. The student can then "Search Courses" and "Browse Available Courses". After selecting a course, the student performs the "Register for Courses" action. The system checks for any time or resource conflicts in the selected courses, leading to a decision node: "No Conflicts?".
If there are conflicts: The student must first "Resolve Conflicts", for example, by changing the course or adjusting the schedule.
If there are no conflicts: The process proceeds directly to the next step.

Regardless of whether conflicts are resolved, the process ultimately executes "Update Schedule" (Update Course Timetable), saving the registration results into the student's course schedule. The process ends with a red solid circle, indicating the completion of the operation.

Throughout the process, the student can also choose "Drop Courses", which bypasses the registration steps and directly enters the conflict-checking phase, ultimately updating the schedule.
<br></br>
<img src="diagrams/OSC-AD.svg" alt="allscopes" title="scope" width="auto" style="margin: 0;"/>

<br></br>
##### • Course Schedule Query
This diagram illustrates the process through which students and teachers access their relevant course information after identity authentication. The system, acting as an intermediary, is responsible for identity verification and data presentation, ensuring the security and personalized display of information. Students log in and enter the course schedule section to perform a query operation; after the system verifies their identity, it displays the details of the courses they have selected. Teachers also perform query operations, upon which the system verifies their identity and displays detailed information about the courses they are responsible for. Teachers can then view the details of the courses they teach, concluding the process.
<br></br>
<img src="diagrams/OSQ-AD1.svg" alt="allscopes" title="scope" width="auto" style="margin: 0;"/>

<br></br>
In course schedule management, administrators manage course arrangements through operations such as querying, approving, and modifying. The system is responsible for data display and persistent updates. The process begins at the administrator's end, where the administrator performs a query operation, to which the system responds by displaying the complete schedule for all courses. Based on the displayed results, the administrator conducts management operations. They have the option to either review and approve change requests, prompting the system to execute the changes, or directly add, edit, or delete course schedules, leading the system to update the database accordingly.
<br></br>
<img src="diagrams/OSQ-AD2.svg" alt="allscopes" title="scope" width="auto" style="margin: 0;"/>



## 5. Glossary of terms

At least 10 terms related to the problem's domain.

## 6. Supplementary specification
### 6.1 Usability
#### 4.1 Learning Time
The platform features an intuitive user interface design and operational workflows that align with users' mental models, significantly reducing the difficulty for new users to get started. With a simple navigation structure, clear functional icons, and consistent interaction patterns, ordinary users can independently perform core operations upon first login without undergoing complex training. Key business processes are designed in a wizard-style or form-based manner, guiding users step by step to ensure clear operation logic and reduced cognitive load.

#### 4.2 Language Support
To meet the needs of an international campus environment and enhance accessibility for users with different language backgrounds, the platform provides comprehensive multilingual support. The platform natively supports both Simplified Chinese and English as interface languages. Users can switch between display languages in real time; the switching process does not require a page refresh, and the user's language preference will be persistently saved.

#### 4.3 Help System
Next to complex functional modules, a "?" help icon is provided. Clicking the icon displays concise operation instructions or frequently asked questions directly related to the current page content, reducing the effort required for users to locate information. The platform integrates a centralized "Help Center" or "User Guide" module. Users can search by keywords to quickly find required operation instructions, policy explanations, or troubleshooting solutions. Content is presented in structured articles, illustrated tutorials, or short videos for easy comprehension. A convenient "Feedback" entry is provided, allowing users to submit usage issues, feature suggestions, or bug reports with one click.



## 7. Initial snapshots of the system's user interface

### 7.1 Course Schedule Query Interface Snapshot
• By clicking on the "Individual Course Schedule" section on the homepage, you can enter the Individual Course Schedule interface where different roles, upon logging in, can view their corresponding personal course schedules. By querying the current semester and time period, you can filter the course arrangements for the current time. The course schedule can be viewed by scrolling left and right, and detailed information about each course can be accessed by clicking and sliding to view further details. By clicking the "More" button at the top right corner, detailed course information can be viewed; for example, students can check course credits, course nature, course notes, etc., while teachers can view the number of students attending, classmate lists, etc.

<img src="diagrams/CSQ-MD.png" alt="allscopes" title="scope" width="auto" style="margin: 0;"/>
<br></br>
• Click on the search box for academic year and semester to select the desired academic year and semester.
<br></br>
<img src="diagrams/CSQ-MD1.png" alt="allscopes" title="scope" width="auto" style="margin: 0;"/>
<br></br>

### 7.2 Course Selection Interface Snapshot
• By clicking on the course selection section on the homepage, students enter the Online Course Selection interface. They can search for the course types they are interested in, and add desired courses to their course selection list. The specific course schedules for selected course types will be displayed in the related courses list, showing detailed information and the current number of enrolled students. When the number of enrolled students reaches the maximum capacity, the course can no longer be selected. After selecting courses, students can view their current schedule in this interface. If there are any scheduling conflicts, a conflict warning will appear. Schedules without conflicts will be displayed in the temporary timetable, and students must click the "Save" button to finalize and save their course schedule.

<img src="diagrams/CSQ-MD2.png" alt="allscopes" title="scope" width="auto" style="margin: 0;"/>
<br></br>
<img src="diagrams/CSQ-MD3.png" alt="allscopes" title="scope" width="auto" style="margin: 0;"/>
<br></br>
<img src="diagrams/CSQ-MD4.png" alt="allscopes" title="scope" width="auto" style="margin: 0;"/>



## 8. AI tool usage disclosure

**yyl** : I uploaded an activity diagram and requested the AI to help analyze the logical structure of the UML activity diagram. During the process, I frequently asked for Chinese-to-English and English-to-Chinese translations. I also asked the AI to help find books related to the subject area, and it recommended several relevant monographs, providing information such as authors, publishers, and core content to help me further understand the topic.

## 9. References
### Related Books:

1. "Fundamentals of Smart Campus: Opening the Door to Smart Education"<br>
Author: **Li Jinsheng** <br>
This book provides a comprehensive introduction to the architecture of smart campuses, key technologies , and their applications in education. It covers planning, design, implementation, operation and maintenance, and evaluation systems for smart campuses. The book also offers in-depth discussions on practical explorations and future development trends of smart campuses, encompassing the underlying technological support required for one-stop service platforms.


## 10. Team member contributions

| Members               | Part 1 | Part 2 | Part 3 | Part 4 | Part 5 | Part 6 | Part 7 | Part 8 | Part 9 | Percent |
| --------------------- | ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------ | ------- |
| Feng Juncai  2353924  | ✓      | ✓      | ✓      |        |        |        |        |        |        |         |
| Ji Peng  2351869      |        |        | ✓      |        |        |        |        |        |        |         |
| Zhang Shikou  2353240 |        |        | ✓      |        |        |        |        |        |        |         |
| Yu Yilian  2352993    |        |        | ✓      |        |        |        |        |        |        |         |

## 11. Agile artifacts

### • User story map
Based on an in-depth understanding of user needs, we focused on the Smart Campus One-Stop Service Platform, identifying the most critical and urgent functional requirements while uncovering numerous innovative points for optimization. By evaluating the priority of these needs, we created a clear user story map. This map not only provides a blueprint for defining the platform's functional modules and facilitating team collaboration, but also ensures that the development direction is highly aligned with user expectations. Leveraging the agile development model, we were able to quickly build a prototype of the platform and, in subsequent iterations, continuously gather feedback from teachers and students to dynamically adjust features, ensuring the platform genuinely addresses pain points in campus life and delivers a convenient, efficient user experience. This development approach has also significantly improved project efficiency, enabling the team to flexibly respond to the rapid changes in educational informatization and continuously refine services that better fit the campus context.
<br></br>
<img src="diagrams/USM.png" alt="allscopes" title="scope" width="auto" style="margin: 0;"/>