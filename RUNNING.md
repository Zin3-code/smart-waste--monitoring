# Running the Smart Waste Monitoring System

This guide shows how to run the local development stack (backend API + dashboards) and optionally the ESP32 firmware.

## Prerequisites
- Node.js (v14+)
- MongoDB (v4.4+)
- Python 3 (for serving the static dashboards) or `npx http-server`
- Optional: PlatformIO (for ESP32 firmware)

## 1) Start MongoDB

Windows (first-time only, create the data directory):
```powershell
mkdir C:\data\db
mongod
```

Linux/Mac:
```bash
mongod
```

Keep MongoDB running in this terminal.

## 2) Start the backend API
```bash
cd backend
npm install
npm run dev
```

The backend listens on `http://localhost:5000`.

If you need to change ports or database settings, edit `backend/.env`.

## 3) Serve the Admin dashboard
```powershell
cd C:\Users\user\Documents\monitoring\frontend\admin
python -m http.server 3000
```

Admin dashboard: `http://localhost:3000`

## 4) Serve the Collector dashboard
Open a **new terminal** so both dashboards can run at the same time, then:
```powershell
cd C:\Users\user\Documents\monitoring\frontend\collector
python -m http.server 3001
```

Collector dashboard: `http://localhost:3001`

Note: If both `http://localhost:3000/` and `http://localhost:3001/` show the **admin** site, you likely started the second server from the wrong folder. Make sure the collector server is started from `frontend\collector` (not `frontend\admin`).

## 5) First-time setup (users, bins, tasks)

Create the initial admin user:
```bash
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Admin User",
    "email": "admin@example.com",
    "password": "admin123",
    "role": "admin",
    "phone": "+639261013245",
    "gsmNumber": "+639261013245"
  }'
```

Then in the Admin dashboard:
- Create a Collector user.
- Add at least one Smart Bin.
- Create a Task and assign it to the Collector.

Finally, log in to the Collector dashboard with the collector account.

## 6) Optional: ESP32 firmware
1. Update `esp32-firmware/include/config.h` with WiFi credentials, backend host, `BIN_ID`, and `DEVICE_ID`.
2. Build and upload:
```bash
cd esp32-firmware
pio run --target upload
```
3. Monitor serial output:
```bash
pio device monitor
```

## Notes and troubleshooting
- If port 5000 is already in use, change `PORT` in `backend/.env` and update the `API_BASE_URL` in `frontend/admin/app.js` and `frontend/collector/app.js`.
- Socket.io origin is limited by `CLIENT_URL` in `backend/.env`. If realtime updates do not connect from a dashboard, set `CLIENT_URL` to that dashboard URL or update the server to allow both origins.
- If you see MongoDB connection errors, make sure `mongod` is running and `MONGODB_URI` is correct.
