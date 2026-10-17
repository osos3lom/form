# نموذج استرجاع التذاكر (Ticket refund form)

A static page (`index.html`) that adds strict validation to the Microsoft Form
"نموذج استرجاع التذاكر لموظفي استقبال المكالمات". After the agent checks the data, the
page opens the official Microsoft Form **already filled in**, and the agent presses **Submit** there.

## Validation rules

| Field | Rule |
|---|---|
| رقم الحجز (type) | Required choice; shows only the matching field (not sent to Microsoft Forms) |
| رمز الحجز | Exactly 6 chars, A–Z / 0–9, auto-uppercased, must contain a letter |
| رقم الحجز | Exactly 12 digits |
| رقم التذاكر | One box per ticket (add / remove, max 8); each exactly 11 digits starting with `065`; the leading `0` is already typed in every box; no duplicates |
| الجوال | Country code picked from a list (default +966); number checked against that country's mobile format; sent as `+966512345678` |
| رقم القسيمة/القسائم | Optional. One box per voucher (max 8); each exactly 10 digits starting with `0652`, no duplicates |
| رقم الهوية | 10 digits, starts with 1 or 2, checksum verified (`1000000000` is always accepted; see `CONFIG.allowedIds`) |
| الإسم الأول / الأخير لصاحب الحساب | English letters only (A–Z), 2–30 chars; entered once, just before the IBAN |
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

## Why it opens the Microsoft Form instead of submitting directly

Microsoft Forms does not allow cross-origin submissions (no CORS), so a GitHub Pages site
cannot post responses silently. Pre-filled links are the supported way to pass data in.

## Notes

- Validation runs in the browser. Anyone who has the original Microsoft Forms link can
  still bypass it, so share only the GitHub Pages link with agents.
- Pre-filled links must be turned on in Microsoft Forms (**⋯ → Get pre-filled URL**),
  otherwise the official form opens empty.
- If questions are added or removed in Microsoft Forms, update the question IDs in `CONFIG.q`.
  The booking-format choice exists only on this page; it is not sent to Microsoft Forms.
