## How to Enable Preview Mode in Next.js for your CMS

I’m using Notion as a CMS by utilizing the Notion API (which was just released to the general public) for my website and blog. There are a lot of benefits to using Notion as a CMS and if you’d like to read more, [I wrote a little blurb about it here](https://braydoncoyer.dev/blog/introducing-my-new-blogfolio#notion-as-a-cms).

Writing articles in Notion is great and all, but I didn’t have a way to preview what the article would look like on my blog before publishing. I would cross my fingers and hope that everything worked on my live site. But, as you may suspect, there were times when I didn’t catch small issues before I published the article and it wasn’t uncommon for people to message me on Twitter to inform me that something wasn’t right.

There had to be a way that I could preview unpublished content without affecting visitors to my site. Turns out, Next.js provides a very simple but powerful solution. It’s called Preview Mode.

## What is the Next.js Preview Mode?

Preview Mode renders pages at request time at a specific URL so you can preview draft content without having to worry that visitors to your website will see your unfinished work. This was specifically created for headless Content Management Systems like Strapi, Contentful, Sanity.io and Notion - though the implementation is CMS agnostic. 

Let’s take a look at how the Next.js Preview Mode works.

### How does Preview Mode work?

Next.js needs to understand that we’re trying to preview the site and any unpublished content. In order to do that, Next.js created a special function that sets specific cookies in our browser and turns on the Preview Mode. 

Because those cookies were set, any incoming requests to the browser with these cookies will allow us to make quick checks in our logic to filter out draft content if the visitor is not browsing in Preview Mode. 

> 📢 Please note that Preview Mode is only available for Next.js versions 9.3 and up.

Make sense? Let’s see it in action.

## How to Use the Next.js Preview Mode

Remember, we first need to inform Next.js that we want to view our site in preview mode. To do that, create a new API Route. The name of the API Route doesn’t matter, but for the sake of this tutorial let’s call it `pages/api/preview.js`.

Next.js provides a special function that we can call which will set cookies in our browser. Inside the API Route, call the `setPreviewData` function.

```jsx
export default function handler(req, res) {
  res.setPreviewData({});
}
```

That’s literally all you have to do to enable Preview Mode in your Next.js application. 

But let’s take it a step further. Let’s pass in a query parameter so we can redirect to a specific page once the cookies have been set. 

Thankfully, Next.js makes this extremely easy. Simply call the `redirect` function on the `res` object and pass in the appropriate value.

```jsx
export default function handler(req, res) {
  res.setPreviewData({});
	res.redirect(req.query.redirect);
}
```

## Display Draft Content with Preview Mode Enabled

Now that Preview Mode is active, we need to add some logic to our application that will allow unpublished content to be displayed on our website. 

You may have a certain flag in your content object that identifies if it's published or not - since I’m using Notion as a CMS, I have an `isPublic` boolean on each of my articles that controls whether or not the article is available for visitors to read.

I’m fetching this data in `getStaticProps` method so I can take advantage of static generation. If you’re doing the same thing, destructure the new `preview` prop and use it to filter content based on its boolean value.

```jsx
export const getStaticProps = async ({ preview = false }) => {
  const articles = await getAllArticles();

  if (!preview || preview === undefined) {
    articles = articles.filter((article) => article.isPublic === true);
  }

  return {
    props: {
      articles,
    },
    revalidate: 30
  };
};
```

Notice our conditional check - if Preview Mode is disabled, we only want to return articles that *have been published*. Otherwise, we skip that block and return all of the content. 

Nifty - and super simple to use! 

## Try it out!

Now that everything is set up, let’s give it a shot!

Try navigating to `www.yoursitename.com/api/preview?redirect=/blog`.

You should be automatically redirected to your intended path and you should see your draft content as well!

If you open the storage tab in your browser, you’ll see the Preview Mode cookies that Next.js set when we hit that special URL!

![Preview Mode cookies that were set when we hit the API Route](https://res.cloudinary.com/braydoncoyer/image/upload/v1647100336/Screen_Shot_2022-03-12_at_9.51.35_AM_tbuabf.png)


## Conclusion

I feel much more confident writing articles for my blog with Preview Mode enabled; I finally have a way to check the content before I hit the publish button!