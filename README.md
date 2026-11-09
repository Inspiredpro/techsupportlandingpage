# TechSupport Africa – Enquiry Page

A single-page website (`index.html`) for TechSupport Africa with an enquiry form that sends submissions to an n8n webhook.

## Files

| File | Purpose |
|------|---------|
| `index.html` | The complete page: HTML, CSS and JavaScript in one file |
| `README.md` | This document |

## The form

The form has exactly three fields, all required:

| Field | Input type | JSON key |
|-------|-----------|----------|
| Name | text | `name` |
| Email | email | `email` |
| Enquiry | textarea | `enquiry` |

## Webhook

On submit, the page sends a `POST` request with a JSON body to:

```
https://inspiredpro.app.n8n.cloud/webhook-test/customer_enquiry
```

Example payload:

```json
{
  "name": "Amina Okafor",
  "email": "amina@example.com",
  "enquiry": "My laptop will not connect to Wi-Fi."
}
```

The page shows a success message when the webhook responds with a 2xx status, and an error message otherwise. The form clears after a successful send.

To change the address, edit the `WEBHOOK_URL` variable near the bottom of `index.html`.

## How to run

1. Open `index.html` in any modern browser, or upload it to any static host (Netlify, GitHub Pages, cPanel, etc.).
2. No build step or dependencies are needed. The only external resource is Google Fonts, and the page falls back to system fonts if it cannot load.

## n8n setup notes

- **Test URL:** `/webhook-test/` only works while the workflow is open in the n8n editor and you have clicked **Listen for test event**. Each test URL accepts one request per click. For live use, activate the workflow and switch the address to the production URL, which uses `/webhook/` instead of `/webhook-test/`.
- **HTTP method:** Set the Webhook node's method to `POST`.
- **CORS:** Because the form is sent from a browser, the Webhook node must allow your site's origin. In the Webhook node, open **Options → Allowed Origins (CORS)** and enter your site's address, or `*` while testing. Without this, the browser blocks the request and the page shows the error message.
- **Reading the data:** In n8n the fields arrive under `body`: `{{ $json.body.name }}`, `{{ $json.body.email }}` and `{{ $json.body.enquiry }}`.

## Customising

- **Branding:** Colours are CSS variables at the top of the `<style>` block (`--teal`, `--sun`, and so on). The page switches to a dark palette automatically when the visitor's device uses dark mode.
- **Copy:** Edit the headline, introduction and the three service descriptions directly in the HTML.
