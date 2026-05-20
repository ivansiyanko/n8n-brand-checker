# Brand Consistency Checker: AI-Powered Visual QC for Brand Guidelines

Automatically validate AI-generated images against brand guidelines using GPT-4o vision — checks color palette, logo placement, typography, composition, and overall brand alignment. Flags off-brand assets before they reach the client. Built with **n8n** and **GPT-4o Vision**.

![n8n workflow](https://img.shields.io/badge/n8n-workflow-orange) ![AI Powered](https://img.shields.io/badge/AI-Powered-blue) ![license MIT](https://img.shields.io/badge/license-MIT-green)

## What It Does

This n8n workflow acts as an automated brand guardian, scoring every generated image against your brand guidelines:

- **Color Palette Check** — Extracts dominant colors from the image and compares against approved brand palette with tolerance thresholds
- **Logo & Placement** — Detects logo presence, size ratio, clear space, and positioning against brand zone rules
- **Typography Audit** — Checks for on-brand font usage, sizing hierarchy, and readability against background
- **Composition Analysis** — Validates product positioning, white space balance, visual hierarchy, and safe zones
- **Style Consistency** — GPT-4o vision compares the image against approved reference images for mood, lighting, and aesthetic match
- **AI Artifact Detection** — Flags common AI generation artifacts: distorted text, extra fingers, unnatural reflections, inconsistent shadows
- **Brand Score** — Calculates an overall brand alignment score (0-100) with detailed breakdown
- **Auto-Flag** — Assets scoring below threshold are flagged for revision with specific feedback

## What Gets Checked

| Check | What It Validates | Weight |
|-------|------------------|--------|
| **Color Palette** | Dominant colors match brand palette within Delta-E tolerance | 20% |
| **Logo Compliance** | Correct placement, size, clear space, version (light/dark) | 15% |
| **Typography** | Font family, size hierarchy, contrast ratio, alignment | 10% |
| **Composition** | Product placement, rule of thirds, visual balance, safe zones | 15% |
| **Style Match** | Mood, lighting, texture, and aesthetic vs. reference images | 20% |
| **AI Artifacts** | Distorted elements, unnatural features, rendering errors | 10% |
| **Technical Quality** | Resolution, sharpness, noise, color banding | 10% |

## How It Works

```
Webhook → Download Image → Extract Colors → GPT-4o Vision Analysis
→ Compare Against Brand Guide → Score & Report → Log → Notify
```

1. **Webhook Trigger** — Receives image URL, brand guide ID, reference image URLs, and context (campaign, channel, product)
2. **Download & Analyze** — Downloads the image, extracts dominant colors and technical metadata
3. **GPT-4o Vision** — Sends the image + brand guidelines + reference images to GPT-4o for comprehensive visual analysis
4. **Color Comparison** — Calculates Delta-E color distance between extracted colors and brand palette
5. **Scoring** — Combines all checks into a weighted brand alignment score with pass/fail per category
6. **Report** — Generates a detailed report with specific issues, suggested fixes, and visual annotations
7. **Log** — Saves results to Google Sheets for tracking brand consistency over time
8. **Notification** — Posts the brand check report to Slack with score, issues, and action items

## Quick Start

### Prerequisites
- n8n installed (self-hosted or cloud)
- OpenAI API key (GPT-4o with vision)
- Google Sheets credentials
- Slack webhook (optional)

### Setup
1. **Import the workflow** — Open n8n, go to Workflows → Import from File, select `workflow.json`
2. **Configure OpenAI** — Add your API key (GPT-4o vision access required)
3. **Set up brand guide** — Create a JSON brand guide file with your color palette (hex codes), logo specs, typography rules, and reference image URLs
4. **Set up Google Sheet** — Create a sheet named "Brand Checks" with columns: Date, Campaign, Asset, Channel, Score, Palette Score, Logo Score, Typography Score, Composition Score, Style Score, Artifacts, Issues, Status
5. **Activate the workflow** — Toggle to Active

### Usage

```bash
curl -X POST https://your-n8n-instance.com/webhook/brand-check \
  -H "Content-Type: application/json" \
  -d '{
    "image_url": "https://storage.example.com/generated/summer-ad-v3.jpg",
    "brand_guide_id": "luxury_beauty_2025",
    "reference_images": [
      "https://storage.example.com/brand/ref-hero-01.jpg",
      "https://storage.example.com/brand/ref-lifestyle-02.jpg"
    ],
    "campaign": "Summer Fragrance 2025",
    "product": "Eau de Soleil",
    "channel": "instagram_feed",
    "creator": "ivan@siyanko.com",
    "notify_channel": "#brand-review"
  }'
```

## Customization

- **Multi-Brand Support** — Store multiple brand guides and select by ID for agencies managing several clients
- **Approval Workflow** — Route flagged assets through a Slack approval flow with revision requests
- **Trend Analysis** — Track brand consistency scores over time to catch drift across campaigns
- **Batch Checking** — Feed it an entire campaign folder and get a consolidated brand consistency report
- **Custom Checks** — Add industry-specific validations (e.g., cosmetics packaging regulations, accessibility contrast ratios)

## License

MIT — free to use, modify, and share.

## Author

**Ivan Siyanko** — AI Automator
Website: [siyanko.com](https://siyanko.com) | GitHub: [@ivansiyanko](https://github.com/ivansiyanko)

---
Built with [n8n](https://n8n.io) — the workflow automation platform
