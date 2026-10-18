# NBK Command Center

Private business tracker for **Notarized By Kristy LLC** (Texas Notary Kristy), built as a single-page app that installs on a phone's Home Screen.

**Live app:** https://texasnotarykristy.com/NBK-Command-Center/

## What it does

| Tab | Purpose |
|---|---|
| **Home** | Month-to-date income, net profit, money owed, goal progress, active apostille jobs, next tax payment, expiring credentials |
| **Appts** | Mobile notary, loan signing, RON, deposition, wedding officiant and translation appointments |
| **Invoices** | Branded invoices built from the rate card, numbered NBK-YYYY-####, Draft → Sent → Paid, print or save as PDF |
| **Apostille** | Apostille jobs by stage, with trips, mileage, and job costs |
| **Expenses** | Business expenses and standalone mileage |
| **Tax** | Self-employment tax estimate, P&L by service line, quarterly payment tracker |
| **Goals** | Monthly revenue and appointment targets |
| **Pricing** | Editable rate card, Texas notarial fees, IRS mileage rates, invoice header and terms |
| **Vault** | Commission, E&O, certifications, and registrations with expiration alerts |
| **Export** | Monthly archive CSV, year-end package for the accountant, full backup and restore |

## How the tax math works

- **Notarial fees** are excluded from self-employment tax under IRC §1402(c)(1) and still count as income. The exempt amount follows Tex. Gov't Code §406.024: $10 for the first signer on an acknowledgment, $1 for each additional signer, $10 per oath or jurat, and $25 per RON act (§406.111). It is never more than the fee actually collected.
- **Travel, printing, coordination, officiant, and translation fees** are fully subject to self-employment tax.
- **Pass-through costs** billed at cost (shipping, translation, state record fees) are booked as offsetting expenses.
- **Mileage** uses the IRS standard rate in effect on each trip's date. Add new rates in the Pricing tab.
- **Deposition income** (1099) is tracked separately.

Figures are estimates for bookkeeping. Confirm with a tax professional before filing.

## Data and privacy

All data is stored only in the browser on the device using the app. Nothing is sent to a server. Clearing browser data or deleting the app can erase it, so download a **Full Backup** from the Export tab regularly and keep it in Google Drive or Files.

**Monthly routine:** Archive Month CSV + Full Backup JSON.

## Files

| File | Purpose |
|---|---|
| `index.html` | The entire app |
| `manifest.json`, `sw.js` | Home Screen install and offline support |
| `favicon.png`, `icon-*.png` | Browser and Home Screen icons |
| `.nojekyll` | Tells GitHub Pages to serve the app as-is |

## Updating

Replace `index.html` (and any icons) in this repo. GitHub Pages publishes the change within a few minutes. If a phone still shows the old version, close the app fully and reopen it.

---

© Notarized By Kristy LLC · Cibolo, Texas
