<p align="center">
  <img src="docs/images/dashboard.png"
       alt="Recruitment system recruiter dashboard with candidate scoring and application management"
       width="1200">
</p>

> 🇬🇧 English | [🇫🇷 Français](./README_FR.md)

![License](https://img.shields.io/badge/License-LICENSE.md-lightgreen.svg)
![PHP](https://img.shields.io/badge/PHP-777BB4?style=flat)
[![Recruitment System](https://img.shields.io/badge/Recruitment-System-2ea44f?style=flat-square)](https://palks-studio.com/en/recruitment-without-saas)
[![View the system](https://img.shields.io/badge/Palks%20Studio-Recruitment%20system-0095b1?style=for-the-badge)](https://palks-studio.com/en/recruitment-without-saas)

<p align="center">
  <a href="https://palks-studio.com">
    <img src="https://img.shields.io/badge/Palks%20Studio-Website-0095b1?style=for-the-badge" />
  </a>
</p>

# Palks Studio — Autonomous recruitment system  
**Structured recruitment with automatic candidate scoring, application management and autonomous server deployment**

> This repository is a technical presentation and documentation repository.  
> It does not contain downloadable source code or production files.

This README documents design principles and system architecture.  
It intentionally avoids operational procedures and sensitive details.

---

## Overview

> 🇫🇷 French version available — English coming soon.
> Automatic matching recruitment system — PHP 8.x, no database, no SaaS.

CANDIDATE_SYSTEM is an autonomous recruitment engine deployable on any standard Apache / PHP 8.x hosting.

The applicant fills out a structured form. Their answers are automatically scored against a job profile defined by the recruiter. The dashboard displays applications ranked by matching score — from most to least relevant.

No database. No SaaS. No subscription. Data stays on the client's server.

---

## Project Structure

```
candidate_system/
│
├── public/
│  ├── panel.php                      → Administrator interface
│  ├── record.php                     → Detailed application view
│  ├── finalize.php                   → Campaign closing and cleanup
│  ├── success.php                    → Post-submission confirmation page
│  ├── overview.php                   → Recruiter dashboard (restricted access)
│  ├── file.php                       → Secure uploaded file download
│  ├── extract.php                    → CSV generation
│  ├── form.php                       → Application form
│  └── process.php                    → Processing, validation, scoring, storage
│
└── core/
   ├── settings/
   │   ├── init.php                   → Centralized absolute paths
   │   └── profile.php                → User data
   │
   ├── presets/
   │   ├── fields.json                → Default question template
   │   └── template.php               → Default profile template
   │
   ├── storage/                       → Campaign storage
   │   └── batch_1/
   │        ├── settings/
   │        │   ├── fields.json       → Questions and scoring configuration
   │        │   └── template.php      → Job profile configuration
   │        │
   │        └── records/
   │            ├── entries/          → JSON applications
   │            ├── files/            → Uploaded CVs and documents
   │            ├── archives/         → (reserved)
   │            ├── logs/             → (reserved)
   │            └── closed.lock       → Campaign closure lock
   │
   ├── engine.php                     → Scoring engine
   ├── notify.php                     → Acknowledgement email sender
   ├── LICENCE.md                     → Terms of use and legal framework
   │
   └── docs/
       ├── USER_GUIDE.md              → User guide
       └── README_EN.md               → Technical documentation
```


---

## Features

- Multi-section application form with conditional fields  
- Automatic multi-criteria scoring (section weights, question weights, per-answer scores)  
- Configurable penalty rules on specific answers  
- Password-protected recruiter dashboard  
- Unlimited multi-campaign management — each campaign is fully independent  
- Detailed applicant view with per-section scores and progress bars  
- Tech stack display with matched skills highlighted  
- CSV export compatible with Excel (UTF-8 BOM, `;` separator)  
- Full administration interface — no technical knowledge required  
- Automatic emails: confirmation receipt and campaign closure notification  
- Campaign closure and reopening from the dashboard  
- Unique form link per campaign, copyable in one click

[![View the system](https://img.shields.io/badge/Palks%20Studio-Recruitment%20system-0095b1?style=for-the-badge)](https://palks-studio.com/en/recruitment-without-saas)

---

## Requirements

- PHP 8.x  
- Apache with `mod_rewrite` enabled  
- FTP or SSH access  
- No database  
- No external dependencies (no Composer, no npm)

---

## Scoring Logic

The final score is calculated across three nested levels:

```
contribution = scores[answer] × (weight / 100) × (global_weight / 100)
```


| Level | Parameter       | Description                               |
|-------|-----------------|-------------------------------------------|
| 1     | `global_weight` | Section weight in the final score         |
| 2     | `weight`        | Question weight within its section        |
| 3     | `scores`        | Raw score assigned to each answer         |

**Default distribution:**

| Section              | Global weight |
|----------------------|---------------|
| Field experience     | 60%           |
| Classic experience   | 20%           |
| Tech stack           | 15%           |
| Availability         | 5%            |

Weights are fully reconfigurable from the administration interface.

**Score levels:**

| Score | Label       |
|-------|-------------|
| ≥ 80  | Excellent   |
| ≥ 60  | Good fit    |
| ≥ 40  | Partial     |
| < 40  | Insufficient|

---

## Security

- Strict public / private separation — no config files accessible via the web  
- CSRF protection on the application form  
- HTTP security headers on all pages  
- Full sanitization of user inputs  
- Upload validation: PDF only, 5 MB maximum  
- Session required for all administration pages  
- Email deduplication per campaign  
- `Options -Indexes` enabled on the public directory

---

## Data Storage

Each application is stored as a JSON file:

```json
{
    "id": "20260514_143000_abc123",
    "date": "2026-05-14 14:30:00",
    "poste": "Job title",
    "prenom": "First name",
    "nom": "Last name",
    "email": "email@applicant.com",
    "score_final": 74.5,
    "score_label": "Good fit",
    "score_detail": {},
    "reponses": {},
    "trigger_fields": {}
}
```


---

© Palks Studio — see LICENSE.md  
- https://palks-studio.com
