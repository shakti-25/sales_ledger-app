# Daily Sales Ledger (Google sign-in version)

A static website. Sign-in uses Google (Firebase Authentication) and records live in Firestore.
Location capture works here because the page runs as a normal website, not inside an embedded frame.

## What you need
A Google account and about 15 minutes. The free Firebase plan is enough for a small team.

## Setup

1. **Create a Firebase project** at https://console.firebase.google.com (Add project).
2. **Add a web app**: Project overview > the `</>` icon. Copy the config shown and paste the values into `firebase-config.js`.
3. **Turn on Google sign-in**: Build > Authentication > Get started > Sign-in method > Google > Enable.
4. **Create the database**: Build > Firestore Database > Create database. Pick a Mumbai region (`asia-south1`) and start in production mode.
5. **Add the security rules**: Firestore > Rules tab. Replace everything with the contents of `firestore.rules` and press Publish.
6. **Create the first admin** (one time, by hand): Firestore > Start collection `allowedUsers`.
   - Document ID: your Google email in lowercase, for example `you@gmail.com`
   - Fields: `role` (string) = `admin`, `name` (string) = your name
7. **Host the site** (pick one):
   - **Firebase Hosting**: install Node.js, then run `npm i -g firebase-tools`, `firebase login`, `firebase init hosting` (public directory: `.`, single-page app: No, do not overwrite index.html), and `firebase deploy`.
   - **Render**: put these files in a GitHub repository, create a **Static Site** on Render, leave the build command empty and set the publish directory to `.`.
8. **Authorise your website address**: Authentication > Settings > Authorized domains > Add domain. Add your site address (for example `your-app.onrender.com`). Firebase Hosting domains are already included.

## First use
1. Open your site and sign in with the Google account you made admin.
2. Open the menu > **Manage team** and add each salesperson's Google email, with the role Rep (or Admin).
3. Only people on that list can sign in. Everyone else sees "not on the access list".
4. The default PIN is `1234`. Admins can change it from the menu (Change PIN).

## Roles
- **Rep**: sees all records, adds schedules, edits or deletes only their own (PIN needed), opens Rep analysis (PIN needed).
- **Admin**: all of the above for every record, plus Manage team, Change PIN and sample data.

## Notes
- The PIN is an extra screen lock in the app. The real access control is Google sign-in plus the Firestore rules.
- Automatic place names from GPS use the free OpenStreetMap Nominatim service, which suits a small team. For heavier use, switch to a paid geocoding API.
- The Firebase config values are not secrets, so it is fine that they are visible in the site code.
