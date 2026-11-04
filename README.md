# 🌿 explove

A free, private web app for a group of people to keep shared lists of **movies, restaurants, places, activities and recipes**: what you want to do, what you've already done, and how you liked it.

It works for any small group: friends, family, flatmates, or just two people. Everyone gives their own 1–5 star rating and comment, so you can see at a glance where you agree and where you don't.

It runs in the browser, on a computer or a phone. You can add it to your home screen like an app, iPhone included. Nothing to install and no code to write: all you need is a GitHub account and a free Firebase project.

**Make it yours:** the first time you sign in, the app asks you to **choose a title**, so your copy can have any name you like. The original copy is called *Basilico napoletano*.

> **Want to use it? Make your own copy.** This repository runs the author's own app, connected to the author's Firebase project, and only the author's accounts can sign in to it. To use the app, create your own copy in your GitHub account and connect it to your own free Firebase project, as described in [Setup](#setup). Please keep the credit to the original author (see [License and credit](#license-and-credit)).

---

## Features

- **Five sections:** Movies, Restaurants, Places, Activities and Recipes. Each item has a "to do" / "done" status, fields that fit the section, and free-form notes.
- **Everyone's reviews:** each person gives their own stars and comment. Other people's reviews stay hidden until you've added yours, so nobody is influenced. Your saved review can be edited whenever you want.
- **Map:** restaurants and places appear on an OpenStreetMap map. To set a location you can search an address, tap the map, use your current location, or paste a Google Maps link. Every item has "Open in Google Maps" and "Directions" links.
- **Visit dates:** marking something as done saves the date. You can also add repeat visits or past dates.
- **Recipe photos:** up to 8 photos per person per recipe, with thumbnails shown right in the list.
- **Links:** save links on any item, such as the online recipe, the menu or the trailer.
- **Stats:** how much your ratings match each person's, the biggest disagreements, rating distribution, the group's top items per section, what you did month by month, and what's waiting for your rating.
- **Your own title:** on first sign-in the app asks you to name it. Anyone can rename it later by tapping the title at the top. The name is shared, so everyone sees the same one.
- **Google sign-in:** only the email addresses you list can get in.

## What you need

- **A Google account for each person**, for signing in.
- **A GitHub account**, to publish the page. One person in the group is enough.
- **A Firebase project on the free "Spark" plan.** No credit card required.

Time needed: about 15–20 minutes, once.

---

## Setup

### 1. Copy the repository

At the top of this GitHub page, click **Fork** (or **Use this template**, if available) to create **your own copy in your account**. Fork is preferred: GitHub then shows a link back to the original on your copy. All the steps below happen in your copy and your own Firebase project. Don't change the original repository: it is the author's live app.

Keep the `LICENSE` and `NOTICE` files in your copy.

The repo must be **public** to use GitHub Pages for free. That's fine: the code contains no passwords, and your data stays in Firebase, protected by the rules from step 4.

### 2. Create the Firebase project

1. Go to [console.firebase.google.com](https://console.firebase.google.com) and click **Create a project** (or **Add project**). Any name works, and you can turn Google Analytics off.
2. Open the project settings: **gear icon ⚙️ → Project settings → General**.
3. At the bottom of the page, in the **Your apps** section, click the **`</>`** (Web) icon.
4. Give the app any nickname. Do **not** tick "Firebase Hosting". Click **Register app**.
5. Firebase shows you a code block containing `const firebaseConfig = { ... }`. Keep it open: you'll need it in step 5.

### 3. Create the Realtime Database

1. In the left menu, open **Realtime Database** (also listed under *Databases & Storage*) and click **Create database**.
2. Choose a location, for example `europe-west1` if you're in Europe.
3. Choose **locked mode**. You'll set the proper rules in the next step.

The app uses the **Realtime Database**, not Firestore.

### 4. Set the access rules

In the Realtime Database, open the **Rules** tab. Replace everything with the block below, put the Google emails of everyone in your group in **lowercase**, and click **Publish**:

```json
{
  "rules": {
    ".read": "auth != null && auth.token.email_verified == true && (auth.token.email.toLowerCase() == 'first.person@gmail.com' || auth.token.email.toLowerCase() == 'second.person@gmail.com' || auth.token.email.toLowerCase() == 'third.person@gmail.com')",
    ".write": "auth != null && auth.token.email_verified == true && (auth.token.email.toLowerCase() == 'first.person@gmail.com' || auth.token.email.toLowerCase() == 'second.person@gmail.com' || auth.token.email.toLowerCase() == 'third.person@gmail.com')"
  }
}
```

The example lets three people in. To add a person, add another `|| auth.token.email.toLowerCase() == 'their.email@gmail.com'` before the closing `)`. To remove one, delete their `|| ...` part. Keep the same list on **both** lines, `.read` and `.write`.

Anyone who isn't on the list and tries to sign in will see "doesn't have access to this app". You can come back and change the list whenever someone joins or leaves the group.

### 5. Put your configuration in `config.js`

All the settings for your copy live in one small file, `config.js`. It's the only file you need to change. Right now it holds the original author's Firebase project and data, so you replace all of it.

1. In your repo on GitHub, open `config.js` and click the **pencil** icon (Edit).
2. Delete everything in the file and paste this in its place:

   ```js
   /* Settings for this copy of the app. */

   /* Your Firebase project (Firebase console → Project settings → General → Your apps) */
   export const firebaseConfig = {
     apiKey: "...",
     authDomain: "your-project.firebaseapp.com",
     databaseURL: "https://your-project-default-rtdb.europe-west1.firebasedatabase.app",
     projectId: "your-project",
     storageBucket: "your-project.firebasestorage.app",
     messagingSenderId: "...",
     appId: "..."
   };

   /* Name shown until someone picks one in the app */
   export const DEFAULT_TITLE = "explove";
   ```

3. Replace the values inside `firebaseConfig` with your project's values from step 2. Keep the word `export` at the start.
4. Click **Commit changes**.

The block must also include `databaseURL`. If Firebase didn't show it to you, you'll find it at the top of the Realtime Database page (e.g. `https://your-project-default-rtdb.europe-west1.firebasedatabase.app`).

The `firebaseConfig` values aren't secrets and can safely live in a public repo: security comes from the rules in step 4.

### 6. Enable Google sign-in

1. In the Firebase menu, open **Authentication → Get started**.
2. In the **Sign-in method** tab, choose **Google**, turn on **Enable**, pick a support email, and click **Save**.

### 7. Publish the page with GitHub Pages

1. In your repo, go to **Settings → Pages**.
2. Under **Source**, choose **Deploy from a branch**, then `main` and `/ (root)`, and click **Save**.
3. After about a minute, **"Your site is live at …"** appears at the top of the same page. That's your app's link, in the form `https://YOUR-USERNAME.github.io/REPO-NAME/`.

### 8. Authorize the domain in Firebase

In Firebase, go to **Authentication → Settings → Authorized domains**, click **Add domain**, and enter:

```
YOUR-USERNAME.github.io
```

No `https://` and no repo name. If you skip this step, Google sign-in won't work.

### 9. First sign-in

Open the link, click **Sign in with Google**, and sign in.

The app then asks you to **name it**. Type a new title (for example *Movie Club* or *Family Recipes*) and tap **Save name**. Everyone in the group will see this title at the top of the page and in the browser tab. If you tap **Not now**, the app keeps its default name, and anyone can still rename it later by tapping the title.

Send the same link to the rest of the group. Each person appears in the app after their first sign-in, as long as their email is in the rules from step 4.

---

## Using it on your phone like an app

**iPhone:**
1. Open the link in **Safari**.
2. Tap **Share**, then **Add to Home Screen**.
3. The first time, sign in again inside the app.

**Android:**
1. Open the link in **Chrome**.
2. From the **⋮** menu, choose **Add to Home screen** (or **Install app**).

If you change the icons, remove the home screen shortcut and add it again to see the new ones.

---

## Customizing

- **App name:** choose it on first sign-in, or tap the title at the top of the app at any time to type a new one. No code changes needed.
- **Default name and home screen name (optional):** the title you choose in the app doesn't change the default name or the name under the home screen icon. To change those too, edit:
  - `DEFAULT_TITLE` in `config.js` (shown before anyone has chosen a name);
  - `apple-mobile-web-app-title` in `index.html` (name under the icon on iPhone);
  - `name` and `short_name` in `manifest.webmanifest` (name under the icon on Android).
- **Who can get in:** edit the email list in the Realtime Database rules (step 4).
- **Icon:** replace `apple-touch-icon.png` (180×180), `icon-192.png` and `icon-512.png` with square PNG images without transparency, keeping the same file names.
- **Sections, colors and fields:** defined in `index.html`, in the `KINDS` object and in CSS variables such as `--movie` and `--restaurant`. Each person's color comes from `PERSON_COLORS`.

## Updating the app

Every change you commit on GitHub is published in about a minute. To see the new version right away:
- on a computer, reload with **Ctrl+Shift+R** (**Cmd+Shift+R** on Mac);
- on a phone, close and reopen the app.

---

## Limits and costs

Everything runs on free plans.

- **Firebase Spark:** 1 GB of stored data and 10 GB of downloads per month, plenty for a small group. Photos are resized to about 200 KB each and only downloaded when you open that recipe.
- **Group size:** there's no fixed limit, but the app is designed for small groups. The header shows the first four people, and the lists show the first few reviews of each item, with a "+N more" note for the rest.
- **Map and address search:** use the free public services of [OpenStreetMap](https://www.openstreetmap.org/copyright) and Nominatim, which are meant for light use like this. The map is detailed down to street level, but it isn't Google Maps: for directions and opening hours, use the Google Maps links.
- **No notifications:** you see other people's updates when you open the app.

## Privacy

- Your data (items, ratings, comments, photos, dates) lives in **your own** Firebase project, and only the emails listed in the rules can access it.
- Everyone on the list can see, edit and delete every item. Hiding other people's reviews until you've added yours is a display feature, not a privacy one.
- Address search sends the text you search for to Nominatim (OpenStreetMap).
- Map tiles are downloaded from OpenStreetMap's servers.

## Troubleshooting

| What happens | What to check |
|---|---|
| "…doesn't have access to this app" | Is the email spelled correctly and in lowercase on **both** lines of the Realtime Database rules? Did you click **Publish**? The rules go in the *Realtime Database*, not in Firestore. |
| "This site isn't authorized in Firebase yet", or Google sign-in doesn't open | Is `YOUR-USERNAME.github.io` in the **authorized domains** (step 8)? Is the Google provider enabled (step 6)? Does `config.js` have your project's values, not the original author's (step 5)? |
| "Sign-in failed: auth/api-key-not-valid…" | There's a typo in `apiKey` in `config.js`. Paste your project's `firebaseConfig` again (step 5). |
| "Connection problem", or the list doesn't load | Is `databaseURL` in `config.js` correct? Does the Realtime Database exist? |
| The page stays blank | Open `config.js` and check that it still starts with `export const firebaseConfig = {` and that every value is in quotes. |
| On iPhone, from the home screen app, sign-in doesn't return to the app | In `index.html`, delete the line `<meta name="apple-mobile-web-app-capable" content="yes">`. In the manifest, change `"display": "standalone"` to `"display": "browser"`. Then add the app to the home screen again. |
| I still see the old version | Reload with Ctrl+Shift+R, or close and reopen the app. GitHub Pages can take a couple of minutes. |
| The map is grey | Check your connection. If OpenStreetMap is having temporary issues, try again later. |
| The app didn't ask me to choose a name | It only asks while no name has been saved, once per device. Someone else in the group may have chosen one already, or you tapped **Not now**. Tap the title at the top to rename it. |
| Someone in the group doesn't appear | People appear after their first sign-in. Check that their email is in the rules (step 4). |

---

## Repository layout

```
config.js               your settings: Firebase project and default name (the only file to edit)
index.html              the app (HTML, CSS and JavaScript in a single file)
manifest.webmanifest    name and icons for installing on a phone
apple-touch-icon.png    iPhone icon (180×180)
icon-192.png            Android and browser icon
icon-512.png            large Android icon
LICENSE                 Apache License 2.0
NOTICE                  credit to the original author (keep it in your copy)
```

## License and credit

Created by **Francesca Vergara** ([fvergara39](https://github.com/fvergara39)). Released under the [Apache License 2.0](LICENSE).

You're free to use, copy and adapt this project. If you do:
- keep the `LICENSE` and `NOTICE` files in your copy;
- credit the original author, for example with this line in your README:

  ```markdown
  Based on [explove](https://github.com/fvergara39/explove) by [Francesca Vergara](https://github.com/fvergara39).
  ```

## Credits

- [Firebase](https://firebase.google.com) for Google sign-in and the Realtime Database
- [Leaflet](https://leafletjs.com) for the map
- [OpenStreetMap](https://www.openstreetmap.org/copyright) for map data and address search (Nominatim)
- Fonts: Bricolage Grotesque and Instrument Sans, from Google Fonts
