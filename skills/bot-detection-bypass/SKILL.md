# 🤖 Bot Detection Bypass

*For when Cloudflare Turnstile, reCAPTCHA, or headless detection blocks your automation.*

## The Problem

Many sites detect and block headless browsers:
- Cloudflare Turnstile challenges
- reCAPTCHA v3 scoring you as a bot
- Navigator/WebDriver fingerprinting
- Missing browser features detection

Standard Puppeteer/Playwright gets blocked instantly.

## The Solution

Use `puppeteer-real-browser` — a wrapper that:
- Uses a real Chrome installation
- Patches common detection vectors
- Handles Turnstile automatically
- Maintains realistic fingerprints

### Installation

```bash
npm install puppeteer-real-browser
```

### Basic Usage

```javascript
const { connect } = require("puppeteer-real-browser");

async function main() {
  const { browser, page } = await connect({
    headless: false,  // Important: many detections check this
    turnstile: true,  // Auto-solve Cloudflare Turnstile
    fingerprint: true,
    args: ["--no-sandbox"]
  });

  await page.goto("https://target-site.com");
  
  // Do your automation...
  
  await browser.close();
}

main();
```

## Running on Headless Servers

The catch: `headless: false` requires a display. On servers, use `xvfb`:

```bash
# Install xvfb
apt-get install -y xvfb

# Run your script with virtual display
xvfb-run --auto-servernum --server-args='-screen 0 1920x1080x24' node your-script.js
```

### Full Example Script

```javascript
// kofi-login.js - Real-world example that passes Cloudflare
const { connect } = require("puppeteer-real-browser");

async function login(email, password) {
  const { browser, page } = await connect({
    headless: false,
    turnstile: true,
    fingerprint: true,
    args: ["--no-sandbox", "--disable-setuid-sandbox"]
  });

  try {
    await page.goto("https://ko-fi.com/account/login", { 
      waitUntil: "networkidle0",
      timeout: 30000 
    });
    
    // Wait for Turnstile to auto-solve (if present)
    await page.waitForTimeout(3000);
    
    // Fill credentials
    await page.type('input[type="email"]', email, { delay: 50 });
    await page.type('input[type="password"]', password, { delay: 50 });
    
    // Click login
    await page.click('button[type="submit"]');
    
    // Wait for navigation
    await page.waitForNavigation({ waitUntil: "networkidle0" });
    
    console.log("Login successful!");
    
  } finally {
    await browser.close();
  }
}

login("your@email.com", "yourpassword");
```

Run it:
```bash
xvfb-run --auto-servernum --server-args='-screen 0 1920x1080x24' node kofi-login.js
```

## Tips

1. **Add delays** — Real humans don't type at 1000 WPM
2. **Move the mouse** — Some detections track mouse movement
3. **Don't run parallel** — Multiple instances = suspicious
4. **Rotate user agents** — If doing multiple sessions
5. **Screenshots for debugging** — `await page.screenshot({path: 'debug.png'})`

## When This Won't Work

- Sites with advanced behavioral analysis (mouse patterns, timing)
- CAPTCHA that requires human solving (image selection)
- Sites that require phone verification
- Rate limiting (this bypasses detection, not rate limits)

## Dependencies

- Node.js 18+
- Chrome/Chromium installed
- xvfb (for headless servers)
- `puppeteer-real-browser` package

## Disclaimer

This technique is documented for educational purposes. Use responsibly and in compliance with applicable terms of service.
