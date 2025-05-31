# Entity-Relationship Diagram - Use Case Style
## Drowsiness Detection System Database Schema

### 🎯 System Overview
This ER diagram represents the database schema for a comprehensive drowsiness detection system designed to monitor driver alertness and prevent accidents through real-time analysis.

---

## 📋 Entity Catalog

### 🟦 **Primary Entities**

#### 👤 **USERS Entity**
**Purpose**: Central entity for user management and authentication
**Shape**: Rectangle (Primary Entity)

| Attribute | Type | Constraint | Description |
|-----------|------|------------|-------------|
| 🔑 `username` | VARCHAR(50) | PRIMARY KEY | Unique user identifier |
| 🔒 `password` | VARCHAR(255) | NOT NULL | Encrypted password |
| 👤 `first_name` | VARCHAR(50) | NOT NULL | User's first name |
| 👤 `last_name` | VARCHAR(50) | NOT NULL | User's last name |
| 📧 `email` | VARCHAR(100) | UNIQUE, NOT NULL | Contact email |
| 📞 `phone` | VARCHAR(15) | NULL | Contact phone number |
| 👥 `gender` | ENUM | NULL | Gender (M/F/Other) |
| 📅 `birth_date` | DATE | NULL | Date of birth |
| 🏠 `address` | TEXT | NULL | Physical address |
| 🚗 `license_number` | VARCHAR(20) | UNIQUE | Driving license |
| ⏱️ `driving_experience` | INT | NULL | Years of experience |
| 📅 `created_at` | TIMESTAMP | DEFAULT NOW() | Account creation |
| 📅 `last_login` | TIMESTAMP | NULL | Last login time |

---

#### 📊 **DETECTION_SESSIONS Entity**
**Purpose**: Track individual monitoring sessions
**Shape**: Rectangle (Primary Entity)

| Attribute | Type | Constraint | Description |
|-----------|------|------------|-------------|
| 🔑 `session_id` | INT | PRIMARY KEY, AUTO_INCREMENT | Unique session ID |
| 🔗 `username` | VARCHAR(50) | FOREIGN KEY | References USERS.username |
| ⏰ `start_time` | TIMESTAMP | NOT NULL | Session start time |
| ⏰ `end_time` | TIMESTAMP | NULL | Session end time |
| ⚠️ `total_alerts` | INT | DEFAULT 0 | Total alerts in session |
| 👁️ `avg_ear` | DECIMAL(5,3) | NULL | Average Eye Aspect Ratio |
| 👄 `avg_mar` | DECIMAL(5,3) | NULL | Average Mouth Aspect Ratio |
| 📐 `avg_tilt` | DECIMAL(5,2) | NULL | Average head tilt angle |
| ✅ `session_status` | ENUM | DEFAULT 'ACTIVE' | ACTIVE/COMPLETED/TERMINATED |
| 🌍 `location_start` | VARCHAR(100) | NULL | Starting location |
| 🌍 `location_end` | VARCHAR(100) | NULL | Ending location |
| 🚗 `vehicle_info` | VARCHAR(100) | NULL | Vehicle details |

---

#### 🚨 **DROWSINESS_EVENTS Entity**
**Purpose**: Log individual drowsiness detection incidents
**Shape**: Rectangle (Primary Entity)

| Attribute | Type | Constraint | Description |
|-----------|------|------------|-------------|
| 🔑 `event_id` | INT | PRIMARY KEY, AUTO_INCREMENT | Unique event ID |
| 🔗 `session_id` | INT | FOREIGN KEY | References DETECTION_SESSIONS.session_id |
| ⏰ `timestamp` | TIMESTAMP | NOT NULL | Event occurrence time |
| 🎯 `detection_type` | ENUM | NOT NULL | EYES_CLOSED/YAWNING/HEAD_TILT |
| 👁️ `ear_value` | DECIMAL(5,3) | NULL | EAR at detection |
| 👄 `mar_value` | DECIMAL(5,3) | NULL | MAR at detection |
| 📐 `tilt_value` | DECIMAL(5,2) | NULL | Tilt angle at detection |
| 🚨 `severity` | ENUM | NOT NULL | LOW/MEDIUM/HIGH/CRITICAL |
| 🔔 `alarm_triggered` | BOOLEAN | DEFAULT FALSE | Alarm activation status |
| ⏱️ `duration` | DECIMAL(4,2) | NULL | Event duration in seconds |
| 📷 `frame_data` | TEXT | NULL | Base64 encoded frame |
| 🎵 `sound_file` | VARCHAR(255) | NULL | Alert sound file path |

---

#### ⚙️ **USER_SETTINGS Entity**
**Purpose**: Store personalized detection parameters
**Shape**: Rectangle (Primary Entity)

| Attribute | Type | Constraint | Description |
|-----------|------|------------|-------------|
| 🔑 `setting_id` | INT | PRIMARY KEY, AUTO_INCREMENT | Unique setting ID |
| 🔗 `username` | VARCHAR(50) | FOREIGN KEY | References USERS.username |
| 👁️ `ear_threshold` | DECIMAL(5,3) | DEFAULT 0.250 | Eye closure threshold |
| 👄 `mar_threshold` | DECIMAL(5,3) | DEFAULT 0.700 | Yawn detection threshold |
| 📐 `tilt_threshold` | DECIMAL(5,2) | DEFAULT 20.00 | Head tilt threshold (degrees) |
| 🎞️ `frames_threshold` | INT | DEFAULT 15 | Consecutive frames for alert |
| ⏲️ `cooldown_period` | INT | DEFAULT 5 | Alert cooldown (seconds) |
| 👁️ `detect_eyes` | BOOLEAN | DEFAULT TRUE | Enable eye detection |
| 👄 `detect_yawn` | BOOLEAN | DEFAULT TRUE | Enable yawn detection |
| 📐 `detect_tilt` | BOOLEAN | DEFAULT TRUE | Enable tilt detection |
| 🔊 `sound_enabled` | BOOLEAN | DEFAULT TRUE | Audio alerts enabled |
| 📱 `vibration_enabled` | BOOLEAN | DEFAULT FALSE | Vibration alerts |
| 🌙 `night_mode` | BOOLEAN | DEFAULT FALSE | Night mode settings |
| 📅 `last_updated` | TIMESTAMP | DEFAULT NOW() | Settings modification time |
| ✅ `is_active` | BOOLEAN | DEFAULT TRUE | Current active settings |

---

#### 📹 **CAMERA_CALIBRATION Entity**
**Purpose**: Manage camera configurations and baseline measurements
**Shape**: Rectangle (Primary Entity)

| Attribute | Type | Constraint | Description |
|-----------|------|------------|-------------|
| 🔑 `calibration_id` | INT | PRIMARY KEY, AUTO_INCREMENT | Unique calibration ID |
| 🔗 `username` | VARCHAR(50) | FOREIGN KEY | References USERS.username |
| 📹 `camera_index` | INT | NOT NULL | Camera device ID |
| 🏷️ `camera_name` | VARCHAR(100) | NULL | Camera device name |
| 📏 `resolution_width` | INT | NOT NULL | Camera width resolution |
| 📏 `resolution_height` | INT | NOT NULL | Camera height resolution |
| 🎥 `fps` | INT | DEFAULT 30 | Frames per second |
| 👁️ `baseline_ear` | DECIMAL(5,3) | NULL | User's normal EAR |
| 👄 `baseline_mar` | DECIMAL(5,3) | NULL | User's normal MAR |
| 📐 `baseline_tilt` | DECIMAL(5,2) | NULL | User's normal head position |
| 📅 `calibration_date` | TIMESTAMP | NOT NULL | Calibration performed |
| 🎯 `accuracy_score` | DECIMAL(5,3) | NULL | Calibration accuracy |
| ✅ `is_active` | BOOLEAN | DEFAULT FALSE | Currently used camera |
| 🔧 `calibration_notes` | TEXT | NULL | Additional calibration info |

---

## 💎 **Relationship Diamonds**

### 🔗 **Primary Relationships**

#### 1️⃣ **USERS ↔ DETECTION_SESSIONS**
```
💎 Relationship: "manages_sessions"
📊 Cardinality: 1:N (One-to-Many)
🔗 Connection: USERS.username → DETECTION_SESSIONS.username
📝 Description: Each user can have multiple detection sessions
```

#### 2️⃣ **USERS ↔ USER_SETTINGS**
```
💎 Relationship: "configures_preferences"
📊 Cardinality: 1:N (One-to-Many)
🔗 Connection: USERS.username → USER_SETTINGS.username
📝 Description: Each user can have multiple setting configurations
```

#### 3️⃣ **USERS ↔ CAMERA_CALIBRATION**
```
💎 Relationship: "calibrates_devices"
📊 Cardinality: 1:N (One-to-Many)
🔗 Connection: USERS.username → CAMERA_CALIBRATION.username
📝 Description: Each user can calibrate multiple cameras
```

#### 4️⃣ **DETECTION_SESSIONS ↔ DROWSINESS_EVENTS**
```
💎 Relationship: "contains_events"
📊 Cardinality: 1:N (One-to-Many)
🔗 Connection: DETECTION_SESSIONS.session_id → DROWSINESS_EVENTS.session_id
📝 Description: Each session can contain multiple drowsiness events
```

---

## 🎭 **Actor Use Cases**

### 👤 **Driver (Primary Actor)**
- **UC-001**: Register Account
- **UC-002**: Login to System
- **UC-003**: Configure Detection Settings
- **UC-004**: Calibrate Camera
- **UC-005**: Start Monitoring Session
- **UC-006**: View Real-time Alerts
- **UC-007**: End Monitoring Session
- **UC-008**: Review Session History

### 🤖 **System (Secondary Actor)**
- **UC-009**: Detect Eye Closure
- **UC-010**: Detect Yawning
- **UC-011**: Detect Head Tilt
- **UC-012**: Trigger Alerts
- **UC-013**: Log Detection Events
- **UC-014**: Calculate Session Statistics

### 👨‍💼 **Administrator (Supporting Actor)**
- **UC-015**: Monitor System Performance
- **UC-016**: Manage User Accounts
- **UC-017**: Generate Reports
- **UC-018**: Update System Settings

---

## 🔄 **Data Flow Scenarios**

### 📊 **Scenario 1: New User Registration**
1. 👤 User provides personal information
2. 💾 System creates record in **USERS** entity
3. 🔧 System generates default **USER_SETTINGS**
4. ✅ User account is ready for use

### 📊 **Scenario 2: Detection Session**
1. 👤 User starts monitoring session
2. 📊 System creates **DETECTION_SESSIONS** record
3. 📹 System uses **CAMERA_CALIBRATION** settings
4. 🚨 System logs events in **DROWSINESS_EVENTS**
5. 📈 System updates session statistics
6. ✅ Session ends with summary data

### 📊 **Scenario 3: Alert Triggering**
1. 👁️ System detects drowsiness pattern
2. ⚙️ System checks **USER_SETTINGS** thresholds
3. 🚨 System creates **DROWSINESS_EVENTS** record
4. 🔔 System triggers appropriate alert
5. ⏲️ System applies cooldown period

---

## 🎨 **Entity Attribute Categories**

### 🔑 **Key Attributes (Yellow Ellipses)**
- Primary Keys: Unique identifiers
- Foreign Keys: Relationship connectors

### 🔵 **Descriptive Attributes (Blue Ellipses)**
- Simple attributes with single values
- Basic data fields

### 🟢 **Derived Attributes (Green Ellipses)**
- Calculated from other attributes
- Statistics and aggregations

### 🔴 **Composite Attributes (Red Ellipses)**
- Complex attributes with sub-parts
- Address, name components

### 🟡 **Multi-valued Attributes (Orange Ellipses)**
- Attributes that can have multiple values
- Phone numbers, email addresses

---

## 📈 **System Metrics & KPIs**

### 🎯 **Detection Accuracy Metrics**
- Eye closure detection rate
- Yawn detection accuracy
- Head tilt sensitivity
- False positive rate

### 📊 **User Engagement Metrics**
- Average session duration
- Daily active users
- Alert response time
- Settings customization rate

### 🔧 **System Performance Metrics**
- Processing frame rate
- Alert latency
- Camera calibration success rate
- Database query performance

---

## 🛡️ **Data Integrity Constraints**

### 🔒 **Primary Key Constraints**
- All entities have unique primary keys
- Auto-increment for system-generated IDs

### 🔗 **Foreign Key Constraints**
- Referential integrity maintained
- Cascade delete for related records

### ✅ **Check Constraints**
- Threshold values within valid ranges
- Enumeration values validation
- Date/time logical consistency

### 🚫 **Unique Constraints**
- Email addresses must be unique
- License numbers must be unique
- Active camera per user

---

*This ER diagram provides a comprehensive view of the drowsiness detection system's database schema, presented in a use case diagram style with detailed entity specifications, relationship mappings, and system interaction scenarios.*
