# YouTube Auto — Project Guide

Generated 10 Nov 2026

One place for each project: what it is, what it produces, what counts as "complete", and what is still to do.
Projects that aren't finished don't get a published template.

This guide is written in plain language. You do not need to run any of this yourself unless you want to: your assistant can do the setup, and you only step in to sign in or to say when something may be switched on.

**Rules that apply to every project**

- Finished YouTube videos are uploaded **private or unlisted**. You publish them yourself.
- Until Google approves the YouTube app, API uploads stay **private**.
- Keys, tokens and passwords are never put in chat. They go in secure boxes only.
- New workflows stay switched off until a test works and you say it is OK to turn them on.

## Snapshot

| Project | Type | Produces | Complete? | Template |
|---|---|---|---|---|
| n8n YouTube automation | Workflow set | Faceless long videos and Shorts, uploaded private or unlisted, plus a weekly channel report | No (see below) | Draft (unpublished) |
| Free AI video-generation app | App | Downloadable videos, including an Animated Story feature | No (see below) | Part of the unpublished draft |
| Kaggle free-GPU video path | Workflow and worker | Short AI video clips on free GPUs, with a free image-and-voice fallback | No (see below) | Part of the unpublished draft |
| MoneyPrinter YouTube Upload | Workflow | A private or unlisted YouTube upload of a MoneyPrinter Short | No (see below) | Part of the unpublished draft |
| Worldforge optional backend | Workflow and optional backend | Higher-quality AI clips when a paid GPU is available | No (switched off) | Part of the unpublished draft |
| TikTok posting | Not built yet | TikTok posts of finished videos | No | Not started |

### Other workflows on the same n8n server

Your n8n server also holds workflows for other projects (for example a digital-product hub, a book project, and some official n8n sample templates). Those are not part of YouTube Auto and are not covered here. They should be left as they are.

MoneyPrinter YT may add its own helper workflows on the same server. It can reuse the Kaggle path described above; it should not start a second Kaggle launcher, because both would share one weekly GPU allowance.

## 1. n8n YouTube automation

**Type:** Workflow set, running on your n8n server.

This is the main faceless YouTube pipeline. Ideas come from a trends email and from your own idea list. A language model on your server writes the script and scene prompts. A voice is generated on the server. AI clips are rendered on a GPU (Kaggle first; see project 3), then stitched with captions and uploaded.

The workflows in this set:

- **A – Ideas + Scripts:** collects ideas, writes scripts and scene prompts, and queues jobs.
- **B – Assemble + Upload:** builds the long video and uploads it as unlisted or private.
- **C – Weekly channel report:** each Monday, reviews recent uploads and writes insights back into the Control Sheet.
- **D – Niche finder:** looks for small, fast-growing channels in your seed niches.
- **S1 / S2 – Shorts:** the Shorts version of ideas-and-scripts and assemble-and-upload (vertical, about 30–58 seconds).

**What it produces:**

1. Faceless narrated long videos, uploaded private or unlisted for you to review and publish.
2. Faceless narrated Shorts, uploaded the same way, with `#shorts` in the description.
3. A weekly per-channel report and planning insights in the Control Sheet.

**Complete means all of these:**

- The workflows are built, imported, and signed in to the right Google accounts and YouTube channels.
- Each channel slot has its channel name and niche in the Control Sheet, and unused slots are paused.
- One private test upload has succeeded.
- The workflows have been switched on, one at a time, with your OK, and a real long video and a real Short have come out.
- You get a notice when a video is ready (Telegram or Gmail).
- The n8n encryption key has been backed up off the server.

**Done:**

- [x] Workflows A, B, C, D, S1 and S2 are built and imported on your n8n server (all still switched off).
- [x] Google sign-ins for Gmail, Sheets, Drive, and YouTube channel slots 1 to 5.
- [x] YouTube channel slots 6 to 10 have been created, ready for you to sign each one in to its own channel.
- [x] The Google consent screen is in production. The app is not yet verified by Google, so API uploads stay private until that audit.

**To do:**

- [ ] Tell me which YouTube channel is in each slot, and which slot MoneyPrinter finance Shorts should use (slot 1 unless you say otherwise).
- [ ] Fill in the niche for each channel in the Control Sheet, and keep unused channels paused.
- [ ] Sign in YouTube slots 6 to 10 to their own channels, if you want to use them. Right now slots 1 to 5 all point at the same channel, **Moon Fantasy**. That is fine if it is your only channel for now.
- [ ] Run one private test upload, then switch the workflows on one at a time after you OK each one.
- [ ] Choose Telegram alerts (bot token and chat ID, through a secure box) or Gmail alerts.
- [ ] Back up the n8n encryption key off the server.

**Where the files live:** `n8n/` (workflows A–D), `n8n/shorts_only/` (S1 and S2), and `oracle-setup/` (how the n8n server is installed). See [n8n/README.md](../n8n/README.md) and [n8n/shorts_only/README.md](../n8n/shorts_only/README.md).

## 2. Free AI video-generation app

**Type:** App (a website and API, plus a worker that makes the clips).

A free video app in the same spirit as tools like TubeGen or TubeLab. You pick a topic and a format (a vertical Short or a horizontal long video). It writes a script, records a voice, makes AI clips, adds captions, and gives you a downloadable video. It also has an **Animated Story** feature: a style guide, reusable characters and props, a shot list, a simple preview, then a rendered story.

**What it produces:**

1. Finished Short or long videos (MP4) with narration and captions, ready to download.
2. Animated Story videos, built from a consistent style and shot list.
3. Titles, descriptions, tags and a thumbnail for each video, plus optional research and remix tools.

**Complete means all of these:**

- The app is online with HTTPS, so you can use it in a browser.
- User logins work (planned with Supabase).
- Backups are in place.
- A real-GPU test of Animated Story has been done (not only the local mock test).

**Done:**

- [x] The app code is built: a FastAPI backend, a web front end, and a GPU or CPU worker.
- [x] Features include channel projects, several script styles, voices, captions, an optional AI presenter, remix, and a niche finder.
- [x] Local end-to-end tests exist for ordinary videos, Animated Story (with a mock worker), and the Kaggle path (offline).
- [x] The free clip path is wired: Kaggle first, then an instant fallback of AI images, voice and gentle motion, with Colab and Worldforge optional and off by default.

**To do:**

- [ ] Put the app online with HTTPS.
- [ ] Add user logins with Supabase.
- [ ] Set up backups.
- [ ] Run a real-GPU test of Animated Story and confirm the result.

**Where the files live:** [app/](../app/README.md), with setup notes in [app/docs/KAGGLE_SETUP.md](../app/docs/KAGGLE_SETUP.md) and [app/docs/WORLDFORGE_SETUP.md](../app/docs/WORLDFORGE_SETUP.md).

## 3. Kaggle free-GPU video path

**Type:** Workflow and worker.

This is the primary free way to generate AI video clips for automated Shorts (and for long videos too). Two n8n workflows, **GPU Bridge** and **GPU Router**, work with a Kaggle batch job that runs on two free T4 GPUs. The default model is Wan 2.1, a smaller text-to-video model that fits those cards. Other options are LTX-Video and, experimentally, a larger Wan model.

If Kaggle cannot run a job (switched off, out of weekly hours, or not picking work up in time), the **instant free fallback** makes the video from AI still images, a voice, and a gentle pan-and-zoom, so a job still gets finished.

**What it produces:**

1. Short vertical AI video clips (the first test was 480×832, about 3 seconds) that later get chained, voiced and captioned into a full Short or long video.
2. Automatic start when a new job arrives, and automatic stop when work is finished or stuck, so free GPU hours are not wasted.
3. A fallback still-image-and-voice video when Kaggle is unavailable.

**Complete means all of these:**

- Your Kaggle account is connected and a test clip has been made.
- Auto-stop and auto-relaunch behave as designed.
- GPU Bridge and GPU Router have been switched on, with your OK, after a successful test, and a real Short has used a Kaggle clip.
- Only one Kaggle launcher runs on your account (MoneyPrinter YT shares the same weekly GPU hours).

**Done:**

- [x] GPU Bridge and GPU Router are built and imported (both still switched off).
- [x] Your Kaggle account is connected. You have the usual weekly allowance of about 30 free GPU hours.
- [x] Auto-stop safeguards are in place: it shuts down when the queue is empty (after a short wait), if nothing progresses for 20 minutes, if several jobs fail in a row, or if it cannot reach the server. A master off-switch stops it at once.
- [x] Auto-relaunch is in place: a new job starts Kaggle again if it is off (never two sessions at once; a short cool-down after repeated errors).
- [x] The first test clip succeeded on 10 Nov 2026: a short vertical clip (480×832) made with Wan 2.1 on two T4 GPUs. The session took about 16 minutes, mostly to download the model the first time.

**To do:**

- [ ] Switch on GPU Bridge, then GPU Router, after you OK each one.
- [ ] Confirm a real Short (not only the short test clip) is made from chained Kaggle clips plus voice and captions.
- [ ] Optional keys for the instant fallback (Cloudflare, Gemini, Pixabay) can be added later through secure boxes. The fallback is weaker without them, but they are not required to use Kaggle.

**Kaggle rules to respect:** use batch jobs only, with no always-on connection, and one launcher per account. Kaggle's terms say personal, non-commercial use, so treat this as a free bonus that could stop. Your weekly hours are shared with MoneyPrinter YT.

**Where the files live:** [n8n/gpu_kaggle/](../n8n/gpu_kaggle/README.md) and [app/docs/KAGGLE_SETUP.md](../app/docs/KAGGLE_SETUP.md).

## 4. MoneyPrinter YouTube Upload

**Type:** Workflow.

A dedicated n8n webhook that MoneyPrinter YT can call to upload a finished finance Short to YouTube. It uses the same channel slots as the main pipeline. Uploads stay private or unlisted. The workflow is built but still switched off, waiting for one successful private test upload.

**What it produces:**

1. A private or unlisted YouTube upload of a MoneyPrinter Short, on the channel slot you choose (slot 1 unless you say otherwise).
2. The video ID and link, so MoneyPrinter can record that the upload happened.
3. The same review-then-you-publish path as the other YouTube workflows.

**Complete means all of these:**

- The Google sign-in for the chosen channel slot is done (slot 1 is signed in).
- One private test upload has succeeded.
- The workflow has been switched on with your OK, and MoneyPrinter has been told it can use it (the address and how to authenticate, with no secret values in chat).

**Done:**

- [x] The workflow is built and imported (still switched off).
- [x] It can target the same channel slots as the main pipeline.
- [x] Google sign-in for slot 1 is done. Remember that slots 1 to 5 currently all point at **Moon Fantasy**.

**To do:**

- [ ] Confirm which slot MoneyPrinter should use (slot 1 unless you say otherwise).
- [ ] Run one private test upload.
- [ ] Switch the workflow on after that test works, then tell MoneyPrinter YT it can post through it.

## 5. Worldforge optional backend

**Type:** Workflow and optional backend (switched off by default).

Worldforge is an optional, higher-quality way to make AI clips. It is not part of the daily free path. It needs a **paid** GPU (a paid Colab plan, or a rented GPU). The free T4 cards used on Kaggle are only enough for Worldforge's smallest models, and Worldforge's usual setup does not fit Google's free-Colab rules, so free GPU work stays on Kaggle.

An n8n workflow named **Worldforge** is already imported and switched off. If Worldforge is unreachable, jobs fall back to Kaggle or to the free image-and-voice path.

**What it produces:**

1. Higher-quality AI video clips when you choose to rent a bigger GPU.
2. An n8n entry point so a job can request a Worldforge clip and get a file back, or a clear "use the normal path" reply if Worldforge is off.

**Complete means all of these:**

- A paid GPU is available and the Worldforge server is started with a password gate.
- The connection details have been saved securely on your n8n server.
- A test clip has been made through the Worldforge workflow, and fallback still works when it is off.

**Done:**

- [x] The optional backend and the n8n workflow named Worldforge are built.
- [x] The workflow is imported and switched off by default.
- [x] Fallback to Kaggle or the free image-and-voice path is in place if Worldforge is off or fails.

**To do:**

- [ ] Only when you want this path: provide a paid GPU, start Worldforge there, and send the connection details through a secure box (never in chat).
- [ ] Run a test clip, then switch the workflow on only if you want it used.

**Where the files live:** `n8n/worldforge/` and [app/docs/WORLDFORGE_SETUP.md](../app/docs/WORLDFORGE_SETUP.md).

## 6. TikTok posting

**Type:** Not built yet.

Finished videos are meant to go to TikTok as well as YouTube, but this has not been set up. You need to choose a posting service first: **Buffer** (there is a free plan) or **Postiz**.

**What it will produce (once built):**

1. A TikTok post of a finished Short, using the service you choose.
2. A record of what was posted, so the same video is not sent twice.
3. The same "you approve the setup first" rule as YouTube: nothing goes out until you say so.

**Complete means all of these:**

- You have chosen Buffer or Postiz and signed in.
- A test post has been made (or drafted, if you prefer to approve each one).
- The posting step is connected to the Shorts pipeline and only runs with your OK.

**Done:**

- [x] The choice has been presented: Buffer (free plan) or Postiz.

**To do:**

- [ ] Choose Buffer or Postiz.
- [ ] Sign in to the service you pick.
- [ ] Build the posting step and run a test, then switch it on only with your OK.

## Templates

No project is fully complete, so **no shareable template has been published**.

An **unpublished draft** of a YouTube Auto bot template already exists, including a getting-started guide for a new owner. It is being left unpublished on purpose until the work above is finished.

Before a template is published, at least these need to be true:

- A private YouTube test upload has succeeded, and the YouTube workflows you want have been switched on with your OK.
- Channel slots and niches are filled in on the Control Sheet, and you have said which slot MoneyPrinter uses.
- Notices (Telegram or Gmail) are working.
- The Kaggle path has been switched on with your OK and has made a real Short, not only the short test clip.
- TikTok is either built and tested, or clearly documented as out of scope for that template version.
- The draft has been checked so it contains no secrets, keys, private links or personal details.
