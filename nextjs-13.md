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

## Error
You'll be able to create an Error Boundary that will catch errors within a subtree by adding a error.js file and default exporting a React Component.


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

### Data fetching
It will be possible to fetch data inside multiple segments in a route. This is different from the pages directory, where data fetching was limited to the page-level.
Data fetching can be performed in the layout.
You can also fetch data in multiple segments of a route. For example, a layout that fetches data can also wrap a page that fetches its own data.

Statically generated routes improve this further, as the client navigation reuses the cache (Server Components data) and doesn't recompute work, leading to less CPU time because you're rendering a snapshot of the Server Components.

(getServerSideProps and getStaticProps) can only be used in Server Components in the app f

Next.js will eagerly initiate data fetches in parallel to minimize waterfalls. With parallel fetching, however, each segment can eagerly start data fetching at the same time.

Since rendering may depend on Context, rendering for each segment will start once its data has been fetched and its parent has finished rendering.

In the future, with Suspense, rendering could also start immediately - even if the data is not completely loaded. If the data is read before it's available, Suspense will be triggered. React will start rendering Server Components optimistically, before the requests have completed, and will slot in the result as the requests resolve.

When navigating between sibling route segments, Next.js will only fetch and render from that segment down. It will not need to re-fetch or re-render anything above.

On navigation, data is fetched and React renders the components server-side. The output from the server are special instructions (not HTML or JSON) for React on the client to update the DOM. These instructions hold the result of the rendered Server Components meaning that no JavaScript for that component has to be loaded in the browser to render the result.

This is in contrast to the current default of Client components, which the component JavaScript to the browser to be rendered client-side.
Notes: The router leverages a new streaming protocol so that rendering can start before all data is loaded.

As users navigate around an app, the router will store the result of the React Server Component payload in an in-memory client-side cache. The cache is split by route segments which allows invalidation at any level and ensures consistency across concurrent renders. This means that for certain cases, the cache of a previously fetched segment can be re-used.

The new router will use Suspense for instant loading states and default skeletons. This means loading UI can be shown immediately while the content for the new segment loads. The new content is then swapped in once rendering on the server is complete.

Errors inside a layout.js file in the same segment as an error.js will not be caught as the automatic error boundary wraps the children of a layout and not the layout itself.

## Templates
Templates are similar to Layouts in that they wrap each child Layout or Page.

Unlike Layouts that persist across routes and maintain state, templates create a new instance for each of their children
Note: We recommend using Layouts unless you have a specific reason to use a Template.

A template can be defined by exporting a default React component from a template.js file

There may be cases where you need to mount and unmount shared UI, and templates would be a more suitable option. For example:

- Enter/exit animations using CSS or animation libraries
- Features that rely on useEffect (e.g logging page views) and useState

With this convention, you can share templates across routes that create a new instance on navigation. This means DOM elements will be recreated, state will not be preserved, and effects will be re-synchronized.
