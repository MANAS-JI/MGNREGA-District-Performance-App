# MGNREGA-District-Performance-App
A web application to visualize MGNREGA district performance data for rural citizens.

# Our Voice, Our Rights – MGNREGA District Performance App
(Empowering rural citizens to understand district-level MGNREGA performance easily)

📌 Overview
Our Voice, Our Rights is a web application that presents district-level performance data of the Mahatma Gandhi National Rural Employment Guarantee Act (MGNREGA) program in a simple, visual, and local-language format.

The app allows any citizen to:

View monthly and historical MGNREGA performance for their district.

Compare with nearby districts and state averages.

Access data in Hindi and English with audio narration for low-literacy users.

Auto-detect their district via GPS or manually select it.

The project focuses on making open data accessible, inclusive, and meaningful to rural communities.

🏗️ System Architecture
java
Copy code
Frontend (Next.js / React) 
        │
        ▼
Backend API (Node.js / Express)
        │
        ▼
PostgreSQL + PostGIS (District Data + Geo Mapping)
        │
        ▼
Redis Cache (Fast access + offline fallback)
        │
        ▼
ETL Worker (Data.gov.in API Fetcher)
        │
        ▼
S3 / DigitalOcean Spaces (Daily snapshots)
Hosting:

VPS (DigitalOcean / AWS EC2 / Hetzner)

Nginx reverse proxy

PM2 / systemd for Node.js process management

HTTPS via Let’s Encrypt

🌍 Tech Stack
Layer	Technology
Frontend	Next.js + Tailwind CSS + Chart.js / Recharts
Backend	Node.js + Express
Database	PostgreSQL + PostGIS
Cache	Redis
Storage	AWS S3 / DigitalOcean Spaces
Background Jobs	BullMQ + Cron
Maps	Leaflet + GeoJSON
Hosting	Ubuntu VPS (Nginx + PM2)
Monitoring	Prometheus + Grafana (optional)

⚙️ Features
✅ Select or auto-detect district (via geolocation)
✅ View key metrics: Payments, Workdays, Pending Payments, Female Participation
✅ Compare district with state average and best district
✅ Audio narration for all data
✅ Multi-language (English + Hindi)
✅ Offline & low-data mode
✅ PDF report sharing via WhatsApp
✅ Daily data refresh via ETL worker
✅ Caching to handle API downtime

🧩 Data Source
Source: MGNREGA Monthly Performance API – data.gov.in

Frequency: Updated daily via Cron job

Fallback: Cached snapshots stored in S3

🗄️ Database Schema
Table: districts
Column	Type	Description
id	SERIAL PRIMARY KEY	Unique district ID
name	TEXT	District name
state	TEXT	State name
geom	GEOMETRY	District boundary (GeoJSON polygon)

Table: mgnrega_monthly
Column	Type	Description
id	SERIAL PRIMARY KEY	Entry ID
district_id	INT (FK)	Link to districts table
year	INT	Year
month	INT	Month
payments_total_rupees	BIGINT	Total payments (₹)
workdays	BIGINT	Total workdays generated
pending_payments	INT	Pending payments count
female_participation_percent	FLOAT	Female workers %
updated_at	TIMESTAMP	Last update time

Table: snapshots
Column	Type	Description
id	SERIAL PRIMARY KEY	Snapshot ID
fetched_at	TIMESTAMP	API fetch timestamp
s3_key	TEXT	Stored snapshot file path
status	TEXT	success / failed

🧠 API Endpoints
Endpoint	Description
/api/districts	List all districts
/api/district/:id/monthly	Monthly performance data
/api/state/:state/compare	Compare district vs state
/api/geo/locate?lat=&lon=	Identify district from coordinates
/api/cache/status	Redis cache health

Example:

bash
Copy code
GET /api/district/101/monthly?year=2025
🔄 ETL Worker (Data Fetch & Cache)
File: worker/fetchData.js

js
Copy code
import fetch from 'node-fetch';
import { saveSnapshot, upsertData } from './db.js';

const API_URL = "https://api.data.gov.in/resource/<mgnrega_id>?api-key=<your_api_key>&format=json";

async function fetchData() {
  try {
    const res = await fetch(API_URL);
    const data = await res.json();
    await saveSnapshot(data);
    await upsertData(data);
    console.log("✅ Data updated successfully");
  } catch (err) {
    console.error("⚠️ Fetch failed, using previous snapshot", err);
  }
}

fetchData();
Cron Job (every 6 hours):

bash
Copy code
crontab -e
0 */6 * * * /usr/bin/node /var/www/app/worker/fetchData.js >> /var/log/mgnrega.log 2>&1
🧭 Auto-Detect District (Geo Mapping)
sql
Copy code
SELECT id, name FROM districts
WHERE ST_Contains(geom, ST_SetSRID(ST_Point(:lon, :lat), 4326));
Frontend (browser):

js
Copy code
navigator.geolocation.getCurrentPosition((pos) => {
  fetch(`/api/geo/locate?lat=${pos.coords.latitude}&lon=${pos.coords.longitude}`)
});
🛠️ Installation & Setup
Step 1: Clone Repo
bash
Copy code
git clone https://github.com/<your-username>/our-voice-our-rights.git
cd our-voice-our-rights
Step 2: Setup Environment Variables (.env)
ini
Copy code
DATABASE_URL=postgres://user:pass@localhost:5432/mgnrega
REDIS_URL=redis://localhost:6379
S3_ACCESS_KEY=xxxx
S3_SECRET_KEY=xxxx
S3_BUCKET=mgnrega-snapshots
API_KEY_DATA_GOV=xxxx
Step 3: Setup PostgreSQL + PostGIS
bash
Copy code
sudo apt install postgresql postgis
sudo -u postgres psql
CREATE DATABASE mgnrega;
\c mgnrega;
CREATE EXTENSION postgis;
Step 4: Import District GeoJSON
bash
Copy code
ogr2ogr -f "PostgreSQL" PG:"dbname=mgnrega user=postgres password=yourpass" \
"data/UP_districts.geojson" -nln districts -nlt MULTIPOLYGON
Step 5: Install Node.js + Redis
bash
Copy code
sudo apt install nodejs npm redis-server
npm install
Step 6: Run Backend
bash
Copy code
npm run dev
Step 7: Deploy with Nginx + PM2
bash
Copy code
sudo npm install -g pm2
pm2 start server.js --name mgnrega
sudo apt install nginx
sudo nano /etc/nginx/sites-available/mgnrega
Nginx Config:

nginx
Copy code
server {
  listen 80;
  server_name _;
  location / {
    proxy_pass http://localhost:3000;
  }
}
Enable site:

bash
Copy code
sudo ln -s /etc/nginx/sites-available/mgnrega /etc/nginx/sites-enabled/
sudo systemctl restart nginx
🧑‍💻 Deployment Checklist
✅ Node.js backend deployed with PM2
✅ Next.js frontend built and served
✅ Nginx reverse proxy configured
✅ PostgreSQL + Redis running
✅ Cron job fetching latest data
✅ HTTPS enabled via Let’s Encrypt
✅ Logs & metrics monitored

🗣️ Accessibility Features
Audio narration (TTS) for every metric

Color-coded indicators

Large text + high contrast UI

Hindi / English toggle

Offline cached mode

Simplified explanations (“What does this mean?” tooltip)

🚀 Performance & Resilience
Cached summaries for 12 hours

Fallback to last successful snapshot if API fails

Pre-rendered pages via Next.js Incremental Static Regeneration (ISR)

Lazy loading of graphs and map tiles

🧩 Bonus Feature
If location permission is granted, the app automatically detects the user’s district using GeoJSON boundaries and shows localized MGNREGA data instantly.

📸 Submission Requirements
Loom Video (<2 min)

Show the website

District selection

Code walkthrough

Database & ETL snapshot demo

Hosted App URL

Example: http://<your-vps-ip>/

Public GitHub Repo

Include this README.md

🏁 Example Output Snapshot
Dashboard:

Payments Made (₹): 1,25,00,000

Workdays Generated: 3,45,000

Pending Payments: 2,145

Female Participation: 48%

Color-coded trend lines and audio readout available.

🙌 Acknowledgements
Government of India – Open Government Data Platform (data.gov.in) for the API.

MGNREGA Program for enabling transparency through open data.

Built for the Public Tech Challenge 2025.
