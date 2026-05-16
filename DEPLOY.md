# How To Go Live

This is the plain-English version. No engineering jargon. Follow the steps in order.

You have two free hosting choices: **Vercel** or **Cloudflare Pages**. Both are free for what you're doing. I recommend Vercel — it's the slickest dashboard and the deploys are basically instant.

The domain stays at GoDaddy. You're not paying GoDaddy for hosting anymore (you cancel the GoDaddy Website Builder when you're confident the new site is live). You're only paying GoDaddy ~$20/year for the domain itself. That's it.

---

## Step 1 — Make a free GitHub account (if you don't have one)

1. Go to **github.com**.
2. Click "Sign up" if you don't have an account. Use your gmail.
3. Pick any username. The free tier is fine.

GitHub is where the code lives. Vercel reads from there. Don't worry — your site files are public-readable but nobody cares, this is the standard.

---

## Step 2 — Put the site code on GitHub

You have two options for this:

### Option A: Use GitHub Desktop (easiest, no terminal)
1. Download **GitHub Desktop** from desktop.github.com. Install. Sign in.
2. In GitHub Desktop: **File → New Repository**.
3. Name it `stephenjamesthomas-site`. Local path: pick this folder (the one with `index.html`, `work.html`, `about.html`, `styles.css`).
4. Click **Create Repository**.
5. Click **Publish repository** in the top bar. Leave "Keep this code private" UNCHECKED (Vercel free tier wants public, and it's fine, no one cares). Click Publish.

Done. Your files are now on GitHub.

### Option B: Drag-and-drop on github.com
1. On github.com, click the **+** in the top-right → **New repository**.
2. Name it `stephenjamesthomas-site`. Public. **Don't** check "Add a README."
3. Click **Create repository**.
4. On the next screen, click **"uploading an existing file"**.
5. Drag every file from this folder (`index.html`, `work.html`, `about.html`, `styles.css`, plus the `mood_A_brutal.html` etc. if you want — or skip those, they're old mockups) into the upload area.
6. Click **Commit changes** at the bottom.

Done.

---

## Step 3 — Deploy on Vercel (5 minutes)

1. Go to **vercel.com**. Click **Sign Up**. Choose **Continue with GitHub**. Authorize.
2. On the Vercel dashboard, click **Add New → Project**.
3. You'll see a list of your GitHub repositories. Find `stephenjamesthomas-site`. Click **Import**.
4. Vercel will ask you about framework settings. **Just click "Deploy"** — don't change anything. It auto-detects that it's a plain static site.
5. Wait ~30 seconds. You'll see confetti and a URL like `stephenjamesthomas-site.vercel.app`.
6. Click that URL. **Your site is live on the internet.**

Test it. Click around. Pull it up on your phone. Make sure everything looks right.

---

## Step 4 — Connect your real domain (stephenjamesthomas.com)

This is the only step with any nerves involved. We're changing where `stephenjamesthomas.com` points to. Once you do this, the *old* GoDaddy Website Builder site stops appearing at that URL and the *new* Vercel site appears instead. Worldwide. Usually within minutes.

### On Vercel:
1. In your Vercel project, click **Settings** → **Domains**.
2. Type `stephenjamesthomas.com` in the box. Click **Add**.
3. Vercel will show you a screen that says "Configure your DNS." It will give you either:
   - **An A record** value (looks like `76.76.21.21`), OR
   - **A CNAME** value (looks like `cname.vercel-dns.com`)
4. Also add `www.stephenjamesthomas.com` the same way. Vercel will give you another CNAME for the www version.
5. **Keep this Vercel tab open.** You'll need the values.

### On GoDaddy:
1. Log into GoDaddy. Go to **My Products** → **Domains** → click **stephenjamesthomas.com** → click **DNS** (or "Manage DNS").
2. You'll see a list of records (A, CNAME, NS, etc.).
3. **Find any A record for `@` (the bare domain)** — there should be one pointing to a GoDaddy IP. Edit it. Change the value to the A record Vercel gave you (e.g., `76.76.21.21`). Save.
4. **Find any CNAME record for `www`** — change its value to whatever Vercel told you to use (e.g., `cname.vercel-dns.com`). Save.
5. If GoDaddy has any "Forwarding" or "Website Builder" toggles, turn them OFF. They'll fight you otherwise.

Now wait. DNS updates can take anywhere from 5 minutes to 24 hours, but usually it's under an hour. Go back to Vercel — the domain status will flip from "Invalid Configuration" to a green check.

### Once it's green:
Visit **stephenjamesthomas.com** in your browser. **You should see the new site.** Test it on your phone too. Test the email and phone links. Click every nav link.

---

## Step 5 — Cancel GoDaddy Website Builder (only after Step 4 is confirmed working)

In GoDaddy: My Products → find the Website Builder subscription → cancel. Make sure you're **only** canceling the Website Builder product, **not** the domain renewal. Keep the domain.

You're done. Site is live, modern, on free hosting, you only pay for the domain.

---

## How to update the site later

You have three ways:

### Option 1: Edit files locally, push to GitHub
1. Open the files in any text editor (VS Code is free and the standard).
2. Make changes. Save.
3. In GitHub Desktop, write a short note about what you changed, click **Commit to main**, then click **Push origin**.
4. Vercel auto-deploys within 30 seconds. Refresh the site, it's there.

### Option 2: Edit on github.com directly
1. Go to your GitHub repo. Click any file. Click the pencil icon. Edit. Save.
2. Vercel auto-deploys.

### Option 3: Just ask me (Claude) to update it
Tell me what you want changed ("add a new piece called X to the work page, replace the about photo with this new shot, change the tagline to Y") and I'll edit the files in this folder, then you push using Option 1 or 2.

---

## Dropping in real video

Right now the work tiles are gradient placeholders. When you have your actual reels:

1. Put the video files (MP4 format, optimized for web — use HandBrake or CloudConvert to compress them to ~5-10 MB each) into a new folder called `videos/` inside this folder.
2. In `index.html` or `work.html`, find the tile you want to replace. It looks like:
   ```html
   <a class="tile landscape t1" href="#" data-cat="ai">
     <div class="play-pulse"></div>
     <div class="tile-label"><span class="client">Jiffy Shirts</span>AI Spot · 16:9 · :30</div>
   </a>
   ```
3. Add a `<video>` tag right after the opening `<a>` tag:
   ```html
   <a class="tile landscape" href="#" data-cat="ai">
     <video class="tile-video" autoplay muted loop playsinline poster="videos/jiffy-poster.jpg">
       <source src="videos/jiffy-spot.mp4" type="video/mp4">
     </video>
     <div class="tile-overlay"></div>
     <div class="play-pulse"></div>
     <div class="tile-label"><span class="client">Jiffy Shirts</span>AI Spot · 16:9 · :30</div>
   </a>
   ```
4. Remove the placeholder class (`t1`, `t2`, etc.) since you don't need the gradient anymore.

Same pattern for the hero video on `index.html` — there's a comment in the file telling you exactly where to swap it.

---

## Quick reference

| Thing | Where |
|---|---|
| Domain registrar | GoDaddy (pay ~$20/yr, keep) |
| Site hosting | Vercel (free) |
| Source code | GitHub (free, public) |
| Edit text/copy | Open the .html files in any text editor |
| Update colors / type | Edit `styles.css` once, applies everywhere |
| Add new work piece | Copy/paste a `<a class="tile ...">` block in `work.html` |
| Need help | Open Claude, tell me what to change |

---

## Things to know before launch

- The site uses Google Fonts loaded from a CDN — fast, free, no setup.
- All animations are pure CSS + a tiny bit of JavaScript. No frameworks, no build step.
- Mobile responsive is built in. Test on your phone before you cancel GoDaddy.
- The favicon is a tiny SVG embedded in the HTML — that's the orange "S" you'll see in browser tabs. We can replace with a real logo later.
- SEO meta tags are set so when someone shares your link in iMessage / Slack / Twitter, it pulls a nice preview.

That's it. Once you're live on Vercel + your domain, you have a real site you control, not a GoDaddy template, and updating it is one of three options above. Welcome to the other side.
