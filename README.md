# Antoni D. Marinov — Personal Real Estate Website

A personal real estate website built with [Astro](https://astro.build/), designed to showcase listings, client reviews, and a property search feature.

---

## Table of Contents

1. [Before You Start — Fill In Your Content](#1-before-you-start--fill-in-your-content)
2. [Running the Website Locally (Preview on Your Computer)](#2-running-the-website-locally-preview-on-your-computer)
3. [Setting Up a GitHub Repository](#3-setting-up-a-github-repository)
4. [Deploying to GitHub Pages (Publishing Your Site Online)](#4-deploying-to-github-pages-publishing-your-site-online)
5. [Making Future Updates](#5-making-future-updates)

---

## 1. Before You Start — Fill In Your Content

Before deploying, you should update the placeholder content in the source files.
Open the files below in a text editor (like Notepad, VS Code, etc.) and follow the `TODO` comments inside each one:

| File | What to update |
|------|---------------|
| `src/components/About.astro` | Paste your Zillow bio; add headshot photo |
| `src/components/Reviews.astro` | Paste your Zillow reviews (name, date, rating, text) |
| `src/components/Listings.astro` | Add your active and sold listings |
| `src/components/Contact.astro` | Add your phone number, email, and license number |
| `src/components/PropertySearch.astro` | Add your IDX embed code when ready |
| `astro.config.mjs` | Update your GitHub username and repository name |

### Adding a headshot photo

1. Save your professional headshot as `headshot.jpg`
2. Create a folder: `public/images/`
3. Place your photo there: `public/images/headshot.jpg`
4. Open `src/components/About.astro` and uncomment the `<img>` tag (remove the `<!--` and `-->` around it)

---

## 2. Running the Website Locally (Preview on Your Computer)

Before publishing, you can preview the site on your own computer.

### Prerequisites

You need **Node.js** installed. If you don't have it:

1. Go to [https://nodejs.org/](https://nodejs.org/)
2. Download the **LTS** version (the one labeled "Recommended For Most Users")
3. Run the installer — click Next through all the steps

### Steps

1. Open **Command Prompt** or **PowerShell**
   - Press `Windows Key + R`, type `cmd`, press Enter

2. Navigate to the project folder:
   ```
   cd C:\Users\Owner\Desktop\my-real-estate-website
   ```

3. Install the project's dependencies (only needed the first time):
   ```
   npm install
   ```

4. Start the local preview server:
   ```
   npm run dev
   ```

5. Open your web browser and go to: **http://localhost:4321**

   You'll see your website! Any changes you save to the files will automatically refresh in the browser.

6. To stop the server, go back to Command Prompt and press `Ctrl + C`.

---

## 3. Setting Up a GitHub Repository

GitHub is a free service that stores your website's code and lets GitHub Pages host it for free.

### Step 1: Create a GitHub Account

If you don't have one:
1. Go to [https://github.com/](https://github.com/)
2. Click **Sign up** and follow the instructions

### Step 2: Install Git

Git is the tool that sends your code to GitHub.

1. Go to [https://git-scm.com/download/win](https://git-scm.com/download/win)
2. Download and run the installer
3. Click Next through all the steps (the defaults are fine)
4. When asked about the default editor, you can choose **Notepad** if you prefer

### Step 3: Configure Git with Your Name and Email

Open Command Prompt and run these two commands (replace the example values with your real name and email):

```
git config --global user.name "Antoni Marinov"
git config --global user.email "your-email@example.com"
```

### Step 4: Create a New Repository on GitHub

1. Go to [https://github.com/new](https://github.com/new)
2. Under **Repository name**, type something like `my-real-estate-website`
3. Leave it set to **Public** (required for free GitHub Pages hosting)
4. Do **NOT** check "Add a README file" (the project already has one)
5. Click **Create repository**

### Step 5: Connect Your Local Project to GitHub

GitHub will show you a set of commands after you create the repository. You need the ones under **"…or push an existing repository from the command line"**.

Open Command Prompt, navigate to your project folder, and run:

```
cd C:\Users\Owner\Desktop\my-real-estate-website
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_GITHUB_USERNAME/YOUR_REPO_NAME.git
git push -u origin main
```

> Replace `YOUR_GITHUB_USERNAME` with your actual GitHub username and `YOUR_REPO_NAME` with the repository name you chose.

When prompted, enter your GitHub username and password (or a Personal Access Token — see below).

#### About Personal Access Tokens

GitHub no longer accepts your regular password for command-line use. You need a Personal Access Token:

1. Go to [https://github.com/settings/tokens/new](https://github.com/settings/tokens/new)
2. Give it a name (e.g. "my website")
3. Set **Expiration** to "No expiration" or 1 year
4. Under **Scopes**, check **repo**
5. Scroll down and click **Generate token**
6. **Copy the token immediately** — you won't be able to see it again
7. Use this token as your "password" when Git asks for it

---

## 4. Deploying to GitHub Pages (Publishing Your Site Online)

### Step 1: Update astro.config.mjs

Open `astro.config.mjs` in a text editor and update the two commented-out lines:

```js
export default defineConfig({
  site: 'https://YOUR_GITHUB_USERNAME.github.io',
  base: '/YOUR_REPO_NAME',
});
```

Replace:
- `YOUR_GITHUB_USERNAME` with your actual GitHub username (e.g. `antonimarinov`)
- `YOUR_REPO_NAME` with your repository name (e.g. `my-real-estate-website`)

Make sure to **remove the `//` at the start of both lines** to uncomment them.

### Step 2: Create the GitHub Actions Workflow File

This file tells GitHub how to automatically build and publish your site whenever you push changes.

1. In your project folder, create these folders: `.github/workflows/`
2. Inside that folder, create a new file named `deploy.yml`
3. Paste the following content into that file exactly as shown:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: "20"
          cache: npm
      - name: Install dependencies
        run: npm install
      - name: Build with Astro
        run: npm run build
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./dist

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

### Step 3: Enable GitHub Pages in Your Repository Settings

1. Go to your repository on GitHub (e.g. `https://github.com/YOUR_USERNAME/YOUR_REPO_NAME`)
2. Click the **Settings** tab (near the top of the page)
3. In the left sidebar, click **Pages**
4. Under **Source**, select **GitHub Actions**
5. Click **Save**

### Step 4: Push Your Changes to Trigger Deployment

In Command Prompt, run:

```
cd C:\Users\Owner\Desktop\my-real-estate-website
git add .
git commit -m "Add GitHub Pages deployment"
git push
```

### Step 5: Check the Deployment

1. Go to your repository on GitHub
2. Click the **Actions** tab
3. You'll see a workflow running — wait for it to show a green checkmark (usually takes 1–2 minutes)
4. Once it's done, your website will be live at:
   ```
   https://YOUR_GITHUB_USERNAME.github.io/YOUR_REPO_NAME/
   ```

---

## 5. Making Future Updates

Whenever you want to update content on your website (add new listings, update your bio, etc.):

1. Edit the relevant `.astro` file in the `src/components/` folder
2. Preview your changes locally with `npm run dev`
3. When you're happy with the changes, push them to GitHub:

```
cd C:\Users\Owner\Desktop\my-real-estate-website
git add .
git commit -m "Update listings"
git push
```

GitHub Actions will automatically rebuild and redeploy your site within a minute or two.

---

## Project Structure

```
my-real-estate-website/
├── public/                     ← Static files (images, favicon, etc.)
│   └── images/
│       └── headshot.jpg        ← Add your headshot here
├── src/
│   ├── components/             ← Each section of the page
│   │   ├── Nav.astro           ← Navigation bar
│   │   ├── Hero.astro          ← Top banner/hero section
│   │   ├── About.astro         ← Bio section (update with your Zillow bio)
│   │   ├── WhyWork.astro       ← "Why Work With Me" section
│   │   ├── PropertySearch.astro← Property search (add IDX embed here)
│   │   ├── Listings.astro      ← Featured listings (update with your listings)
│   │   ├── Reviews.astro       ← Client reviews (update with your Zillow reviews)
│   │   ├── Contact.astro       ← Contact form and info
│   │   └── Footer.astro        ← Page footer
│   ├── layouts/
│   │   └── Layout.astro        ← HTML wrapper (head, meta tags)
│   └── pages/
│       └── index.astro         ← Main page (assembles all components)
├── astro.config.mjs            ← Astro configuration (update for GitHub Pages)
├── package.json                ← Project dependencies
└── README.md                   ← This file
```

---

## Need Help?

If you run into issues:
- **Astro documentation:** [https://docs.astro.build/](https://docs.astro.build/)
- **GitHub Pages documentation:** [https://docs.github.com/en/pages](https://docs.github.com/en/pages)
- **Formspree (contact form):** [https://formspree.io/](https://formspree.io/)
- **IDX providers:** Ask your broker at Charles Rutenberg Realty, or look into Showcase IDX or iHomefinder
