SCHOOLTRACK EAPIHS — SHARED ADMIN + GUEST INVENTORY
Maroon and green · Shared across phones · Offline cache with online sync

IMPORTANT
This is a starter web app package, not a preconfigured live service. It will NOT connect until you create a Firebase project and paste your project's Web App configuration. This is necessary because the school must own/control the database and administrator identity. Do not share passwords or service-account keys.

WHAT IT DOES
- One administrator UID can add, edit, and delete inventory and manage category choices.
- Signed-in guests can view the same shared inventory on their phones.
- Firestore security rules enforce write permissions on the server. Hiding buttons in the app is NOT the security boundary.
- Firestore offline persistence may show cached data when offline. New changes synchronize when a device reconnects. Login and first-time data loading require internet.
- This starter has item name, category, quantity, condition, location, responsible person, status, and recent activity.

SETUP (needs an adult/school-authorized project owner; easiest on a computer, but can be done in a phone browser)
1. Open https://console.firebase.google.com/ and create a project named SchoolTrack EAPIHS.
2. Add a Web App in Project settings and copy its firebaseConfig values.
3. In Authentication > Sign-in method, enable Email/Password and Anonymous.
4. In Authentication > Users, create the one administrator user with the school's chosen email and a strong password. Do not give admin credentials to guests.
5. Open that user and copy the Authentication UID.
6. In public/config.js, replace PASTE_FIREBASE_WEB_API_KEY, YOUR_PROJECT_ID, PASTE_FIREBASE_APP_ID, and PASTE_ADMIN_AUTH_UID_HERE with the real values. Keep the config file public; it is not a private key.
7. In firestore.rules, replace ADMIN_UID_HERE with the exact same administrator UID. Publish the rules in Firebase Console > Firestore Database > Rules.
8. Create Firestore Database in production mode. Publish the rules above. Do not leave test-mode rules active.
9. Host the contents of the public folder on an HTTPS static host (for example GitHub Pages). Upload firestore.rules separately into Firebase Console; it is not hosted as a website file.
10. Open the HTTPS site on each phone and choose Add to Home Screen / Install. Each guest taps Continue as guest. The admin signs in using the admin email and password.

HOW ADMIN-ONLY WORKS
- The app uses adminUid only to show admin controls in the UI.
- The real protection is firestore.rules, which checks request.auth.uid against one fixed administrator UID for every write.
- A guest changing JavaScript or pretending to be admin cannot write to Firestore because the server rules reject the request.
- Never use a shared admin password for guests. If the administrator account is compromised, revoke/reset it in Firebase Authentication and update the UID/rules if needed.

OFFLINE LIMITS
- Previously loaded data may remain readable from local cache when offline.
- Guests cannot receive other devices' newest updates without reconnecting.
- A new write made while offline may be queued by Firestore and checked by security rules when it reaches the server. Test carefully before school use.

FILES
public/index.html       User interface
public/style.css        Maroon/green styling
public/app.js           Firebase sign-in, shared inventory, UI
public/config.js        Your Firebase Web App configuration + admin UID
public/manifest.json    Installable PWA metadata
public/service-worker.js Offline app shell cache
firestore.rules         Server-enforced admin-only permissions


ADDED IN THIS UPDATE
- EAPIHS is shown prominently on the login screen.
- The configured administrator email is displayed on the login screen. Edit public/config.js and replace SET_SCHOOL_AUTHORIZED_ADMIN_EMAIL_HERE with the real school-authorized administrator email.
- Button click sounds are enabled by default, with a checkbox to turn them off.
- The offline app-shell cache includes local JavaScript and CSS files.

IMPORTANT LIMITS
- The administrator email shown is informational. The actual login and write permission still depend on Firebase Authentication and the administrator UID in config.js and firestore.rules.
- Firebase login and first-time data loading require internet. Cached data may be available offline after it has loaded once. Test offline writes and synchronization before school use.
- Firebase placeholders must be configured before the shared inventory will connect.


LOGIN BEHAVIOR
- Administrator: enter the school-authorized administrator email and password.
- Guest: tap “Continue as guest”; no email or password is required.
- Guest accounts are view-only. Firebase must be configured for authentication and shared inventory to work.
