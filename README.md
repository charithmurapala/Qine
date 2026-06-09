# Qinematics Website — Complete Setup Guide
### Your site will be live at: https://qinematics.vercel.app

---

## 📁 FOLDER STRUCTURE

```
qinematics/
├── index.html          ← Your website (all code is here)
├── vercel.json         ← Vercel deployment config (don't touch)
├── assets/
│   ├── favicon.svg     ← Your logo icon (browser tab)
│   ├── og-cover.jpg    ← Preview image when sharing link on WhatsApp/social (1200×630px)
│   ├── cafe-1.jpg      ← Your portfolio photos & videos go here
│   ├── cafe-2.jpg
│   └── ...
└── README.md           ← This file
```

---

## STEP 1 — ADD YOUR PHOTOS & VIDEOS (5 minutes)

1. Put all your photo/video files inside the `assets/` folder
2. Open `index.html` in Notepad or any text editor
3. Search for `data-src="assets/cafe-1.jpg"` (Ctrl+F)
4. Replace `cafe-1.jpg` with your actual filename
5. For every portfolio card, update:
   - `data-src="assets/YOUR-FILE.jpg"` ← photo file
   - `data-src="assets/YOUR-FILE.mp4"` ← video file
   - `data-thumb="assets/YOUR-THUMB.jpg"` ← video thumbnail
   - `data-title="Your Project Name"`
   - `data-cat="cafe"` or `"saas"` or `"premium"` or `"brand"`
   - `data-type="photo"` or `"video"`

> TIP: Compress photos to under 500KB using https://squoosh.app
> TIP: Compress videos using https://www.freeconvert.com/video-compressor

---

## STEP 2 — GOOGLE SHEETS SETUP (10 minutes)
*(Every form submission saved as a spreadsheet row)*

1. Go to https://sheets.google.com → Create a new spreadsheet
2. Name it: **Qinematics Enquiries**
3. Add these headers in Row 1 (one per column):
   ```
   Date | Name | Brand | Email | Phone | Service | Budget | Message
   ```
4. Click **Extensions** → **Apps Script**
5. Delete all existing code, paste this EXACTLY:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var data  = JSON.parse(e.postData.contents);
  sheet.appendRow([
    data.date,
    data.name,
    data.brand,
    data.email,
    data.phone,
    data.service,
    data.budget,
    data.message
  ]);
  return ContentService
    .createTextOutput(JSON.stringify({status:"ok"}))
    .setMimeType(ContentService.MimeType.JSON);
}
```

6. Click **Save** (floppy disk icon), name it anything (e.g. "Qinematics")
7. Click **Deploy** → **New deployment**
8. Click the gear ⚙️ next to "Type" → select **Web app**
9. Set:
   - Description: Qinematics Form
   - Execute as: **Me**
   - Who has access: **Anyone**
10. Click **Deploy** → Click **Authorize access** → Choose your Google account → Allow
11. Copy the **Web app URL** (looks like: `https://script.google.com/macros/s/ABC.../exec`)
12. Open `index.html`, find this line:
    ```
    SHEET: 'YOUR_GOOGLE_APPS_SCRIPT_URL',
    ```
    Replace with your URL:
    ```
    SHEET: 'https://script.google.com/macros/s/YOUR_ACTUAL_URL/exec',
    ```

---

## STEP 3 — EMAIL SETUP with EmailJS (10 minutes)
*(Every submission sent to your Gmail inbox)*

1. Go to https://emailjs.com → **Sign up free** (use your Gmail)
2. Click **Email Services** → **Add New Service** → **Gmail**
3. Click **Connect Account** → sign in with the Gmail you want to receive on
4. Copy the **Service ID** (looks like: `service_abc123`)
5. Click **Email Templates** → **Create New Template**
6. Set Subject to: `New Qinematics Enquiry from {{from_name}}`
7. Set the body to:
   ```
   New project enquiry on Qinematics!

   Name:    {{from_name}}
   Brand:   {{brand}}
   Email:   {{from_email}}
   Phone:   {{phone}}
   Service: {{service}}
   Budget:  {{budget}}
   Date:    {{date}}

   Message:
   {{message}}
   ```
8. Click **Save**
9. Copy the **Template ID** (looks like: `template_xyz456`)
10. Click your profile (top right) → **Account** → copy **Public Key**
11. Open `index.html`, find the CFG section and fill in:
    ```javascript
    EJ_PUB:  'your_public_key_here',
    EJ_SVC:  'service_abc123',
    EJ_TPL:  'template_xyz456',
    EJ_TO:   'your.gmail@gmail.com',
    ```

---

## STEP 4 — WHATSAPP SETUP with CallMeBot (5 minutes)
*(Instant WhatsApp notification when someone enquires)*

1. Save this number in your phone contacts: **+34 644 68 63 91** (name it "CallMeBot")
2. Open WhatsApp → send this exact message to that number:
   ```
   I allow callmebot to send me messages
   ```
3. Wait 2–3 minutes → you'll receive a reply with your **API Key** (e.g. `12345`)
4. Open `index.html`, find the CFG section and fill in:
   ```javascript
   WA_NUM: '91XXXXXXXXXX',   // your number with country code, no + or spaces
                              // Example: '919876543210'
   WA_KEY: '12345',          // the API key CallMeBot sent you
   ```

---

## STEP 5 — UPDATE YOUR CONTACT DETAILS (2 minutes)

In `index.html`, search and replace these placeholders:

| Find                        | Replace with                    |
|-----------------------------|---------------------------------|
| `hello@qinematics.in`       | your actual email               |
| `+91 XXXXX XXXXX`           | your WhatsApp number            |
| `91XXXXXXXXXX`              | your number (WhatsApp link)     |
| `Chennai, Tamil Nadu`       | your city (if different)        |
| `₹25,000`                   | your actual starting price      |

---

## STEP 6 — DEPLOY TO VERCEL (10 minutes — one time only)
*(Your site goes live at qinematics.vercel.app)*

### Method A: Drag & Drop (Easiest — no coding needed)

1. Go to https://vercel.com → click **Sign Up** → sign up with Google
2. On the dashboard, look for **"Add New… → Project"**
3. Click **"Deploy from file upload"** or drag your entire `qinematics` folder
4. Wait ~30 seconds
5. Vercel gives you a URL like `qinematics.vercel.app` 🎉

### Method B: Via GitHub (Recommended — lets you update easily)

1. Create free account at https://github.com
2. Click **+** → **New repository** → name it `qinematics` → Public → Create
3. Click **uploading an existing file** → drag your entire `qinematics` folder → Commit
4. Go to https://vercel.com → Sign up with GitHub
5. Click **New Project** → select your `qinematics` repo → **Deploy**
6. Your site is live! Next time you update files on GitHub, the site auto-updates.

---

## STEP 7 — SHARE YOUR WEBSITE

Your site is now live at: **https://qinematics.vercel.app**

Share it anywhere:
- WhatsApp: paste the link directly
- Instagram: put in bio
- Email signature: add as a hyperlink
- Google Business: add as website

---

## UPDATING YOUR PORTFOLIO LATER

When you shoot new projects:
1. Add your new photos/videos to the `assets/` folder
2. In `index.html`, edit the portfolio card `data-src` and `data-title`
3. Save → upload to GitHub (or re-drag to Vercel)
4. Site updates live within 60 seconds

---

## TROUBLESHOOTING

| Problem | Fix |
|---------|-----|
| Photos not showing | Check filename matches exactly (case-sensitive). `Cafe.jpg` ≠ `cafe.jpg` |
| Form not sending | Double-check all keys are pasted correctly, no extra spaces |
| WhatsApp not working | Make sure you sent the activation message first and waited 2+ mins |
| Sheets not saving | Re-deploy your Apps Script with "Anyone" access |
| Site not updating | Hard refresh: Ctrl+Shift+R (Windows) or Cmd+Shift+R (Mac) |

---

## NEED HELP?

All three services are free:
- EmailJS free tier: 200 emails/month
- CallMeBot: unlimited WhatsApp messages
- Google Sheets: unlimited rows
- Vercel: unlimited hosting

For more than 200 emails/month, upgrade EmailJS ($9/month) or switch to Formspree.
