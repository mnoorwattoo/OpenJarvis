# Marketing agents workspace

This fork adds 15 marketing agent templates (`src/openjarvis/agents/templates/`).
They research and draft, then hand finished work to a separate publishing bot
through this folder. See [HANDOFF.md](HANDOFF.md) for the outbox format.

| Agent ID                  | Name                            | Hands off                       |
|---------------------------|---------------------------------|---------------------------------|
| `social_media_researcher` | Social Media Researcher         | research reports                |
| `social_media_poster`     | Social Media Posting            | `publish_post`                  |
| `competitor_analyst`      | Competitor Analyst              | research reports                |
| `seo_master`              | SEO Master                      | `publish_post` (website)        |
| `lead_finder`             | Lead Finder (Skip Tracer)       | leads CSV (B2B, public data only) |
| `facebook_ads_expert`     | Facebook & Instagram Ads Expert | `launch_ad_campaign`            |
| `google_ads_expert`       | Google Ads Expert               | `launch_ad_campaign`            |
| `tiktok_ads_expert`       | TikTok Ads Expert               | `launch_ad_campaign`            |
| `linkedin_ads_expert`     | LinkedIn Ads Expert             | `launch_ad_campaign`            |
| `graphics_designer`       | Graphics Designer               | `create_asset`                  |
| `video_editor`            | Video Editor (Adobe)            | `create_asset`                  |
| `sales_agent`             | Sales Agent                     | `send_message`                  |
| `closer`                  | Closer                          | `send_message`                  |
| `appointment_setter`      | Appointment Setter              | `send_message`                  |
| `business_developer`      | Business Developer              | `send_message`                  |

## Setup

1. Copy this folder to where the agents and the publishing bot can both reach
   it, for example `~/OpenJarvis-Marketing`.
2. Point the file tools at it in `~/.openjarvis/config.toml`. Agents use
   relative paths, which resolve inside the first allowed directory:

   ```toml
   [security]
   allowed_dirs = ["/Users/you/OpenJarvis-Marketing"]
   ```

   Use the full absolute path. This setting also stops agents from reading or
   writing files anywhere else.
3. Fill in `brand/brand.md`, `brand/offers.md`, `brand/audience.md` and
   `brand/competitors.md`. The agents never invent prices, proof or results,
   so they can only be as good as these files.
4. Create an agent and run it:

   ```bash
   jarvis agent launch        # pick a template, give it an instruction
   jarvis agent list
   jarvis agent run <agent-id>
   ```

   `jarvis gui` also lists the templates.

## Notes

- Use a strong model. The small default local model is too weak for strategy
  and copywriting.
- `web_search` uses Tavily when `TAVILY_API_KEY` is set, otherwise DuckDuckGo.
  `graphics_designer` only generates images when an OpenAI key is configured
  for `image_generate`; without one it delivers specs and prompts.
- No agent publishes, sends or spends. Every ad campaign and first-contact
  message is marked `needs_human_review: true`.
