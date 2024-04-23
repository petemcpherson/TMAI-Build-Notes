# AI Coding 5 - Create pages, layout, and style

Learn how to create pages, i.e. routes, and a little bit of styling. 

## 1 - Set up some pages
NOTE: This section is JUST for our SvelteKit app.

How you build out different "routes" in your app (i.e. different pages, navigation, etc) is HIGHLY different depending on your framework!

### Routing in SvelteKit
First, you're gonna wanna watch some videos on YouTube...but don't worry, it's actually simpler than you think!

I recommend this video from Net Ninja: [SvelteKit Crash Course Tutorial #4 - Pages & Routes](https://youtu.be/ftiTVitDbx0?si=CUpU5T6GEhhssEG6)

*Side note: routing in SvelteKit is way way way easier than React haha.*

For our project, let's create some routes for the following:
- Terms of Use
- Account
- Login
- Logout (this is going to be a weird work-around page that'll automatically log users out when they land on the page)
- Maps Dashboard (this will display a list of my users' maps,)
- Add New Map (this will be a subpage of Maps, maybe `/new` or something
- Individual Map Page (another subpage, the url will contain the specific ID for a map)

For our individual map page, it'll have a folder name like `/[mapId]`

We'll use this 'param' to fetch the proper map info! **This page will be dynamic...showing different content for each specific map.**

## 2 - Implement a top Navigation bar + Basic layout
Again, HOW you accomplish this step will depend on your framework.

For our project (SvelteKit), we'll be creating a simple `+layout.svelte` file that'll have a basic header and footer.

First, locate that layout file in the `/routes` folder!

Note that the `<slot />` thing is actually our ENTIRE SITE at this point. 

We're going to be "wrapping" that with a header and footer. Hopefully this will make sense when you see this implemented.

First, let's head to the [DaisyUI site ](https://daisyui.com/components/navbar/)and choose a navbar to copy/paste in.

Then, customize with our details (which is just a shell right now!)

Then we'll do the same with the **footer**!

## 3 - Style the homepage
Let's just work on the base layout for all pages of the site (basic margin, padding, etc)

Then, we'll launch a SUPER simple landing page, completely with an email capture form

We'll be borrowing design stuff from other various apps, *implementing with Tailwind & DaisyUI components.*

