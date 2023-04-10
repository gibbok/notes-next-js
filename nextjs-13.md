# Next.js 13

New feature in version 13.

## app directorey (in beta for now)

Support for:
- nexsted layouts
- server components
- data-fetching to avoid waterfalls
- use react 18 features like streaming, transitions and suspense
- client server routing like spa behaviour
- advanced routing patterns

- app features can be incrementally adopted
- the app folder can co-exists with pages folder
- routes are defined using a folder hierarchy


With this system, you'll use folders to define routes, and files to define UI (with new file conventions such as layout.js, page.js, and in the second part of the RFC loading.js).
This allows you to colocate your own project files (UI components, test files, stories, etc) inside the app directory.

https://nextjs.org/blog/layouts-rfc

## layout
A layout is UI that is shared between route segments in a subtree. Layouts do not affect URL paths and do not re-render (React state is preserved) when a user navigates between sibling segments.

There are 2 types of layouts:

Root layout: Applies to all routes
Regular layout: Applies to specific routes

Root Layout
You can create a root layout that will apply to all routes of your application by adding a layout.js file inside the app folder.

Notes:
NextJs allow to have incremntal static generation ISR, this mean deploy only page changes without rebuild the complete website.
