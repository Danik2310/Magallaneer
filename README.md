# Magallaneer — Cryptocurrency Project Web Platform

> Corporate web presence for a Solana-based cryptocurrency project ($MAGAL). Features a real-time token dashboard powered by the DexScreener API, an Airdrop user acquisition system, an automated email/CRM microservice, a user portal with Solana wallet verification, and a private admin dashboard — all backed by a custom PHP API and MySQL database.

** Private codebase — developed under client confidentiality agreement.**

[![Live Demo](https://img.shields.io/badge/🌐_Live_Demo-magallaneer.io-f59e0b?style=for-the-badge)](https://magallaneer.io)
[![API](https://img.shields.io/badge/API-api.magallaneer.io-8b5cf6?style=for-the-badge)](https://api.magallaneer.io)

---

## Project Overview

Magallaneer is the official web platform for a Solana-based meme/utility crypto token ($MAGAL). The site serves as both a public-facing investor hub and an internal campaign management tool. It includes live on-chain token data, a full Airdrop registration system that validates Solana wallet balances via the Helius RPC API, an automated email engine for campaign notifications, and a private dashboard for registered users to track their referrals.

Built end-to-end as a solo fullstack project — from React frontend to a custom PHP + MySQL backend with Cron job automation, Firebase integration, and on-chain Solana data reads.

---

## Key Features

### Public Platform
- **Live token dashboard** — Real-time $MAGAL price, 24h change, volume, market cap, and liquidity fetched from the DexScreener API every 30 seconds via `swr` with auto-refresh
- **Price chart** — Interactive price history chart built with Recharts, pulling OHLCV data from DexScreener
- **Direct buy/trade links** — Integrated links to DexScreener swap and Solscan token explorer
- **Roadmap, Mission, Press, and Whitepaper** — Full investor information pages
- **Game download section** — Dedicated page for the project's associated game (`/Air-Treasures`)
- **Technical SEO** — Canonical URLs, Open Graph, PWA manifest, robots.txt

### Airdrop & User Acquisition System
- **Treasure Callers registration** (`/treasure-callers`) — Airdrop form collecting Twitter/X handle, Telegram username, Solana wallet address, email, and pirate alias, with Zod schema validation and Solana wallet format enforcement
- **Treasure Seekers registration** (`/treasure-seekers`) — Extended registration with on-chain wallet balance verification via Helius RPC API before submission is accepted
- **On-chain token balance check** — Validates that the user's Solana wallet holds $MAGAL tokens before allowing Airdrop participation
- **Automated registration emails** — PHP backend sends confirmation emails via PHPMailer on successful submission
- **Airdrop ranking algorithm** — Proprietary backend logic that classifies users by investment volume and generates personalized rankings for targeted Airdrop reward distribution

### Automated Email / CRM Microservice
- **Mass notification engine** — PHP + Cron job system that sends automated campaign emails to all registered users
- **SMTP queue management** — Custom queuing system that batches sends (50 emails every 4 hours) to respect server SMTP limits and maintain high deliverability, avoiding spam filters
- **Targeted communications** — Personalized emails generated per user tier based on the ranking algorithm

### User Portal & Dashboard
- **Login system** — Alias + password authentication against the PHP backend (`/api/login.php`)
- **Session management** — Session stored in `localStorage` as a `pirateUser` object; protected routes redirect to login if session is absent
- **Password change** — In-dashboard password change dialog with current password verification (`/api/change_password.php`)
- **Password reset flow** — Token-based password recovery via email (`/api/reset-password.php`)
- **Referral dashboard** — Authenticated users can view their referral stats and history
- **Logout** — Server-side session invalidation via `POST /api/logout.php`

---

## Tech Stack

### Frontend
![React](https://img.shields.io/badge/React_18-61DAFB?style=flat&logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-646CFF?style=flat&logo=vite&logoColor=white)
![TailwindCSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=flat&logo=tailwind-css&logoColor=white)
![Framer Motion](https://img.shields.io/badge/Framer_Motion-0055FF?style=flat&logo=framer&logoColor=white)

### UI & Components
![shadcn/ui](https://img.shields.io/badge/shadcn%2Fui-000000?style=flat&logo=shadcnui&logoColor=white)
![Radix UI](https://img.shields.io/badge/Radix_UI-161618?style=flat&logo=radix-ui&logoColor=white)
![Recharts](https://img.shields.io/badge/Recharts-22C55E?style=flat)

### Key Libraries
| Library | Purpose |
|---|---|
| `swr` | Data fetching with auto-refresh for live DexScreener data |
| `@solana/web3.js` | Solana wallet address validation and on-chain token balance reads |
| `@solana/wallet-adapter-*` | Solana wallet connection infrastructure |
| `framer-motion` | Page and section entrance animations |
| `react-hook-form` + `zod` | Schema-based form validation with Solana wallet format rules |

| `@tanstack/react-query` | Server state management |
| `recharts` | Price history chart rendering |

### External APIs
- **DexScreener API** — Live token price, volume, market cap, liquidity, and OHLCV data for $MAGAL on Solana
- **Helius RPC API** — Solana mainnet RPC for on-chain token balance verification before Airdrop registration
- **Solscan** — Token explorer links for investor transparency

### Backend
![PHP](https://img.shields.io/badge/PHP-777BB4?style=flat&logo=php&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white)

Custom PHP REST API with MySQL and automated Cron jobs:

| Endpoint / Service | Purpose |
|---|---|
| `POST /api/register.php` | Airdrop user registration + email confirmation trigger |
| `POST /api/login.php` | User authentication |
| `POST /api/logout.php` | Server-side session invalidation |
| `POST /api/change_password.php` | Authenticated password update |
| `POST /api/reset-password.php` | Token-based password recovery via email |
| `Cron: email_campaign.php` | Mass campaign emails — 50 per batch, every 4 hours |
| `Cron: ranking.php` | Recalculates user investment rankings for Airdrop targeting |

### Infrastructure & Deploy
![Namecheap](https://img.shields.io/badge/Namecheap-DE3723?style=flat&logo=namecheap&logoColor=white)
![cPanel](https://img.shields.io/badge/cPanel-FF6C2C?style=flat&logo=cpanel&logoColor=white)

- Hosted on **Namecheap with cPanel**
- **SSL** on main domain and API subdomain
- **Cron jobs** configured for email campaign scheduling and ranking recalculation
- **`.htaccess`** for SPA routing and API CORS configuration
- **Domain and server security** hardened for personal and financial data storage

---

## Project Architecture

```
Frontend (React + Vite)              magallaneer.io
src/
├── components/
│   ├── LiveDexScreener    # Real-time token stats (swr, 30s refresh)
│   ├── PriceChart         # Interactive OHLCV chart (Recharts + DexScreener)
│   ├── DashboardHeader    # User dashboard nav with password change dialog
│   ├── Hero / Mission / Roadmap / Press / CoinInfo / Contact / Footer
│   └── SEOHead            # Dynamic meta and Open Graph tags
├── pages/
│   ├── Index              # Public landing page
│   ├── TreasureCallers    # Airdrop registration form (Zod + Solana validation)
│   ├── TreasureSeekers    # Extended registration with on-chain balance check
│   ├── Subscribe          # Helius RPC wallet verification + PHP registration
│   ├── Dashboard          # Authenticated referral dashboard
│   ├── Login              # Alias + password auth
│   ├── PasswordReset      # Token-based recovery flow
│   ├── AirTreasures       # Game download page
│   ├── Whitepaper         # Full investor whitepaper page
│   └── Legal/             # Terms and privacy policy
└── hooks/

Backend (PHP + MySQL + Cron)         api.magallaneer.io
api/
├── register.php           # User registration + email trigger
├── login.php              # Auth + session creation
├── logout.php             # Session invalidation
├── change_password.php    # Authenticated password update
├── reset-password.php     # Recovery token flow
├── email_campaign.php     # Batched mass mailer (Cron, 50 per 4h)
└── ranking.php            # Investment-based ranking algorithm (Cron)
```

---

## Web3 & Blockchain Integration

**On-chain wallet balance verification**
Before allowing a user to complete Airdrop registration, the platform calls the Helius RPC API (`mainnet.helius-rpc.com`) with the user's Solana public key to verify they hold $MAGAL tokens. If the balance check fails, registration is blocked — preventing fake or empty wallets from claiming Airdrop rewards.

**Solana wallet validation**
All registration forms enforce Solana wallet address format via Zod: base58 encoding, 32–44 character length, valid character set, and `PublicKey.isOnCurve()` check from `@solana/web3.js` before any API call is made.

**DexScreener live data**
The token dashboard fetches `price`, `priceChange24h`, `volume`, `marketCap`, and `liquidity` directly from DexScreener's REST API using the $MAGAL/SOL pair address on Solana, refreshing every 30 seconds automatically.

---

## Email Automation & CRM

The backend implements a complete email automation microservice for Airdrop campaigns:

- **Registration confirmation** — Triggered immediately on successful `/api/register.php` call via PHPMailer
- **Mass campaigns** — A dedicated Cron job runs `email_campaign.php` every 4 hours, sending batches of 50 emails to respect SMTP server limits and avoid spam classification
- **Ranking-based targeting** — The `ranking.php` Cron job recalculates user tiers based on investment volume, enabling personalized communications for Airdrop reward winners vs. general participants

---

## Technical Challenges

**On-chain Airdrop eligibility gate**
Integrated the Helius RPC API to read live Solana token balances client-side before submitting registration. This required handling async wallet lookups, edge cases for wallets with no token account, and graceful error UX when the RPC call fails.

**SMTP deliverability under volume**
Direct bulk email sending triggers spam filters. Designed a custom PHP queuing system that processes registrations in controlled batches (50 emails per 4-hour Cron cycle), respecting server SMTP rate limits while ensuring all users receive timely confirmations.

**Airdrop ranking algorithm**
Built a proprietary MySQL-based ranking engine that sorts users by token investment volume, assigns them to reward tiers, and feeds the targeted email system — ensuring Airdrop distributions are proportional and verifiable.

**Real-time crypto data without a paid provider**
Used the free DexScreener API with `swr`'s `refreshInterval` for polling instead of a paid WebSocket data feed. Handled edge cases including API unavailability, missing pair data, and stale price display with graceful fallback states.

---

## My Role

**Fullstack — sole developer.** I handled every layer:

- UI design and full frontend implementation
- Web3 integration: Solana wallet validation and on-chain balance reads via Helius RPC
- DexScreener API integration for live token data and price chart
- Custom PHP REST API design and development (auth, registration, password flows)
- MySQL database schema and query design
- Automated email/CRM microservice with PHPMailer and Cron job scheduling
- Airdrop ranking algorithm design and implementation
- Namecheap server setup, SSL configuration, Cron job scheduling, and cPanel deployment
- Domain and server security hardening

---

## Timeline

**~2 months** of development (November – December 2025).

---

## Contact

Interested in a live demo or more details about this project?

[![Portfolio](https://img.shields.io/badge/Portfolio-melvin--dev.vercel.app-f59e0b?style=for-the-badge)](https://melvin-dev.vercel.app)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Melvin_Añez-0077B5?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/melvin-a%C3%B1ez-berrio-897168164/)
[![GitHub](https://img.shields.io/badge/GitHub-danik2310-181717?style=for-the-badge&logo=github)](https://github.com/danik2310)

---

*Developed under client confidentiality agreement. Source code is not publicly available.*
