# Frequently Asked Questions

> Quick answers to the most common questions about **PIIK.ME**.

---

## Table of Contents

- [General](#general)
- [Setup & Installation](#setup--installation)
- [Features](#features)
- [Contributing](#contributing)
- [Self-Hosting & Privacy](#self-hosting--privacy)
- [Related Documentation](#related-documentation)

---

## General

**What is PIIK.ME?**

PIIK.ME is an open-source link intelligence platform. It lets you shorten URLs, create personalized bio link pages, and track real-time analytics — including click counts, device breakdowns, referrer sources, and more. Think of it as a self-hostable alternative to Bitly with bio links similar to Linktree.

---

**Is PIIK.ME free?**

Yes. The source code is licensed under GPL-3.0 and is free to use, modify, and self-host. You will need a free Firebase project for authentication and database storage.

---

**Do I need an account to use short links?**

You need a Google account to create and manage short links. Clicking or sharing a short link requires no account.

---

**What technologies does PIIK.ME use?**

The backend runs on Node.js with Express.js and Socket.IO. Authentication and the database are handled by Firebase (Google Auth + Firestore). The frontend is plain HTML, CSS, and Vanilla JavaScript, with Three.js and Globe.gl for visual effects. See the full stack in [DEVELOPMENT.md](./DEVELOPMENT.md).

---

## Setup & Installation

**What are the minimum requirements to run PIIK.ME locally?**

- Node.js v14 or higher
- npm (bundled with Node.js)
- A free Firebase project with Google Auth and Firestore enabled

---

**Where do I get my Firebase credentials?**

1. Create a project at [console.firebase.google.com](https://console.firebase.google.com).
2. Go to **Project Settings → Service accounts → Generate new private key**.
3. Copy `project_id`, `client_email`, and `private_key` into your `.env` file.
4. Go to **Project Settings → General → Your apps** and add a Web App to get the client-side config for `public/js/firebase-config.js`.

A full walkthrough is available in [docs/FIREBASE_SETUP.md](https://github.com/xthxr/piik.me/blob/main/docs/FIREBASE_SETUP.md).

---

**The app starts but Google Sign-In fails. What's wrong?**

Your domain is probably not in Firebase's authorized list. Go to **Firebase Console → Authentication → Settings → Authorized domains** and add `localhost` (for development) or your production domain.

---

**Why does the server crash with a Firebase private key error?**

When you paste a private key into `.env`, the literal string must use `\n` to represent newlines — not actual line breaks. The value should look like:

```
FIREBASE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\nMIIE...\n-----END PRIVATE KEY-----\n"
```

---

**Can I deploy PIIK.ME on platforms other than Vercel?**

Yes. PIIK.ME is a standard Express.js application. It can run on any platform that supports Node.js — Railway, Render, Fly.io, a VPS, and so on. The `vercel.json` file is only needed for Vercel deployments.

---

## Features

**How does real-time analytics work?**

PIIK.ME uses Socket.IO to maintain a persistent WebSocket connection between the browser and the server. When someone clicks a short link, the server records the event in Firestore and immediately broadcasts an `analyticsUpdate` event to any open analytics dashboard for that link — no page refresh required.

---

**What analytics data is collected per click?**

Each click records the timestamp, device type (mobile or desktop), browser (Chrome, Firefox, Safari, Edge, or other), and the HTTP referrer (the website that sent the visitor). No personally identifiable information is stored.

---

**Can I use a custom domain for my short links?**

Custom domains are not currently supported out of the box. You can self-host PIIK.ME on your own domain and set `BASE_URL` accordingly. Full custom domain support is planned — see [ROADMAP.md](./ROADMAP.md).

---

**What is a "verified badge" on a bio page?**

Verified badges (blue checkmarks) are awarded to early adopters of the platform. They are set manually by the project maintainer using the `scripts/set-verified-badges.js` utility. There is currently no automated verification process.

---

**Is there a limit on how many short links I can create?**

There is no hard limit in the application code. Practical limits are determined by your Firestore plan (Firebase's free Spark plan has generous quotas for most personal and small-project use cases).

---

**Does PIIK.ME support link expiry or password protection?**

Not currently. Both features are on the roadmap. See [ROADMAP.md](./ROADMAP.md) for the planned timeline.

---

**Can I export my analytics data?**

There is no built-in export feature yet. As a workaround, you can access your data directly in the Firestore console or build a script that reads from the `analytics` collection using the Firebase Admin SDK.

---

## Contributing

**How do I contribute to PIIK.ME?**

Read [CONTRIBUTING.md](https://github.com/xthxr/piik.me/blob/main/CONTRIBUTING.md) for the full guide. The short version: fork the repo, create a feature branch, make your changes, and open a Pull Request.

---

**I found a bug. Where do I report it?**

Open a [GitHub Issue](https://github.com/xthxr/piik.me/issues/new/choose) using the Bug Report template. Please include steps to reproduce, expected vs. actual behavior, and your environment details.

---

**How are Pull Requests reviewed?**

The maintainer reviews all PRs. PRs with a clear description, focused scope, and no unrelated changes are merged fastest. Please make sure your code follows the style of the existing codebase and that there are no merge conflicts with `main`.

---

**Can I add a new dependency?**

Yes, but please discuss significant new dependencies in your PR description or in a GitHub Discussion first. Prefer well-maintained, minimal packages and avoid duplicating functionality already present in the stack.

---

## Self-Hosting & Privacy

**What data does PIIK.ME store?**

PIIK.ME stores the links you create, per-click analytics (timestamp, device, browser, referrer), and bio link profiles (username, display name, bio, links). No passwords are stored — authentication is delegated entirely to Firebase/Google.

**Is data shared with third parties?**

PIIK.ME itself does not sell or share data. However, it relies on Firebase (Google) for auth and storage, which is subject to Google's privacy policy.

---

## Related Documentation

| Document | Description |
|---|---|
| [CONTRIBUTING.md](https://github.com/xthxr/piik.me/blob/main/CONTRIBUTING.md) | How to contribute code, report bugs, and open PRs |
| [DEVELOPMENT.md](./DEVELOPMENT.md) | Local setup, architecture, API reference, and coding standards |
| [ROADMAP.md](./ROADMAP.md) | Planned features and the future direction of the project |
| [docs/FIREBASE_SETUP.md](https://github.com/xthxr/piik.me/blob/main/docs/FIREBASE_SETUP.md) | Step-by-step guide to configuring Firebase for PIIK.ME |

---

*Last updated: 2026 · PIIK.ME open-source project*
