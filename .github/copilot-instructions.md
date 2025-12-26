**Repository Overview**
- **Purpose:** Static marketing/shop site for Afya Asilia (natural wellness products). Main UX, product data and UI behavior live in a single file: `index.html`.
- **Important files:** [index.html](index.html), [README.md](README.md), [tailwind.js](tailwind.js), `bootstrap/` (local CSS/JS assets), `images/` (product images).

**Big-picture architecture**
- Single-page static site: HTML + inline JS drives product data and UI. There is no backend; contact form is a dummy front-end send (setTimeout) and ordering uses WhatsApp links.
- Product data is embedded as the `products` array inside `index.html` — treat this as the canonical product store for edits.

**Key patterns & conventions**
- DOM IDs are authoritative for content and localization (e.g. `heroTitle`, `productsTitle`, `contactForm`). Update those when changing copy or localization.
- Language handling: a `langSwitcher` selects `en`/`sw`. Strings are localized in-place by the `langSwitcher` change handler; update both branches when adding UI text.
- Product descriptions may contain headings (`"What it does:"` / `"Who it is for:"` or Swahili equivalents). Code splits and bolds these headings when rendering.
- Categories are plain strings on each product (e.g. `weight loss`, `detox`). Filtering uses exact match against `category` or `all`.

**External integrations / important values**
- WhatsApp ordering: all order actions construct URLs targeting `https://api.whatsapp.com/send?phone=255692538955...` — update the phone number in `index.html` to change order routing.
- Tailwind is loaded via CDN + local `tailwind.js`. Bootstrap assets exist under `bootstrap/` but the page primarily uses Tailwind; be cautious before removing bootstrap files.

**Developer workflows & commands**
- Serve locally via XAMPP (site placed in `htdocs`) or any static file server. Example (from project root):

  - With XAMPP: place repo in `htdocs` and open `http://localhost/AfyaAsilia/` in browser.
  - With Node (quick test): `npx http-server -c-1 -p 8080` then open `http://localhost:8080`.

**How to make common edits (examples)**
- Add/modify a product: edit the `products` array inside `index.html`. Fields: `id`, `name`, `category`, `price`, `desc_en`, `desc_sw`, `img`.
- Change product image: add file to `images/` and update `img` path in the `products` array.
- Change copy or add UI text: update the DOM element text and both localization branches in the `langSwitcher` handler.
- Update WhatsApp number: search `api.whatsapp.com/send?phone=` and replace the phone value in `index.html` for header, modal, and floating button links.

**Debugging tips & gotchas**
- Data source: `index.html` is the single source of truth — don't look for JSON or database files.
- Form behavior: `#contactForm` uses a frontend `setTimeout` to simulate sending; real message flow is WhatsApp links.
- LocalStorage: language selection persists to `localStorage` under `afya_lang`. Clear it during testing when language changes don't apply.
- CSS sources: Tailwind classes are used; inline styles and a small `<style>` block exist at top of `index.html` for component-specific rules.

**When adding features**
- Keep product rendering logic in the same file or extract carefully; ensure any moved code preserves localization logic and `getWhatsAppPrefill` behavior.
- If you extract product data to a separate JSON file, update filtering/rendering functions and ensure `getProductDesc` still resolves `desc_en`/`desc_sw` correctly.

**Optional: Serverless email (EmailJS) integration**
- A lightweight option to send emails from the contact form without relying on the user's mail client is EmailJS. The repo now includes a placeholder EmailJS integration in `index.html`.
- To enable: create an EmailJS account, add an email service and template, then set `EMAILJS_ENABLED = true` and fill `EMAILJS_SERVICE_ID`, `EMAILJS_TEMPLATE_ID`, and `EMAILJS_PUBLIC_KEY` in `index.html` (near the EmailJS SDK include).
- Behavior: when enabled the contact form sends via EmailJS; if it fails or is disabled it falls back to opening a `mailto:` link.

**References (start here)**
- Product & UI code: [index.html](index.html)
- Project summary: [README.md](README.md)

If anything above is unclear or you'd like me to expand with examples (e.g. automated build script, splitting `index.html` into modules), tell me which area to expand.
