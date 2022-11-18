# Research

## SSG, SSR, CSR and ISR

### SSG - Static Site Generation

**Static Generation** is the pre-rendering method that generates the HTML at **build time**. The pre-rendered HTML is then _reused_ on each request.

You can use Static Generation for many types of pages, including:

* Marketing pages
* Blog posts
* E-commerce product listings
* Help and documentation

### SSR - Server-side Rendering

**Server-side Rendering** is the pre-rendering method that generates the HTML on **each request**.

It will be slower, but the pre-rendered page will always be up-to-date. Or you can skip pre-rendering and use client-side JavaScript to populate frequently updated data.

### CSR - Client-side Rendering

If you **do not** need to pre-render the data, you can also use the following strategy (called **Client-side Rendering**):

This approach works well for user dashboard pages, for example. Because a dashboard is a private, user-specific page, SEO is not relevant, and the page doesn’t need to be pre-rendered. The data is frequently updated, which requires request-time data fetching.

### ISR - Incremental Static Regeneration

Incremental Static Regeneration (ISR) allows you to create or update content _without_ redeploying your site. ISR has three main benefits for developers: better performance, improved security, and faster build times.

Both ISR and `Cache-Control` headers (including `s-max-age` and `stale-with-error`) help reduce backend load by making fewer requests to your data source. However, there are key architectural differences between the two.

ISR abstracts common issues with HTTP-based caching implementations, adds additional features for availability and global performance, and provides a better developer experience for implementation.
