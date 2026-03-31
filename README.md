# 💌 Love Notes Automation Tool
### Medleys A Cappella — Fundraiser Automation Tool

This tool automates the behind-the-scenes work for **Love Notes**, Medleys A Cappella's annual singing valentine fundraiser for the LA Downtown Women's Center. It handles availability matrix generation, order management, Slack announcement drafting, confirmation message writing, Google Calendar event creation, and virtual love note email drafting.

---

## Table of Contents

1. [What This Tool Does](#what-this-tool-does)
2. [What You Need Before Starting](#what-you-need-before-starting)
3. [One-Time Setup (Do This Once, At The Start)](#one-time-setup)
   - [Step A: Put the Tool on GitHub Pages](#step-a-put-the-tool-on-github-pages)
   - [Step B: Google Cloud Project Setup](#step-b-google-cloud-project-setup)
   - [Step C: Configure the Tool](#step-c-configure-the-tool)
4. [Annual Setup (Do This Each Year)](#annual-setup-do-this-each-year)
5. [Week-of Workflow](#week-of-workflow)
   - [Before Orders Open: Generate the Availability Matrix](#before-orders-open-generate-the-availability-matrix)
   - [As Orders Come In: Order Dashboard](#as-orders-come-in-order-dashboard)
   - [Each Night: Review and Export](#each-night-review-and-export)
6. [File Formats Reference](#file-formats-reference)
7. [Troubleshooting](#troubleshooting)
8. [Changing the Form Format](#changing-the-form-format)
9. [Passing This On to the Next Manager](#passing-this-on-to-the-next-manager)

---

## What This Tool Does

| Task | Before This Tool | With This Tool |
|---|---|---|
| Availability Matrix | Manually fill in 0s and 1s for every member × every time slot | Upload When2Meet CSV → download finished matrix |
| Slack Announcements | Type out every performance entry by hand | Auto-generated from order data, one click to copy |
| Confirmation Messages | Write each one individually | Auto-generated per order, one click to copy |
| Google Calendar Events | Manually create each event and fill in description | One click per order |
| Virtual Love Note Emails | Copy-paste template and edit for each recipient | Auto-generated drafts sent straight to Gmail |

**What the tool does NOT do automatically** (and shouldn't — these need human judgment):
- Post Slack announcements (you still copy-paste and post them yourself)
- Send confirmation messages (you still copy-paste and send them yourself)
- Handle last-minute customer changes (you update the order info manually and regenerate)

---

## What You Need Before Starting

- A **GitHub account** (free at github.com) — this is where the tool lives
- A **Google account** with access to the club's Google Calendar
- A **Google account** for the club's Gmail/Sheets (may be the same or different account)
- The **love-notes-automation-tool.html** file (this is the tool itself)
- Basic comfort with clicking through websites — no coding required

> **Note for the future manager reading this:** If the previous Love Notes manager is still around, ask them to walk you through Steps A and B in person. The Google Cloud setup in Step B is the only part that might feel unfamiliar — it's about 15 minutes of clicking through Google's website, and the guide below walks you through every screen.

---

## One-Time Setup

> Do this once when you first take over as Love Notes manager. You will not need to do this again next year — just update the config (see [Annual Setup](#annual-setup-do-this-each-year)).

---

### Step A: Put the Tool on GitHub Pages

GitHub Pages lets you host a website for free using a GitHub repository. The tool runs entirely in your browser — no servers, no monthly costs.

**1. Create a GitHub account (if you don't have one)**
- Go to [github.com](https://github.com) and sign up with any email address.
- Choose the free plan.

**2. Create a new repository**
- Once logged in, click the green **"New"** button on the left sidebar, or go to [github.com/new](https://github.com/new).
- **Repository name:** `love-notes-automation-tool` (or anything you like)
- **Visibility:** Set to **Private** (this keeps your code accessible only to you)
- Check **"Add a README file"**
- Click **"Create repository"**

**3. Upload the tool file**
- Inside your new repository, click **"Add file"** → **"Upload files"**
- Drag and drop `love-notes-automation-tool.html` onto the page
- **Important:** Rename the file to `index.html` before uploading, OR after uploading, click the file, click the pencil ✏️ icon to edit, and rename it at the top to `index.html`
- Scroll down and click **"Commit changes"**

**4. Upload this README**
- Repeat the upload process for `README.md` — this replaces the auto-generated README with this guide

**5. Enable GitHub Pages**
- Click the **"Settings"** tab (gear icon) at the top of your repository
- In the left sidebar, click **"Pages"**
- Under "Source," click the dropdown that says **"None"** and change it to **"main"**
- Leave the folder set to **"/ (root)"**
- Click **"Save"**
- Wait 1–2 minutes, then refresh the page. You'll see a box that says: **"Your site is live at https://YOUR-USERNAME.github.io/love-notes-automation-tool/"**

**6. Bookmark that URL** — that's your Love Notes Automation Tool. Share it only with future Love Notes managers.

> **Security note:** Even though your repository is private, GitHub Pages URLs are publicly accessible by default. The tool itself doesn't contain any passwords or sensitive data — but to be safe, don't share the URL publicly.

---

### Step B: Google Cloud Project Setup

This step connects the tool to Google so it can create Calendar events and Gmail drafts on your behalf. You only need to do this once. It takes about 15 minutes.

#### What you're setting up

Google requires any app that accesses Gmail or Google Calendar to be registered in their "Google Cloud Console." You're creating a free project there and getting an **OAuth Client ID** — a public identifier that tells Google "this is the Medleys Love Notes tool." It does **not** expose any passwords.

---

#### Part 1 — Create the Google Cloud Project

1. Go to [console.cloud.google.com](https://console.cloud.google.com)
2. Sign in with the **club Gmail account** (the one used for sending virtual love note emails and accessing the responses spreadsheet)
3. At the very top of the page, you'll see a dropdown that says **"Select a project"** (or shows an existing project name). Click it.
4. In the popup, click **"New Project"** (top right corner of the popup)
5. **Project name:** `Love Notes Automation Tool``
6. Leave "Organization" as-is
7. Click **"Create"**
8. Wait a few seconds, then click the **"Select a project"** dropdown again and select **"Love Notes Automation Tool"**

---

#### Part 2 — Enable the Required APIs

You need to turn on three Google services for the tool.

1. In the left sidebar, hover over **"APIs & Services"** and click **"Library"**
2. In the search bar, type **`Gmail API`** and press Enter
3. Click the **"Gmail API"** result
4. Click the blue **"Enable"** button
5. Go back to the Library (click "Library" in the left sidebar again)
6. Search for **`Google Calendar API`** and press Enter
7. Click the result, then click **"Enable"**
8. Go back to the Library one more time
9. Search for **`Google Sheets API`** and press Enter
10. Click the result, then click **"Enable"**

---

#### Part 3 — Configure the OAuth Consent Screen

This is the screen users see when they click "Sign in with Google" in the tool.

1. In the left sidebar, click **"OAuth consent screen"** (under APIs & Services)
2. For "User Type," select **"External"**
3. Click **"Create"**
4. Fill in the form:
   - **App name:** `Love Notes Automation Tool`
   - **User support email:** your club email
   - **Developer contact information:** your club email
   - Leave everything else blank
5. Click **"Save and Continue"**
6. On the **"Scopes"** page, click **"Save and Continue"** (no changes needed)
7. On the **"Test users"** page:
   - Click **"+ Add Users"**
   - Add the club Gmail address
   - Add the club Google Calendar address (if different)
   - Add your personal email (so you can test)
   - Click **"Add"**, then **"Save and Continue"**
8. On the summary page, click **"Back to Dashboard"**

> **Why "Test users"?** Because the app is in "testing" mode, only emails you add here can log in. This is actually ideal — you don't want random people connecting to the club accounts. Future managers just need to be added to this list.

---

#### Part 4 — Create the OAuth Client ID

This generates the identifier that goes into the tool.

1. In the left sidebar, click **"Credentials"** (under APIs & Services)
2. Click **"+ Create Credentials"** at the top
3. Select **"OAuth client ID"**
4. For **"Application type"**, select **"Web application"**
5. **Name:** `Love Notes Automation Tool Web Client`
6. Under **"Authorized JavaScript origins"**, click **"+ Add URI"** and add:
   ```
   https://YOUR-USERNAME.github.io
   ```
   Replace `YOUR-USERNAME` with your actual GitHub username. Do **not** include a trailing slash.
7. Under **"Authorized redirect URIs"**, click **"+ Add URI"** and add the same URL:
   ```
   https://YOUR-USERNAME.github.io
   ```
8. Click **"Create"**
9. A popup appears showing your **Client ID** — it looks like:
   ```
   123456789-abcdefghijklmnop.apps.googleusercontent.com
   ```
10. **Copy this Client ID** (click the copy icon next to it)
11. Click **"OK"**

> **Keep this Client ID safe but it's not a secret** — it's designed to be used in a web app that anyone can view the source of. It only works from the specific GitHub Pages URL you authorized.

---

#### Part 5 — Enter the Client ID in the Tool

1. Open your Love Notes Automation Tool at `https://YOUR-USERNAME.github.io/love-notes-automation-tool/`
2. Go to the **⚙️ Setup & Config** tab
3. Paste your Client ID into the **"OAuth Client ID"** field
4. Click **"💾 Save Client ID"**

Now when you click **"Connect"** next to Gmail or Calendar, Google's sign-in popup will appear and you can log in with the appropriate account.

---

### Step C: Configure the Tool

Once you've completed Steps A and B, do an initial configuration save:

1. Open the tool and go to **⚙️ Setup & Config**
2. Review the **Member Roster** — all 2025–26 members are pre-loaded. Update as needed.
3. Review the **Song → YouTube URL Mapping** — pre-loaded from your 2026 VLN URL Bank. Update as needed.
4. Review the **Fundraiser Days** table — pre-loaded with the 2025–26 dates (Mon 2/9 – Sat 2/14). Each row is one performance day and contains the date, the responses spreadsheet column number for that day's time slot, performance start/end hours, and the Slack announcement intro line. Update dates and any other details to match the current year.
5. Check the **Order Form Column Mapping** — matches the 2025–26 Google Form. Only change these if non-day-related questions changed (see [Changing the Form Format](#changing-the-form-format)).
6. Review the **Output Templates** — these match the formats used in 2025–26. Edit if you want different wording.
7. Click **💾 Save** on each section, or use any of the save buttons (they all save everything at once)

Everything saves to your browser's local storage and persists between sessions.

---

## Annual Setup (Do This Each Year)

At the start of each new Love Notes cycle, do the following before anything else:

**1. Update the Member Roster**
- Go to **⚙️ Setup & Config → Member Roster**
- Remove members who have left the group
- Add new members (make sure the "When2Meet Name" matches exactly how they'll enter their name in When2Meet)
- Update beatboxer status if it changed
- Click **💾 Save Roster**

**2. Update the Song List and YouTube URLs**
- The group will have a recording day before Love Notes week where you record each song
- Upload each recording as an unlisted YouTube video
- Go to **⚙️ Setup & Config → Song → YouTube URL Mapping**
- Update song names to match whatever songs are on this year's Google Form
- Paste in the YouTube URL for each song
- OR: update your VLN URL Bank spreadsheet and upload it to auto-fill the table
- Click **💾 Save Songs**

**3. Update the Fundraiser Days table**
- Go to **⚙️ Setup & Config → Fundraiser Days**
- This is the most important annual update — everything else in the tool flows from it
- For each row, update the **Date** to the correct calendar date for this year
- Add or remove rows if the number of days changed (e.g., 5 days instead of 6, or starting on Tuesday instead of Monday)
- Adjust **Start Hour** and **End Hour** per day if needed (e.g., Saturday might have a longer window)
- Update the **Responses Column #** for each day if your Google Form column order changed
- Edit the **Slack Intro Line** if you want to change the opening message for any day
- Click **💾 Save Days** — the day tabs in Order Dashboard and Export update automatically

**4. Add New Managers as Google Cloud Test Users**
- Go to [console.cloud.google.com](https://console.cloud.google.com) → APIs & Services → OAuth consent screen
- Scroll to "Test users" and add the new manager's email address
- This allows them to log into the tool with their Google account

**5. Reuse the Same Google Form (Recommended)**
- The column mapping in the tool is configured for the 2025–26 form
- The easiest option is to duplicate that form and update the time slot options each year
- If you add or reorder questions, update the **Order Form Column Mapping** in Setup (the non-day fields) and update the **Responses Column #** in each Fundraiser Days row — see [Changing the Form Format](#changing-the-form-format)

---

## Week-of Workflow

### Before Orders Open: Generate the Availability Matrix

**What you need:**
- The When2Meet CSV export

**Steps:**
1. Create your When2Meet poll as usual, covering each day of Love Notes week in 15-minute increments
2. Once all members have filled it out, go to your When2Meet and click **"Export to CSV"** (on the results page)
3. Make sure your **Fundraiser Days** table in Setup is up to date with this year's dates and hours before generating
4. Open the tool and go to **📊 Availability Matrix**
5. Upload the CSV file
6. The tool will show you which members it found and flag any names that don't match your roster
   - If names are flagged, go to Setup → Member Roster and fix the "When2Meet Name" column to match exactly
7. Step 2 will display a summary of the fundraiser days and per-day hours pulled from your Setup config — verify these look correct
8. Click **"📊 Generate Availability Matrix"**
9. A completed Excel file downloads automatically — open it to review. Each day gets its own sheet with its specific hour range.
10. The time slot header cells are already color-coded: 🟢 green (can perform with beatboxer), 🟡 yellow (can perform, no beatboxer), 🔴 red (cannot perform)
11. Review the matrix, make any manual corrections if needed, and use it to set up your Google Form time slot options

---

### As Orders Come In: Order Dashboard

**What you need:**
- The Google Form responses spreadsheet (download as .xlsx from Google Sheets)
- The completed availability matrix (.xlsx)

**Steps:**
1. Go to **📋 Order Dashboard**
2. Upload the responses spreadsheet and the availability matrix
3. Click **"🔍 Load Orders"**
4. All orders appear as cards, sorted and filterable by day
5. For each in-person order, the tool automatically assigns all available performers for that time slot
6. **To override an assignment:** click any order card to expand it, then click performer chips to toggle them on/off
7. Virtual orders are automatically identified and shown separately under the "Virtual" filter

> **Tip:** Re-upload the responses spreadsheet any time new orders come in — the tool will re-parse everything. Your manual performer overrides are stored in the browser session, so re-uploading doesn't lose them.

---

### Each Night: Review and Export

Do this the night before each performance day, ideally around 9pm.

**Steps:**

1. Go to **📤 Review & Export**
2. Click the tab for tomorrow's day (e.g., "Mon 2/9")
3. **Slack Announcement:**
   - The full announcement is pre-built with your intro line and all performance entries in time order
   - Review it carefully — check that all performer names, locations, and times are correct
   - Make any manual edits in the Order Dashboard if something is wrong, then come back
   - Click **"📋 Copy All"** to copy the entire announcement
   - Paste it into Slack and post it
4. **Per-Order Actions** (expand each order card):
   - **Confirmation Message:** click "📋 Copy" → paste into iMessage → send to the customer's phone number the morning of their performance
   - **Google Calendar Event:** click **"📅 Create Event"** → the event appears on the club calendar immediately. Verify the time and info are correct before creating.
5. **For Virtual Love Notes** (click the "💻 Virtual" tab):
   - Click **"📧 Create Gmail Draft"** for each virtual order
   - Each draft will appear in the club Gmail's Drafts folder, ready to review and schedule
   - Schedule them to send on February 14th at 6:00 AM

---

## File Formats Reference

### When2Meet CSV
- Export directly from When2Meet's results page using "Export to CSV"
- First row is headers (blank cell, then one column per 15-minute time slot)
- Each subsequent row is one member (their name in the first column, "Yes"/"No" in each time slot)
- **The name in the first column must match the "When2Meet Name" in the Member Roster exactly** (case-insensitive)

### Google Form Responses Spreadsheet
- Download from Google Sheets as .xlsx (File → Download → Microsoft Excel)
- The tool expects the header row to be at row index 1 (row 0 is the instruction row you added)
- Column positions are configurable in Setup → Column Mapping
- Time slots are stored across six separate columns (one per day): only one will have a value per order

### Availability Matrix
- Output from the Matrix Generator, or manually filled in
- One sheet per performance day, named exactly: "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"
- First row is time slot labels, first column is member display names
- Values are 0 (unavailable) or 1 (available)
- SUM rows (containing "SUM" in their label) are automatically skipped

### VLN URL Bank Spreadsheet
- Two-column spreadsheet: Song Name | YouTube URL
- Row 1 is the header row (skipped)
- Song names must match exactly what appears in the Google Form's song dropdown

---

## Troubleshooting

**"The When2Meet names have no match in my roster"**
Go to Setup → Member Roster and check the "When2Meet Name" column. It must match character-for-character (spaces, capitalization) how the member's name appears in the When2Meet CSV. Open the CSV in a text editor or Google Sheets to see the exact spelling.

**"The tool isn't finding time slots for orders"**
The time slot parsing handles most common formats ("4-4:30pm", "11:00am-11:30am", etc.). If a particular format isn't working, check the order in the dashboard and manually adjust the performer assignment.

**"Google sign-in popup is blocked"**
Your browser is blocking the popup. Click the popup blocker icon in your address bar and allow popups for the site, then try again.

**"Calendar/Gmail says 'unauthorized'"**
Your access token has expired (they last about 1 hour). Go to Setup and click "Reconnect" to get a fresh token.

**"I get a 403 error when creating calendar events or drafts"**
Make sure you've enabled the Gmail API and Google Calendar API in your Google Cloud project (Step B, Part 2). Also make sure the account you're signing in with is listed as a Test User (Step B, Part 3).

**"The Slack announcement performer list looks wrong"**
Go back to the Order Dashboard, expand the order card, and manually toggle the performer chips to correct it. The Export tab will update immediately.

**"Configuration isn't saving between sessions"**
The tool uses your browser's local storage. Make sure you're not in private/incognito mode, and that your browser allows local storage for GitHub Pages.

---

## Changing the Form Format

If you change the Google Form significantly (different questions, different order, new questions added), two things may need updating.

**For non-day fields** (sender name, recipient name, location, song, contact, etc.):
1. Download the responses spreadsheet as .xlsx
2. Open it in Google Sheets or Excel
3. Look at the header row — count from the left, starting at 0 (Column A = 0, Column B = 1, etc.)
4. Find which column number now corresponds to each field
5. Go to **⚙️ Setup & Config → Order Form Column Mapping** and update the numbers
6. Click **💾 Save**

**For day-specific time slot columns** (i.e., the column that holds "what time?" for Monday, Tuesday, etc.):
- These are configured per-day in the **Fundraiser Days** table, not in the Column Mapping section
- Each row in the Fundraiser Days table has a **Responses Column #** field — update that number for any day whose column position changed

---

## Changing Which Days the Tool Recognizes

Every year the fundraiser days may shift — it might run Tuesday to Sunday instead of Monday to Saturday, or last only 4 days instead of 6. **All of this is now handled entirely through the Setup tab — no code editing required.**

Go to **⚙️ Setup & Config → Fundraiser Days** and use the table:

**To add a day** (e.g., add Sunday): click **+ Add Day**, select the weekday, fill in the date, column number, hours, and Slack intro line, then save. The day tabs in Order Dashboard and Export update automatically.

**To remove a day** (e.g., remove Monday): click the ✕ button on that row, then save.

**To shift which days of the week** (e.g., change from Mon–Sat to Tue–Sun): remove the Monday row, add a Sunday row, update dates on all remaining rows, and save.

**To change a date** (e.g., this year's Friday falls on 2/12 instead of 2/13): click the date field for that row, update it, and save.

**To adjust performance hours for a specific day** (e.g., Saturday starts at 8 AM but ends at 10 PM): update the Start Hour and End Hour fields for that row. Hours are in 24-hour format (8 = 8 AM, 22 = 10 PM).

**To update a Slack intro line**: edit the text in the Slack Intro column for that day and save.

After clicking **💾 Save Days**, the Order Dashboard filter tabs and Export day tabs immediately reflect your changes — no page refresh needed.

---

## Passing This On to the Next Manager

When you hand off Love Notes to the next manager, give them all of the following:

1. **This README** — walk them through it in person if possible
2. **The GitHub repository URL** and your login (or transfer ownership to their account)
3. **The Google Cloud Console login** — log in together and add their email as a Test User (Step B, Part 3, "Test users")
4. **The club Google account credentials** for both the Gmail account and the Calendar account
5. **The Google Form** — either transfer ownership or share it so they can duplicate it
6. **The OAuth Client ID** — it's saved in the tool's Setup tab, but write it down separately as a backup

> **Recommendation:** Keep the same GitHub repository and Google Cloud project from year to year. The only things that need updating annually are the Member Roster, Song URLs, and Fundraiser Days — all done through the tool's Setup tab without touching any code.

---

*Built for Medleys A Cappella at UCLA. Fundraising for the LA Downtown Women's Center.*
*Tool created 2026. Questions? Contact Max Liu.*
