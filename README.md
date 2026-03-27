# 💌 Love Notes Manager
### Medleys A Cappella — Fundraiser Automation Tool

This tool automates the behind-the-scenes work for **Love Notes**, Medleys A Cappella's annual singing valentine fundraiser for the LA Downtown Women's Center. It handles availability matrix generation, order management, Slack announcement drafting, confirmation message writing, Google Calendar event creation, and virtual love note email drafting.

---

## Table of Contents

1. [What This Tool Does](#what-this-tool-does)
2. [Annual Setup (Do This Each Year)](#annual-setup-do-this-each-year)
3. [Week-of Workflow](#week-of-workflow)
   - [Before Orders Open: Generate the Availability Matrix](#before-orders-open-generate-the-availability-matrix)
   - [As Orders Come In: Order Dashboard](#as-orders-come-in-order-dashboard)
   - [Each Night: Review and Export](#each-night-review-and-export)
4. [File Formats Reference](#file-formats-reference)
5. [Troubleshooting](#troubleshooting)
6. [Passing This On to the Next Manager](#passing-this-on-to-the-next-manager)

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
2. Once all members have filled it out, go to your When2Meet and, in the URL, add "&csv" at the end of the URL. That will download the csv file of the When2Meet.
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
   - **Confirmation Message:** click "📋 Copy" → paste into google voice → send to the customer's phone number the morning of their performance
   - **Google Calendar Event:** click **"📅 Create Event"** → the event appears on the club calendar immediately. Verify the time and info are correct before creating.
5. **For Virtual Love Notes** (click the "💻 Virtual" tab):
   - Click **"📧 Create Gmail Draft"** for each virtual order
   - Each draft will appear in the club Gmail's Drafts folder, ready to review and schedule
   - Schedule them to send on February 14th at 6:00 AM

---

## File Formats Reference

### When2Meet CSV
- Export directly from When2Meet by adding "&csv" at the end of the URL
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
Make sure you've enabled the Gmail API and Google Calendar API in your Google Cloud project. Also make sure the account you're signing in with is listed as a Test User. (Ask a previous love note manager about this if you don't understand what this means).

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

**If you add a new day:**
Add a new time column entry in the Column Mapping section, and add the corresponding day tab logic. (This requires editing the HTML file — ask a technical member of the group for help, or contact the previous manager.)

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
