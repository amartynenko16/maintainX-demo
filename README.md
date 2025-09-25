# MaintainX Integrations Assessment  
## 👤 Author

**Alex Martynenko**  

📍 Toronto, Canada  
📧 alex.martynenko1@gmail.com  
📱 +1-647-986-5062  
🔗 [LinkedIn](https://www.linkedin.com/in/alex-martynenko/)  

---

## 📂 Contents
- `/screenshots/` → Evidence of sandbox setup tasks  
- `/code/` → Node.js examples for Q1 (due dates) & Q2 (PO sync)  
- `/postman/` → Postman collection for sample API calls  
- `/docs/` → One-pager summary & slide deck  

---

## ✅ Mapping to Assessment Requirements

### Sandbox Setup
- **10-person team** → `screenshots/2025-09-25_demo_team.png`  
- **Assets with sub-assets** → `screenshots/2025-09-25_demo_assets.png`
- **Locations with sub-locations** → `screenshots/2025-09-25_demo_locations.png`  
- **Work orders** → `screenshots/2025-09-25_demo_work_orders.png`
- **Vendors & Parts (linked)** → `screenshots/2025-09-25_demo_work_vendors.png`, `screenshots/2025-09-25_demo_work_parts.png`  
- **Generated Purchase Orders** → `screenshots/2025-09-25_demo_work_purchase_orders.png`

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
Copy `.env.example` → `.env` and fill in values:
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
Import `/postman/maintainx_coupa_collection.json` and run sample requests.

---

## 📊 Notes & Assumptions

### Q1 — Auto-Set Due Date by Priority
- **Priority Mapping (example configuration):**
  - Critical → 4 hours  
  - High → 24 hours  
  - Medium → 3 days  
  - Low → 7 days  
- **Assumptions:**
  - Due dates are based on **calendar days** (not business hours).  
  - All times are stored in **UTC** for consistency; display is handled by MaintainX in the local timezone.  
  - The middleware only updates a work order’s due date if:  
    1. No due date exists, OR  
    2. The priority field changes, OR  
    3. The existing due date does not align with the mapping (ensures **idempotency**).  
  - Mapping values are configurable so clients can adjust rules without redeploying code.  

### Q2 — Sync Purchase Orders to Coupa
- **Integration Flow:**  
  MaintainX webhooks → Middleware (transform, validate, log) → Coupa Purchase Orders API.  
- **Authentication:**  
  - Default: OAuth2 Client Credentials (preferred for enterprise security).  
  - Alternate: API key, if required by customer policy.  
- **Entity Mapping (examples):**
  - MaintainX `po.number` → Coupa `purchase_order.number`  
  - MaintainX `vendor.external_id` → Coupa `supplier.id`  
  - MaintainX `part.sku` → Coupa `line_item.sku`  
  - Quantities, unit prices, and currency mapped directly.  
- **Error Handling & Data Quality:**  
  - If a vendor does not exist in Coupa, the PO is flagged for **manual reconciliation**.  
  - Sync retries use **exponential backoff**. Failures after threshold are logged to a **dead letter queue (DLQ)** for review.  
- **Operational Considerations:**  
  - Middleware maintains `{maintainx_po_id, coupa_po_id, status}` for idempotency and reconciliation.  
  - Monitoring includes **last successful sync time** and **failure counts** for operational visibility.  
  - Secrets (API keys, client credentials) stored in environment variables, never hardcoded.  
  - Designed with **rate limit awareness** — batching can be introduced if transaction volume is high.  

