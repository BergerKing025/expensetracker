# Evenly

A two-person shared expense tracker that runs as a web app on GitHub Pages and installs on an iPhone home screen.

## How it works

- The app is static files (`index.html` and friends) hosted on GitHub Pages.
- Expenses are stored in one JSON file (`ledger.json`) in a **separate, private** GitHub repo.
- Each phone talks to that repo directly through the GitHub API using a shared access token.
- Every save pulls the latest file, merges your changes with anything your partner added, and pushes. Edits made offline sync when you reconnect.

## Setup (about 10 minutes)

### 1. Create the private data repo
1. On GitHub, create a new repository, e.g. `evenly-data`.
2. Set it to **Private** and tick **Add a README** (the repo must not be empty).

### 2. Create the access token
1. GitHub > Settings > Developer settings > Personal access tokens > **Fine-grained tokens** > Generate new token.
2. Repository access: **Only select repositories** > `evenly-data`.
3. Permissions > Repository permissions > **Contents: Read and write**.
4. Choose the longest expiration offered and add a calendar reminder to renew it.
5. Copy the token (starts with `github_pat_`). Share it with your partner over a private channel.

### 3. Publish the app
1. Create a second repository, e.g. `evenly` (public is fine; it contains no data or secrets).
2. Upload all files from this folder: `index.html`, `manifest.webmanifest`, `sw.js`, `icon-180.png`, `icon-192.png`, `icon-512.png`.
3. Settings > Pages > Deploy from a branch > `main` / root > Save.
4. Your app will be at `https://YOURNAME.github.io/evenly/`.

### 4. Install on each iPhone
1. Open the Pages URL in **Safari**.
2. Share button > **Add to Home Screen**.
3. Open Evenly from the home screen, enter `yourname/evenly-data` and the token.
4. First person: enter both names. Second person: tap which one they are.

## Using it

- **Add purchase**: description, date, total, who paid, and who is responsible.
  - *Split evenly*: the other person owes half.
  - *The other person*: the payer bought it entirely for them, so they owe the full amount.
  - *The payer*: their own purchase, nobody owes anything.
- The top of the screen shows the running balance between you.
- **Settle up** records one payment that zeroes the balance. Settled purchases are dimmed. Tap a settlement to undo it.
- Tap any purchase to edit or delete it.

## Security notes

- **Never put the token in the public app repo.** The app asks for it on each phone and keeps it only in that phone's local storage.
- Keep the data repo private. Scope the token to that one repo with only Contents access.
- If the token leaks or expires, create a new one and re-enter it under Settings > Disconnect, then reconnect.
- Anyone with the token can read and edit the expenses, so treat it like a shared password.

## Notes

- Currency is set at the top of the script (`CURRENCY = 'USD'`).
- Deleted items are kept as markers in the file so deletions sync correctly between phones.
- The Contents API handles files up to 1 MB, which is several thousand entries.
