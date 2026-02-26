# IoT-Based Smart Waste Collection and Monitoring System

A comprehensive IoT-enabled Smart Waste Collection and Monitoring System for Cantilan urban areas that uses ESP32-based smart bins, ultrasonic sensors, and GSM communication to monitor waste bin fill levels, manage collection operations, and ensure reliable communication through two dedicated dashboards: Admin and Collector.

## 📋 Table of Contents

- [Features](#features)
- [System Architecture](#system-architecture)
- [Technology Stack](#technology-stack)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [API Documentation](#api-documentation)
- [Hardware Setup](#hardware-setup)
- [Contributing](#contributing)
- [License](#license)

## ✨ Features

### Admin Dashboard
- Real-time monitoring of all smart bins (fill-level percentage and online/offline status)
- View bin locations and capacity status
- Receive GSM-triggered alerts when bins reach near-full or full thresholds
- Create, assign, update, and track waste collection tasks
- Monitor collector activity and task progress
- Manage user accounts and roles (Admin and Collector)
- View GSM message logs from bins and collectors
- Generate collection reports and performance analytics
- Configure system thresholds and device settings

### Collector Dashboard
- View assigned waste collection tasks
- Receive task notifications via GSM/SMS and in-app alerts
- View bin locations, task priority, and instructions
- Start and complete collection tasks
- Send task updates and issue reports via GSM when internet is unavailable
- View completed task history and personal performance summary
- Communicate with the Admin through GSM-based or in-app messaging

### GSM Messaging & Communication
- Two-way GSM-based communication between smart bins, collectors, and administrators
- Smart bins send GSM data or SMS alerts when:
  - Bin reaches near-full or full capacity
  - Device goes offline or fails to send data
- Collectors send GSM messages to:
  - Confirm task start and completion
  - Report bin-related issues or access problems
- All GSM messages are received and displayed in the Admin Dashboard
- Messages are logged, timestamped, and linked to related bins or tasks

## 🏗️ System Architecture

```
┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│   Smart Bins    │         │   Backend API   │         │   MongoDB DB    │
│   (ESP32)       │◄────────┤   (Node.js)     │◄────────┤                 │
│                 │         │                 │         │                 │
│ - Ultrasonic    │         │ - REST API      │         │ - Users         │
│   Sensor        │         │ - Socket.io     │         │ - Bins          │
│ - GSM Module    │         │ - JWT Auth      │         │ - Tasks         │
│ - WiFi          │         │ - GSM Service   │         │ - Messages      │
└─────────────────┘         └────────┬────────┘         └─────────────────┘
                                    │
                    ┌───────────────┼───────────────┐
                    │               │               │
            ┌───────▼──────┐ ┌────▼─────┐ ┌────▼─────┐
            │ Admin        │ │ Collector│ │  GSM     │
            │ Dashboard    │ │ Dashboard│ │ Network  │
            └──────────────┘ └──────────┘ └──────────┘
```

## 🛠 Technology Stack

### IoT Hardware
- **ESP32** – Microcontroller
- **Ultrasonic Sensor (HC-SR04)** – Waste fill-level detection
- **GSM Module (SIM800L)** – Data and SMS communication

### Backend
- **Node.js** – Server runtime
- **Express.js** – REST API framework
- **MongoDB** – Database
- **Mongoose** – Object Data Modeling (ODM)
- **JWT (JSON Web Token)** – Authentication and authorization
- **Socket.io** – Real-time data updates and notifications

### Frontend
- **HTML5 / CSS3** – User interface structure and styling
- **Vanilla JavaScript** – Frontend logic
- **Chart.js** – Data visualization and analytics
- **Responsive Design** – Desktop and mobile support

## 📦 Prerequisites

### Software
- Node.js (v14 or higher)
- MongoDB (v4.4 or higher)
- PlatformIO (for ESP32 development)
- Git

### Hardware
- ESP32 development board
- HC-SR04 Ultrasonic Sensor
- SIM800L GSM Module
- Jumper wires and breadboard
- Power supply (5V for ESP32, 4V for GSM module)

## 🚀 Installation

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/smart-waste-monitoring.git
cd smart-waste-monitoring
```

### 2. Backend Setup

```bash
# Navigate to backend directory
cd backend

# Install dependencies
npm install

# Create .env file
cp .env.example .env
# Edit .env with your configuration

# Start MongoDB (if not running)
mongod

# Start the server
npm run dev
```

The backend server will run on `http://localhost:5000`

### 3. Frontend Setup

#### Admin Dashboard

```bash
# Navigate to admin dashboard directory
cd frontend/admin

# Serve the files (using any static file server)
# Example using Python:
python -m http.server 3000

# Or using Node.js http-server:
npx http-server -p 3000
```

The Admin Dashboard will be available at `http://localhost:3000`

#### Collector Dashboard

```bash
# Navigate to collector dashboard directory
cd frontend/collector

# Serve the files
python -m http.server 3001

# Or using Node.js http-server:
npx http-server -p 3001
```

The Collector Dashboard will be available at `http://localhost:3001`

### 4. ESP32 Firmware Setup

```bash
# Install PlatformIO (if not installed)
pip install platformio

# Navigate to firmware directory
cd esp32-firmware

# Build the firmware
pio run

# Upload to ESP32
pio run --target upload

# Monitor serial output
pio device monitor
```

## ⚙️ Configuration

### Backend Configuration (.env)

```env
# Server Configuration
PORT=5000
NODE_ENV=development

# MongoDB Configuration
MONGODB_URI=mongodb://localhost:27017/smart-waste-monitoring

# JWT Configuration
JWT_SECRET=your-super-secret-jwt-key-change-this-in-production
JWT_EXPIRE=7d

# GSM Configuration
GSM_API_URL=http://localhost:3001/api/sms
GSM_API_KEY=your-gsm-api-key

# System Thresholds
NEAR_FULL_THRESHOLD=75
FULL_THRESHOLD=90

# CORS Configuration
CLIENT_URL=http://localhost:3000
```

### ESP32 Configuration (include/config.h)

```cpp
// WiFi Configuration
#define WIFI_SSID "Your_WiFi_SSID"
#define WIFI_PASSWORD "Your_WiFi_Password"

// Server Configuration
#define SERVER_HOST "your-server-domain.com"
#define SERVER_PORT 5000

// Bin Configuration
#define BIN_ID "BIN-001"
#define DEVICE_ID "ESP32-DEVICE-001"

// Ultrasonic Sensor Configuration
#define TRIGGER_PIN 5
#define ECHO_PIN 18
#define MAX_DISTANCE 200  // Maximum distance in cm
#define BIN_HEIGHT 100    // Bin height in cm

// GSM Module Configuration
#define GSM_TX_PIN 16
#define GSM_RX_PIN 17
#define GSM_BAUD_RATE 9600

// Update Interval (milliseconds)
#define UPDATE_INTERVAL 30000  // 30 seconds

// Alert Thresholds
#define NEAR_FULL_THRESHOLD 75  // 75%
#define FULL_THRESHOLD 90       // 90%
```

## 📖 Usage

### Initial Setup

1. **Start MongoDB**
   ```bash
   mongod
   ```

2. **Start Backend Server**
   ```bash
   cd backend
   npm run dev
   ```

3. **Serve Admin Dashboard**
   ```bash
   cd frontend/admin
   python -m http.server 3000
   ```

4. **Serve Collector Dashboard**
   ```bash
   cd frontend/collector
   python -m http.server 3001
   ```

### Creating Initial Admin User

1. Access the Admin Dashboard at `http://localhost:3000`
2. Use the API to create an admin user:
   ```bash
   curl -X POST http://localhost:5000/api/auth/register \
     -H "Content-Type: application/json" \
     -d '{
       "name": "Admin User",
       "email": "admin@example.com",
       "password": "admin123",
       "role": "admin",
       "phone": "+1234567890",
       "gsmNumber": "+1234567890"
     }'
   ```
# reset admin pass
# Running

cd backend && node reset-admin-password.js
Admin password reset to password123
Updated user: {
  _id: new ObjectId("697ffc41b80003b3b5bb87b5"),
  name: 'Admin User',
  email: 'admin@example.com',
  password: '$2a$10$WvhjVsrmQp2l.aBkXJ5iDuoLCwk2znBkRCgjTpE7eL.Ubn9eJwpmq',
  role: 'admin',
  phone: '+1234567890',
  gsmNumber: '+1234567890',
  isActive: true,
  tasksCompleted: 0,
  lastActive: 2026-02-02T14:22:42.184Z,
  createdAt: 2026-02-02T01:22:09.163Z,
  updatedAt: 2026-02-02T23:21:59.285Z,
  __v: 0
}


3. Login with the admin credentials

### Adding Smart Bins

1. Navigate to the "Smart Bins" section in the Admin Dashboard
2. Click "Add Bin"
3. Fill in the bin details:
   - Name
   - Address
   - Area
   - Latitude/Longitude
   - GSM Number
   - Device ID (must match ESP32 config)
4. Click "Add Bin"

### Creating Collection Tasks

1. Navigate to the "Tasks" section in the Admin Dashboard
2. Click "Create Task"
3. Select a bin and assign a collector
4. Set priority and instructions
5. Click "Create Task"
6. The collector will receive a notification via GSM and in-app

### Collector Workflow

1. Login to the Collector Dashboard
2. View assigned tasks in the "My Tasks" tab
3. Click "Start Task" when beginning collection
4. Click "Complete Task" when finished
5. Optionally add notes or report issues
6. View completed tasks in the "History" tab

## 📚 API Documentation

### Authentication

#### Register User
```http
POST /api/auth/register
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123",
  "role": "collector",
  "phone": "+1234567890",
  "gsmNumber": "+1234567890"
}
```

#### Login
```http
POST /api/auth/login
Content-Type: application/json

{
  "email": "john@example.com",
  "password": "password123"
}
```

### Bins

#### Get All Bins
```http
GET /api/bins
Authorization: Bearer <token>
```

#### Create Bin
```http
POST /api/bins
Authorization: Bearer <token>
Content-Type: application/json

{
  "name": "Market Bin",
  "location": {
    "address": "123 Main St",
    "area": "Downtown",
    "latitude": 9.1234,
    "longitude": 125.5678
  },
  "gsmNumber": "+1234567890",
  "deviceId": "ESP32-DEVICE-001"
}
```

#### Update Bin Level (from ESP32)
```http
POST /api/bins/:id/update-level
Content-Type: application/json

{
  "deviceId": "ESP32-DEVICE-001",
  "level": 75
}
```

### Tasks

#### Get All Tasks
```http
GET /api/tasks
Authorization: Bearer <token>
```

#### Create Task
```http
POST /api/tasks
Authorization: Bearer <token>
Content-Type: application/json

{
  "binId": "bin_id_here",
  "assignedTo": "collector_id_here",
  "priority": "high",
  "instructions": "Collect waste from the bin",
  "estimatedDuration": 30
}
```

#### Start Task
```http
POST /api/tasks/:id/start
Authorization: Bearer <token>
```

#### Complete Task
```http
POST /api/tasks/:id/complete
Authorization: Bearer <token>
Content-Type: application/json

{
  "notes": "Collection completed successfully",
  "issueReported": ""
}
```

### Messages

#### Get All Messages
```http
GET /api/messages
Authorization: Bearer <token>
```

#### Receive GSM Message
```http
POST /api/messages/gsm
Content-Type: application/json

{
  "senderGsm": "+1234567890",
  "content": "BIN|BIN-001|FULL - Fill level: 95%"
}
```

## 🔌 Hardware Setup

### ESP32 Wiring Diagram

```
ESP32                    HC-SR04 Ultrasonic Sensor
─────────────────────────────────────────────────
5V    ────────────────── VCC
GND   ────────────────── GND
GPIO5 ────────────────── TRIGGER
GPIO18────────────────── ECHO

ESP32                    SIM800L GSM Module
─────────────────────────────────────────────
5V    ────────────────── VCC (via 4V regulator)
GND   ────────────────── GND
GPIO16────────────────── TX
GPIO17────────────────── RX

ESP32                    LEDs
────────────────────────────────
GPIO2 ────────────────── Online LED (Green)
GPIO4 ────────────────── Alert LED (Red)
```

### Hardware Components

1. **ESP32 Development Board**
   - Main microcontroller
   - WiFi connectivity
   - GPIO pins for sensors and modules

2. **HC-SR04 Ultrasonic Sensor**
   - Measures distance to waste level
   - Connected to GPIO5 (Trigger) and GPIO18 (Echo)

3. **SIM800L GSM Module**
   - Sends SMS alerts
   - Connected to GPIO16 (TX) and GPIO17 (RX)
   - Requires 4V power supply

4. **LEDs**
   - Green LED (GPIO2): Online status indicator
   - Red LED (GPIO4): Alert indicator

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 👥 Authors

- **Your Name** - Initial work

## 🙏 Acknowledgments

- ESP32 Arduino Core
- ArduinoJson library
- Chart.js
- Socket.io
- Express.js
- MongoDB

## 📞 Support

For support, please email support@example.com or open an issue in the repository.

## 🗺️ Project Structure

```
smart-waste-monitoring/
├── backend/
│   ├── config/
│   │   └── database.js
│   ├── controllers/
│   │   ├── authController.js
│   │   ├── binController.js
│   │   ├── taskController.js
│   │   ├── messageController.js
│   │   └── userController.js
│   ├── middleware/
│   │   └── auth.js
│   ├── models/
│   │   ├── User.js
│   │   ├── Bin.js
│   │   ├── Task.js
│   │   └── Message.js
│   ├── routes/
│   │   ├── authRoutes.js
│   │   ├── binRoutes.js
│   │   ├── taskRoutes.js
│   │   ├── messageRoutes.js
│   │   └── userRoutes.js
│   ├── utils/
│   │   ├── generateId.js
│   │   └── gsmService.js
│   ├── .env
│   ├── package.json
│   └── server.js
├── frontend/
│   ├── admin/
│   │   ├── index.html
│   │   ├── styles.css
│   │   └── app.js
│   └── collector/
│       ├── index.html
│       ├── styles.css
│       └── app.js
├── esp32-firmware/
│   ├── include/
│   │   └── config.h
│   ├── src/
│   │   └── main.cpp
│   └── platformio.ini
├── docs/
├── config/
└── README.md
```
