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

## Layout

A layout is UI that is shared between route segments in a subtree. Layouts do not affect URL paths and do not re-render (React state is preserved) when a user navigates between sibling segments.
Notes: layouut are nested by default.

There are 2 types of layouts:

- Root layout: Applies to all routes
- Regular layout: Applies to specific routes


Files inside app will be rendered on the server as React Server Components.

### Root Layout

You can create a root layout that will apply to all routes of your application by adding a layout.js file inside the app folder.

### Regular Layouts

You can also create a layout that only applies to a part of your application by adding a layout.js file inside a specific folder.

## Page

A page is UI that is unique to a route segment. You can create a page by adding a page.js file inside a folder.

![page-example](https://user-images.githubusercontent.com/17195702/230880845-0d43a01c-96d1-455b-bb6a-96c670bf2c1a.png)

Notes:
NextJs allow to have incremntal static generation ISR, this mean deploy only page changes without rebuild the complete website.

Data fetching
It will be possible to fetch data inside multiple segments in a route. This is different from the pages directory, where data fetching was limited to the page-level.
Data fetching can be performed in the layout.
You can also fetch data in multiple segments of a route. For example, a layout that fetches data can also wrap a page that fetches its own data.

Statically generated routes improve this further, as the client navigation reuses the cache (Server Components data) and doesn't recompute work, leading to less CPU time because you're rendering a snapshot of the Server Components.

(getServerSideProps and getStaticProps) can only be used in Server Components in the app f


