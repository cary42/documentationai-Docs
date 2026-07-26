# Campium Docs Audit — July 2026

Full accuracy audit of every documentation page against the current Campium code
(`campium-all`). Each finding cites `file:line` evidence in the app. Use this as
the work list for the `docs/audit-2026-07` branch.

**Legend:** STALE = doc contradicts code (fix first) · THIN = correct but missing
real functionality · STUB/EMPTY = little or no content · GAP = feature exists in
code with no doc page.

Coverage: ~100 pages reviewed. Headline: **2 empty pages**, **~20 pages with
STALE (wrong) content**, **~30 pages missing real sub-features**, and **a large
set of shipped features with no page at all** (Medical/Medication is the biggest).

---

## 1. STALE — pages with wrong content (fix first)

### discounts.mdx
- No field "Apply To Entire Cart". Real field is **Apply To All Sessions** (`entire_sale_flag`, new-discount.html:90-98); product targeting is independent (:189-201).
- Undocumented field **Combine All Children's Sessions** (`entire_sale_flag2`, :100-109).
- "Skip Deposit?" and "Bypass payment at portal checkout?" are the SAME field (`skip_deposit_flag`); current label is "Bypass payment at portal checkout?" (:133-142). No field literally "Skip Deposit?".
- Wrong error strings: real is "Discount Code can only be letters and numbers." (save-new-discount.php:8) and "You must enter a Discount Code/Description" (:16-17).
- Diagnosis is a standalone **Discount Checker** panel (discounts.php:35-37), not a per-row action-menu item. Row menu = Edit/Archive/Delete only.
- Delete only shows for UNUSED discounts (`{{^used_flag}}`).

### account-balance.mdx
- Real columns: Primary Contact, Users (not "Campers"), Products, Email, Reg Date, **Revenue** (not "Charges"), Balance, Scheduled, Note (account-balance.html:82-101). No "Account Number" or "Plan" column. Red highlight is only the Balance+Scheduled cells.
- Missing: summary cards, Send Email, Display Columns selector, per-family Note editor, School column.

### deposit-summary.mdx
- Fees/Net columns are conditional and come from the **Finix settlement API** (index.php:6256-6265), not agency-configured rates. Legacy table has no Fees/Net.
- Missing By-batch/All-transactions toggle; pending cash/check is a collapsed card; button is "Create Deposit #".

### average-family-spend.mdx
- Documents a per-season **bar chart that does not exist** (only a line `trendChart`, average-family-spend.php:257-259). Remove it.
- Missing the real second filter **Minimum Family Spend** (#min-spend, :93-101).

### ach-returns.mdx
- Invents a "Transaction ID" column. Real columns: Actions, Name, Amount, Reason, Transaction Date, Return Date, **Payment since?** (ach-returns.html:82-88). Replace Transaction ID with "Payment since?".

### authentication.mdx
- Claims a special character is required and server-validated. The server validator has **no special-char check** (functions.validation.php:193-212). Portal min 8 / admin 12 and account lock after 5 attempts are correct. Add passkey/WebAuthn login.

### seasons-and-sessions.mdx
- New Session: "Days of Week" is a multiselect (not Sun-Sat "For Attendance Only" checkboxes); labels are "Start Date"/"End Date" under "Schedule" (new-product.html:133-160).
- Subproduct Status is `wait_flag` Open/Closed (not Active/Inactive).
- Cancellation-fee button is **Save Cancellation Fee** (no "Create…"/"Update…" buttons). Clone modal also has End Date + "Increase Form and Payment Plan dates by one year"; menu label is **Copy Season**.

### manage-seasons.mdx
- Add New Season modal has only Season Name, **Season End Date (required)**, Membership Season. Snapshot/dropdowns/status are in the EDIT modal only.
- Inactivation targets campers (CA) + season-assigned staff, not "users". Missing "Also inactivate staff…" and "Email invoice to all families…" options.

### memberships.mdx
- Staff-visibility section is stale: recurring-billing fields are no longer super-user-only and membership seasons are no longer hidden (functions.php:1991-1996).

### settings.mdx
- "Accept AMEX" and "Send Payment Decline Email" are NOT admin-only. "Authorize and Capture" renders on no tab. Admin tab is gated by `isCampiumEmployee`, not `isSuperUser`. Missing many module toggles and label-override fields.

### business-info-and-logo.mdx
- No contact-email field on this page; contact email lives in Site Settings. Three independent saves (General Info / DKIM / Logo Upload), not one. Missing BAA, Subdomain, Website, socials, Tax ID.

### profiles.mdx
- Password rule wrong: reset enforces **8** chars, not 12 (reset-password.html:20). Missing Welcome Sequence card, Attendance tab.

### profile-fields.mdx
- Missing 5th field type **"Link to a User" (UL)** (custom/index.php:179).

### import-users.mdx
- Menu is Admin → Import & Export → Import → Users; gated by `custom_module` (not "Administrator"). Template URL is relative/auth'd (not a public campiumdb.com link). Grouping falls back email → Person ID → parent-name. Missing Staff import mode, Person-ID re-import, on-the-fly field creation, dry-run preview.

### campiumdb-users.mdx
- Staff search does NOT filter by active status (get-staff.php:2). Broken link `/staff-organization-account-access` (real page: admin/staff-agency-access.php). Missing Payment Digest column; role change needs `modify_user_role`.

### tiered-pricing.mdx
- No "Forbidden"/permission gate on saving tiers (doc invents it). Duplicate-effective-date rejection is unverifiable. Base "Tiers" button is on the Products **list** rows, not the edit modal.

### bunk-assignments.mdx
- Access gate is the **Bunking** permission (`bunking_module`, bunk-assignments.php:11), not "view users".

### photo-hosting.mdx
- "Photo Hosting Module" and "Gallery Module" are the SAME single toggle (`gallery_module`). Document the separate **Facial Recognition** module toggle.

### food-and-per-day-ordering.mdx
- Order cutoff is off by one. `leadDays=0` locks today (earliest orderable = tomorrow); each doc example understates the lead requirement by a day (class.product_week.php:127-134).

### birthdays.mdx
- The "Age" column shows **current** age (getAge), not "the age they'll turn on their next birthday" (index.php:5110).

### campaigns.mdx
- List columns are Name/Status/Clicks/Unique/**Conversions**/Created. Doc's "Registrations" is really Conversions; "Description" is a sub-line, not a column.

### portal-news.mdx
- **End Date is required** (not optional). There are **4** news types (add Push Notification, PU), not 3. "Pin To Top" toggle undocumented. The "Post News → use File field" step is garbled (File belongs to "Post files to Portal").

### sms.mdx
- Stale helper "153 chars = 1 msg". Current copy: "Up to 160 characters sends as one message; longer text sends as multiple messages, each billed at $0.01" (edit-sms.html:74). Add $10/1,000 billing note + `manage_sms` permission.

### system-report-family-scheduled-payments.mdx
- Title is "Scheduled Payments Potential Issues" (no season). The documented "Issues" filter does not exist — remove it.

### system-report-scheduled-payments.mdx
- "Filter by Type"/"Type" is by payment **method** (Credit Card / ACH / No Account on File), not payment categories like tuition/fees.

### system-report-charts.mdx
- No unified "Charts" screen exists; Registration Patterns, Weeks Sold (Year Over Year), and Product YoY are separate individual reports.

### Menu-label mismatches (nav paths in docs are wrong)
- Waitlist page = "Wait List Management"; payment-method-updates = "Card Updates – Families with Failed Payments"; families-without-primary = "No Primary Payment Method".

---

## 2. THIN — accurate but missing real functionality

- **forms.mdx** — reminder audience is Not Started + In Progress only (not Pending); reminders are enabled via Before/After due-date multiselects (no on/off toggle); tab label is dynamic `{{product_plural}} Layout`. Missing Reference One/Two Form, Charge Deposit On Approval, Pay-In-Full Discount override fields.
- **payment-plans.mdx** — missing "Require paid by session start" flag and "Show Only For These Sessions" restriction (both belong in the "why a plan isn't showing" list).
- **cancellation-fees.mdx** — missing the key caveat: fees are NOT auto-applied on cancel; you add them via a profile's Sales tab.
- **revenue-summary.mdx** — missing "Group revenue by profile field" setting + the resulting breakdown table.
- **canteen.mdx** — missing "Allow overspend to clear balances" setting; POS credit-card is inline Payabli/Finix (not a link); "Adjustment" funding option; the 3-permission model.
- **payment-processors.mdx** — documents only Finix; the app is multi-processor (usio, payabli, payconex, paysafe, nmi, finix, stripe).
- **what-is-a-chargeback.mdx** — generic; add Campium specifics (Finix dispute emails INQUIRY/PENDING/LOST/WON; Payabli disputes surface in ACH Returns).
- **subproducts.mdx** — missing "Parent site" column + `parent_show_flag` visibility toggle.
- **roles-and-permissions.mdx** — never lists the ~50 real permissions; add a reference list. Missing Members, Delete role, Permission Catalog.
- **my-groups.mdx** — add `app_my_group` mobile toggle, the MU/MM "All Users" field requirement, and `my_groups_only` restricted login.
- **account.mdx** — missing the whole **Passkeys** tab.
- **mass-emails.mdx / email-templates.mdx** — missing BCC, AI Rewrite, Display Login Button; template Visibility options are "Everyone at your organization" / "Only you".
- **push-notifications.mdx / app-chat.mdx** — state prerequisites (`enable_mobile_app`, `staff_chat_module`) or the menu never appears.
- **scheduling.mdx** — missing the published-schedule ICS calendar attachment / add-to-calendar links. (Also a likely code bug: view preference is written per-agency, read per-user.)
- **waitlist.mdx / system-report-waitlist.mdx** — missing "Email me when someone joins" opt-in, "Cancel Invitation" action, and Sibling column.
- **system-report-payment-plans.mdx** — missing the Guardians/Enrollees view toggle.
- **system-report-chronological-user-registration.mdx** — missing the Status filter buttons and "# Seasons" column.

---

## 3. EMPTY / STUB pages

- **email.mdx** — 0 bytes. Should cover **System Emails** (admin/system-emails.php) + email logs, or redirect to mass-emails.
- **permissions-recipients.mdx** — 0 bytes. Should document form user-type gating vs recipient filters (recipients.html:250-275).
- **introduction.mdx** — frontmatter only; write a real landing overview.
- **features.mdx** — boilerplate with a stale PayPal reference and fabricated JS-SDK samples; either rewrite to reflect the real product or retire it.

---

## 4. GAPS — shipped features with NO doc page (new pages to write)

**Biggest gap — Medical / Medication:** medication log, health reports, combined
health reports (profile/action/*medication*, report/medication-log.php,
combined-health-reports.php). Only a thin health-report.mdx exists.

Other notable gaps:
- **Staff Hub** (admin/staff-hub.php) — only a section inside staff.mdx.
- **System Emails**, **Email logs/diagnostics**, **SMS Usage report**, **Email Sequences / drip**.
- **Merge Users**, **Move User Into Family (reparent)**.
- **Payment processor setup** (Payabli, Finix onboarding, USIO, Stripe) — distinct from the marketing Integrations page; plus USIO/Payabli behavior + where chargebacks appear.
- **Activity / program scheduling reports** (report/activity-*).
- **Bunk labels & bunk reports**.
- **Note Types**, **Legal Documents**, **Portal Settings**.
- **Financial reports** with no page: ledgers (family-ledger, ledger-summary), aging balance, negative-balance, deposit-detail drill-down, discount reports, tipping, expense, donor/fundraising suite, program/rolling/invoiced revenue.
- **Other system reports** with no page: att.php (printable PDF), pickup-report, geo (family map), matrix/cross-tab, duplicates, bounced-emails, incorrect-age, unverified, all-emails log, mobile-app-usage, note-summary, new-users-by-season, staff_assignments.
- Feature-level: automatic Pay-In-Full Discount, Sibling discount, Welcome Sequence, Profile Layouts, Season launch checklist, per-season invoice footer, API v2 auth (Bearer JWT + refresh), WebAuthn/passkey login, Canteen POS on mobile.

Housekeeping: `form-status-summary.php`, `payment-plans.php`, and `future-family.php` don't call `setReportDescription()` even though the registry has their entries.

---

## Recommended fix order
1. **STALE corrections** (§1) — content that is actively wrong; fastest trust win.
2. **Empty pages** (§3) — email.mdx, permissions-recipients.mdx.
3. **THIN additions** (§2) — fill in the missing real controls per page.
4. **New pages for gaps** (§4) — starting with **Medical/Medication** and Staff Hub.
