# Chrome Web Store listing — copy-paste reference

Everything below is ready to paste into the Chrome Web Store Developer Dashboard for the "Web Monitor RSS - Visual Selector" item. Fields not listed here (category, language, visibility, distribution countries) are just dashboard dropdowns with no drafted text needed.

## Store listing tab

**Product name** (from manifest.json, do not retype): `Web Monitor RSS - Visual Selector`

**Summary** (132 char max):

```
Select any part of any webpage and get a private RSS feed when it changes. Self-hosted on your own Cloudflare Worker.
```

**Description** (detailed listing description):

```
Web Monitor RSS lets you point-and-click the exact part of any webpage you care about — a price, a stock-status line, the newest entry on a notices page — and get a private RSS feed item only when that specific thing actually changes.

HOW IT WORKS
Click the toolbar icon on any page, hover to highlight candidate elements, click to select one (or several, with labels), then save. That's it — the extension registers your selection with a backend you deploy yourself.

REQUIRES YOUR OWN DEPLOYMENT
This extension is the frontend half of an open-source, self-hosted project. It does not work standalone: you deploy the backend (Cloudflare Workers + D1, one click via a "Deploy to Cloudflare" button) to your own free Cloudflare account, then point this extension at it. There is no hosted service to sign up for, and the developer never sees your selections or your data — everything goes only to the backend URL you configure.
See: https://github.com/isitest1/web-monitor-rss

WHAT MAKES IT DIFFERENT
• Nothing has to stay running on your machine — checks run on GitHub Actions (free tier) or, if you prefer, a background tab in your own Chrome.
• A watchdog for the watchdog — an independent Cloudflare Cron job notices if the scheduled checker itself ever goes silent, and raises a separate alert feed.
• If a Selector stops matching a page, that's reported as an error — it never silently starts tracking a different element instead.
• No CAPTCHA or bot-detection evasion of any kind. If a site blocks automated access, that's reported honestly, not worked around.
• Minimal data retention by design — no full-page HTML, cookies, or login sessions are ever stored, only the specific values you selected.

WHAT IT DOESN'T DO
No multi-tenant accounts, no billing, no login-page monitoring, no macro recording, no AI content analysis. This is a personal tool for public pages, built and open-sourced by an individual developer.

Full source, setup guide, and technical spec: https://github.com/isitest1/web-monitor-rss
```

**Category**: Productivity

**Language**: English

## Graphic assets tab

- Store icon: uses the 128×128 icon from the extension package automatically (no separate upload needed unless the dashboard asks — if so, upload `assets/app-icon.png` from this repo).
- Screenshots (1280×800, upload in this order): `assets/screenshots/ss01_selector_overlay.png`, `assets/screenshots/ss02_multiple_selections.png`. (`ss03`/`ss04` in this repo are sized for the support page, not the Store's screenshot slot — see note below.)
- Small promo tile (440×280): `assets/promo_440x280.png`

**Note on ss03/ss04**: the popup and options screenshots in this repo are not 1280×800 — Chrome Web Store screenshots must be exactly 1280×800 or 640×400. If you'd like them included as official Store screenshots too, they need to be re-rendered at one of those two sizes (e.g. padded onto a 1280×800 canvas) — ask Claude to do this before submitting, or drop them from the Store listing and keep them on the support page only (they still show the extension well there).

## Privacy practices tab

**Single purpose description**:

```
Lets the user select a specific element on a webpage and registers it with a backend (deployed by the user) that periodically checks whether that element's content has changed, publishing changes as a private RSS feed.
```

**Permission justifications**:

| Permission | Justification |
|---|---|
| `storage` | Stores the extension's own configuration (the user's backend URL and API token) and short-lived per-tab selection state, entirely locally. |
| `activeTab` | Lets the toolbar-icon click inject the Visual Selector into the current tab, without requiring broader always-on access. |
| `scripting` | Injects the Visual Selector content script into the active tab when the user clicks the toolbar icon, and into a background tab for local-execution-mode Monitor checks. |
| `alarms` | Schedules periodic checks (per-Monitor interval, minimum 1 hour) for Monitors set to run locally in the browser rather than on the user's server-side runner. |
| `tabs` | Opens a hidden background tab to re-check a locally-scheduled Monitor's page, and opens/focuses the correct tab when the user chooses to edit an existing Monitor's selection. |
| `webNavigation` | Detects when a background tab (opened for a local Monitor check) has finished loading, so extraction only runs once the page is ready — or detects a failed navigation, so it can be reported as an error rather than silently skipped. |
| `webRequest` | Reads the HTTP response status code of a background tab's navigation, so a local check's result can be classified (success/HTTP error/etc.) the same way the server-side runner classifies results. Does not modify or block any request. |
| `host_permissions` (`http://*/*`, `https://*/*`) | The user can choose to monitor any public webpage, so the extension needs the ability to run on any site the user opens it on or configures for local checks. It never accesses a site the user hasn't explicitly opened the extension on or configured. |

**Data usage disclosure** (the dashboard's checkbox questionnaire): answer that the extension does **not** collect any of the listed categories (personally identifiable information, health info, financial info, authentication info, personal communications, location, web history, user activity, website content) **for the developer** — because everything the extension does collect (the values of elements the user selects) is sent only to a backend the user deploys and controls themselves, never to the developer. If the dashboard's wording forces a "website content" checkbox regardless (since the extension does read parts of pages), check it, and use this text for the required explanation field:

```
The extension reads only the specific page element(s) the user explicitly selects via the Visual Selector overlay. This data is sent only to a backend URL and API token the user configures themselves (their own self-hosted deployment) — it is never transmitted to, or received by, the developer. See the privacy policy for full detail.
```

**Privacy policy URL**:

```
https://webmonitorrss.margheritaworks.com/privacy.html
```

## Before submitting

- [ ] Confirm `webmonitorrss.margheritaworks.com` resolves (DNS CNAME added, GitHub Pages shows "Published" for `web-monitor-rss-support`).
- [ ] Upload the extension package: `pnpm --filter @web-monitor/extension build`, then zip the `apps/extension/dist/` folder contents (not the folder itself) and upload that zip.
- [ ] Double-check `apps/extension/dist/manifest.json`'s permissions match the justification table above (they will, unless permissions changed since this doc was written).
