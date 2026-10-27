# Money Matters: Step-by-Step Deployment Guide

**GitHub + Cloudflare + Supabase, all on free plans**

This guide takes you from the `money-matters-repo.zip` file to a live website, one click at a
time. You don't need to be a developer. Plan on about **60–90 minutes** the first time.

**When you're done you'll have:**

- A website (for example `https://money-matters.pages.dev`) where anyone can use week 1 of
  every grade for free.
- School access codes that unlock all 36 weeks, workshops, and Spanish and Portuguese lessons.
- Your code kept private on GitHub, and your lessons and licenses stored in Supabase.
- Automatic updates: every time you change something and push it to GitHub, the website
  updates itself.

**Cost:** $0. All three services have free plans that cover this. No credit card is needed.

---

## Contents

- [Before you start](#before-you-start)
- [Part 1: Install the tools on your computer](#part-1-install-the-tools-on-your-computer)
- [Part 2: GitHub (stores your code privately)](#part-2-github-stores-your-code-privately)
- [Part 3: Supabase (stores lessons and licenses)](#part-3-supabase-stores-lessons-and-licenses)
- [Part 4: Cloudflare (runs the website)](#part-4-cloudflare-runs-the-website)
- [Part 5: GitHub automation](#part-5-github-automation)
- [Part 6: Check that everything works](#part-6-check-that-everything-works)
- [Part 7: Optional features](#part-7-optional-features)
- [Part 8: Everyday tasks](#part-8-everyday-tasks)
- [Troubleshooting](#troubleshooting)
- [Free-plan limits](#free-plan-limits)
- [Keep these secret](#keep-these-secret)

---

## Before you start

You need:

- [ ] A computer (Windows, Mac or Linux) where you can install programs
- [ ] An email address you can check
- [ ] The file `money-matters-repo.zip`
- [ ] A place to keep notes. **Copy every key and password into it as you go.** You will paste
  them into several places later. (A password manager is best.)

Throughout this guide:

- **Type commands** into a *terminal*: **Terminal** on a Mac, **PowerShell** on Windows.
  Type or paste one line at a time and press **Enter**.
- Text in `<angle brackets>` is something you replace with your own value. Don't type the brackets.

---

## Part 1: Install the tools on your computer

*About 10 minutes, done once.*

### 1.1 Install Node.js

1. Go to **nodejs.org** and download the **LTS** version (22 or newer).
2. Run the installer and accept the defaults.
3. Open a **new** terminal window and check it worked:

   ```sh
   node -v
   ```

   You should see something like `v22.x.x`.

### 1.2 Install Git

- **Mac:** in Terminal, type `git --version`. If Git isn't installed, your Mac offers to
  install it. Click **Install**.
- **Windows:** go to **git-scm.com**, download **Git for Windows**, and install it with the
  default options. Then close and reopen PowerShell.

Check it worked:

```sh
git --version
```

### 1.3 Unzip the project

1. Unzip `money-matters-repo.zip`. You get a folder called `money-matters`.
2. Move it somewhere easy to find, such as your **Documents** folder.
3. In the terminal, go into it:

   ```sh
   cd ~/Documents/money-matters
   ```

   (Windows: `cd $HOME\Documents\money-matters`)

4. Install the project's helper packages:

   ```sh
   npm ci
   ```

   This takes a minute or two. Warnings are normal. Errors in red are not: see
   [Troubleshooting](#troubleshooting).

---

## Part 2: GitHub (stores your code privately)

*About 10 minutes.*

### 2.1 Create an account

1. Go to **github.com** and choose **Sign up**. The **Free** plan is all you need.
2. Verify your email address.

### 2.2 Create a private repository

1. In the top-right corner, click **+** → **New repository**.
2. **Repository name:** `money-matters`
3. Choose **Private**. ⚠️ This matters: the full lessons are in this code.
4. Leave **Add a README**, **.gitignore** and **license** all **unchecked**.
5. Click **Create repository**.
6. Keep this page open. You need its address, which looks like
   `https://github.com/<your-name>/money-matters.git`.

### 2.3 Tell Git who you are (first time only)

```sh
git config --global user.name "<Your Name>"
git config --global user.email "<the email you used for GitHub>"
```

### 2.4 Upload the code

In the terminal, inside the `money-matters` folder, run these one at a time:

```sh
git init -b main
git add .
git commit -m "Money Matters V22"
git remote add origin https://github.com/<your-name>/money-matters.git
git push -u origin main
```

The first time you push, a window asks you to sign in to GitHub. Sign in and approve.

✅ **Check:** refresh the repository page on GitHub. You should see folders such as `src`,
`functions` and `supabase`, and a 🔒 **Private** label next to the name.

> The project already has a `.gitignore` file, so your private `.env` file (created in Part 3)
> is never uploaded.

---

## Part 3: Supabase (stores lessons and licenses)

*About 20 minutes.*

### 3.1 Create an account and a project

1. Go to **supabase.com** and choose **Start your project**. Sign up; signing in with GitHub is
   easiest.
2. Click **New project**.
3. Fill in:
   - **Name:** `money-matters`
   - **Database password:** click **Generate a password**, then **copy it into your notes**.
   - **Region:** the one closest to your schools
   - **Plan:** Free
4. Click **Create new project** and wait about 2 minutes while it sets up.

### 3.2 Create the database tables

You'll run three files, **in this order**. Each one is in the `supabase/migrations` folder.

For **each** file:

1. In Supabase's left menu, open **SQL Editor** and click **New query**.
2. On your computer, open the file in a text editor (Notepad or TextEdit is fine), select
   everything (**Ctrl+A** / **Cmd+A**) and copy it.
3. Paste it into the SQL Editor and click **Run**.
4. You should see **Success. No rows returned.**

| Order | File | What it creates |
|---|---|---|
| 1 | `20261026000000_money_matters.sql` | lessons, licenses, usage log |
| 2 | `20261026010000_sign_in_licenses.sql` | licenses by email or domain |
| 3 | `20261027000000_school_sign_in_and_sync.sql` | Clever/ClassLink districts, class sync, usage counts, quote requests |

✅ **Check:** in the left menu, open **Table Editor**. You should see tables including
`licenses`, `content_packs`, `sync_items` and `quote_requests`.

### 3.3 Copy your keys

1. In the left menu, open **Project Settings** (the ⚙️ gear) → **API Keys**. (On some projects
   the page is called **API**.)
2. Copy these three values into your notes:

| Name in this guide | Where to find it | Example |
|---|---|---|
| **Project URL** | Project Settings → **Data API** (or API) | `https://abcdefgh.supabase.co` |
| **Publishable key** | API Keys → **Publishable key** (older projects: `anon` key) | `sb_publishable_…` |
| **Secret key** | API Keys → **Secret keys** → create one if none exists (older projects: `service_role` key) | `sb_secret_…` |

> ⚠️ The **secret key** can read and change everything. Never put it in an email, a chat, a
> web page or a GitHub file. It only goes into your `.env` file, Cloudflare, and GitHub
> *Secrets*, exactly as described below.

### 3.4 Create your `.env` file

1. In the `money-matters` folder, find the file `.env.example`.
   > On a Mac, files starting with a dot are hidden in Finder. Press **Cmd+Shift+.** to show them.
2. Make a copy of it named exactly `.env` (no `.example`).
3. Open `.env` in a text editor and fill in your values:

   ```
   SUPABASE_URL=https://abcdefgh.supabase.co
   SUPABASE_SECRET_KEY=sb_secret_xxxxxxxxxxxxxxxx
   ```

4. Save it.

### 3.5 Upload the lessons

In the terminal, inside the `money-matters` folder:

```sh
npm run build:public
npm run seed
```

The first command prepares the lessons; the second uploads them (about 5 MB). You should see
nine lines with a ✓ (one per content pack) and then `done.`

✅ **Check:** in Supabase → **Table Editor** → `content_packs`, you should see 9 rows.

### 3.6 Create your first school access code

```sh
npm run license -- new --label "Test School" --expires 2027-07-31
```

The code is printed **once**, for example `MM-7KQ2-X9PD-H4TW`. **Copy it into your notes now.**
Only a scrambled version is stored, so it can't be shown again.

---

## Part 4: Cloudflare (runs the website)

*About 20 minutes.*

### 4.1 Create an account

Go to **dash.cloudflare.com**, sign up for the **Free** plan, and verify your email.
You don't need to add a domain.

### 4.2 Create a random secret

Sign-in features need one long random password. In the terminal, run:

```sh
node -e "console.log(require('crypto').randomBytes(32).toString('base64url'))"
```

Copy the result into your notes as **MM_TOKEN_SECRET**.

### 4.3 Connect your GitHub repository

1. In the Cloudflare dashboard's left menu, open **Workers & Pages**.
2. Click **Create application** → choose the **Pages** tab → **Connect to Git**.
3. Click **+ Add account** (or **Connect GitHub**) and sign in to GitHub. When GitHub asks
   which repositories to allow, choose **Only select repositories**, pick `money-matters`, and
   click **Install & Authorize**.
4. Back in Cloudflare, select the `money-matters` repository and click **Begin setup**.

### 4.4 Build settings

Fill in exactly:

| Setting | Value |
|---|---|
| Project name | `money-matters` (this becomes `money-matters.pages.dev`) |
| Production branch | `main` |
| Framework preset | **None** |
| Build command | `node build.js --public` |
| Build output directory | `dist/public` |
| Root directory | *(leave empty)* |

Open **Environment variables (advanced)** and add these two:

| Variable name | Value |
|---|---|
| `NODE_VERSION` | `22` |
| `PLAYWRIGHT_SKIP_BROWSER_DOWNLOAD` | `1` |

Click **Save and Deploy**. The first build takes 1–3 minutes. When it says **Success**, click
**Continue to project**.

### 4.5 Add your keys

1. In your Pages project, open **Settings** → **Variables and Secrets**.
2. For **Production**, add each row below. Click **+ Add**, choose the **Type**, enter the
   name and value, then **Save**.

| Variable name | Type | Value |
|---|---|---|
| `SUPABASE_URL` | Text | your **Project URL** |
| `SUPABASE_SECRET_KEY` | **Secret** | your **Secret key** |
| `SUPABASE_PUBLISHABLE_KEY` | Text | your **Publishable key** |
| `MM_TOKEN_SECRET` | **Secret** | the random value from 4.2 |

> Already set in 4.4 as build variables: `NODE_VERSION` and `PLAYWRIGHT_SKIP_BROWSER_DOWNLOAD`.
> If they don't show here, add them too, both as Text.

### 4.6 Add the free cache (recommended)

The cache keeps Supabase traffic tiny and keeps working through short Supabase outages.

1. In the Cloudflare left menu, open **Storage & Databases** → **KV** (on some accounts: **Workers & Pages** → **KV**).
2. Click **Create** (or **Create namespace**), name it `money-matters-cache`, and click **Add**.
3. Go back to **Workers & Pages** → your `money-matters` project → **Settings** → **Bindings**.
4. Click **+ Add** → **KV namespace**.
   - **Variable name:** `MM_KV` (exactly)
   - **KV namespace:** `money-matters-cache`
5. Click **Save**.

### 4.7 Redeploy so the settings take effect

1. Open the **Deployments** tab.
2. On the latest deployment, click **⋯** → **Retry deployment**.
3. Wait for **Success**.

### 4.8 Open your site

Visit `https://money-matters.pages.dev` (or whatever name Cloudflare shows on the project page).

---

## Part 5: GitHub automation

*About 5 minutes.* These let GitHub upload lesson changes for you, keep Supabase from
pausing, and test the app.

1. On GitHub, open your `money-matters` repository → **Settings** → **Secrets and variables** → **Actions**.
2. Click **New repository secret** and add:
   - Name `SUPABASE_URL`, value: your **Project URL**
   - Name `SUPABASE_SECRET_KEY`, value: your **Secret key**
3. Open the **Actions** tab. If GitHub asks, click **I understand my workflows, go ahead and enable them**.

What now runs on its own:

| Workflow | When | What it does |
|---|---|---|
| `test` | every push | builds the app and runs every automated test (about 7 minutes) |
| `seed-content` | when lesson files change | uploads the new lessons to Supabase |
| `supabase-keepalive` | every 3 days | a tiny query so your free Supabase project never pauses |
| `buttons` | Mondays, or by hand | clicks every button and reports any that do nothing |

> ⚠️ **Why the keep-alive matters:** free Supabase projects pause after 7 days without
> activity. The keep-alive prevents that. If a project does pause, open it in Supabase and
> click **Restore**.

---

## Part 6: Check that everything works

Open your site in a browser and tick these off:

- [ ] The sign-in screen appears. Set up the administrator account (name, password,
      security questions).
- [ ] Week 1 opens for any grade.
- [ ] Week 2 asks for an access code. (That's the free edition working.)
- [ ] Top bar → **Unlock full program** → enter the code from step 3.6. The page reloads and
      week 2 opens.
- [ ] Admin Console → **Class sync** shows **Turn on class sync**.
- [ ] `https://<your-site>/quote` shows the "Get a quote" page.
- [ ] On GitHub → **Actions**, the latest `test` run has a ✅ green check.

All ticked? **You're live.** 🎉

---

## Part 7: Optional features

Turn these on whenever you're ready. Each one is independent. After changing Cloudflare
variables, always **redeploy** (step 4.7).

### 7.1 Your own web address

Cloudflare project → **Custom domains** → **Set up a custom domain**, then follow the steps.
If you add a domain later, also update any sign-in redirect addresses below.

### 7.2 "Sign in with Google" and "Sign in with Microsoft" (teachers and parents)

1. **Supabase** → **Authentication** → **URL Configuration**:
   - **Site URL:** your site address
   - **Redirect URLs:** add the same address
2. **Supabase** → **Authentication** → **Sign In / Providers**. Open **Google** (or
   **Azure** for Microsoft) and copy the **Callback URL** it shows.
3. **Google:** go to console.cloud.google.com → **APIs & Services** → **Credentials** →
   **Create credentials** → **OAuth client ID** → **Web application**. Paste the Callback URL
   under **Authorized redirect URIs**. On the **OAuth consent screen**, add your site's privacy
   page link. Copy the **Client ID** and **Client secret** back into Supabase's Google page and
   turn it **on**.
4. **Microsoft:** go to entra.microsoft.com → **App registrations** → **New registration** →
   choose "Accounts in any organizational directory and personal Microsoft accounts". Set the
   **Web** redirect URI to the Callback URL. Then **Certificates & secrets** → **New client
   secret**. Paste the **Application (client) ID** and the secret into Supabase's Azure page
   and turn it **on**.
5. License teachers by their email domain:

   ```sh
   npm run license -- new --label "Lincoln Elementary" --domain lincoln.k12.us --expires 2027-07-31
   ```

> Facebook is off by default. To add it, set the Cloudflare variable `MM_AUTH_PROVIDERS` to
> `google,azure,facebook` and set up Facebook in Supabase the same way.

### 7.3 Clever and ClassLink (students and staff, by district)

1. **Clever:** create an app at **dev.clever.com**. Add the redirect URI
   `https://<your-site>/api/sso/clever`. Copy its **Client ID** and **Client secret**.
2. **ClassLink:** register an SSO app through ClassLink's partner program. Use the redirect URI
   `https://<your-site>/api/sso/classlink` and scope `profile`. Copy its client ID and secret.
3. In Cloudflare, add these variables (secrets as **Secret**):
   `CLEVER_CLIENT_ID`, `CLEVER_CLIENT_SECRET`, `CLASSLINK_CLIENT_ID`, `CLASSLINK_CLIENT_SECRET`.
4. Redeploy. **Clever** and **ClassLink** buttons appear on the sign-in screen.
5. License a whole district. The district ID is in the provider's dashboard.

   ```sh
   npm run license -- new --label "Lincoln District" --districts clever:<district id> --no-code --expires 2027-07-31
   ```

### 7.4 Class sync

There's nothing to set up: it works once Part 3 is done. A licensed administrator turns it on
in **Admin Console → Class sync**, then:

- opens the **Teacher link** on other teacher devices (keep this link private), and
- shares the **Student link** with students, for example using **Share to Google Classroom**.

### 7.5 The "Get a quote" page

1. Open `src/hosting/pricing.json` in a text editor.
2. Fill in your business details under `seller`, especially `email`.
3. Add prices, if you want them shown, and set `"showPrices": true`.
4. Save, then publish the change (see [8.1](#81-publish-any-change)).
5. Optional: to be told about each request in Slack, Teams or Discord, create an incoming
   webhook there and add its address to Cloudflare as the secret `MM_QUOTE_WEBHOOK`.

You can see every request in Supabase → **Table Editor** → `quote_requests`.

### 7.6 Sponsor cards

Follow `docs/SPONSORS.md`. In short:

1. Edit `src/hosting/ads.json`.
2. Add your sponsor's card.
3. Set `"enabled": true` and publish.

### 7.7 Visitor statistics (Cloudflare Web Analytics)

1. In Cloudflare → **Analytics & Logs** → **Web Analytics** → **Add a site**. Choose to add
   the JavaScript snippet yourself, and copy the **token** (32 letters and numbers).
2. Add it to your Pages project as the variable `MM_CF_BEACON` and redeploy.

> Don't use the "automatic setup" option: the site's security settings would block it.
> Web Analytics uses no cookies.

---

## Part 8: Everyday tasks

### 8.1 Publish any change

After editing any file in the folder:

```sh
git add .
git commit -m "<what you changed>"
git push
```

Cloudflare rebuilds the site in 1–3 minutes. If you changed lessons in `src/data/`, GitHub
also uploads them to Supabase automatically.

### 8.2 Sell a license to a school

```sh
npm run license -- new --label "Roosevelt Middle" --expires 2027-07-31 --contact office@roosevelt.org
```

Send the school the code that's printed.

### 8.3 Manage licenses

```sh
npm run license -- list                                         # every license, its expiry and use
npm run license -- extend "Roosevelt Middle" --expires 2028-07-31
npm run license -- disable "Roosevelt Middle"                   # stops working within 7 days
npm run license -- enable "Roosevelt Middle"
```

### 8.4 See what's being used

```sh
npm run usage                       # last 30 days
npm run usage -- --prefix lesson    # lessons only
```

Counts are anonymous: there are no names, devices or schools.

---

## Troubleshooting

| Problem | What to do |
|---|---|
| `node` or `git`: "command not found" | Close the terminal, open a new one, and try again. If it still fails, reinstall (Part 1). |
| `npm ci` shows red errors | Check that `node -v` shows 22 or newer. Delete the `node_modules` folder and run `npm ci` again. |
| `git push` asks for a password and rejects it | GitHub needs a browser sign-in, not your account password. On Windows, Git opens a sign-in window. On a Mac, install GitHub's helper (**cli.github.com**) and run `gh auth login`, then push again. |
| `npm run seed`: "Set SUPABASE_URL and SUPABASE_SECRET_KEY" | The `.env` file is missing or misnamed. It must be called exactly `.env` (not `.env.txt`) and sit in the `money-matters` folder. In Windows Explorer, turn on **View → File name extensions** to check. |
| `npm run seed`: 401 or "Invalid API key" | You used the publishable key instead of the **secret** key. |
| Cloudflare build fails | Open the failed deployment and read the log. Check the build command is `node build.js --public`, the output directory is `dist/public`, and `NODE_VERSION` is `22`. |
| "Unlock" says the code is unknown | Check the variables from step 4.5 are set for **Production**, then redeploy (4.7). Make sure step 3.5 uploaded the lessons. |
| Unlock says "server not configured" | `SUPABASE_URL` or `SUPABASE_SECRET_KEY` is missing in Cloudflare. Add it and redeploy. |
| Site worked, then lessons stopped loading | Supabase may have paused. Open the project in Supabase and click **Restore**. Check the keep-alive workflow is enabled (Part 5). |
| Sign-in buttons don't appear | Check the variables for that feature in Part 7 and that `MM_TOKEN_SECRET` is set, then redeploy. |
| Class sync says "not set up on this site" | `SUPABASE_URL` / `SUPABASE_SECRET_KEY` are missing in Cloudflare, or migration 3 (step 3.2) was not run. |
| GitHub `test` workflow fails | Open it under **Actions** to see which test failed. Your live site keeps working. Tests check your changes, they don't block the website. |

---

## Free-plan limits

| Service | Free plan includes | How this app uses it |
|---|---|---|
| **Cloudflare Pages** | Unlimited page visits. Server functions: 100,000 calls a day (shared with Workers). | About 1 call per licensed device per week for lessons. Class sync about 12 calls per open device per hour. A 300-device school using it 6 hours a day is about 40,000 a day. |
| **Cloudflare KV** | 100,000 reads and 1,000 writes a day | A few reads per call |
| **Supabase** | 500 MB database, 5 GB transfer a month; pauses after 7 idle days | About 6 MB of lessons plus about 20 KB per synced student. The keep-alive prevents pausing. |
| **Supabase Auth** | 50,000 monthly active users | Only adults who use Google or Microsoft sign-in |
| **GitHub** | Private repositories; 2,000 Actions minutes a month | About 7 minutes per push plus about 30 minutes a week for the button check |

If you grow past these, each service has an inexpensive paid tier, and nothing in the app
needs to change.

---

## Keep these secret

| Secret | Where it may go | Never |
|---|---|---|
| Supabase **secret key** | `.env` on your computer, Cloudflare (Secret), GitHub Actions secrets | email, chat, web pages, GitHub files |
| `MM_TOKEN_SECRET` | Cloudflare (Secret) | anywhere else |
| Supabase database password | your password manager | anywhere else |
| Clever/ClassLink client secrets | Cloudflare (Secret) | anywhere else |
| School access codes | sent to that school only | posted publicly |
| Class sync **Teacher link** | teacher and administrator devices | students, or public posts |

If a secret leaks, create a new one in that service, update it everywhere it's used, and
redeploy. To stop a leaked access code: `npm run license -- disable "<label>"`.

---

*For deeper technical detail (how licensing, sign-in and class sync work), see `DEPLOY.md`.*
