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
8. [Passing This On to the Next Manager](#passing-this-on-to-the-next-manager)

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
4. Check the **Column Mapping** — matches the 2025–26 Google Form. Only change these if the form structure changed.
5. Review the **Output Templates** — these match the formats used in 2025–26. Edit if you want different wording.
6. Click **"💾 Save"** on each section, or use any of the save buttons (they all save everything at once)

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

**3. Update Fundraiser Dates**
- Go to **📊 Availability Matrix** tab
- Update the **Fundraiser Start Date** and **Fundraiser End Date** to this year's dates

**4. Add New Managers as Google Cloud Test Users**
- Go to [console.cloud.google.com](https://console.cloud.google.com) → APIs & Services → OAuth consent screen
- Scroll to "Test users" and add the new manager's email address
- This allows them to log into the tool with their Google account

**5. Reuse the Same Google Form (Recommended)**
- The column mapping in the tool is configured for the 2025–26 form
- The easiest option is to duplicate that form and update the time slot options each year
- If you make significant changes (different questions, different order), update the Column Mapping in Setup accordingly — see [Changing the Form Format](#changing-the-form-format)

---

## Week-of Workflow

### Before Orders Open: Generate the Availability Matrix

**What you need:**
- The When2Meet CSV export

**Steps:**
1. Create your When2Meet poll as usual, covering each day of Love Notes week in 15-minute increments
2. Once all members have filled it out, go to your When2Meet and click **"Export to CSV"** (on the results page)
3. Open the tool and go to **📊 Availability Matrix**
4. Upload the CSV file
5. The tool will show you which members it found and flag any names that don't match your roster
   - If names are flagged, go to Setup → Member Roster and fix the "When2Meet Name" column to match exactly
6. Set the **Fundraiser Start Date** and **End Date**
7. Click **"📊 Generate Availability Matrix"**
8. A completed Excel file downloads automatically — open it to review
9. The time slot header cells are already color-coded: 🟢 green (can perform with beatboxer), 🟡 yellow (can perform, no beatboxer), 🔴 red (cannot perform)
10. Review the matrix, make any manual corrections if needed, and use it to set up your Google Form time slot options

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

If you change the Google Form significantly (different questions, different order, new questions added), the column mapping will need to be updated.

**To find the new column numbers:**
1. Download the responses spreadsheet as .xlsx
2. Open it in Google Sheets or Excel
3. Look at the header row — count from the left, starting at 0
   - Column A = 0, Column B = 1, Column C = 2, etc.
4. Find which column number corresponds to each field
5. Go to **Setup → Order Form Column Mapping** and update the numbers
6. Click **💾 Save**

---

## Changing Which Days the Tool Recognizes

> **When to do this:** Every year the days of Love Notes week may shift. For example, this year it ran Monday–Saturday; next year it might run Tuesday–Sunday, or only last 4 days. This section explains exactly what to edit in the HTML file to add days, remove days, or change which days of the week the tool looks for.

### How to Edit the HTML File

The tool is a single file called `index.html` in your GitHub repository. To edit it:

1. Go to your GitHub repository
2. Click on `index.html`
3. Click the **pencil icon** (✏️ Edit this file) in the top right
4. Make your changes following the instructions below
5. When done, scroll to the bottom, type a short description like "Updated days for 2027", and click **"Commit changes"**

The tool will update automatically within a minute or two.

> **Tip:** Use your browser's Find function (Ctrl+F or Cmd+F) to locate the exact text you need to change. The section names below are the exact labels to search for.

---

### The Six Places You Need to Change

When you add, remove, or change a day, you must update **all six** of the following locations in the HTML file. They are listed in the order they appear in the file.

---

#### Location 1 — Column Mapping defaults (`DEFAULT_COL_MAP`)

Search for: `DEFAULT_COL_MAP`

This section maps each day to its column number in the Google Form responses spreadsheet. It looks like this:

```javascript
const DEFAULT_COL_MAP = {
  ...
  timeMon: 9,
  timeTue: 10,
  timeWed: 11,
  timeThu: 12,
  timeFri: 13,
  timeSat: 14,
  ...
};
```

**What to change:**
- Each `time___` entry corresponds to one day's time slot column in the spreadsheet
- The number after the colon (9, 10, 11, etc.) is the column index — count from 0 in your spreadsheet's header row
- **To add a day** (e.g., Sunday): add a new line like `timeSun: 15,` with the correct column index
- **To remove a day** (e.g., remove Monday): delete the `timeMon: 9,` line
- **To change a day** (e.g., change Monday to Tuesday as the first day): rename `timeMon` to `timeTue` and update its column number

The short three-letter codes used here (`Mon`, `Tue`, `Wed`, `Thu`, `Fri`, `Sat`, `Sun`) are called **day keys**. Use these exact codes — they connect all six locations together.

---

#### Location 2 — Order Dashboard filter buttons

Search for: `id="orderDayFilter"`

This is the row of buttons at the top of the Order Dashboard that lets you filter orders by day. It looks like this:

```html
<div class="day-tabs" id="orderDayFilter">
  <button class="day-tab-btn active" data-filter="all" onclick="filterOrders('all',this)">All</button>
  <button class="day-tab-btn" data-filter="Mon" onclick="filterOrders('Mon',this)">Mon 2/9</button>
  <button class="day-tab-btn" data-filter="Tue" onclick="filterOrders('Tue',this)">Tue 2/10</button>
  <button class="day-tab-btn" data-filter="Wed" onclick="filterOrders('Wed',this)">Wed 2/11</button>
  <button class="day-tab-btn" data-filter="Thu" onclick="filterOrders('Thu',this)">Thu 2/12</button>
  <button class="day-tab-btn" data-filter="Fri" onclick="filterOrders('Fri',this)">Fri 2/13</button>
  <button class="day-tab-btn" data-filter="Sat" onclick="filterOrders('Sat',this)">Sat 2/14</button>
  <button class="day-tab-btn" data-filter="Virtual" onclick="filterOrders('Virtual',this)">💻 Virtual</button>
</div>
```

**What to change:**
- **To add a day** (e.g., Sunday Feb 15): copy any existing button line and paste it. Change `data-filter="Sun"`, the `onclick` text to `'Sun'`, and the button label to `Sun 2/15`
- **To remove a day**: delete its button line entirely
- **To change a date label**: update the text between `>` and `</button>` (e.g., change `Mon 2/9` to `Mon 2/8`)
- Keep the `Virtual` button — do not remove it

---

#### Location 3 — Export tab day buttons

Search for: `id="exportDayTabs"`

This is the row of buttons in the Review & Export tab. It looks almost identical to Location 2:

```html
<div class="day-tabs" id="exportDayTabs">
  <button class="day-tab-btn active" data-day="Mon" onclick="showExportDay('Mon',this)">Mon 2/9</button>
  <button class="day-tab-btn" data-day="Tue" onclick="showExportDay('Tue',this)">Tue 2/10</button>
  ...
  <button class="day-tab-btn" data-day="Virtual" onclick="showExportDay('Virtual',this)">💻 Virtual</button>
</div>
```

**What to change:** Same rules as Location 2, but the attribute is `data-day=` instead of `data-filter=`, and the `onclick` says `showExportDay` instead of `filterOrders`. Add, remove, or update buttons to match what you did in Location 2.

---

#### Location 4 — Day-to-column mapping in the order parser (`dayTimeCols`)

Search for: `dayTimeCols`

This is inside the `parseOrders` function and tells the parser which spreadsheet column to look at for each day's time slot. It looks like this:

```javascript
const dayTimeCols = {
  Mon: cm.timeMon,
  Tue: cm.timeTue,
  Wed: cm.timeWed,
  Thu: cm.timeThu,
  Fri: cm.timeFri,
  Sat: cm.timeSat
};
```

**What to change:**
- **To add a day** (e.g., Sunday): add `Sun: cm.timeSun,` on a new line (the `cm.timeSun` refers to the value you set in Location 1)
- **To remove a day**: delete its line
- The day keys here must exactly match what you used in Locations 1, 2, and 3

---

#### Location 5 — Slack announcement intro lines (`slackIntros`)

Search for: `slackIntros`

This object contains the opening line of the Slack announcement for each day. It looks like this:

```javascript
const slackIntros = {
  Mon: '@channel Hi guys tomorrow is the FIRST DAY OF LOVE NOTES WEEK OH YEAHHHH. Ok here\'s the schedule:',
  Tue: '@channel Hi guys tomorrow is the SECOND DAY OF LOVE NOTES WEEK LETS GOOOOOOOOOO. Ok here is the schedule:',
  Wed: '@channel Hi guys tomorrow is the THIRD DAY OF LOVE NOTES WEEK LETS GOOOOOOOOOO. Ok here da schedule:',
  Thu: '@channel Hi guys tomorrow is the FOURTH DAY OF LOVE NOTES WEEK YEAHHHHHHHH OH YEAHHHHHHHH OH YEAHHHHHHH WAHHHHHHHH. Ok here\'s the schedule:',
  Fri: '@channel Hi guys tomorrow is the FIFTH DAY OF LOVE NOTES WEEK YURRRRRRRRR. Ok schedule:',
  Sat: '@channel Hi guys tomorrow is the SIXTH DAY OF LOVE NOTES WEEK YUUUUUUUUH. Ok schedule:',
};
```

**What to change:**
- **To add a day**: add a new line like `Sun: '@channel Hi guys tomorrow is the SEVENTH DAY...',`
- **To remove a day**: delete its line
- **To change the wording**: edit the text between the single quotes — go wild, this is the fun part
- Note: the `\'` inside the text is just how you write an apostrophe inside JavaScript single-quoted strings — keep it as-is

---

#### Location 6 — Day-to-date mapping for Google Calendar (`dates` inside `buildEventDateTime`)

Search for: `buildEventDateTime`

This function converts a day key + time slot into an actual calendar date and time. Inside it there is a `dates` object like this:

```javascript
const dates = {
  Mon: '2026-02-09',
  Tue: '2026-02-10',
  Wed: '2026-02-11',
  Thu: '2026-02-12',
  Fri: '2026-02-13',
  Sat: '2026-02-14'
};
```

**What to change:**
- **Every year**, update all six date strings to match the actual calendar dates of Love Notes week
- **To add a day** (e.g., Sunday): add `Sun: '2026-02-15',`
- **To remove a day**: delete its line
- Dates must be in `YYYY-MM-DD` format (year-month-day, zero-padded)

---

### Complete Example: Changing from Mon–Sat to Tue–Sun

Say next year Love Notes runs Tuesday February 9 through Sunday February 14. Here is exactly what you would change at each location:

| Location | Remove | Add |
|---|---|---|
| 1. DEFAULT_COL_MAP | `timeMon: 9,` | `timeSun: 14,` (update all column numbers to match new form) |
| 2. Order Dashboard buttons | Mon button | `<button class="day-tab-btn" data-filter="Sun" onclick="filterOrders('Sun',this)">Sun 2/14</button>` |
| 3. Export tab buttons | Mon button | Same Sun button with `data-day="Sun"` and `showExportDay` |
| 4. dayTimeCols | `Mon: cm.timeMon,` | `Sun: cm.timeSun,` |
| 5. slackIntros | `Mon: '...',` | `Sun: '@channel Hi guys...',` |
| 6. buildEventDateTime dates | `Mon: '2026-02-09',` | `Sun: '2027-02-14',` (and update all other dates) |

---

### Complete Example: Shortening to 4 Days (e.g., Wed–Sat only)

Remove `Mon`, `Tue` from all six locations. Everything else stays the same.

---

## Passing This On to the Next Manager

When you hand off Love Notes to the next manager, give them all of the following:

1. **This README** — walk them through it in person if possible
2. **The GitHub repository URL** and your login (or transfer ownership to their account)
3. **The Google Cloud Console login** — log in together and add their email as a Test User (Step B, Part 3, "Test users")
4. **The club Google account credentials** for both the Gmail account and the Calendar account
5. **The Google Form** — either transfer ownership or share it so they can duplicate it
6. **The OAuth Client ID** — it's saved in the tool's Setup tab, but write it down separately as a backup

> **Recommendation:** Keep the same GitHub repository and Google Cloud project from year to year. The only things that need updating annually are the Member Roster, Song URLs, and fundraiser dates — all of which are done through the tool's Setup tab without touching any code.

---

*Built for Medleys A Cappella at UCLA. Fundraising for the LA Downtown Women's Center.*
*Tool created 2026. Questions? Contact the Love Notes manager.*
