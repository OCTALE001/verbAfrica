# Forms Setup — Form-to-Sheet Plumbing

*v1 — 2026-06-09 | One-off setup. Once done, every enquiry and brief lands in your tracking sheet automatically.*

---

## What This Does

Every time someone clicks "Send Enquiry" or "Submit Brief" on the verbAfrica platform:
1. **A new row appears** in your booking tracker (Google Sheet).
2. **You get an email notification** with the details.

The Sheet is the source of truth. The email is the alarm bell. You can swap notification channels later without losing any data.

---

## The Setup (15 minutes, one-off)

You'll do steps 1–8. I'll do step 9 once you send me the URL from step 7.

### Step 1 — Create the tracking sheet

1. Go to https://sheets.google.com → click the **+ Blank** spreadsheet.
2. Rename it: **`verbAfrica Bookings`** (top-left corner).
3. Rename the first tab from "Sheet1" to **`Bookings`** (right-click the tab at the bottom).
4. **Paste this row** into row 1 (these are the column headers — match exactly):

```
Timestamp	Status	Type	Client name	Client contact	Event date	Event location	Scope / description	Creative(s) requested	Roster (after assembly)	Price (pass-through)	Confirmation date	Delay reason	Notes / learnings	Raw payload
```

The simplest way: copy that whole line, click cell A1, paste. Google Sheets will split it across A1–O1.

### Step 2 — Grab the Sheet ID

The Sheet ID is the long string in the URL between `/d/` and `/edit`. Example:

```
https://docs.google.com/spreadsheets/d/1AbCdEfGhIjKlMnOpQrStUvWxYz1234567890/edit
                                       └──────── this is the Sheet ID ──────────┘
```

Copy that ID — you'll need it in step 4.

### Step 3 — Open Apps Script

In your new sheet: **Extensions → Apps Script**. A new tab opens with a code editor.

Delete whatever's in the editor (usually just `function myFunction() {}`).

### Step 4 — Paste the script

Copy the entire code block below and paste it into the Apps Script editor.

**Then change line 2 and line 3** — replace the placeholder values with:
- `SHEET_ID` → the Sheet ID you copied in step 2
- `NOTIFY_EMAIL` → your email address (`alextober5@gmail.com`)

```javascript
const SHEET_ID = 'PASTE_YOUR_SHEET_ID_HERE';
const NOTIFY_EMAIL = 'alextober5@gmail.com';

function doPost(e) {
  try {
    const payload = JSON.parse(e.postData.contents);
    const sheet = SpreadsheetApp.openById(SHEET_ID).getSheetByName('Bookings');

    sheet.appendRow([
      new Date(),                                          // Timestamp
      'new',                                                // Status
      payload.type || '',                                   // Type
      payload.clientName || '',                             // Client name
      payload.clientContact || '',                          // Client contact
      payload.eventDate || '',                              // Event date
      payload.eventLocation || '',                          // Event location
      payload.message || payload.description || '',         // Scope / description
      payload.creativeName || payload.projectType || '',    // Creative(s) requested
      '',                                                   // Roster (filled later)
      '',                                                   // Price (filled later)
      '',                                                   // Confirmation date
      '',                                                   // Delay reason
      '',                                                   // Notes / learnings
      JSON.stringify(payload)                               // Raw payload (audit)
    ]);

    if (NOTIFY_EMAIL) {
      const subject = `verbAfrica · new ${payload.type || 'submission'} · ${payload.clientName || 'unknown'}`;
      const body = [
        `New ${payload.type || 'submission'} on verbAfrica`,
        ``,
        `From: ${payload.clientName || 'unknown'}`,
        `Contact: ${payload.clientContact || '—'}`,
        ``,
        `Creative(s): ${payload.creativeName || payload.projectType || '—'}`,
        `Event date: ${payload.eventDate || '—'}`,
        `Event location: ${payload.eventLocation || '—'}`,
        ``,
        `Message:`,
        payload.message || payload.description || '—',
        ``,
        `—`,
        `Logged to sheet at row ${sheet.getLastRow()}.`
      ].join('\n');
      MailApp.sendEmail(NOTIFY_EMAIL, subject, body);
    }

    return ContentService.createTextOutput(JSON.stringify({ok: true, row: sheet.getLastRow()}))
      .setMimeType(ContentService.MimeType.JSON);
  } catch (err) {
    return ContentService.createTextOutput(JSON.stringify({ok: false, error: err.toString()}))
      .setMimeType(ContentService.MimeType.JSON);
  }
}

function doGet() {
  return ContentService.createTextOutput(JSON.stringify({ok: true, hint: 'verbAfrica form endpoint — POST submissions here.'}))
    .setMimeType(ContentService.MimeType.JSON);
}
```

### Step 5 — Save the script

Press **Ctrl/Cmd + S** (or click the floppy-disk icon). Give the script project a name when prompted: **`verbAfrica Forms`**.

### Step 6 — Deploy as a web app

1. Top-right of the editor: click **Deploy → New deployment**.
2. Click the gear icon next to "Select type" and choose **Web app**.
3. Fill the form:
   - **Description:** `verbAfrica forms endpoint v1`
   - **Execute as:** Me (`alextober5@gmail.com`)
   - **Who has access:** **Anyone** *(important — the platform needs to POST anonymously)*
4. Click **Deploy**.
5. Google will ask you to authorise the script. Click through:
   - **Authorize access**
   - Pick your Google account
   - You may see a "Google hasn't verified this app" screen — click **Advanced → Go to verbAfrica Forms (unsafe)**. It's safe because you wrote it (well, pasted it). This warning shows for every personal script.
   - Click **Allow**.

### Step 7 — Copy the Web app URL

After deploying, you'll see a screen with a **Web app URL** that ends in `/exec`. Copy that whole URL.

It looks like: `https://script.google.com/macros/s/AKfycby.../exec`

### Step 8 — Send me the URL

Paste it into the chat. I'll wire it into the HTML.

### Step 9 — (Me)

I paste it into the `VA_CONFIG` block in `index.html`. Then we test by submitting a fake enquiry — a row should appear in your sheet and an email in your inbox within seconds.

---

## After Setup

### Testing

Once wired, fill out a test enquiry on a creative's profile and submit. Within a few seconds you should see:
- A new row in `verbAfrica Bookings` → `Bookings` tab
- An email in `alextober5@gmail.com`

### Updating the script later

If you need to change something (new notification email, additional fields, etc.):
1. Open the sheet → Extensions → Apps Script
2. Edit the code
3. **Deploy → Manage deployments → ✏️ (edit) → Version: New version → Deploy**

The URL stays the same across versions — no need to re-wire the HTML.

### Common issues

- **"Script function not found"** — you skipped Step 5 (save) or named the function wrong. The function must be called `doPost`.
- **"Unauthorized"** in the email or logs — you authorised, but Google forgot. Re-run Deploy → Manage deployments → edit → Deploy.
- **Submissions reach the sheet but no email** — your Google account's daily mail quota (~100/day) is exhausted, or `NOTIFY_EMAIL` has a typo.
- **Nothing reaches the sheet** — open the Apps Script editor → **Executions** tab on the left → check the latest run's logs. The error will be there.

### When the URL gets rotated

If you ever need a new URL (security, account change): just re-do steps 6–8. Send me the new URL, I'll swap it in `VA_CONFIG`. Past data stays in the sheet.

---

## What this *doesn't* do (yet)

- **No WhatsApp notification.** Google Apps Script can't send WhatsApp messages directly. Adding this later means using Twilio or similar; we'll do it when the volume justifies it.
- **No automated reply to the client.** The client sees a "thanks, we'll be in touch" confirmation in the browser, but no follow-up email is sent to them by the system. *You* reply manually — that's the concierge MVP by design.
- **No spam filtering.** If we get spam, we'll add a honeypot field in the form. Wait until it's a problem.

---

## Change Log

- **2026-06-09** — Initial setup guide.
