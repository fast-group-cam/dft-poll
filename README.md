# 🏆 DFT March Madness: Water-Solid Interfaces

A dynamic, gamified polling web application designed to benchmark community preferences for Density Functional Theory (DFT) setups, specifically for water-solid interfaces. 

This app serves random head-to-head matchups between Functionals, Dispersion Corrections, and DFT Codes, collecting the data into a Google Sheet for analysis.

## 🚀 How it Works
1. Users are presented with a random head-to-head matchup.
2. The matchups are weighted: 50% Functionals, 25% Dispersion Corrections, 25% Codes.
3. Users click their preferred winner or use the write-in box.
4. The vote is instantly sent to a Google Sheet via a background POST request.
5. The poll ends after 10 rapid-fire rounds to maximize completion rates.

---

## 🛠️ Setup Guide: The Database (Google Sheets)

We use Google Apps Script as a free, serverless backend to catch the votes.

1. **Create the Sheet:** Create a new Google Sheet.
2. **Set Headers:** In row 1, name columns A through E:
   `Timestamp` | `Category` | `Winner` | `Loser` | `Write-in`
3. **Add the Code:** * Click **Extensions > Apps Script**.
   * Delete any default code and paste the following:

   ```javascript
   function doPost(e) {
     var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
     
     var rowData = [
       new Date(),
       e.parameter.category,
       e.parameter.winner,
       e.parameter.loser,
       e.parameter.writein
     ];
     
     sheet.appendRow(rowData);
     return ContentService.createTextOutput("Success").setMimeType(ContentService.MimeType.TEXT);
   }
   ```
4. **Deploy the Web App:**
   * Click **Deploy > New deployment**.
   * Select **Web app**.
   * Execute as: **Me**.
   * Who has access: **Anyone** *(Crucial step!)*.
   * Click **Deploy** and authorize the permissions.
   * **Copy the resulting Web App URL.**

*(Note: If you ever change the Apps Script code in the future, you MUST deploy a "New version" for the changes to take effect.)*

---

## 💻 Setup Guide: The Frontend (HTML/JS)

1. Open `index.html`.
2. Locate the `submitVote()` function near the bottom of the file.
3. Find the `scriptURL` variable and replace the placeholder with your Google Web App URL:
   ```javascript
   const scriptURL = 'YOUR_GOOGLE_WEB_APP_URL_HERE';
   ```
4. Save the file.

---

## 🌐 Deployment (GitHub Pages)

1. Upload `index.html` to the `main` branch of this repository.
2. Go to your repository **Settings**.
3. On the left sidebar, click **Pages**.
4. Under *Build and deployment*, set the Source to **Deploy from a branch**.
5. Select the **main** branch and click **Save**.
6. Wait 1-2 minutes. GitHub will provide a live URL where your poll is hosted.

---

## 🔧 Updating the Poll (Future-Proofing)

If certain write-ins become highly popular and you want to add them to the official head-to-head rotation, you do not need to rewrite any logic. 

Simply open `index.html`, find the `tournamentData` object, and add the new method to the appropriate array as a string. 

**Example:**
```javascript
"Plane-Wave Powerhouses": ["VASP", "Quantum ESPRESSO", "CASTEP", "ABINIT", "Qbox"]
```
Because the JavaScript dynamically measures array length, the new option will immediately enter the random draw rotation once you push the update to GitHub.
