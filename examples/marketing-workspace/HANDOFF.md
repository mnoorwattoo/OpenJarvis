# Outbox handoff spec (for the publishing bot)

The OpenJarvis marketing agents research and draft. They never publish, post,
send messages or spend money. A separate publishing bot does that by reading
`outbox/`.

## Where things are

| Folder      | What's in it                                                   |
|-------------|----------------------------------------------------------------|
| `brand/`    | Business facts the agents rely on. The owner keeps it current. |
| `research/` | Agent reports and working files, one subfolder per agent.      |
| `outbox/`   | Finished items, ready for the publishing bot to act on.        |
| `assets/`   | Images, videos and other files that outbox items refer to.     |

## Outbox item format

Each item is one Markdown file named `YYYY-MM-DD-<agent_id>-<slug>.md` that
starts with a YAML header:

```yaml
---
status: ready            # ready | published | rejected | failed
action: publish_post     # publish_post | launch_ad_campaign | send_message | book_meeting | create_asset | none
platform: instagram      # facebook | instagram | tiktok | linkedin | x | youtube | google_ads | email | sms | website | other
from_agent: social_media_poster
created: 2026-11-12
schedule: 2026-11-13 09:00 Asia/Karachi   # or "asap" or "unscheduled"
needs_human_review: false
---
```

After the header comes the exact final content, such as copy, hashtags,
targeting, budget, recipients or asset file names. The file ends with a
`Notes for publisher` section.

## Rules for the publishing bot

1. Only act on files where `status: ready`.
2. If `needs_human_review: true`, get the owner's explicit OK first. This
   applies to every `launch_ad_campaign` item and to every first-contact
   `send_message` item.
3. Never change the copy, budget or targeting without the owner's OK.
4. After acting, update the header: set `status` to `published`, `rejected` or
   `failed`, and add `published_at:` and `result:` (a link or an error). You can
   also move the file to `outbox/done/`.
5. `action: none` items are information only. Don't act on them.
6. `create_asset` items describe a file that has to be produced. Act on them
   only if you can make that asset, then save it under the `assets/` path the
   item names.
