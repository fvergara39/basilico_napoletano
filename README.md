# 🌿 Basilico napoletano

A private web app for two people to keep a shared list of **movies, restaurants, places, activities and recipes**: what you want to do, what you've already done, and how you liked it.

Each of you gives your own 1–5 star rating and comment, so you can see at a glance where you agree and where you don't.

It runs in the browser, on a computer or a phone. You can add it to your home screen like an app, iPhone included. Nothing to install and no code to write: all you need is a GitHub account and a free Firebase project.

---

## Features

- **Five sections:** Movies, Restaurants, Places, Activities and Recipes. Each item has a "to do" / "done" status, fields that fit the section, and free-form notes.
- **Reviews for two:** separate stars and comment for each person. Your saved review stays fixed and can be edited whenever you want.
- **Map:** restaurants and places appear on an OpenStreetMap map. To set a location you can search an address, tap the map, use your current location, or paste a Google Maps link. Every item has "Open in Google Maps" and "Directions" links.
- **Visit dates:** marking something as done saves the date. You can also add repeat visits or past dates.
- **Recipe photos:** up to 8 photos each per recipe, with thumbnails shown right in the list.
- **Links:** save links on any item, such as the online recipe, the menu or the trailer.
- **Stats:** how much you agree, rating distribution, your top items per section, what you did month by month, and what's waiting for your rating.
- **Editable title:** tap the title at the top to rename the app. The new name applies for both of you.
- **Google sign-in:** only the two email addresses you choose can get in.

## What you need

- **Two Google accounts**, one each, for signing in.
- **A GitHub account**, to publish the page. One of you is enough.
- **A Firebase project on the free "Spark" plan.** No credit card required.

Time needed: about 15–20 minutes, once.

---

## Setup

### 1. Copy the repository

At the top of this GitHub page, click **Fork** (or **Use this template**, if available) to create a copy in your account.

The repo must be **public** to use GitHub Pages for free. That's fine: the code contains no data and no passwords, and your data stays in Firebase, protected by the rules from step 4.

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

In the Realtime Database, open the **Rules** tab. Replace everything with the block below, put your two Google emails in **lowercase**, and click **Publish**:

```json
{
  "rules": {
    ".read": "auth != null && auth.token.email_verified == true && (auth.token.email.toLowerCase() == 'first.person@gmail.com' || auth.token.email.toLowerCase() == 'second.person@gmail.com')",
    ".write": "auth != null && auth.token.email_verified == true && (auth.token.email.toLowerCase() == 'first.person@gmail.com' || auth.token.email.toLowerCase() == 'second.person@gmail.com')"
  }
}
```

Change the emails on **both** lines, `.read` and `.write`. Anyone else who tries to sign in will see "doesn't have access to this app".

### 5. Put your configuration in `index.html`

1. In your repo on GitHub, open `index.html` and click the **pencil** icon (Edit).
2. Find the `const firebaseConfig = {` block near the top of the script.
3. Replace the values with your project's values from step 2.

The block must also include `databaseURL`. If Firebase didn't show it to you, you'll find it at the top of the Realtime Database page (e.g. `https://your-project-default-rtdb.europe-west1.firebasedatabase.app`).

```js
const firebaseConfig = {
  apiKey: "...",
  authDomain: "your-project.firebaseapp.com",
  databaseURL: "https://your-project-default-rtdb.europe-west1.firebasedatabase.app",
  projectId: "your-project",
  storageBucket: "your-project.firebasestorage.app",
  messagingSenderId: "...",
  appId: "..."
};
```

In the same file, find the line starting with `const OLD_ITEMS =` and replace it with:

```js
const OLD_ITEMS = [];
```

It was used to import the original author's data, and you don't need it.

Finally, click **Commit changes**.

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

Open the link, click **Sign in with Google**, and sign in. Send the same link to your partner: after their first sign-in, you'll see each other in the app.

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

- **App name:** tap the title at the top of the app. For the default name and the name under the icon, edit:
  - `DEFAULT_TITLE` and `<title>` in `index.html`;
  - `apple-mobile-web-app-title` in `index.html`;
  - `name` and `short_name` in `manifest.webmanifest`.
- **Icon:** replace `apple-touch-icon.png` (180×180), `icon-192.png` and `icon-512.png` with square PNG images without transparency, keeping the same file names.
- **Sections, colors and fields:** defined in `index.html`, in the `KINDS` object and in CSS variables such as `--movie` and `--restaurant`.

## Updating the app

Every change to `index.html` on GitHub is published in about a minute. To see the new version right away:
- on a computer, reload with **Ctrl+Shift+R** (**Cmd+Shift+R** on Mac);
- on a phone, close and reopen the app.

---

## Limits and costs

Everything runs on free plans.

- **Firebase Spark:** 1 GB of stored data and 10 GB of downloads per month, plenty for two people. Photos are resized to about 200 KB each and only downloaded when you open that recipe.
- **Map and address search:** use the free public services of [OpenStreetMap](https://www.openstreetmap.org/copyright) and Nominatim, which are meant for light use like this. The map is detailed down to street level, but it isn't Google Maps: for directions and opening hours, use the Google Maps links.
- **No notifications:** you see your partner's updates when you open the app.

## Privacy

- Your data (items, ratings, comments, photos, dates) lives in **your own** Firebase project, and only the two emails in the rules can access it.
- Address search sends the text you search for to Nominatim (OpenStreetMap).
- Map tiles are downloaded from OpenStreetMap's servers.

## Troubleshooting

| What happens | What to check |
|---|---|
| "…doesn't have access to this app" | Is the email spelled correctly and in lowercase on **both** lines of the Realtime Database rules? Did you click **Publish**? The rules go in the *Realtime Database*, not in Firestore. |
| Google sign-in doesn't open or shows an error | Is `YOUR-USERNAME.github.io` in the **authorized domains** (step 8)? Is the Google provider enabled (step 6)? |
| "Connection problem", or the list doesn't load | Is `databaseURL` in `firebaseConfig` correct? Does the Realtime Database exist? |
| On iPhone, from the home screen app, sign-in doesn't return to the app | In `index.html`, delete the line `<meta name="apple-mobile-web-app-capable" content="yes">`. In the manifest, change `"display": "standalone"` to `"display": "browser"`. Then add the app to the home screen again. |
| I still see the old version | Reload with Ctrl+Shift+R, or close and reopen the app. GitHub Pages can take a couple of minutes. |
| The map is grey | Check your connection. If OpenStreetMap is having temporary issues, try again later. |

---

## Repository layout

```
index.html              the app (HTML, CSS and JavaScript in a single file)
manifest.webmanifest    name and icons for installing on a phone
apple-touch-icon.png    iPhone icon (180×180)
icon-192.png            Android and browser icon
icon-512.png            large Android icon
```

## Credits

- [Firebase](https://firebase.google.com) for Google sign-in and the Realtime Database
- [Leaflet](https://leafletjs.com) for the map
- [OpenStreetMap](https://www.openstreetmap.org/copyright) for map data and address search (Nominatim)
- Fonts: Bricolage Grotesque and Instrument Sans, from Google Fonts
