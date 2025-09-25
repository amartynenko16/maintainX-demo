# MaintainX Integrations Assessment  
**Submission by Alex Martynenko**  

---

## 📂 Contents
- `/screenshots/` → Evidence of sandbox setup tasks  
- `/code/` → Node.js examples for Q1 (due dates) & Q2 (PO sync)  
- `/postman/` → Postman collection for sample API calls  
- `/docs/` → One-pager summary & slide deck  

---

## ✅ Mapping to Assessment Requirements

### Sandbox Setup
- **10-person team** → `screenshots/team.png`  
- **Assets with sub-assets** → `screenshots/assets_list.png`, `screenshots/asset_detail.png`  
- **Locations with sub-locations** → `screenshots/locations.png`  
- **Work orders (repeatable + non-repeatable)** → `screenshots/workorders_list.png`, `screenshots/workorder_detail.png`  
- **Vendors & Parts (linked)** → `screenshots/vendors.png`, `screenshots/parts.png`  
- **Generated Purchase Orders** → `screenshots/pos_list.png`, `screenshots/po_detail.png`  

---

### Question 1 — Auto-Set Due Date by Priority
- **Design Document** → `docs/q1_solution.pdf`  
- **Example Code** → `code/q1_due_date.js`  
- **Test Evidence (screenshot)** → `screenshots/q1_test_workorder.png`  

### Question 2 — Sync Purchase Orders to Coupa
- **Architecture Diagram** → `docs/q2_architecture.pdf`  
- **Entity Mapping (CSV)** → `docs/q2_mapping.csv`  
- **Example Code** → `code/q2_coupa_sync.js`  
- **Test Evidence** → `screenshots/q2_po_sync.png`  

---

## 🛠 How to Run the Code

1. **Clone or unzip the folder.**  
   ```bash
   git clone <repo_url>
   cd maintainx-assessment
2. **Set up environment variables.** 
Copy .env.example → .env and fill in values:
   ```bash
   MAINTAINX_TOKEN=your_maintainx_api_token
   COUPA_CLIENT_ID=your_coupa_client_id
   COUPA_CLIENT_SECRET=your_coupa_client_secret
3. **Install dependencies.** 
   ```bash
   npm install
4. **Run the server (for webhook handling).** 
   ```bash
   npm start
5. **Test flows with Postman.** 
Import /postman/maintainx_coupa_collection.json and run sample requests.
