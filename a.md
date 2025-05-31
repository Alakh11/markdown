# Entity-Relationship Diagram for Drowsiness Detection System

## Database Schema ER Diagram

```mermaid
erDiagram
    USERS {
        string username PK "🔑 Primary Key"
        string password "🔒 Required"
        string first_name "👤 Personal Info"
        string last_name "👤 Personal Info"
        string email "📧 Contact Info"
        string phone "📞 Contact Info"
        string gender "👥 Demographics"
        string birth_date "📅 Demographics"
        string address "🏠 Location Info"
        string license_number "🚗 Driving Credential"
        string driving_experience "⏱️ Experience Level"
    }

    DETECTION_SESSIONS {
        int session_id PK "🔑 Auto-increment ID"
        string username FK "🔗 Foreign Key to Users"
        datetime start_time "⏰ Session Start"
        datetime end_time "⏰ Session End"
        int total_alerts "⚠️ Alert Count"
        float avg_ear "👁️ Average Eye Aspect Ratio"
        float avg_mar "👄 Average Mouth Aspect Ratio"
        float avg_tilt "📐 Average Head Tilt"
        string session_status "✅ Active/Completed/Terminated"
    }

    DROWSINESS_EVENTS {
        int event_id PK "🔑 Auto-increment ID"
        int session_id FK "🔗 Foreign Key to Sessions"
        datetime timestamp "⏰ Event Time"
        string detection_type "🎯 Eyes/Yawn/Tilt"
        float ear_value "👁️ EAR at detection"
        float mar_value "👄 MAR at detection"
        float tilt_value "📐 Tilt angle at detection"
        string severity "🚨 Low/Medium/High"
        boolean alarm_triggered "🔔 Alarm Status"
    }

    USER_SETTINGS {
        int setting_id PK "🔑 Auto-increment ID"
        string username FK "🔗 Foreign Key to Users"
        float ear_threshold "👁️ Eye Detection Threshold"
        float mar_threshold "👄 Yawn Detection Threshold"
        float tilt_threshold "📐 Head Tilt Threshold"
        int frames_threshold "🎞️ Consecutive Frames"
        int cooldown_period "⏲️ Alert Cooldown (seconds)"
        boolean detect_eyes "👁️ Enable Eye Detection"
        boolean detect_yawn "👄 Enable Yawn Detection"
        boolean detect_tilt "📐 Enable Tilt Detection"
        boolean sound_enabled "🔊 Audio Alerts"
        datetime last_updated "📅 Settings Modified"
    }

    CAMERA_CALIBRATION {
        int calibration_id PK "🔑 Auto-increment ID"
        string username FK "🔗 Foreign Key to Users"
        int camera_index "📹 Camera Device ID"
        int resolution_width "📏 Camera Width"
        int resolution_height "📏 Camera Height"
        float baseline_ear "👁️ User's Normal EAR"
        float baseline_mar "👄 User's Normal MAR"
        datetime calibration_date "📅 Calibration Performed"
        boolean is_active "✅ Currently Used"
    }

    %% Entity Colors and Styling
    USERS {
        color: "#E3F2FD"
        border-color: "#2196F3"
    }
    
    DETECTION_SESSIONS {
        color: "#F3E5F5"
        border-color: "#9C27B0"
    }
    
    DROWSINESS_EVENTS {
        color: "#FFEBEE"
        border-color: "#F44336"
    }
    
    USER_SETTINGS {
        color: "#E8F5E8"
        border-color: "#4CAF50"
    }
    
    CAMERA_CALIBRATION {
        color: "#FFF3E0"
        border-color: "#FF9800"
    }

    %% Relationships
    USERS ||--o{ DETECTION_SESSIONS : "has_sessions"
    USERS ||--o{ USER_SETTINGS : "configures"
    USERS ||--o{ CAMERA_CALIBRATION : "calibrates"
    DETECTION_SESSIONS ||--o{ DROWSINESS_EVENTS : "contains_events"
```

## Relationship Descriptions

### 🔗 **USERS to DETECTION_SESSIONS** (One-to-Many)
- **Relationship**: `has_sessions`
- **Description**: Each user can have multiple detection sessions
- **Cardinality**: 1:N
- **Attributes**: 
  - Foreign Key: `username` in DETECTION_SESSIONS references `username` in USERS

### 🔗 **USERS to USER_SETTINGS** (One-to-Many)
- **Relationship**: `configures`
- **Description**: Each user can have multiple setting configurations (historical settings)
- **Cardinality**: 1:N
- **Attributes**: 
  - Foreign Key: `username` in USER_SETTINGS references `username` in USERS

### 🔗 **USERS to CAMERA_CALIBRATION** (One-to-Many)
- **Relationship**: `calibrates`
- **Description**: Each user can have multiple camera calibrations for different devices
- **Cardinality**: 1:N
- **Attributes**: 
  - Foreign Key: `username` in CAMERA_CALIBRATION references `username` in USERS

### 🔗 **DETECTION_SESSIONS to DROWSINESS_EVENTS** (One-to-Many)
- **Relationship**: `contains_events`
- **Description**: Each detection session can contain multiple drowsiness detection events
- **Cardinality**: 1:N
- **Attributes**: 
  - Foreign Key: `session_id` in DROWSINESS_EVENTS references `session_id` in DETECTION_SESSIONS

## Entity Descriptions

### 👤 **USERS** (Blue Theme)
**Primary Entity**: Stores user account and personal information
- **Primary Key**: `username` 🔑
- **Categories**:
  - **Authentication**: username, password
  - **Personal Info**: first_name, last_name, gender, birth_date, address
  - **Contact**: email, phone
  - **Driving**: license_number, driving_experience

### 📊 **DETECTION_SESSIONS** (Purple Theme)
**Session Entity**: Tracks individual monitoring sessions
- **Primary Key**: `session_id` 🔑
- **Purpose**: Record complete detection sessions with summary metrics
- **Metrics**: avg_ear, avg_mar, avg_tilt, total_alerts

### 🚨 **DROWSINESS_EVENTS** (Red Theme)
**Event Entity**: Records individual drowsiness detection incidents
- **Primary Key**: `event_id` 🔑
- **Purpose**: Log specific moments when drowsiness was detected
- **Detection Types**: Eyes closed, Yawning, Head tilt

### ⚙️ **USER_SETTINGS** (Green Theme)
**Configuration Entity**: Stores user-specific detection parameters
- **Primary Key**: `setting_id` 🔑
- **Purpose**: Personalized thresholds and preferences
- **Thresholds**: EAR, MAR, tilt angles, frame counts

### 📹 **CAMERA_CALIBRATION** (Orange Theme)
**Calibration Entity**: Manages camera setup and baseline measurements
- **Primary Key**: `calibration_id` 🔑
- **Purpose**: Store camera-specific settings and user baselines
- **Calibration**: Device settings, resolution, baseline measurements

## System Workflow

1. **👤 User Registration/Login** → USERS table
2. **⚙️ Settings Configuration** → USER_SETTINGS table
3. **📹 Camera Calibration** → CAMERA_CALIBRATION table
4. **📊 Start Detection Session** → DETECTION_SESSIONS table
5. **🚨 Drowsiness Detection** → DROWSINESS_EVENTS table
6. **📊 End Session** → Update DETECTION_SESSIONS with summary

## Key Features Supported

- 🔐 **User Authentication & Profiles**
- ⚙️ **Personalized Detection Settings**
- 📊 **Session-based Monitoring**
- 🚨 **Real-time Event Logging**
- 📹 **Multi-camera Support**
- 📈 **Historical Analytics**
- 🎛️ **Configurable Thresholds**

---

*This ER diagram represents the complete database schema for a comprehensive drowsiness detection system with user management, session tracking, and detailed event logging capabilities.*
