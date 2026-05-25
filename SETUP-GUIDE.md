# Parapet AI — Launch Pipeline

> **Start date:** 2026-05-25  
> **Domain:** parapetai.dev  
> **GitHub:** github.com/parapet-ai  
> **Total cost:** ~€280 | **Total time:** ~2 hours

---

## Phase 1: Namecheap DNS Records

**Time:** 2 min | **Cost:** Free

1. Log into [Namecheap](https://www.namecheap.com)
2. Domain List → `parapetai.dev` → **Manage** → **Advanced DNS**
3. Delete any existing default records
4. Add these 5 records:

| Type | Host | Value | TTL |
|------|------|-------|-----|
| A Record | `@` | `185.199.108.153` | Automatic |
| A Record | `@` | `185.199.109.153` | Automatic |
| A Record | `@` | `185.199.110.153` | Automatic |
| A Record | `@` | `185.199.111.153` | Automatic |
| CNAME | `www` | `andrewhulab1.github.io` | Automatic |

5. Save changes. Wait 5–30 minutes for propagation.

**Verify:** `host parapetai.dev` should return GitHub IPs.

---

## Phase 2: Transfer Repo to Organization

**Time:** 3 min | **Cost:** Free

### 2a — Create the org

1. Go to https://github.com/account/organizations/new
2. Organization name: **`parapet-ai`**
3. Display name: **Parapet AI**
4. Free tier → Create

### 2b — Transfer the repo

1. Go to https://github.com/andrewhulab1/parapet-ai
2. Settings → Danger Zone → **Transfer ownership**
3. New owner: **`parapet-ai`**
4. Confirm → Transfer

### 2c — Update DNS CNAME (after transfer)

Change the CNAME record in Namecheap:

| Type | Host | Value |
|------|------|-------|
| CNAME | `www` | `parapet-ai.github.io` |

**Verify:** `https://parapetai.dev` shows landing page.

---

## Phase 3: Verify & Harden Site

**Time:** 2 min | **Cost:** Free

1. Visit `https://parapetai.dev` — landing page should load
2. Go to https://github.com/parapet-ai/parapet-ai.github.io
3. Settings → Pages → check **Enforce HTTPS** (available after DNS resolves)
4. Confirm green checkmark: "Your site is published at https://parapetai.dev"

**Verify:** `curl -sI https://parapetai.dev` returns `HTTP/2 200`

---

## Phase 4: Polish Trademark (UPRP)

**Time:** 30 min | **Cost:** ~€200

### 4a — Prepare the application

```
Applicant:    [Your name / business]
Mark:         PARAPET AI (word mark, standard characters)
Type:         Word mark
```

### 4b — Nice Classification goods/services

**Class 9 — Downloadable Software:**
- Downloadable computer software for local AI model execution
- Software for container-based artificial intelligence assistants
- Security-hardened downloadable AI agent software
- Software for offline large language model interaction
- Computer software featuring egress-filtered AI communication

**Class 42 — Software Services:**
- Software as a Service (SaaS) for local artificial intelligence
- Computer security consultancy
- Design and development of computer software for AI security
- Providing temporary use of non-downloadable security-hardened AI software
- Computer security threat analysis and risk assessment for AI systems

### 4c — File at UPRP

1. Go to https://ewyszukiwarka.pue.uprp.gov.pl
2. File electronically (cheaper than paper)
3. Pay fees:

| Stage | Fee (PLN) | Fee (EUR) |
|-------|-----------|-----------|
| Filing — 1st class | 400 | ~€90 |
| Filing — 2nd class | +100 | ~€24 |
| Publication | 90 | ~€20 |
| 10-year protection (×2 classes) | 800 | ~€180 |
| **TOTAL** | **~1,390 PLN** | **~€314** (2 classes) |

*Filing + publication paid now. Protection fee paid after approval (~4-6 months later).*

### 4d — Priority date

Your filing date in Poland becomes your **Paris Convention priority date** for all other jurisdictions. Within 6 months, you can file EUIPO (€850, 27 EU states) and USPTO (€300, US) claiming this date.

---

## Phase 5: Move Airlock Code to Repo

**Time:** 1 hour | **Cost:** Free

### 5a — New repo structure

```
parapet-ai/                        (GitHub: parapet-ai/parapet-ai)
├── README.md                      (project overview + security model)
├── LICENSE                        (MIT / Apache 2.0 dual)
├── .github/
│   └── workflows/
│       └── deploy.yml             (optional CI/CD)
├── agent-container/
│   ├── nanoclaw_agent.py          (→ rename to parapet_agent.py)
│   ├── Dockerfile
│   ├── build.sh
│   └── requirements.txt
├── docker-compose.yml
├── Corefile                        (CoreDNS config)
├── seccomp-profile.json
├── iptables-egress.sh
├── deploy-airlock.sh              (→ rename to deploy.sh)
├── setup.sh
├── docs/
│   ├── index.html                  (landing page, served at parapetai.dev)
│   ├── CNAME                       (→ /docs/CNAME for Pages)
│   ├── security-model.md
│   └── architecture.md
└── tools/                          (plugin system tools)
```

### 5b — Rename internal references

| Old name | New name |
|----------|----------|
| `nanoclaw_agent.py` | `parapet_agent.py` |
| `nanoclaw_workspace` | `parapet-ai` |
| `deploy-airlock.sh` | `deploy.sh` |
| "DeepSeek-Kali Airlock" | "Parapet AI" |
| `/workspace/inbox/` | keep — generic path |
| Docker image: `deep-agent` | `parapet-agent` |

### 5c — Push

```bash
cd /home/kali/parapet-ai
git init
git add -A
git commit -m "Initial import: Parapet AI hardened local AI stack"
git remote add origin https://github.com/parapet-ai/parapet-ai.git
git push -u origin main
```

### 5d — Update GitHub Pages source

Repo Settings → Pages → source: `main` branch, `/docs` folder.

---

## Phase 6: Insurance Domains

**Time:** 5 min | **Cost:** ~€80/yr

Register these to prevent squatting:

| Domain | Registrar | ~Cost/yr | Purpose |
|--------|-----------|----------|---------|
| `parapetai.io` | Namecheap | ~€40 | Redirect to parapetai.dev |
| `ferrokeep.dev` | Namecheap | ~€12 | Backup name, redirect |

Set both to 301 redirect → `parapetai.dev`.

---

## Phase 7: Social & Discoverability (Optional)

**Time:** 30 min | **Cost:** Free

- Register `@parapet_ai` on Twitter/X
- Set up repo description + tags: `ai`, `ollama`, `docker`, `privacy`, `local-llm`, `security`
- Add to GitHub topics: `local-ai`, `offline-ai`, `self-hosted`, `air-gapped`
- Submit to: HN, Reddit r/LocalLLaMA, r/selfhosted

---

## Checklist

- [x] Phase 1: Namecheap DNS records added
- [x] Phase 1: `host parapetai.dev` resolves to GitHub IPs
- [x] Phase 2: `parapet-ai` GitHub org created
- [x] Phase 2: Repo transferred to org
- [x] Phase 2: DNS CNAME updated to `parapet-ai.github.io`
- [x] Phase 3: `https://parapetai.dev` loads landing page
- [ ] Phase 3: HTTPS enforced in repo settings *(GitHub cert provisioning — auto)*
- [x] Phase 4: UPRP trademark filed — **Z.603439** (2026-05-25)
- [x] Phase 5: MIT edition code imported → `github.com/parapet-ai/parapet`
- [x] Phase 5: README bilingual, badges, legal docs, regulatory alignment
- [x] Phase 6: `parapetai.io` registered + redirect ✅
- [ ] Phase 6: `ferrokeep.dev` registered — DNS redirect pending
- [ ] Phase 7: Social handles reserved

### IP Status

| ID | Type | Priority Date | Status |
|----|------|---------------|--------|
| P.455821 | Patent | 2026-05-18 | Pending |
| Z.603439 | Trademark | 2026-05-25 | Pending |
| Paris Convention deadline | — | **2026-11-25** | File EUIPO/USPTO by this date |
