# Search gap test, October 2026

Thirty questions a camp director would ask, each run through the docs search tool that the Campium Navigator plugin uses (`search_documentation`, top 5 results, default 0.75 semantic ratio) on 2026-10-19. Each answer was checked against the page text, and each fix was checked against the admin source in `campium-all`.

## Results

| Verdict | Count |
| --- | --- |
| HIT | 21 |
| WRONG PAGE | 5 |
| MISS | 4 |

- **HIT**: a returned page answers the question.
- **WRONG PAGE**: results came back, but the page that answers it was not among them.
- **MISS**: no page in the knowledge base answered the question.

## Follow-ups: missing pages

These need a new page, so this branch does not fix them.

| # | Question | Product area | Where it lives |
| --- | --- | --- | --- |
| 5 | How do families register a new camper on the parent portal? | Registration | Parent portal home, then the registration form card. On the admin side the form is built under **Forms**. A family-eye walkthrough (add a child, pick sessions, cart, **How much today?**, payment plan, submit) is not documented anywhere. Search returned the Family Registration *report* instead. |

## Fixed on this branch

| # | Verdict | Fix type | Change |
| --- | --- | --- | --- |
| 3 | MISS | Missing section | `seasons-and-sessions.mdx` **Waitlist**: rewrote it to say when a registration goes on the wait list (capacity reached, or **Status** set to **Closed**) and that it is added at $0. The old text told camps to ask support. Source: `www/parent-new/form/action/calc.php:811`, `www/prod/mustache/admin/products/partial/edit-product.html:197`, `www/prod/php/admin/index.php:4191`. |
| 4 | WRONG PAGE | Keywords | `seasons-and-sessions.mdx` **Capacity**: the first sentence now uses "maximum number of campers", "enrollment limit", and "cap". Also corrected the helper text to **0 or blank = unlimited.** (`new-product.html:55`, `edit-product.html:63`). |
| 10 | MISS | Missing section | `profiles.mdx` new **Record a payment or credit** section: **Add Payment**, the **New Payment** window, every **Payment Type**, and **Save Payment**. Linked from `payment-methods.mdx`. Source: `www/prod/mustache/profile/partial/sales-eev2.html:292`, `www/prod/profile/js/local.js:4113`, `www/prod/php/profile/index.php:4840-4868`, `www/prod/mustache/profile/partial/paymentCreate.html:13-525`. |
| 11 | WRONG PAGE | Keywords | Same section as #10. **Adjustment** is the way to give a family a credit, and `payment-methods.mdx` now says so in its first paragraph about office payments. |
| 20 | MISS | Missing section | `seasons-and-sessions.mdx` **Price** field: a changed price applies to new registrations only, because each registration stores its own price. Source: `include/sales/class.sale_product.php:46`. `www/prod/admin/products/action/save-product.php` never rewrites existing lines. |
| 21 | WRONG PAGE | Missing section | `roles-and-permissions.mdx` new **Limit staff to campers in certain sessions** section. It points to the session **Staff** field (`edit-product.html:171`) and to **my groups only** mode. Search had returned CampiumDB Users and Staff Applications. |
| 24 | WRONG PAGE | Keywords | `sms.mdx` **Recipients tab**: added "To text every parent in one session, filter the recipients by that session." Search had returned the waitlist texting section. |
| 26 | WRONG PAGE | Keywords | `mass-emails.mdx` **Logs** tab: now says "who received the email and who opened it" and links to the non-openers resend page. |

## Full table

| # | Question | Pages returned (top hits) | Answer on page? | Verdict |
| --- | --- | --- | --- | --- |
| 1 | How do I add a new session to a season? | guided-season-setup#sessions, seasons-and-sessions#create-a-new-sessionproduct, quickstart, special-weeks | Yes, Seasons and sessions covers the modal field by field | HIT |
| 2 | How do I copy last year's season to this year? | special-weeks#copy-dates, seasons-and-sessions#copy-an-existing-season, payment-plans#copy-all, guided-season-setup#copy-last-seasons-setup | Yes, **Copy Season**. Special Weeks ranked first but the right section was second | HIT |
| 3 | Where do I turn on a waitlist when a session fills up? | seasons-and-sessions#waitlist, waitlist, product-rollup | No. The Waitlist section said the behavior was not documented and to contact support | MISS |
| 4 | How do I set a maximum number of campers for a session? | special-weeks, guided-season-setup#sessions, attendance, waitlist, bunk-assignments | Partly. Guided setup mentions capacity in passing. The **Capacity** definition was not returned | WRONG PAGE |
| 5 | How do families register a new camper on the parent portal? | family-registration (report), attendance-kiosk, photo-hosting, new-users-by-season | No | MISS |
| 6 | How do I withdraw a camper from a session and charge a cancellation fee? | withdraw-a-camper, cancellation-fees, profiles#remove-a-session | Yes | HIT |
| 7 | How do I refund a payment to a parent's credit card? | processing-refunds-and-voids | Yes | HIT |
| 8 | How do I set up a payment plan that charges families monthly? | payment-plans#even-split-plans, #plan-fields, #change-a-familys-payment-plan | Yes | HIT |
| 9 | A scheduled payment failed, how do I retry it? | retry-failed-scheduled-payment | Yes | HIT |
| 10 | How do I record a check payment I received in the mail? | payment-methods#methods-you-record-by-hand, deposit-detail, deposit-summary | No. Payment Methods said to record it on the Sales tab but no page had the steps | MISS |
| 11 | How do I give a family a credit on their account? | balance-transfer-utilization, transfer-season-balances, zero-out-season-credits | No. These pages move or clear credits. None explains how to add one | WRONG PAGE |
| 12 | How do I let a family skip the deposit at checkout? | discounts#bypass-payment-at-portal-checkout, forms#minimum-deposit-settings | Yes | HIT |
| 13 | How do I create a sibling discount? | discounts#create-a-sibling-discount | Yes | HIT |
| 14 | Which report shows who still owes money? | account-balance, account-balance-aging | Yes | HIT |
| 15 | How do I update the credit card on file for a family? | expired-cc-report, payment-methods#saved-methods-on-the-parent-portal | Yes, families update it under **Payment Methods** on the portal | HIT |
| 16 | How do I require a form to be completed before a camper can attend? | camper-hub#choose-which-fields-and-forms-count, permissions-recipients, forms#recipients-tab | Yes, **Required forms by season** in Camper Hub. Campium tracks required forms but does not block attendance | HIT |
| 17 | Which report shows which campers have not completed their forms? | form-status-summary, camper-hub | Yes | HIT |
| 18 | A parent cannot see the health form on the parent portal | permissions-recipients, user-cant-see-the-form-on-their-portal | Yes, **Check Permissions** | HIT |
| 19 | How do I email everyone who has not finished a form? | camper-hub#send-reminders, email-users-who-have-not-completed-a-form, forms#send-reminder-emails | Yes | HIT |
| 20 | How do I change a session price after families have already registered? | tiered-pricing, guided-season-setup#sessions-table, seasons-and-sessions, subproducts | No page said what happens to families who already registered | MISS |
| 21 | How do I give a staff member access to only the campers in their session? | campiumdb-users, staff, product-coverage, my-groups | Partly. My Groups covers attendance-only logins. The session **Staff** field was not returned | WRONG PAGE |
| 22 | How do I collect W-4 forms from my staff? | w4-collection | Yes | HIT |
| 23 | Where do I see staff applications and mark a staff member as hired? | staff#staff-hub, staff-hub | Yes | HIT |
| 24 | How do I send a text message to all parents in a session? | waitlist#text-everyone, sms#sms-message, one-way-messaging, sms#text-families-from-a-report | No. Waitlist texting and message length came back, not how to target a session | WRONG PAGE |
| 25 | Why are my emails landing in spam? | business-info-and-logo#dkim, bounced-emails | Yes, DKIM | HIT |
| 26 | How do I see who opened an email I sent? | resend-email-to-non-openers, donor-management#email-log | Partly. Donor newsletters only. The Mass Emails **Logs** tab and the Email Log were not returned | WRONG PAGE |
| 27 | How do I print a list of campers with allergies? | friends-by-session-daily, profile-photo-report | Yes, Campers by Session (Daily) lists allergies per day | HIT |
| 28 | How do I log medications given to campers at camp? | medications | Yes | HIT |
| 29 | How do I assign campers to a bus stop? | transportation#day-to-day-workflow | Yes, **Add Riders** plus per-rider address and stop time | HIT |
| 30 | Which report shows what was deposited to our bank account this week? | deposit-summary | Yes | HIT |

## Most-hit pages

These pages came back most often and are part of the October prose pass: seasons-and-sessions, guided-season-setup, payment-plans, discounts, deposit-summary, camper-hub, payment-methods, waitlist, sms, processing-refunds-and-voids.
