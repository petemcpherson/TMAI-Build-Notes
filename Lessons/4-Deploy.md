# 4 - Deploying your app

Overview:
1. Choose host
2. Prepare SvelteKit adapter
3. Deploy

[SvelteKit docs here.](https://kit.svelte.dev/docs/adapters)
## 1 - Choose host
This will obviously depend on:
- The features you need
- Your budget
- Your build!

- https://www.netlify.com/
- https://vercel.com/
- https://render.com/ (SvelteKit node adapter, what I'm using currently)

*NOTE: For projects that use AI--you'll be dealing with some long-running API-querying functions. I.e. you might have a function that sends something to ChatGPT--and waits for a response. For longer ChatGPT responses--this could take 60+ seconds!! It stinks--but several hosts have timeouts of 10, 15, 30, or 60 seconds--then throws an error. **This is why I'm using Render for AI-related apps.** You don't have to worry about that there.*

For hosting on Render, we'll be using a Render "web service" to run our app (that's just what they call it).

It's 100% free to start--BUT this server will go into a "standby" mode if nobody's using your app--and it can take several seconds to "boot up" when somebody uses it again.

Great news though--a paid "service" on Render is only $7 a month--way lower than the cheapest paid plans elsewhere.
## 2 - Prepare SvelteKit adapter
**Adapters = kinda-sorta prepare your app for different hosts (making some tweaks and optimizations).**

By default, SvelteKit apps have the "adapter-auto," which auto adapts to most hosts!

But as always--refer to the SvelteKit docs to see IF you should use a different adapter.
### Setting up the adapter-node adapter for Render

For deploy to Render, you need to use adapter-node adapter ([here's their docs!](https://docs.render.com/deploy-sveltekit))

Here's how to setup the node-adapter:

First, install it in the terminal by running:
```
npm i -D @sveltejs/adapter-node
```

Then, head to your `svelte.config.js` file, and add the adapter in there like so:

```
import adapter from '@sveltejs/adapter-node';


const config = {
	kit: {
		adapter: adapter()
	}
};

export default config;
```

*NOTE: You might have some vite preprocessor stuff there as well. Leave it in.*

## 3 - Set up Render web service
[First, here's the Render docs for SvelteKit deployment.](https://docs.render.com/deploy-sveltekit)

Create a free Render account.

Then add a new 'web service.'

Select "Build/Deploy from GitHub repository", etc.

You'll likely have to connect your Render account to GitHub--but once that's done, you should be able to choose the correct GitHub repository for your project.

Fill in the details for the web service, **and make sure to set the runtime, build command, and start commands correctly! Render gives you these in their documentation!**

At the time of this writing, it should be this:

- Runtime = node
- Build Command - `npm install && npm run build`
- Start Command - `node build/index.js`

Choose the free plan *(or the $7/mo plan if you don't want your app to power down after inactivity).* You can always upgrade later on (and you should if you have users!)

Next, you'll need to upload (or manually type out) your `.env` variables. (from the .env file in your root project folder).

If following this guide, you probably only have 3 variables in the file so far. **If you ever add more, be sure to ALSO add them to the .env area in your Render web service! The .env file DOES NOT get uploaded to GitHub!**

It'll take a few minutes, but you should see a successful build message somewhere in the logs--and you should be able to navigate to the Render-generated url for your app, i.e. `https://someapp.onrender.com`.

**OPTIONAL**: If you have a custom domain name already, feel free to add that in the Render web app settings!

You'll need to follow the instructions *to update your domain's DNS settings.* [Render's instructions for that are here](https://docs.render.com/custom-domains#configuring-dns-to-point-to-render)--but this work will be done with wherever you purchased your domain name.