# Grocery Shelf Manager

A personal grocery presence tracker using Google Sheets as the database and Google Apps Script to serve the app. Source files can be version-controlled in GitHub. This version is intentionally hosted by Apps Script so the frontend can call the backend securely through `google.script.run` without exposing a public Sheets API.

## Features
- Dashboard: unique active items present, upcoming/expired items with expiry dates, and restock suggestions.
- Seven locations: Rack 1–Rack 6 and Fridge.
- Weekly review per location: check Present and optional expiry date, then save once.
- Master List with browser autocomplete suggestions.
- Restock suggestions: active items absent from all seven locations.
- Hide action moves an item from MasterList to HideList; it no longer appears in active autocomplete or restock suggestions.
- Restore action returns a hidden item and its prior location statuses.
- No quantities or consumption calculations.

## Setup
1. Create a project at https://script.google.com/ and add two files: `Code.gs` and `Index.html`.
2. Copy the contents of the matching files from this folder into the Apps Script project.
3. In Apps Script, select `setup` in the function dropdown and click **Run**. Approve the Google permissions. The function creates a spreadsheet named **Grocery Shelf Manager Database** with the tabs `MasterList`, `ShelfStatus`, and `HideList`. It returns the spreadsheet URL in the execution log.
4. In **Project Settings**, set the time zone to your local time zone (for India, `Asia/Kolkata`) so expiry dates are interpreted consistently.
5. Select **Deploy → New deployment → Web app**.
6. Set **Execute as** to **Me**. Set access to **Only myself** for a personal app. Deploy and authorize if prompted.
7. Open the Web app URL. Bookmark it on your phone and optionally add it to the home screen.

## GitHub source control
Create a private GitHub repository and upload `Code.gs`, `Index.html`, and `README.md`. Apps Script does not natively deploy directly from GitHub. For a simple first version, copy changes from GitHub into Apps Script. If you later want automated deployment, use `clasp` from a local development environment.

## Data model
- `MasterList`: `itemId`, `name`, `category`, `createdAt`
- `ShelfStatus`: `itemId`, `location`, `present`, `expiryDate`, `updatedAt`
- `HideList`: `itemId`, `name`, `category`, `hiddenAt`, `statusBackupJson`

Every new active item receives one status row per location. A location review updates only that location. An item is considered present if it is checked at one or more locations. Restock suggestions contain active items with no present status anywhere.

## Notes
- The app does not track quantities or consumption.
- Expiry warnings show items marked Present with an expiry date that is within the next 30 days; already-expired items are also included and labeled.
- Hiding an item removes it from the active list and removes its active shelf status rows, but stores a backup in `HideList` so it can be restored.
- This first version is for a single user. Keep the web app access restricted to your own Google account. Do not deploy as “Anyone” unless you intentionally redesign access control.
