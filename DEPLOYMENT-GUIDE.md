# Rebuilding Haiti — setup and everyday use

This guide has two halves.

- **Part 1** is a one-time setup, done by whoever is technical enough to make a GitHub account. It takes about 45 minutes.
- **Part 2** is for the organizer. It has no code in it at all.

---

## The stack, and why

**Recommendation: Netlify + Decap CMS + Netlify Forms.** This is Option A from the brief, with one substitution explained below.

| Piece | What it does | Cost |
|---|---|---|
| GitHub | Stores the site files and photos | Free |
| Netlify | Puts the site on the internet, worldwide | Free |
| Netlify Forms | Collects newsletter, material, and partnership submissions, with a built-in table view and CSV export | Free up to a monthly submission cap |
| Decap CMS | The `/admin` dashboard the organizer logs into | Free, open source |
| DecapBridge | Handles the admin's email-and-password login | Free tier |

**Why not Option B (Node + Express + a database).** A server has to be patched, restarted, backed up, and paid for. This project needs the opposite: something that keeps working untouched for two years while everyone's attention is on the construction site. There is no server here to go down, no database to corrupt, and no monthly bill. Content lives as plain files, so even if every service above disappeared, the site is still a folder you can hand to another host.

**Why not Sanity or Strapi.** Both are good, but they add a second system to learn and a second place where content lives. Decap edits the same files the site is built from.

**The one substitution.** The classic version of this setup used Netlify Identity for the admin login. Netlify has deprecated Identity and its Git Gateway, so new sites should not build on it. DecapBridge is a free drop-in replacement made specifically for Decap CMS, and it is what these instructions use.

**Known limits, so nothing surprises you later:**

- Netlify's free plan caps form submissions per month. If the campaign goes viral, you will need to upgrade or move the forms to a service like Web3Forms. Check Netlify's current pricing page for the exact number.
- Photos are stored in the GitHub repository. That is fine for hundreds of construction photos. If you get into the thousands, move the gallery to a media host like Cloudinary, which Decap supports.
- After the organizer saves a change, the site takes roughly a minute to rebuild before visitors see it. This is normal.

---

# Part 1 — One-time setup

## Step 1. Put the files on GitHub

1. Create a free account at [github.com](https://github.com).
2. Click **New repository**. Name it `rebuilding-haiti`. Choose **Public**. Click **Create repository**.
3. On the new page, click **uploading an existing file**.
4. Drag in everything from this project folder: `index.html`, `thanks.html`, `robots.txt`, `netlify.toml`, and the `admin`, `content`, and `assets` folders.
5. Click **Commit changes**.

## Step 2. Publish it with Netlify

1. Create a free account at [netlify.com](https://www.netlify.com) and choose **Sign up with GitHub**.
2. Click **Add new site → Import an existing project → GitHub**, and pick your `rebuilding-haiti` repository.
3. Leave the build settings alone. Build command stays empty, publish directory stays `.`. Click **Deploy**.
4. About a minute later you get an address like `radiant-brick-a1b2c3.netlify.app`. Open it. The site should be live.
5. In **Site configuration → General → Site details**, click **Change site name** and make it something readable, like `rebuilding-haiti`.

## Step 3. Turn on the forms

Netlify finds the three forms automatically on the first deploy. To confirm and to set up notifications:

1. Go to **Forms** in the left sidebar. You should see `newsletter`, `material-pledge`, and `partnership`.
2. If they are not listed, go to **Site configuration → Forms** and make sure form detection is enabled, then click **Deploys → Trigger deploy → Deploy site**.
3. Click **Form notifications → Add notification → Email notification**, and enter the organizer's email so every submission also arrives in their inbox.

Submissions appear in that Forms tab as a table, and each form has an **Export CSV** button. That is the submissions viewer the brief asked for, with nothing to build.

## Step 4. Set up the admin login

1. Go to [decapbridge.com](https://decapbridge.com) and create a free account.
2. Click **Create site**, connect your `rebuilding-haiti` GitHub repository, and choose the **Classic** login option (email and password).
3. DecapBridge shows you a `backend:` block of configuration. Copy it.
4. Back in GitHub, open `admin/config.yml`, click the pencil icon, and replace the existing `backend:` block — everything from the line `backend:` down to `branch: main` — with the block you copied. Commit the change.
5. If DecapBridge also gives you a `<script>` tag, open `admin/index.html` and paste it where the comment tells you to.
6. In the DecapBridge dashboard, invite the organizer by email. They will get a message asking them to set a password.

> **Simpler alternative:** if the organizer already has a GitHub account and does not mind using it, skip DecapBridge entirely. Just edit `admin/config.yml` and change `YOUR-GITHUB-USERNAME/rebuilding-haiti` to your actual repository name. Sign-in will use GitHub.

## Step 5. Fix the two placeholders

Open `admin/config.yml` on GitHub one more time and replace both instances of `https://YOUR-SITE.netlify.app` with your real address. Commit.

Then visit `yoursite.netlify.app/admin`, sign in, open **Site settings**, and set the contact email. The default in the file is a placeholder and is not a real inbox.

## Step 6. Your own domain (optional, about $12 a year)

1. Buy a domain from any registrar. Cloudflare and Namecheap are both fine.
2. In Netlify: **Domain management → Add a domain**, type it in, and follow the instructions to point your registrar's nameservers at Netlify.
3. HTTPS turns on by itself within an hour. There is nothing to configure.

---

# Part 2 — For the organizer

Everything below happens at **yoursite.com/admin**. Sign in with your email and password. Changes go live about a minute after you click Publish.

### Add construction photos

1. Click **Photo and video gallery** in the sidebar.
2. Click **Add Item**, then choose **Photo**.
3. Drag your photo into the upload box, or click to pick one from your phone or computer.
4. Write a short caption. Say what it is, not how it makes you feel — "Steel gate installed, March 2025" works better than "Amazing progress."
5. Click **Publish**.

To add a video instead, choose **Video** and paste the YouTube address. To remove something, click the trash icon next to it. To reorder, drag the handle on the left.

### Mark a phase as finished

1. Click **Build progress**.
2. Find the phase and change its **Status** to **Completed**.
3. Click **Publish**.

The wall on the home page fills in one more course, and the counters at the top update on their own.

To add a phase that is not listed, click **Add Phase**, give it a name and a one-line description, and drag it into the right position in the order.

### Post an update

1. Click **Progress log**.
2. Click **Add Post** and drag it to the top of the list, so the newest is first.
3. Give it a headline, pick the date, and write a paragraph or two.
4. Click **Publish**.

Two things make these read well: say what physically happened, and say what comes next. People give money to a project that is clearly still moving.

### See who signed up or offered to help

This one is on Netlify, not the dashboard. Sign in at [netlify.com](https://www.netlify.com), open the site, and click **Forms** in the sidebar. You will see three lists:

- **newsletter** — email addresses for progress updates
- **material-pledge** — people offering cement, tools, transport, and so on
- **partnership** — companies, churches, and NGOs

Click any of them to read the submissions, and use **Export CSV** to download the whole list as a spreadsheet.

### Change the GoFundMe link, the video, or the headline

Click **Site settings**. Everything there is a plain text box. The YouTube video ID is the code in the address after `v=` — in `youtube.com/watch?v=zmePAFAUtg8`, the ID is `zmePAFAUtg8`.

---

## If something goes wrong

**The site did not change after I published.** Wait two minutes and refresh. If it still looks old, hold Shift and refresh once.

**I cannot sign in to /admin.** Check that you are going to `yoursite.com/admin` with the slash. If the login box never appears, the `backend:` block in `admin/config.yml` is wrong — that is a job for whoever did the setup.

**A form says it did not send.** Forms only work on the live Netlify address, not on a copy of the file opened from a folder. If it fails on the live site, check **Site configuration → Forms** and redeploy.

**Photos are not showing.** Make sure they were uploaded through the dashboard rather than added to the repository by hand. The dashboard puts them in the right place and writes the correct path.

## Handing this to a developer later

The whole site is `index.html`, plus four JSON files in `content/` that it reads at load time. There is no build step and no framework. Any developer can open it and understand it in ten minutes. Tailwind loads from a CDN for zero-maintenance reasons; if you later want a compiled stylesheet for slightly faster loads, that is a one-hour change and nothing else has to move.
