# Certificate Verification Portal & Standardized API Walkthrough

We have successfully standardized the **Innovation & DIY Club** Certificate Verification system. The single, unified Portal now handles all past, present, and future event certificates with a modern UI, central backend API integration, and automatic redirection for existing printed QR codes.

---

## 🌟 What Was Accomplished

### 1. Unified Standard Verification Portal (`index.html`)
- **Central API Integration**: Connected directly to the official standardized Google Apps Script API endpoint:  
  `https://script.google.com/macros/s/AKfycbxonioWdF6USjAB1OkK97Fe5iuOm_x00IV_b0jC_siPGKIh-12W5jFtjvef6SrIg6j9/exec`
- **Branding & Logos**: Displays official **Innovation & DIY Club** logo (`assets/photos/diy-club-logo.png`), MITS Gwalior logo (`assets/photos/mits-logo.png`), IETE Bhopal, and IEEE Student Chapter logos.
- **Loading State ("jab details load ho tab loading aaye")**: Features an animated gradient spinner ring with real-time feedback while fetching certificate records.
- **Interactive Certificate Search**: Includes a search input bar allowing users to manually verify any Certificate Reference ID on demand.
- **Rich Verified Details View**:
  - Participant Name, Reference ID, Event Name, Role, Email, Enrollment Number, Branch/Course, and Issuing Authority.
  - Action buttons to **Print Certificate** and **Copy Verification Link** (with toast feedback).
- **Multi-Layered Fallback Chain**:
  1. Central Apps Script API (`ref=...`)
  2. Legacy CFI Candidate Generator (handles missing digits in truncated QR codes)
  3. Firebase Firestore database lookup
  4. Local `data.json` fallback

### 2. Full Backward-Compatibility Redirect (`certificate-verify.html`)
- Existing QR codes printed on physical certificates (e.g. `certificate-verify.html?id=IBF-030426-001-1RUBWP1`) now instantly redirect users to the standard URL (`/?ref=IBF-030426-001-1RUBWP1`).
- Ensures zero broken QR codes for previous events (Crafttronics, Innovation Battlefield, etc.).

### 3. Updated Printable Certificate Template (`certificate-template.html`)
- Updated to dynamically retrieve event name, participant name, date, and details from the central Apps Script API endpoint.

---

## 🧪 Verification Results

| Test Scenario | Input URL | Target / Action | Status | Result |
| :--- | :--- | :--- | :--- | :--- |
| **Old CFI QR Code** | `/?ref=CFI-310126-002-LWL48G0` | Central API lookup | ✅ PASS | Verified "Anant kumar singh", "CRAFTTRONICS" |
| **Old IBF QR Code** | `/certificate-verify.html?id=IBF-030426-001-1RUBWP1` | Auto Redirect -> `/?ref=IBF-030426-001-1RUBWP1` | ✅ PASS | Redirected & Verified "Abhay Gupta", "Innovation Battlefield" |
| **Manual Lookup** | Direct input in Search Bar | Query Central API | ✅ PASS | Returns instant verification |
| **Print Template** | `/certificate-template.html?ref=CFI-310126-002-LWL48G0` | Fetch event details | ✅ PASS | Renders printable certificate |

---

## 📸 Key Files Updated
- [`index.html`](file:///c:/Users/Sumit/Desktop/DIY/Website/Certificate/certificate-verify/index.html) - Primary Verification Portal UI & Logic
- [`certificate-verify.html`](file:///c:/Users/Sumit/Desktop/DIY/Website/Certificate/certificate-verify/certificate-verify.html) - Backward-Compatibility Redirector
- [`certificate-template.html`](file:///c:/Users/Sumit/Desktop/DIY/Website/Certificate/certificate-verify/certificate-template.html) - Printable Certificate Loader
