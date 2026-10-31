# again. — Challenge Ideas Page

A single-purpose landing page: TikTok viewers submit rejection challenge ideas for Josh to try on camera.

## Goal
Validate interest in "again." cheaply. If people bother to send ideas, the concept has pull.

## Scope
- Hero: "Send Josh a rejection." + 3 phone mockups (2 TikToks, 1 app tracker screenshot)
- Social proof counter above the form ("X people have sent Josh a rejection"), hidden until 5+
- Free-form text area (500 char max) → submit → thank-you state with "Send another"
- Error toast if submit fails
- Footer link to TikTok
- Not in scope: SEO, accounts, about page, analytics beyond what Vercel gives free

## Stack
| Layer | Choice | Why |
|---|---|---|
| Frontend | One static `index.html` (HTML/CSS/JS, no build step) | Nothing to compile, easy to edit in GitHub |
| Hosting | Vercel (free Hobby plan), connected to a GitHub repo | Push to `main` = live in ~30 sec |
| Backend | Supabase (free plan): Postgres table + one counter function | Stores ideas, powers the counter, has a built-in table viewer |
| Spam | Honeypot field + 500-char limit + DB length check | Enough for this traffic level |

## Security model
The Supabase "anon" key lives in the page and is meant to be public. Row Level Security means visitors can only **insert** ideas. They can't read anyone else's submissions. The counter runs through a function that returns only the number.

## Setup (≈20 min)
1. **Supabase**: create a project → SQL Editor → paste `schema.sql` → Run.
2. Project Settings → API → copy **Project URL** and **anon public key** into the two constants at the top of the `<script>` in `index.html`.
3. Replace `YOUR_HANDLE` in the footer TikTok link.
4. Add screenshots to an `img/` folder (`tiktok-1.jpg`, `tiktok-2.jpg`, `app-tracker.jpg`, portrait ~9:19.5) and uncomment the `<img>` tags.
5. **GitHub**: new repo → upload these files.
6. **Vercel**: Add New Project → import the repo → Deploy (no settings needed). Optionally rename the subdomain or add a custom domain.
7. Submit a test idea, check it appears in Supabase → Table Editor → `challenge_ideas`, then delete it.

Without keys, the page runs in **demo mode** (fake count, submissions not saved) so it can be previewed anywhere.

## Reviewing ideas
Supabase → Table Editor → `challenge_ideas`. Sort by `created_at`, change `status` to `maybe` / `filmed` / `skip` to triage.

## Later, if it gets traction
- Optional TikTok handle field so you can shout people out
- Email/push alert on new submissions (Supabase database webhook)
- Public "filmed" list showing which ideas made it on camera
