# Reusable "Made By" Module


Created as a **Web Component:** Best for any website - Hugo, Jekyll, WordPress, plain HTML, or even React/Vue.

---

## Web Component (`made-by-widget.js`)

Works anywhere with a single `<script>` tag.

### Features

- **Shadow DOM encapsulation** - No CSS conflicts with your site
- **Glassmorphism effect** - Blur + transparency
- **Shine animation** - Sweep effect on the CTA button
- **Contact obfuscation** - Email and Telegram handles are not exposed in HTML
- **Auto source tracking** - Automatically detects hostname for analytics
- **Dark mode** - Auto-detects system preference or set manually

### Basic Usage

```html
<!-- Include the script (in head or before </body>) -->
<script src="path/to/made-by-widget.js"></script>

<!-- Use the component -->
<made-by-widget
  name="Rémi"
  linkedin="remivalade"
  twitter="remivalade"
  tg="cmVtaXZhbGFkZQ=="
  email-u="cmVtaS52YWxhZGU="
  email-d="Z21haWwuY29t"
></made-by-widget>
```

### All Attributes

| Attribute | Default | Description |
|-----------|---------|-------------|
| `name` | "Rémi" | Display name in greeting |
| `subtitle` | "Senior Marketer who likes to do things" | Tagline text |
| `label` | "Let's talk" | Text shown in collapsed state |
| `photo` | (default image) | Profile image URL |
| `linkedin` | - | LinkedIn username |
| `twitter` | - | X/Twitter username |
| `tg` | - | Telegram handle (base64 encoded) |
| `email-u` | - | Email username part (base64 encoded) |
| `email-d` | - | Email domain part (base64 encoded) |
| `cta-text` | "Send an email" | CTA button text |
| `source` | (auto: hostname) | Custom source for tracking |
| `dark` | (auto-detects) | Force dark mode |

### Encoding Values (one-time setup)

Sensitive contact info uses base64 encoding to prevent bot scraping. To encode your values, open your browser console and run:

```javascript
btoa('remivalade')      // → "cmVtaXZhbGFkZQ=="   (for tg)
btoa('remi.valade')     // → "cmVtaS52YWxhZGU="   (for email-u)
btoa('gmail.com')       // → "Z21haWwuY29t"       (for email-d)
```

Or use the command line:
```bash
echo -n "remivalade" | base64
```

### Updating Your Contact Info

If you need to change your email or Telegram handle later:

1. **Encode the new value** using one of the methods above:
   ```javascript
   // Example: changing to a new email
   btoa('new.email')     // → "bmV3LmVtYWls"
   btoa('newdomain.com') // → "bmV3ZG9tYWluLmNvbQ=="
   ```

2. **Update the attributes** in your HTML wherever the widget is used:
   ```html
   <made-by-widget
     ...
     email-u="bmV3LmVtYWls"
     email-d="bmV3ZG9tYWluLmNvbQ=="
   ></made-by-widget>
   ```

3. **To decode and verify** an existing value:
   ```javascript
   atob('cmVtaXZhbGFkZQ==')  // → "remivalade"
   ```

### Usage in Hugo

```html
<!-- In layouts/partials/footer.html or layouts/_default/baseof.html -->
<script src="{{ "js/made-by-widget.js" | relURL }}"></script>
<made-by-widget
  name="Rémi"
  linkedin="remivalade"
  twitter="remivalade"
  tg="cmVtaXZhbGFkZQ=="
  email-u="cmVtaS52YWxhZGU="
  email-d="Z21haWwuY29t"
  source="hugo-blog"
></made-by-widget>
```

### Self-Hosting

1. Copy `made-by-widget.js` to your static assets folder
2. Reference it with a `<script>` tag
3. Add the `<made-by-widget>` element anywhere in your HTML

### CDN Hosting (for multiple sites)

Host the JS file on any CDN (jsDelivr, unpkg, Cloudflare, your own server) and reference it from all your sites:

```html
<script src="https://your-cdn.com/made-by-widget.js"></script>
```

### Security: Contact Obfuscation

Email and Telegram handles use **base64 encoding** to prevent bot scraping:
- Values are stored as encoded strings (e.g., `tg="cmVtaXZhbGFkZQ=="`)
- Bots scanning for patterns like `@gmail.com` or `t.me/` won't find them
- The component decodes values only when the user clicks
- Not cryptographically secure, but stops 99% of automated scrapers

---

### Note

This version does not include the shine animation or contact obfuscation. Consider using the Web Component version instead.
