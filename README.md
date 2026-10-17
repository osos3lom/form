# نموذج استرجاع التذاكر (Ticket refund form)

A static page (`index.html`) that adds strict validation to the Microsoft Form
"نموذج استرجاع التذاكر لموظفي استقبال المكالمات". After the agent checks the data, the
page opens the official Microsoft Form **already filled in**, and the agent presses **Submit** there.

## Validation rules

| Field | Rule |
|---|---|
| الإسم الأول / الأخير | English letters only (A–Z), 2–30 chars |
| رقم الحجز (type) | Required choice; shows only the matching field |
| رمز الحجز | Exactly 6 chars, A–Z / 0–9, auto-uppercased, must contain a letter |
| رقم الحجز | Exactly 12 digits |
| رقم التذاكر | One box per ticket (add / remove, max 8); each exactly 10 digits, no duplicates |
| الجوال | Country code picked from a list (default +966); number checked against that country's mobile format; sent as `+966512345678` |
| اسم صاحب الحساب | English letters only, at least 2 words |
| رقم الهوية | 10 digits, starts with 1 or 2, checksum verified |
| رقم الايبان | `SA` + 22 digits, mod-97 checksum verified |
| اسم البنك | Auto-selected from the IBAN bank code; must match it |
| طريقة الدفع | Fixed list (+ "أخرى" with free text) |
| ملاحظات | 5–300 chars |

Arabic-Indic digits (٠١٢…) are converted to 0–9 automatically.
Rules, country codes, bank codes and payment methods are in the `CONFIG` block at the top of the script.

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
