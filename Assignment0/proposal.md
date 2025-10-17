# 系统设计与分析
## SmartCampus 
——Your Campus Life Helper
### Team name 
CampusCode
### team members 
2353924 Feng Juncai  冯俊财
2351869 Ji Peng      纪鹏
2353240 Zhang Shikou 张诗蔻
2352993 Yu Yilian    于伊莲

## Project description
#### 1. Backgrounds & motivations
##### 1.1. Backgrounds

Modern universities provide various digital services to students, including library systems, academic portals, dining services, and facility management platforms. However, these services typically operate independently with separate interfaces, authentication systems, and data structures. Students must navigate multiple platforms daily to access basic campus functions.While this fragmentation is manageable, it creates several inefficiencies.

Many universities, including ours, have developed integrated campus platforms to consolidate digital services. These existing systems typically provide basic functionality across library, academic, and administrative services through a unified interface.

However, current implementations often face several limitations,for example, Existing platforms focus primarily on academic management, with minimal integration of daily life services.

##### 1.2. Motivations

SmartCampus is not about replacing existing infrastructure, but rather reimagining what an integrated campus platform could be with a student-first design philosophy and modern technical capabilities.

What we want is comprehensive integration: Extending beyond academic services to include daily life functions, such as dining, packages, lost-and-found.

#### 2. Main goals  
 
Create a comprehensive, user-friendly, one-stop digital platform that integrates all essential campus services, enhancing daily experiences for students, faculty, and staff.
 
#### 3. Intended users and key usability goals 
##### 3.1. Students (Primary Users)
***User profile:***
- Age range:18-25 years old
- Time-sensitive needs, value efficiency and convenience
  
***Key Benefits & Goals:***
- *Unified Access*: Single sign-on for all campus services, eliminating multiple logins
- *Time Efficiency*: Reduce daily routine tasks from 30-60 minutes to under 15 minutes
- *Real-time Information*: Live updates on seat availability, course enrollment, package arrivals
- *Personalization*: Customized dashboard and smart recommendations based on usage patterns
- *Mobile Convenience*: Access all services anytime, anywhere
  
***Usability Goals:***
- *Learnability*: New users can quickly understand and complete basic tasks without extensive training
- *Efficiency*: Minimize steps required for frequent operations
- *Performance*: Fast response times for smooth user experience
- *Effectiveness*: High task completion success rate with minimal errors
##### 3.2. Faculty Members
***User Profile:***
  - Age range: 25-65 years old
  - Varying technical proficiency
  - Focus on teaching efficiency and student management

***Key Benefits & Goals:***
  - *Administrative Efficiency*: Reduce routine administrative workload by 40%
  - *Simplified Course Management*: Streamlined grade entry, attendance, and material distribution
  - *Enhanced Communication*: Direct channel to students for announcements and Q&A
  - *Flexible Access*: Manage tasks via both desktop and mobile platforms

***Usability Goals:***
  - Professional, academic-appropriate interface
  - Minimal training required (<30 minutes for full proficiency)
  - Support for batch operations
  - Clear help documentation

##### 3.3. Administrative Staff

***User Categories:***
- Library administrators, Academic affairs officers, Facility managers, Student services staff

***Key Benefits & Goals:***
- **Process Automation**: Reduce manual processing by 60%
- **Data-Driven Decisions**: Real-time dashboards and comprehensive analytics
- **Improved Service Quality**: Faster response to student requests
- **Accountability**: Complete audit trails and reporting capabilities

***Usability Goals:***
- Powerful backend management tools
- Batch operation capabilities
- Role-based access control
- Comprehensive reporting features

 
#### 4. Notes on existing similar products: their utility and limitations and advantages and disadvantages
  
#### 4. Notes on Existing Similar Products
 

| Platform Type | Utility | Advantages | Limitations | Disadvantages |
|--------------|---------|------------|-------------|---------------|
| **Tongxinyun**<br>(Our University) | Provides unified authentication and academic service management for students | • Stable infrastructure<br>• Reliable official data<br>• Comprehensive academic functions<br>• Institutional support | • Academic-focused scope<br>• Lacks daily life services<br>• Limited personalization | • No dining/package/lost-and-found services<br>• Traditional interface design<br>• Missing smart recommendations<br>• No proactive notifications |
| **WeChat Mini Programs** | Offers quick access to individual campus services through WeChat ecosystem | • Lightweight, no installation<br>• Fast access<br>• Low development costs<br>• Works well for single tasks | • Fragmented service structure<br>• No data integration<br>• Platform restrictions | • Requires separate apps for each service<br>• Inconsistent user experience<br>• WeChat account dependency<br>• Limited advanced features |
| **Commercial Platforms**<br>(今日校园, 易班) | Delivers comprehensive campus management solutions across multiple universities | • Professional development<br>• Regular updates<br>• Mature architecture<br>• Rich feature sets | • Generic design approach<br>• Third-party data handling<br>• Customization difficulties | • Not tailored to specific campus<br>• Privacy concerns<br>• Monetization priorities<br>• Integration challenges with existing systems |

**Summary:** Existing solutions each serve specific purposes but lack comprehensive integration. Tongxinyun excels at academic administration but misses daily life services. WeChat mini programs provide convenience but create fragmentation. Commercial platforms offer breadth but sacrifice campus-specific customization and data privacy. SmartCampus aims to combine the institutional reliability of Tongxinyun, the accessibility of mini programs, and the comprehensiveness of commercial platforms, while addressing their collective shortcomings through integrated daily life services, personalized experiences, and modern intelligent features.
 

#### 5. Main functionality and characteristics

SmartCampus integrates four core subsystems to provide comprehensive campus services, combining essential academic functions with daily life conveniences through a unified platform.

##### 5.1 Library Service Subsystem 

**Core Functions:**
- **Seat Reservation & Management**: Real-time seat availability display with advance booking capabilities
- **Book Borrowing & Renewal**: Search, borrow, and renew books with automated due date reminders
- **Study Space Inquiry**: Browse and reserve different types of study spaces (quiet zones, group rooms, etc.)
- **Borrowing History Statistics**: Personal reading analytics and borrowing patterns

**Key Characteristics:**
- Real-time seat availability map with visual interface
- Smart recommendations based on study habits
- Automated renewal reminders before due dates
- Integration with library's existing management system

##### 5.2 Academic Service Subsystem 

**Core Functions:**
- **Online Course Selection**: Browse course catalog, check availability, and enroll in courses
- **Course Schedule Query**: Personal timetable with classroom locations and instructor information
- **Grade Inquiry & Analysis**: View grades with statistical analysis and GPA tracking
- **Exam Schedule Query**: Centralized exam timetable with location and time details
- **Credit Progress Tracking**: Monitor degree requirements and credit completion status

**Key Characteristics:**
- Intuitive course selection with conflict detection
- Visual grade analytics with trend charts
- Automated exam reminders and countdown notifications
- Progress visualization toward graduation requirements
- Integration with university's academic management system

##### 5.3 Daily Life Service Subsystem 

**Core Functions:**
- **Canteen Ordering & Payment**: Browse menus, pre-order meals, and make mobile payments
- **Package Collection Notification**: Real-time alerts when packages arrive at campus collection points
- **Lost & Found Platform**: Report lost items and search for found items with photo uploads
- **Sports Facility Booking**: Reserve gyms, courts, and sports equipment
- **Campus Shuttle Schedule**: Real-time bus locations and timetable information

**Key Characteristics:**
- Real-time canteen crowd information to avoid peak hours
- Push notifications for package arrivals
- Photo-based lost item matching system
- Facility availability calendar with booking conflicts prevention
- Live shuttle tracking with estimated arrival times

##### 5.4 Logistics Management Subsystem 🔧

**Core Functions:**
- **Dormitory Repair Requests**: Submit maintenance requests with photo documentation
- **Utility Bill Inquiry & Payment**: Check and pay electricity and water bills online
- **Campus Card Top-up**: Add funds to campus card for various campus services
- **Facility Maintenance Management**: Track repair status and maintenance schedules

**Key Characteristics:**
- Photo upload for detailed repair descriptions
- Automated bill reminders before due dates
- Multiple payment methods (WeChat Pay, Alipay, bank cards)
- Real-time repair request status tracking
- Maintenance history records

 
#### 6. Novelty of your solution and enhancements suggested 

#### 7. Team organization and preliminary project planning

#### 8. Engineering process and methodologies

#### 9. Team collaboration platforms or systems 

#### 10. Potential for further development 

#### 11. Related technologies (platform, languages, libraries, tools, etc.)

#### 12. Challenges that you think you may encounter during the project’s development

#### 13. A note on how this project can help your professional growth (how you would benefit from it).  