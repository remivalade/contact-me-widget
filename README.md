# Reusable "Made By" Module


Created as a **Web Component:** Best for any website - Hugo, Jekyll, WordPress, plain HTML, or even React/Vue.

Made by [Rémi Valade](https://linkedin.com/in/remivalade) · [𝕏](https://x.com/remivalade)

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
  name="Jane"
  subtitle="Designer & Developer"
  photo="https://your-server.com/photo.jpg"
  linkedin="janedoe"
  twitter="janedoe"
  tg="amFuZWRvZQ=="
  email-u="amFuZS5kb2U="
  email-d="ZXhhbXBsZS5jb20="
></made-by-widget>
```

### All Attributes

| Attribute | Default | Description |
|-----------|---------|-------------|
| `name` | "Rémi" | Display name in greeting |
| `subtitle` | "Senior Marketer who turns ambiguity into systems" | Tagline text |
| `label` | "Let's talk" | Text shown in collapsed state |
| `photo` | (relative to script) | Profile image URL (see below) |
| `linkedin` | - | LinkedIn username |
| `twitter` | - | X/Twitter username |
| `tg` | - | Telegram handle (base64 encoded) |
| `email-u` | - | Email username part (base64 encoded) |
| `email-d` | - | Email domain part (base64 encoded) |
| `cta-text` | "Send an email" | CTA button text |
| `source` | (auto: hostname) | Custom source for tracking |
| `dark` | (auto-detects) | Force dark mode |

### Profile Photo

If no `photo` attribute is provided, the widget looks for an image file named `remi-valade-square.jpg` next to the JS file. You can handle the photo in two ways:

**Option A: Self-host alongside the script**

Place your image in the same directory as `made-by-widget.js`. It will be resolved automatically.

```
your-server.com/widgets/made-by-widget.js
your-server.com/widgets/your-photo.jpg
```

**Option B: Host anywhere and pass the URL**

Host the image wherever you like (your server, Cloudflare Images, Imgur, etc.) and pass the URL as an attribute:

```html
<made-by-widget
  photo="https://wherever.com/your-photo.jpg"
  ...
></made-by-widget>
```

### Encoding Values (one-time setup)

Sensitive contact info uses base64 encoding to prevent bot scraping. To encode your values, open your browser console and run:

```javascript
btoa('janedoe')        // → "amFuZWRvZQ=="       (for tg)
btoa('jane.doe')       // → "amFuZS5kb2U="       (for email-u)
btoa('example.com')    // → "ZXhhbXBsZS5jb20="   (for email-d)
```

Or use the command line:
```bash
echo -n "janedoe" | base64
```

### Updating Your Contact Info

If you need to change your email or Telegram handle later:

1. **Encode the new value** using one of the methods above:
   ```javascript
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
   atob('amFuZWRvZQ==')  // → "janedoe"
   ```

### Usage in Hugo

```html
<!-- In layouts/partials/footer.html or layouts/_default/baseof.html -->
<script src="{{ "js/made-by-widget.js" | relURL }}"></script>
<made-by-widget
  name="Jane"
  photo="/images/photo.jpg"
  linkedin="janedoe"
  twitter="janedoe"
  tg="amFuZWRvZQ=="
  email-u="amFuZS5kb2U="
  email-d="ZXhhbXBsZS5jb20="
  source="hugo-blog"
></made-by-widget>
```

### Self-Hosting

1. Copy `made-by-widget.js` to your static assets folder
2. Optionally place your photo in the same folder (or pass a `photo` URL)
3. Reference it with a `<script>` tag
4. Add the `<made-by-widget>` element anywhere in your HTML

### CDN Hosting (for multiple sites)

Host the JS file on any CDN (jsDelivr, unpkg, Cloudflare, your own server) and reference it from all your sites:

```html
<script src="https://your-cdn.com/made-by-widget.js"></script>
```

### Security: Contact Obfuscation

Email and Telegram handles use **base64 encoding** to prevent bot scraping:
- Values are stored as encoded strings (e.g., `tg="amFuZWRvZQ=="`)
- Bots scanning for patterns like `@gmail.com` or `t.me/` won't find them
- The component decodes values only when the user clicks
- Not cryptographically secure, but stops 99% of automated scrapers
