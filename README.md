# نموذج استرجاع التذاكر (Ticket refund form)

A static page (`index.html`) where call-centre agents enter a ticket refund request with strict
validation. Once every field is valid, a summary screen lets the agent copy the data to paste
into an email, a CRM or a Word template. Nothing is sent anywhere; the data stays in the browser.

## Validation rules

| Field | Rule |
|---|---|
| رقم الحجز (type) | Required choice; shows only the matching field |
| رمز الحجز | Exactly 6 chars, A–Z / 0–9, auto-uppercased, must contain a letter |
| رقم الحجز | Exactly 12 digits |
| رقم التذاكر | One box per ticket (add / remove, max 8); `065` is already typed in every box, the agent adds the remaining 10 digits (13 in total); pasted `TKT-065…` numbers are cleaned up; no duplicates |
| الجوال | Country code picked from a list (default +966); number checked against that country's mobile format; copied as `+966512345678` |
| رقم القسيمة/القسائم | Optional. Same as tickets: `065` already typed, plus 10 digits (13 in total); max 8; no duplicates |
| رقم الهوية | 10 digits, starts with 1 or 2, checksum verified (`1000000000` is always accepted; see `CONFIG.allowedIds`) |
| الإسم الأول / الأخير لصاحب الحساب | English letters only (A–Z), 2–30 chars; entered once, just before the IBAN; copied as one uppercase name |
| رقم الايبان | Exactly 24 characters (letters/digits, spaces ignored). SOP value `SA000000000000000000000` is always accepted for card refunds (see `CONFIG.allowedIbans`) |
| اسم البنك | Auto-selected from a Saudi (`SA…`) IBAN's bank code; must match it |
| طريقة الدفع | Fixed list (+ "أخرى" with free text) |
| ملاحظات | Optional; if filled, 5–300 chars |
| هل تم إشعار الضيف…؟ | Required yes / no |

Arabic-Indic digits (٠١٢…) are converted to 0–9 automatically.
Rules (including the ticket / voucher prefixes and lengths), country codes, bank codes and payment methods are in the `CONFIG` block at the top of the script.

## Publish on GitHub Pages

Deployment is automatic via [`.github/workflows/pages.yml`](.github/workflows/pages.yml):
every push to `main` publishes `index.html` to GitHub Pages.

One-time setup:

1. Push this repo to GitHub.
2. Repo **Settings → Pages → Build and deployment → Source: GitHub Actions**.
3. Push again (or run the workflow from the **Actions** tab). The form is served at
   `https://<user>.github.io/<repo>/`.

## Copying the result

After **مراجعة البيانات**, the summary screen has three copy buttons:

| Button | Pastes as | Use for |
|---|---|---|
| نسخ كنص | `label: value` lines | Email body, CRM notes field |
| نسخ كجدول (Word / Outlook) | A two-column table (plain-text fallback) | Word template, Outlook |
| نسخ لـ Excel | Header row + value row, tab separated | Excel / Sheets, CRM bulk import |

The copied fields and their order are defined in `summaryRows()`.
**طلب جديد** clears the form for the next call.
