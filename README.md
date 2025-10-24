# 🧭 MGNREGA District Performance Visualization App

## 🌍 Overview
The **MGNREGA District Performance Visualization App** is a web platform designed to help citizens — especially from rural India — easily understand the performance of their district under the **Mahatma Gandhi National Rural Employment Guarantee Act (MGNREGA)** program.

While the Government of India provides this data via an API, many citizens lack the technical skills to interpret it. This project bridges that gap by visualizing the data in an easy-to-understand format.

---

## 🎯 Objectives
1. Make MGNREGA district performance data **accessible and understandable** to everyone.
2. Provide **interactive charts and summaries** for monthly and past performance.
3. Offer **comparative views** across districts.
4. Design for **low-literacy rural users** with simple UI/UX and icon-based visuals.
5. Ensure **system reliability** even when the government API is unavailable.

---

## 🏗️ System Architecture

### **Frontend**
- Built using **React.js** / **HTML-CSS-JS** for an interactive and responsive experience.
- Uses **simple visuals, icons, and local language support (Hindi/English)**.
- Provides **dropdown-based district selection** and **auto-detect location** feature (bonus).

### **Backend**
- Built using **Node.js (Express)** / **Python (Flask)** to handle API requests.
- Fetches data from the official [MGNREGA Open API](https://www.data.gov.in/catalog/mahatma-gandhi-national-rural-employment-guarantee-act-mgnrega).
- Implements **caching** to handle API downtime.
- Stores backup data in a local **PostgreSQL/MySQL database** for offline retrieval.

### **Database**
- Stores historical district-level MGNREGA data.
- Updated automatically via a scheduled job (e.g., CRON task) that syncs new data daily.

### **Hosting**
- Hosted on a **Virtual Machine (e.g., AWS EC2 / DigitalOcean VPS)** for production readiness.
- Backend served on **Nginx** or **Apache**.
- Database hosted securely on **PostgreSQL**.
- Frontend served via **static hosting** or integrated with backend routes.

---

## 🧩 Key Features
✅ Select any **district** and view real-time and historical MGNREGA data.  
✅ Visualize data using **charts and icons** for low-literacy understanding.  
✅ Compare performance across months and years.  
✅ **Offline fallback** if API is unavailable.  
✅ Optional **auto-detect location** to identify user's district automatically.  

---

## 🧠 Design for Low-Literacy Users
- Use of **colors, icons, and graphs** instead of long text.  
- Language toggle between **English and Hindi**.  
- Voice-based instructions (optional future enhancement).  
- Tooltips and short explanations for all metrics.

---

## ⚙️ Technical Stack

| Component | Technology Used |
|------------|----------------|
| Frontend | React.js / HTML / CSS / JS |
| Backend | Node.js (Express) / Flask |
| Database | PostgreSQL / MySQL |
| Hosting | AWS EC2 / DigitalOcean VPS |
| Charts | Chart.js / D3.js |
| API Source | data.gov.in MGNREGA API |

---

## 🧱 Project Setup

### **1. Clone the repository**
```bash
git clone https://github.com/<your-username>/MGNREGA-District-Performance-App.git
cd MGNREGA-District-Performance-App
```

### **2. Install dependencies**
```bash
npm install  # or pip install -r requirements.txt
```

### **3. Configure environment variables**
Create a `.env` file:
```
API_URL=https://api.data.gov.in/resource/<resource-id>
DB_USER=your_user
DB_PASS=your_password
DB_NAME=mgnrega_data
```
*(Do not commit your actual .env file)*

### **4. Run the backend**
```bash
npm start
# or
python app.py
```

### **5. Run the frontend**
```bash
npm run dev
```

### **6. Access the app**
Visit → `http://localhost:3000`

---

## 📊 Example Visuals
- **Bar chart:** Total households provided employment.
- **Line chart:** Trend of person-days generated over months.
- **Pie chart:** Gender ratio of beneficiaries.

---

## 🚀 Hosting
Deployed on VPS:  
👉 [Live Website](http://<your-server-ip>:<port>)  

Demo Video (Loom):  
🎥 [Watch Walkthrough](https://www.loom.com/share/<your-video-id>)

GitHub Repository:  
🔗 [MGNREGA-District-Performance-App](https://github.com/<your-username>/MGNREGA-District-Performance-App)

---

## 🧩 Future Enhancements
- Add multilingual voice assistant.  
- Include other welfare scheme comparisons.  
- Build Android app version.  

---

## 🧑‍💻 Author
**Manas Gupta**  
Cybersecurity & Technology Enthusiast  
📧 Email: your.email@example.com  
🌐 GitHub: [github.com/your-username](https://github.com/your-username)

---

## 🛡️ License
This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.
