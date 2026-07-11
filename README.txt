DOC SCANNER PWA — SETUP GUIDE
==================================

1. GOOGLE APPS SCRIPT (Free Backend)
------------------------------------
   a) Create a new Google Sheet (name it anything, e.g. "Scans")
   b) Click Extensions → Apps Script
   c) Delete the default code and paste this exact script:

      function doPost(e) {
        var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
        var text = e.parameter.text || '';
        var date = e.parameter.date || new Date().toLocaleString();
        sheet.appendRow([date, text.substring(0,500), text]);
        return ContentService.createTextOutput(JSON.stringify({
          success: true, message: 'Saved'
        })).setMimeType(ContentService.MimeType.JSON);
      }

   d) Click Deploy → New deployment
      - Type: Web app
      - Execute as: Me
      - Who has access: Anyone
      - Click Deploy and copy the Web App URL

   e) Paste that URL into the app settings field.

2. DEPLOY THE PWA (Required for Install)
----------------------------------------
   You MUST serve this over HTTPS for Android to offer "Install".
   Easiest free method: GitHub Pages

   a) Create a new GitHub repository
   b) Upload ALL files from this folder (index.html, manifest.json,
      service-worker.js, icon-192.png, icon-512.png)
   c) Go to Settings → Pages → Source: Deploy from a branch → main
   d) Wait 1 minute, then visit the provided URL on your Android phone
   e) Chrome will show an "Add to Home Screen" banner (or tap ⋮ → Install)

3. FIRST RUN (Important)
----------------------
   The first time you open the app, you MUST be online so the browser
   can cache:
   - The app itself (offline capable)
   - Tesseract.js OCR engine
   - English language model (~10 MB)
   After that, OCR works completely offline.

4. OFFLINE BEHAVIOR
-------------------
   - If you scan while offline, the text is saved to an internal queue.
   - A red badge shows "X scans queued".
   - When you reconnect to WiFi/mobile data, it auto-syncs to Sheets.
   - You can also tap "Sync Now" manually.

5. FREE FOREVER?
----------------
   Yes. No servers, no hosting fees, no API costs.
   - GitHub Pages: Free
   - Google Apps Script: Free
   - Google Sheets: Free
   - Tesseract.js: Free (runs on your phone)

TIPS
----
   - Use good lighting for best OCR accuracy.
   - Keep the document flat and fill the frame.
   - The app requests the back camera by default.
