1. (Optional) Create/choose folder for project parent
2. Install SvelteKit
3. Install Tailwind & DaisyUI
4. Set up Google Firebase
	1. Firestore
	2. Auth
**Important Reminder: I am NOT a developer and don't pretend to be! I'm sure there are better ways to do things. Also, Pete cannot be held liable for anything you build as a result of these videos.**

## 0 - Prerequisites
### Install node
Head to https://nodejs.org/en
### Install npm
## 1 - New SvelteKit app
Open terminal, navigate to any parent folders ('Coding Projects,' in my case)

Run the following:
```
npm create svelte@latest myapp
```
Go through the step-by-step options (I suggest 'skeleton app' and 'no' to Typescript)

Navigate to the new project directory, then run:
```
npm install
```
You should be able to run:
```
npm run dev
```
And see the localhost port where you're app is running! You can navigate there to see your SvelteKit app up and running!








## 2 - CSS Frameworks
My personal stack = Tailwind and DaisyUI (built on Tailwind)

### Install Tailwind
[Documentation here](https://tailwindcss.com/docs/guides/sveltekit)

Make sure you're in the project directory, then

```
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

*(I usually skip step #3 from that documentation, the PostCSS stuff--I don't know enough about this to suggest you skip it, though!)*

Add the locations into the tailwind.config.js file. **The 'content:' line!**

```
/** @type {import('tailwindcss').Config} */
export default {
  content: ['./src/**/*.{html,js,svelte,ts}'],
  theme: {
    extend: {}
  },
  plugins: []
};
```

Then, create a ./src/app.css file and add the @tailwind directives for each of Tailwind’s layers:
```
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Then, create a ./src/routes/+layout.svelte file and import the newly-created app.css file.
```
<script>
  import "../app.css";
</script>

<slot />
```

You're good to go! You should open your app annnddddddd essentially see NO styling :)
### Install DaisyUI
[Documentation here](https://daisyui.com/docs/install/)

First, navigate to the project directory (if you're not there already), then:
```
npm i -D daisyui@latest
```

Then add daisyUI to your tailwind.config.js files:

```
module.exports = {
  //...
  plugins: [require("daisyui")],
}
```

That's it!

If you want to change DaisyUI themes, you can head to tailwind.config.js and add a theme like so:
```
module.exports = {
  //...
	plugins: [require("daisyui")],

	daisyui: {
		themes: ["forest", "dark", "cupcake", "cyberpunk"],
	}
}
```

Then, go to your app.html file and add the `data-theme="xyz"` line inside the `<html>` tag.

```
<!DOCTYPE html>

<html lang="en" data-theme="cyberpunk">
```

## 3 - Set up Firebase

### 1 - Create Firebase project
Head to https://firebase.google.com/

Login/signup, then create a new project.
1. Name it
2. Choose GA or not (it's easier to chose 'yes' and NOT use it right away, then to try and add it later)

Once you're project is created, navigate to the 'web' option for installing Firebase. **It'll give you code to use in the next step!**

1. Give it a nickname
2. I do NOT use Firebase hosting
3. Copy the code

Screenshot: https://share.cleanshot.com/K78y50Gk
### 2 - Install Firebase in your app

Head to the terminal (your home app directory), then run `npm i firebase`.

In the lib folder, create a new file, `firebase.js`, etc

**Paste in the code from earlier and save.**

### 3 - Enable Firebase apps/SDKs (client side)
We'll use 2 for now:

1. Auth
2. Firestore (our database!)

#### Auth
1. Under authentication in the Firebase console...
2. Enable whatever signin method you want (I strongly suggest just starting with Google, or Email)

#### Database
1. Under Firestore in the Firebase console...
2. Create a database
3. set name and location
4. Start in production mode. **Warning: we'll come back to this BEFORE YOU GET USERS. You'll want to change the security rules!**
5. In the Firestore main page, go to 'rules'
6. Change 'false' to 'true'
7. Hit publish

Technically, this is a very *lax* rule for who can read/write to your database. You'll want to change this later if you plan on getting users, etc!

Add the SDK code to the firebase.js file:

First, the imports at the top.

```
import { getFirestore, onSnapshot, doc } from 'firebase/firestore';

import { getAuth, onAuthStateChanged } from "firebase/auth";
```

Then, initialize them:

```
export const db = getFirestore();

export const auth = getAuth();
```


### 4 - Enable Firebase admin SDK
For server-side authentication (mostly for cookies!)

First, head to the terminal (in project's root directory), then hit

```
npm install firebase-admin
```

Next, in the Firebase console in your browser, navigate to the Project Settings, then the Service Accounts tab.
1. Click the Generate New Private Key button
2. Download the file it gives you
3. Upload this file to the root of your project, and rename it to `service-account.json`
4. In your `.gitignore` file, add that file name! I.e. `service-account.json`

Next, add a new file in the root of your project, titled simply `.env`. *You can think of this file like 'storing variables that you want to keep private from everybody, such as private API keys.*

Then, add 3 variables to the .env file (filling them in with your own details from the `service-account.json` file!)

```
FB_PROJECT_ID="asdf"
FB_CLIENT_EMAIL="asdf"
FB_PRIVATE_KEY="asdf"
```

*yes, the private key is SUPER long, and make sure to copy/paste in EVERYTHING in between the quotation marks!*

[Screenshot example](https://share.cleanshot.com/9Rh7ScNZ)

Then, create a new folder in the /lib directory, called `server`. 

Inside, add a new file, `admin.js`.

Import the getAuth and getFirestore, as well as the variables you just created (make sure the names match what you created!)

Then, initialize the app with the try/catch block.

```
import { getAuth } from 'firebase-admin/auth';
import { getFirestore } from 'firebase-admin/firestore';
import { FB_CLIENT_EMAIL, FB_PRIVATE_KEY, FB_PROJECT_ID } from '$env/static/private'
import pkg from 'firebase-admin';


try {

	pkg.initializeApp({
	
		credential: pkg.credential.cert({
			projectId: FB_PROJECT_ID,
			clientEmail: FB_CLIENT_EMAIL,
			privateKey: FB_PRIVATE_KEY
		})
	
	});

} catch (e) {

	if (!/already exists/u.test(e.message)) {
	
	console.error('Firebase admin initialization error', e.stack);
	
	}
}

  
export const adminDB = getFirestore();
export const adminAuth = getAuth();
```

**NOTE: these adminDB and adminAuth things can ONLY be used on the server, so they can only be used in server.js or page.server.js files.**




