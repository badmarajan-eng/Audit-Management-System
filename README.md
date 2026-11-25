<<<<<<< HEAD
# Audit-Management-System
=======
# Kaveri Heritage Foods Pvt. Ltd. — Order-to-Audit Manufacturing ERP

An end-to-end **Order-to-Audit Management System** built for a South Indian packaged food manufacturer producing **Murukku 200 g**, **Mixture 200 g**, **Chips 200 g**, and **Sweets 200 g** (NIT Tiruchirappalli Internship Project).

## Key Capabilities & Workflow

1. **Role-Based Access Control (5 Roles)**:
   - **Salesman**: Mobile-friendly `< 1 minute` order placement, assigned customer directory, order status tracking.
   - **Factory Admin**: Pre-approval order editing (quantities & unit prices), finished-goods stock shortage checks, approval/rejection, sequential FY GST invoicing, partial/full dispatch & sales returns, customer payment recording, and master catalog management.
   - **Production Supervisor**: Raw & packaging material management, purchase receipts, versioned Bills of Materials (BOM) with unit conversion, batch production runs, actual consumption & wastage tracking, and ±3% variance alerts.
   - **Accountant / Auditor**: Strict read-only compliance workspace covering GST outward supply summaries, ledger audits, outstanding ageing, and 15 exportable reports.
   - **Owner**: Executive oversight across sales, production variance, cost per packet, margins, and receivables, plus workspace role-switching to audit any operational view.

2. **Exact Financial & Inventory Mathematics (`decimal.js`)**:
   - All monetary values, GST splits (`CGST 2.5% + SGST 2.5%` intra-state Tamil Nadu vs `IGST 5%` interstate), BOM conversions (`kg/g`, `litre/ml`, `pieces/packets/cartons`), variance percentages, and ledger balances use `Decimal` fixed-precision arithmetic (never floating-point).
   - **Ledger-Based Inventory**: `Opening Stock + Purchases - Issues - Sales + Returns ± Adjustments = Closing Stock`. Every movement is append-only and linked to its source document (`PUR`, `PROD`, `DISP`, `ADJ`).
   - **Immutable GST Invoices**: Posted invoices cannot be deleted; cancellations are recorded strictly via Credit Notes (`CANCELLED_BY_CN`).

## Mandatory Benchmark Verification

The database automatically seeds the exact benchmark dataset on first sign-in (and can be reset via **Settings → Restore Mandatory Benchmark Dataset**):

| Benchmark Metric | Expected | Verified Formula / Source |
| :--- | :--- | :--- |
| Flour closing balance | `120 kg` | `100 opening + 60 purchase - 40 issued` |
| Cooking Oil closing balance | `19.5 litres` | `30 opening - 10.5 issued` |
| 200 g Pouches closing balance | `306 pieces` | `500 opening - (188 good + 6 damaged)` |
| Finished Murukku after production | `238 packets` | `50 opening + 188 good output` |
| Finished Murukku after Retailer A dispatch | `88 packets` | `238 - 150 dispatched` |
| Yield variance | `-6%` | `((188 - 200) / 200) * 100` (Alert > ±3%) |
| Oil consumption variance | `+5%` | `((10.5 - 10) / 10) * 100` (Alert > ±3%) |
| Retailer A taxable amount | `₹7,500.00` | `150 × ₹50.00` |
| Retailer A CGST / SGST | `₹187.50 / ₹187.50` | `2.5% CGST + 2.5% SGST` (Tamil Nadu) |
| Retailer A Invoice Total | `₹7,875.00` | `₹7,500 + ₹187.50 + ₹187.50` |
| Retailer A Outstanding | `₹3,875.00` | `₹7,875.00 - ₹4,000.00 paid` |
| Distributor B shortage | `32 packets` | `120 requested - 88 available` |

### Assumptions Documented
- **Unit Conversion**: `1 kg = 1,000 g`, `1 litre = 1,000 ml`, `1 carton = 20 packets/pieces` when converting across count units. Cartons are tracked separately in inventory (`pieces`) and never counted as individual packets.
- **Damaged Pouches**: Accounted separately in ledger (`PRODUCTION_DAMAGE`) and deducted from pouch stock alongside good packets (`188 + 6 = 194` pouches consumed).
- **Costing Valuation**: Supports both Weighted Average Cost (default active method) and FIFO layer valuation.

## Running Unit Tests

```bash
npx tsx --test tests/calculations.test.ts
```

## Database & Security Architecture

- **Database**: Firebase Firestore (`firebase-blueprint.json` defines all 15 normalized entity schemas and collection paths).
- **Authentication & RBAC**: Firebase Authentication (Google Sign-In) with `firestore.rules` enforcing role-based access, strict schema key validation (`hasAll`/`hasOnly`), immutable stock ledger & audit logs, and non-deletable GST invoices.

### Firebase Database Setup

The app and Firebase CLI are configured for project `gen-lang-client-0142212768` and Firestore database `ai-studio-c8ba944a-b364-4fe2-9320-9a877de09edb`.

1. In Firebase Console, confirm that this named Firestore database exists. If it does not, create it using this exact database ID and select the required database location before proceeding.
2. Enable Google as a sign-in provider under **Authentication → Sign-in method**, and add `localhost` under **Authentication → Settings → Authorized domains** for local development.
3. From the project directory, authenticate the Firebase CLI and deploy the configured Firestore rules:

   ```bash
   npx firebase-tools login
   npx firebase-tools deploy --only firestore:rules
   ```

The database rules are deny-by-default; the app's reads and writes will fail until the rules have been deployed. Do not replace them with public/test-mode rules.

## 15 Audit Reports & Exports

All 15 reports support date-range filtering and instant export to **Excel (.xlsx)**, **CSV**, and **PDF**, plus audit-ready **GST Tax Invoice PDF** generation via `jsPDF`.
>>>>>>> 4499dbb (Initial commit - Order-to-Audit Management System)
