# Standardized Certificate Verification Portal & Backward Compatibility Redirects Plan

Standardize the certificate verification system for **Innovation & DIY Club**, unifying all event certificates under a single modern UI, connecting to the central Google Apps Script API, and preserving full backward compatibility for all existing printed QR codes.

## User Review Required

> [!IMPORTANT]
> **Existing Printed QR Codes Preservation**:
> Old QR codes (e.g. `certificate-verify.html?id=...` and `index.html?ref=...`) will remain 100% operational. `certificate-verify.html` will automatically redirect users to the primary standard URL (`index.html?ref=...`) while maintaining the certificate ID context.

> [!NOTE]
> **Standard API Endpoint**:
> All event certificates will be fetched from the official central endpoint:  
> `https://script.google.com/macros/s/AKfycbxonioWdF6USjAB1OkK97Fe5iuOm_x00IV_b0jC_siPGKIh-12W5jFtjvef6SrIg6j9/exec?ref=<CERT_ID>`

---

## Proposed Changes

### Certificate Verification System

#### [MODIFY] [index.html](file:///c:/Users/Sumit/Desktop/DIY/Website/Certificate/certificate-verify/index.html)
- Transform into the **Standard Primary Certificate Verification Portal**.
- **URL Parameter Handling**: Accepts `ref`, `id`, `cert`, `certificateId` parameters and canonicalizes to `?ref=<ID>`.
- **Multi-Level API Verification**:
  1. Primary: Fetch from new central Apps Script API endpoint.
  2. Fallback 1: Retain candidate generation (`buildRefCandidates`) for truncated CFI legacy codes.
  3. Fallback 2: Check Firebase Firestore database for legacy documents.
- **UI / UX Features**:
  - Header with official **Innovation & DIY Club** logo (`assets/photos/diy-club-logo.png`), MITS Gwalior logo (`assets/photos/mits-logo.png`), IETE (`assets/photos/iete-logo.png`), and IEEE (`assets/photos/ieee-logo.png`).
  - Animated skeleton / spinner loader state during details fetching.
  - Premium Verified Badge with glowing checkmark shield animation.
  - Clean Metadata Card: Participant Name, Event Name, Certificate Ref ID, Role, Email, Enrollment Number, Branch/Course, Issued By.
  - Manual Search Input Bar so users can verify any certificate directly on the page.
  - "Copy Verification Link" & "Print / Save Certificate" quick action buttons.

#### [MODIFY] [certificate-verify.html](file:///c:/Users/Sumit/Desktop/DIY/Website/Certificate/certificate-verify/certificate-verify.html)
- Turn into a seamless **Backward-Compatibility Redirector**.
- Extracts `id` or `ref` parameter and executes immediate client-side redirection:
  `window.location.replace("./?ref=" + encodeURIComponent(certId))`
- Provides fallback UI with loading indicator and manual click link.

#### [MODIFY] [certificate-template.html](file:///c:/Users/Sumit/Desktop/DIY/Website/Certificate/certificate-verify/certificate-template.html)
- Update data loader script to query central Apps Script API (`ref=...`) first, then Firestore fallback.
- Populate recipient name, event name, date, role dynamically for all events.

---

## Verification Plan

### Automated / Local Browser Verification
1. Run local server: `python -m http.server 8080` in the workspace directory.
2. Test sample URLs using browser subagent:
   - `http://localhost:8080/?ref=CFI-310126-002-LWL48G0` (Verify CRAFTTRONICS event data loads with loading state & standard UI)
   - `http://localhost:8080/certificate-verify.html?id=IBF-030426-001-1RUBWP1` (Verify automatic redirect to `/?ref=IBF-030426-001-1RUBWP1` and Innovation Battlefield data loads cleanly)
   - `http://localhost:8080/` (Verify search bar functionality for manual lookup)
3. Test print certificate view link for both CFI and IBF reference IDs.
