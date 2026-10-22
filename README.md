# TableBooker Agentic AI & CulinaryAudit Platform

A unified, production-ready Agentic AI Restaurant Booking Platform combining the luxury UI/UX of `hotel-ai-main` with the deterministic 6-stage agent brain, multi-branch disambiguation, deposit gates, waitlist management, and SQLite ledger verification of `table-booker-agent`.

---

## 🚀 Key Features

1. **Autonomous 6-Stage Agentic Brain**:
   - **Understand**: Gemini 2.5 NLU with deterministic Regex fallback for extracting party size, time, date, area, and intent.
   - **Read**: Live inventory queries with multi-source provenance logging (URLs, timestamps, latencies, HTTP status).
   - **Decide**: Deterministic rule comparisons, capacity evaluation, and deposit threshold calculations.
   - **Disambiguate**: Prompts human with interactive branch cards when multiple locations match the same name.
   - **Approve**: Human-in-the-loop approval gate for deposits and reservations with side-by-side diffs.
   - **Act**: Idempotent safe reservation execution using stable SHA-256 tokens (`sha256(req_id|rest_id|time|party_size)`).
   - **Verify**: Cryptographic read-back verification against the merchant ledger (`GET /mock/confirmation/:id`).
   - **Dispatch**: Live Kitchen Order Ticket (KOT) generation for pre-ordered dishes.

2. **The 4 Canonical Test Cases**:
   - **TC1 (Booked & Verified)**: *"Dinner for 6 at 8 pm Saturday near Banjara Hills at Spice Garden"* &rarr; Table is free, Rs 0 deposit &rarr; Reserved & independently verified read-back (`RES-XXXXXX`).
   - **TC2 (Waitlist Handling)**: *"Dinner for 6 at 8 pm Saturday near Madhapur at Madhapur Masala"* &rarr; Slot is waitlist &rarr; Status `waitlisted`, never marked as booked.
   - **TC3 (Deposit Required)**: *"Dinner for 6 at 8 pm Saturday near Banjara Hills at Royal Biryani House"* &rarr; Requires Rs 500/head deposit (Rs 3,000 total for 6 guests) &rarr; Status `needs_approval`, refuses to reserve until human authorizes.
   - **TC4 (Two Hotels / Branches with Same Name)**: *"Dinner for 6 at 8 pm Saturday near Banjara Hills at Paradise Bites"* &rarr; Matches Road No. 1 and Road No. 12 &rarr; Status `needs_branch`, prompts user with clickable branch selector cards.

3. **Pluggable Real API Integration**:
   - Built with `IRestaurantAPIAdapter` in `src/services/apiAdapter.ts`.
   - Switch between **Local Mock Agent Engine** and your **External Restaurant API** with 1 click in the Settings modal or via environment variables (`API_MODE=custom`, `CUSTOM_API_URL=https://...`, `CUSTOM_API_KEY=...`).

4. **Live SQLite Database Explorer**:
   - Interactive UI tab viewing all relational tables in real time: `restaurants`, `slots`, `requests`, `sources`, `decisions`, `bookings`, `mock_bookings`, `kitchen_tickets`.

---

## 🛠️ Quick Start

### 1. Start Development Server
```bash
cd "C:\Users\hari2\.gemini\antigravity\scratch\hotel-ai-agentic-booking"
npm run dev
```
Open [http://localhost:3000](http://localhost:3000) in your browser.

### 2. Run Test Cases
- From the UI: Click the **4 Test Cases** tab and click **Run All 4 Test Cases**.
- From the terminal:
```bash
npx tsx -e "import { runAllTests } from './src/services/agentEngine'; runAllTests().then(r => console.log(JSON.stringify(r, null, 2)))"
```

### 3. API Endpoints
- `POST /api/agent/request`: `{ "text": "Book a table for 6 at 8pm at Spice Garden" }`
- `POST /api/agent/choose-branch`: `{ "request_id": "REQ-...", "restaurant_id": "R4" }`
- `POST /api/agent/approve`: `{ "request_id": "REQ-..." }`
- `POST /api/agent/reject`: `{ "request_id": "REQ-..." }`
- `POST /api/agent/tests/run-all`: Runs all 4 test cases
- `GET /api/database/tables`: Dumps all 8 relational tables for inspection
"# finalproject" 
