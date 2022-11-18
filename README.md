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

ISR abstracts common issues with HTTP-based caching implementations, adds additional features for availability and global performance and provides a better developer experience for implementation.

## getInitialProps, getStaticProps, and getServerSideProps in Next.js

### getInitialProps

_**getInitialProps**_ is used to enable Server-Side Rendering or SSR for a page. With SSR, the HTML is generated on the server when it needs to be served to the client.
On the first load, it will run on the server, and on every subsequent run, it will run on the client. This can be useful for fetching data you expect to change often.

```javascript
const Initial = ({ cat }) => {
return (
<div>
    <h1>Random Cat:</h1>
    <Image src={cat} layout="fixed" width="250" height="250"></Image>
</div>
);
};

Initial.getInitialProps = async (ctx) => {
const { data } = await axios.get('https://cataas.com/cat?json=true');
return { cat: 'https://cataas.com' + data.url + '?type=sq' };
};
export default Initial;
```

### getServerSideProps

Like getInitialProps, _**getServerSideProps**_ will run on the server. Unlike getInitialProps however, it will always run on the server.

_**getInitialProps**_ is being deprecated instead of this function, the main reason being to give you greater control over where the code is run. _**getServerSideProps**_ is guaranteed to run on the server, and you can even use server-side-only code, like the fs module to load files to pass as props.

```javascript
const Initial = ({ cat }) => {
return (
<div>
    <h1>Random Cat:</h1>
    <Image src={cat} layout="fixed" width="250" height="250"></Image>
</div>
);
};

export async function getServerSideProps() {
const { data } = await axios.get('https://cataas.com/cat?json=true');
return {
props: { cat: 'https://cataas.com' + data.url + '?type=sq' }, // will be passed to the page component as props
};
}

export default ServerSide;
```

### getStaticProps

_**getStaticProps**_ is slightly different from the previous two functions.
In Static Generation, we try to generate as much of the HTML for the website as possible when you build your app. This means if data is going to change often, you cannot use this function, as it won’t change after you deploy your app.

The benefit you gain from this is performance; nothing needs to be fetched or evaluated on the client or server side; the server just serves the raw HTML for the page. Like _**getServerSideProps,**_ you can also use server-side-only code in your app.

```javascript
import fs from 'fs'; //This will get removed on the client
import path from 'path';

const Static = ({ blogPost }) => {
return <div>{blogPost}</div>;
};

export async function getStaticProps({ params }) {
//We get the filename as a parameter from the context object

//fs is server side only, but this code only runs on the server
const blogPost = fs.readFileSync(
path.join(
process.cwd(),
'src/pages/examples/blog-posts',
params.staticBlogPostId + '.txt'
),
'utf8'
);

return {
props: { blogPost }, // will be passed to the page component as props
};
}
```
