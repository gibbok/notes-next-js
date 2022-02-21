# Notes Next.js by Vercel

Next.js is an open-source development framework built on top of Node.js, it provides server-side rendering and generating of static websites.
The Next.js framework utilizes Jamstack architecture.

Next.js use Babel for transpilation and Webpack for bundling.
It uses page-based routing and dynamic routing.

It supports incremental static regeneration ISR and static site generation SSG.

## Core features

- Page based routing system with the support of dynamic routes
- Pre rendering like static generation SSG and server-side rendering SSR are both supported for a per-page basis
- Code splitting for the last load
- Client-side routing with optimized fetching (pre-fetching)
- Fast refresh for development

## Core concepts

### Pages

- A page is a React component exported from a file in the `pages` directory
- Pages are associated with the route base of their file name, for instance `pages/index.js` is associated with `/` or `pages/posts/first.js` is associated with `/posts/first`
- A component/page must be sported as `default`
- Link between pages must be done using the `Link` Component from next/link, this performs client-side navigation using JavaScript
- Next.js does code-splitting automatically, so each page only loads when necessary (fast loading), so each page become isolated so one error in one page should not affect other pages
- In production, when a `Link` component appears in the viewport, Next.js automatically prefetch the code for the linked page in the background, for an almost instant load.

### Images

- `Image` component from Next.js performs, optimize image, images are responsive, images are loaded only when appearing in the viewport
- `Image` is served in `WebP` format when the browser supports it, even if the source is in a different format by default
- Automatic Image Optimization works with any image source. Even if the image is hosted by an external data source
- It optimizes images on demand instead o build time, images are lazy-loaded by default, so images outside the viewport are not loaded, images are always rendered in a way to avoid cumulative layout shift
- Next.js serves static assets under the top-level public directory, files inside public can be referenced from the root of the application similar to pages

### Meta

- It is possible to decide which HTML metadata we want by using the `Head` component
- It is possible to load external JS into the page by using the `Script` component, which can also perform the loading using different strategies, `lazyOnload` loads script on browser idle time, `onLoad` run after the script has finished loading

### Styling

- It support styled jsx, sass, and CSS modules (the CSS file name must end with .module.css.)
- It automatically generates Unique Class Names
- It splitting feature works on CSS modules, to ensure the minimal amount of CSS is loaded for each page and result in smaller bundle sizes
- Global styles should be added in `pages/_app.js` file
- Next.js compiles CSS using PostCSS. To customize PostCSS config, you can create a top-level file called postcss.config.js

### Pre-rendering
- It pre-renders every page by default, so Next.js generate HTML for each page in advance instead of having it all done on the client-side using JS (SPA style)
- Pre-rendering can result in better performance and SEO
- Each generated HTML is associated with a minimal JS code for what page, when the page loads the browser loads it JS and makes the page interactive (the process is called hydratation).
- Initial load: pre-rendered HTML created by next js is displayed
- Hydration: React components are initialized and the app becomes interactive

Ther are two forms of pre-rendering:
- Static Generation: a pre-rendering method where HTML is generated at build time (with data or no data fetch)
- Server-side Rendering: a pre-rendering method where HTML is generated on each request (default mode in dev mode)

Next.js can support a hybrid method, letting the developer choose which pre-rendering form to use for each page.

Recommendation:
Use Static Generation with or without data when possible so it can be served from CDN (instance marketing pages, blog posts, doc, e-commerce product listing)
Use Server-side Rendering for the page where data is very frequently updated or changes on every request.
It is possible to skip pre-rendering and use client-side JS for frequently updated data

#### Static generation without data fetching

For pages that require no fetching of external data, those are created at build time.

#### Static generation with data fetching

For pages that can be generated after fetching external data at build time. It can be used for pre-render page ahead of a user's request.
It is possible to bypass static generation for specific pages using Next.js Preview Mode

#### Static generation with data using `getStaticProps`

The `getStaticProps` is a synch function that runs at build time, in that function we can fetch external data and pass it to the props of the component for that page. Notes:

- Next.js polyfills `fetch()` by default on both client and server.
- `getStaticProps` only run on the server-side. It will never run on the client-side. It won’t even be included in the JS bundle for the browser. That means you can write code such as direct database queries without them being sent to browsers.
- in dev mode, `getStaticProps` run on each request
- in prod mode, `getStaticProps` run at build time
- `getStaticProps` can be only exported from a page

### Server-side rendering

Fetch data at request time instead of build time, by exporting  `getServerSideProps` instead of `getStaticProps` from a page.
`getServerSideProps` parameter (context) contains request specific parameters.

Time to first byte (TTFB) will be slower than `getStaticProps` because the server must compute the result on every request, and the result cannot be cached by a CDN without extra configuration.

### Client side rendering

Use if you do not need to pre-render the data, which consists of pre-rendering some static part of the page when the page load using JS to populate the remaining parts. Useful for a project where the data is updated frequently, or private projects where SEO is not relevant.

##  Dynamic URLs using dynamic routes

Statically generate pages with paths that depend on external data, for instance, defaults for a blog post `/posts/<id>,` where `<id>` is the name of markdown file present in a folder at build time.

- In Next.js pages that begin with `[` and end with `]` are dynamic routes in Next.js.
- More on [feedback](https://nextjs.org/learn/basics/dynamic-routes/dynamic-routes-details), summary of the behaviour:

feedback = false: 404 is returned it no result found
feedback = true: path will be rendered, or Next.js will server fallback version of the path, in background next will statically generate the request page on next request the pre-rendered content will be shown
feedback = blocking, it will apply server-side rendering and cached for further requests
 
## API Routes

API routes provide a solution to build your API endpoint with Next.js for instance in a new project you could build your entire API with API Routes.
 
Do Not Fetch an API Route from `getStaticProps` or `getStaticPaths`` instead use helper functions, because both functions are running at the server-side and never at the client-side.

Notes: The API Route code will not be part of your client bundle, so you can safely write server-side code which cannot be seen in the client.
 
## Others

Front Matter:
Is a [metadata](https://jekyllrb.com/docs/front-matter) section used in MarkDown file

Rendering strategies:
https://nextjs.org/learn/seo/rendering-and-ranking/rendering-strategies

Next.js support AMP although Google dropped AMP pages as a requirement

Dynamic Imports for Components used defer the loading of this component until it's required by the user which can help to improve First Input Delay (FID). 

Next.js has built-in Automatic Webfont Optimization. By default, Next.js will automatically inline font CSS at build time, eliminating an extra round trip to fetch font declarations. This results in improvements to First Contentful Paint (FCP) and Largest Contentful Paint (LCP).

Next.js provides a built-in Script component that optimizes loading for any third-party script while giving developers the option to decide when to fetch and execute it. use the `Script` which helps to improve Largest Contentful Paint (LCP).
