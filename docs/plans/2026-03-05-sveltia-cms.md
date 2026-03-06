# Sveltia CMS Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Add Sveltia CMS so Matt can edit site content at `https://sufishientcharters.com/admin/` without touching code or GitHub.

**Architecture:** Two static files — `static/admin/index.html` (loads Sveltia from CDN) and `static/admin/config.yml` (defines all editable collections) — are added to the Hugo site. Netlify Identity handles authentication; Netlify Git Gateway handles committing changes back to GitHub, which triggers an automatic redeploy.

**Tech Stack:** Hugo, Sveltia CMS (CDN), Netlify Identity, Netlify Git Gateway, YAML

---

### Task 1: Create `static/admin/index.html`

**Files:**
- Create: `static/admin/index.html`

**Step 1: Create the file**

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>SuFISHient Charters — Content Manager</title>
  </head>
  <body>
    <!-- Netlify Identity Widget -->
    <script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>
    <!-- Sveltia CMS -->
    <script src="https://unpkg.com/@sveltia/cms/dist/sveltia-cms.js"></script>
  </body>
</html>
```

**Step 2: Verify the file exists**

Run: `hugo serve` and visit `http://localhost:1313/admin/`

Expected: A blank or loading page with no console errors about missing scripts (the CMS will show a config error until Task 2 is done — that's fine).

**Step 3: Commit**

```bash
git add static/admin/index.html
git commit -m "feat: add Sveltia CMS admin index page"
```

---

### Task 2: Create `static/admin/config.yml`

**Files:**
- Create: `static/admin/config.yml`

This is the core configuration. It tells Sveltia which GitHub branch to write to, where to store uploaded images, and how to map each collection to the Hugo content files.

**Step 1: Create the file**

```yaml
backend:
  name: git-gateway
  branch: main

media_folder: static/images
public_folder: /images

collections:

  # ── Folder collections (Matt can add, edit, and delete entries) ──────────

  - name: charters
    label: Charters
    folder: content/charters
    create: true
    slug: "{{slug}}"
    fields:
      - {label: Title, name: title, widget: string}
      - {label: Summary, name: summary, widget: string}
      - {label: Hero Heading, name: heroHeading, widget: string}
      - {label: Hero Sub-Heading, name: heroSubHeading, widget: string}
      - {label: Hero Background Image, name: heroBackground, widget: image}
      - {label: Icon (SVG), name: icon, widget: image}
      - {label: Featured, name: featured, widget: boolean, default: true}
      - {label: "Display Order (lower = first)", name: weight, widget: number}
      - {label: Button Text, name: button, widget: string}
      - {label: Button Link, name: buttonLink, widget: string}
      - {label: FareHarbor Item ID, name: fareharborId, widget: number}
      - {label: Page Content, name: body, widget: markdown}

  - name: reviews
    label: Reviews
    folder: content/reviews
    create: true
    slug: "{{slug}}"
    fields:
      - {label: Author Name, name: author, widget: string}
      - {label: Google Maps Review URL, name: permalink, widget: string}
      - {label: Featured, name: featured, widget: boolean, default: true}
      - {label: "Display Order (lower = first)", name: weight, widget: number}
      - {label: Review Text, name: body, widget: markdown}

  # ── File collections (Matt can edit, but not add new pages) ─────────────

  - name: homepage
    label: Homepage
    files:
      - label: Hero Banner
        name: hero
        file: content/_index.md
        fields:
          - {label: Heading, name: heroHeading, widget: string}
          - {label: Sub-Heading, name: heroSubHeading, widget: string}
          - {label: Background Image, name: heroBackground, widget: image}

      - label: Welcome Section
        name: welcome
        file: content/homepage/welcome.md
        fields:
          - {label: Title, name: title, widget: string}
          - {label: Background Image, name: background, widget: image}
          - {label: Body, name: body, widget: markdown}

      - label: Our Difference Section
        name: our_difference
        file: content/homepage/about.md
        fields:
          - {label: Title, name: title, widget: string}
          - {label: Button Text, name: button, widget: string}
          - {label: Button Link, name: buttonLink, widget: string}
          - {label: Body, name: body, widget: markdown}

  - name: about
    label: About
    files:
      - label: Why Us
        name: why
        file: content/about/why.md
        fields:
          - {label: Title, name: title, widget: string}
          - {label: Button Text, name: button, widget: string}
          - {label: Button Link, name: buttonLink, widget: string}
          - {label: Body, name: body, widget: markdown}

      - label: Captain
        name: captain
        file: content/about/captain.md
        fields:
          - {label: Title, name: title, widget: string}
          - {label: Hero Heading, name: heroHeading, widget: string}
          - {label: Hero Background Image, name: heroBackground, widget: image}
          - {label: Bio, name: body, widget: markdown}

      - label: The Boat
        name: boat
        file: content/about/boat.md
        fields:
          - {label: Title, name: title, widget: string}
          - {label: Hero Heading, name: heroHeading, widget: string}
          - {label: Hero Background Image, name: heroBackground, widget: image}
          - {label: Description, name: body, widget: markdown}

  - name: contact
    label: Contact
    files:
      - label: Contact Page
        name: contact
        file: content/contact/_index.md
        fields:
          - {label: Title, name: title, widget: string}
          - {label: Body, name: body, widget: markdown}

  - name: book
    label: Book
    files:
      - label: Book Page
        name: book
        file: content/book/_index.md
        fields:
          - {label: Title, name: title, widget: string}
          - {label: Body, name: body, widget: markdown}
```

**Step 2: Verify locally**

Run: `hugo serve` and visit `http://localhost:1313/admin/`

Expected: The Sveltia CMS login screen appears. (You won't be able to log in locally without Netlify Identity — that's expected. The important thing is that the CMS UI loads without errors.)

**Step 3: Commit**

```bash
git add static/admin/config.yml
git commit -m "feat: add Sveltia CMS collection config"
```

---

### Task 3: Push and configure Netlify

**Step 1: Push the branch to GitHub**

```bash
git push origin HEAD
```

**Step 2: Verify the Netlify deploy succeeds**

Watch the deploy in the Netlify dashboard. Expected: build passes, site deploys.

Visit `https://sufishientcharters.com/admin/` — the Sveltia login screen should appear.

**Step 3: Enable Netlify Identity**

In the Netlify dashboard for this site:
1. Go to **Site settings → Identity**
2. Click **Enable Identity**
3. Under **Registration**, select **Invite only**
4. Click **Save**

**Step 4: Enable Git Gateway**

Still in Netlify dashboard:
1. Go to **Site settings → Identity → Services**
2. Click **Enable Git Gateway**

This allows Sveltia to commit to GitHub on Matt's behalf without needing a GitHub account.

**Step 5: Invite Matt**

1. Go to **Identity** tab in the Netlify dashboard
2. Click **Invite users**
3. Enter Matt's email address
4. Matt receives an email, sets his password, and can log in at `https://sufishientcharters.com/admin/`

**Step 6: Smoke test as Matt**

Log in to `/admin/` with Matt's credentials and verify:
- All collections appear in the sidebar (Charters, Reviews, Homepage, About, Contact, Book)
- Opening a charter shows all the expected fields with current values pre-filled
- Opening a review shows the author, URL, and review text
- The media library opens and shows existing images in `static/images/`

**Step 7: Make a test edit and verify the deploy**

Edit a small piece of text (e.g., the Contact page body), click Save, and watch:
- Sveltia shows a "Changes saved" confirmation
- A new commit appears in the GitHub repository
- Netlify starts a new deploy automatically
- After ~1 minute, the change is live on the site

---

### Done

Sveltia CMS is live. Matt can edit any content collection from his browser. Each save commits to GitHub and triggers an automatic Netlify redeploy.
