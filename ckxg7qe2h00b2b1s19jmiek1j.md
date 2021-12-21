## How to Dynamically Create Open Graph Images with Cloudinary and Next.js

Have you wondered how sites like [dev.to](http://dev.to) create dynamic and engaging social sharing banners on Twitter, LinkedIn and Facebook?

I [revamped my blogfolio](https://braydoncoyer.dev/blog/introducing-my-new-blogfolio) this year and knew I didn't want to continue to create banner images for my articles, and manually create Open Graph images for my social outlets. 

I'm extremely happy with the result - now when I share my articles online, my Open Graph images look something like this:

![Open Graph image contains article title, author, domain and article banner as an image underlay aligned on the right](https://res.cloudinary.com/braydoncoyer/image/upload/v1640022889/dynamic_og_images_preview_ovwxw9.png)

Open Graph image contains article title, author, domain and article banner as an image underlay aligned on the right

In this article, I'll show you how to leverage the powerful Cloudinary API to create dynamic Open Graph images and banners for your website or blog. 

## TL;DR

Want to know the secret right away? We'll be passing various variables to the Cloudinary image request URL which will transform a template image and add an article title and banner image.

Read on to learn how to do this, or check out my [open source blogfolio on GitHub](https://github.com/braydoncoyer/braydoncoyer.dev) and see how I accomplished this.

## What are Open Graph meta tags?

Open Graph meta tags help make your content more clickable, sharable and visible on the web, especially on social media.

`meta` tags are small self-closing tags that inform the web how to display your content. The [Open Graph protocol](https://ogp.me) is part of Facebook's endeavor to consolidate the various technologies and provide developers a single protocol to adhere to in order to allow content to display more richly on the internet. 

## Sign up for Cloudinary

First, head to [Cloudinary](https://cloudinary.com/invites/lpov9zyyucivvxsnalc5/gle5rlywpxclhxkdtqur) and create an account.

Cloudinary has a free tier containing 25 monthly credits, which can be consumed by transforming images, storing images and videos, and spending the bandwidth needed to access assets in your bucket.

<aside>
✨ Cloudinary gifts you extra monthly credits if you follow their social accounts and spread the word about the service through a message on your timeline. Look for these options on your Dashboard.

</aside>

## Upload your OG template to Cloudinary

Next, either find or create a template image that will be used as a starting point for all of the Open Graph banners. This takes care of a lot of initial layout positioning and creates consistency for the OG images.

The Twitter card images shown in the feed are a 1.91:1 ratio. ****If you're creating your own template, ensure to design it at the recommended resolution of 1200x630.

As an example, here is a preview of the template I created for my blog. It contains the basic layout, a transparent section on the right-hand side for the article banner to be used as an underlay, and most importantly, contains the text that will remain constant for each social sharing image we create.

![For the purpose of this preview, I’ve included a visual transparent section of the template. When you export to png, this will not be visible.](https://res.cloudinary.com/braydoncoyer/image/upload/v1640023518/dynamic_og_template_preview_tjknx7.png)

For the purpose of this preview, I’ve included a visual transparent section of the template. When you export to png, this will not be visible.

Once you've found or created a template, upload the image to Cloudinary under the Media Library.

![https://res.cloudinary.com/braydoncoyer/image/upload/v1640023795/dynamic_og_upload_cloudinary_vzpacl.png](https://res.cloudinary.com/braydoncoyer/image/upload/v1640023795/dynamic_og_upload_cloudinary_vzpacl.png)

### Add Article images to Cloudinary

It’s also important that your article images are hosted on Cloudinary so you can easily reference the image name when performing the transformation via the API.

You can either upload images to Cloudinary from your computer, or use one of their integrated tools to discover and import images into your media library. I use the built-in Unsplash integration to add my article banners to my library, but you can use other tools like Shutterstock and iStock.

With the template and article images uploaded to Cloudinary, we're ready to move to Next.js.

## Create a Shared SEO Component

This part is optional depending on your setup. 

I tend to create reusable layout components that I consume on each page depending on the need and purpose. 

If you already have a preexisting Next.js project, you may already have a reusable layout component. Either way, here's the general idea:

- Create a layout component to be used on your article pages.
- Pass children (the actual page content) and render accordingly.
- Pass meta information to be used for SEO purposes, including information which will be used with Cloudinary.

Here's an example of a layout component I've created called `Container`

```tsx
export function Container(props) {
  const { children } = props;

  return (
    <div className={`bg-white dark:bg-dark min-h-screen'}>
      <NavMenu />
      <main className="flex flex-col mx-auto max-w-6xl justify-center px-4 bg-white dark:bg-dark prose prose-lg md:prose-xl dark:prose-dark relative">
        {children}
      </main>
    </div>
  );
}
```

From the snippet above, you can see that I have passed `children` to this component which is rendered inside a `main` element with appropriate Tailwind utility classes to achieve my desired layout for my blog.

Since this component will be reused on every page of my application, we can also include SEO information and dynamically pass information based on which page is rendered. 

```tsx
import { useRouter } from 'next/router';
import Head from 'next/head';

export function Container(props) {
  const { children, ...customMeta } = props;

	const router = useRouter(); // create a router to be used in the meta object below

	const meta = {
    title: "My site",
    description: "A description about my site",
    imageUrl: "path-to-an-image",
    type: 'article'
    twitterHandle: "https://twitter.com/BraydonCoyer",
    canonicalUrl: `https://braydoncoyer.dev${router.asPath}`,
    date: null,
    ...customMeta // this replaces any properties that we pass to the component as props
  };

  return (
    <div className={`bg-white dark:bg-dark min-h-screen'}>
			
			<Head>
        <title>{meta.title}</title>
        <meta name="robots" content="follow, index" />
        <meta content={meta.description} name="description" />
        <meta
          property="og:url"
          content={`https://braydoncoyer.dev${router.asPath}`}
        />
        <link rel="canonical" href={meta.canonicalUrl} />
        <meta property="og:type" content={meta.type} />
        <meta property="og:site_name" content="Braydon Coyer" />
        <meta property="og:description" content={meta.description} />
        <meta property="og:title" content={meta.title} />
        <meta property="og:image" content={meta.imageUrl} />
        <meta name="twitter:card" content="summary_large_image" />
        <meta name="twitter:site" content={meta.twitterHandle} />
        <meta name="twitter:title" content={meta.title} />
        <meta name="twitter:description" content={meta.description} />
        <meta name="twitter:image" content={meta.imageUrl} />
        {meta.date && (
          <meta property="article:published_time" content={meta.date} />
        )}
      </Head>

      <NavMenu />
      <main className="flex flex-col mx-auto max-w-6xl justify-center px-4 bg-white dark:bg-dark prose prose-lg md:prose-xl dark:prose-dark relative">
        {children}
      </main>
    </div>
  );
}
```

While this looks like a lot of code, we are simply crafting a meta object to be consumed inside the `Head` component that Next.js exposes.

This is enough to properly have your application leverage SEO: simply pass a few props to the `Container` component and you should be good to go! 

However, notice that the `meta` tags containing `og:image` and `twitter:image` using a static image URL. 

Let's make it dynamic with Cloudinary.

## Building a Dynamic OG Image with the Cloudinary API

Cloudinary's API supports text and image overlays, providing an easy way to dynamically transform images. 

Utilizing the API is as simple as appending variables to the URL of an image hosted on Cloudinary. 

In the end, the URL may look something like this: 

```
https://res.cloudinary.com/braydoncoyer/image/upload/w_1200,h_630,c_fill,f_auto/w_580,h_630,c_fill,u_learn_tailwindplay_banner.jpg/fl_layer_apply,g_east/w_630,h_450,c_fit,co_rgb:FFFFFF,g_west,x_45,y_-40,l_text:arial_60_bold:Learn%20Tailwind%20with%20TailwindPlay/og_social_large.png
```

The URL is a bit cumbersome, but let me break it down from top to bottom:

- `[https://res.cloudinary.com/braydoncoyer/](https://res.cloudinary.com/braydoncoyer/)` - a base URL containing my Cloudinary account name.
- `image/upload` - the asset type.
- `w_1200,h_630` - the width and height for the entire image.
- `c_fill` - crop mode.
- `f_auto` - automatically chooses the best format based upon which browser is being used.
- `w_580,h_630` - the size of the image underlay.
- `u_learn_tailwindplay_banner.jpg` - the name of the banner associated with the article.
- `fl_layer_apply` - applies all chained transformations on the underlaid image.
- `g_east` - informs Cloudinary which sector on the image to place the underlay.
- `w_630,h_450` - the size of a text box
- `co_rgb:FFFFFF` - specifies the text color
- `g_west,x_45,y_-40` - determines which sector to place the text, and includes exact pixel positions.
- `text:arial_60_bold:` - font name and size.
- `Learn%20Tailwind%20with%20TailwindPlay` - the encoded text value to display on the left-side of the image.
- `og_social_large.png` - the name of the template uploaded to Cloudinary.

### Configure a function to generate the Cloudinary URL

Manually creating a URL like this would be extremely tedious and timeconsuming. To make the process easier, let's create a function to build the Cloudinary URL and return it to us. 

I've created a file called `generateSocialImage` in my `lib` directory.

```tsx
export default function generateSocialImage({
  title,
  cloudName,
  imagePublicID,
  cloudinaryUrlBase = 'https://res.cloudinary.com',
  version = null,
  titleFont = 'arial',
  titleExtraConfig = '_bold',
  underlayImageWidth = 580,
  underlayImageHeight = 630,
  underlayImage = '',
  imageWidth = 1200,
  imageHeight = 630,
  textAreaWidth = 630,
  textAreaHeight = 450,
  textLeftOffset = 45,
  textBottomOffset = -40,
  textColor = 'FFFFFF',
  titleFontSize = 60
}): string {

  // configure social media image dimensions, quality, and format
  const imageConfig = [
    `w_${imageWidth}`,
    `h_${imageHeight}`,
    'c_fill',
    'f_auto'
  ].join(',');

	// configure the underlay - the actual article banner
  const underlayClonfig = [
    `w_${underlayImageWidth}`,
    `h_${underlayImageHeight}`,
    `c_fill`,
    `u_${underlayImage}/fl_layer_apply`,
    `g_east`
  ];

  // configure the title text
  const titleConfig = [
    `w_${textAreaWidth}`,
    `h_${textAreaHeight}`,
    'c_fit',
    `co_rgb:${textColor}`,
    'g_west',
    `x_${textLeftOffset}`,
    `y_${textBottomOffset}`,
    `l_text:${titleFont}_${titleFontSize}${titleExtraConfig}:${encodeURIComponent(
      title
    )}`
  ].join(',');

  // combine all the pieces required to generate a Cloudinary URL
  const urlParts = [
    cloudinaryUrlBase,
    cloudName,
    'image',
    'upload',
    imageConfig,
    underlayClonfig,
    titleConfig,
    version,
    imagePublicID
  ];

  // remove any falsy sections of the URL (e.g. an undefined version)
  const validParts = urlParts.filter(Boolean);

  // join all the parts into a valid URL to the generated image
  return validParts.join('/');
}
```

For the most part, you can plug in your information and the function will work as expected. You can tinker with the destructured props to change the position of the text and image to fit your needs.

I call this function on my article page, where I can pass the article title and banner image to the function. The function returns the new Cloudinary URL and is then provided to the `Container` component. 

<aside>
📣 Recall that the `Container` component hosts the `meta` tags needed for proper search engine optimization.

</aside>

Please note the image named passed as `imagePublicID` - this is the name of the template image uploaded to Cloudinary. Make sure you swap this name out to match the name of the template you uploaded in your Cloudinary media library.

```tsx
// [slug].ts

const socialImageConf = generateSocialImage({
  title,
  underlayImage: coverImage.slice(coverImage.lastIndexOf('/') + 1),
  cloudName: 'braydoncoyer',
  imagePublicID: 'og_social_large.png' // the OG template image name uploaded in Cloudinary 
});

...

return (
	<Container
	title={title}
	description={description}
	imageUrl={socialImageConf} // pass the dynamic URL here
	date={new Date(publishedDate).toISOString()}
  type='article'
>
		...
	</Container>
)
```

## Testing your social sharing Open Graph images

Once everything is hooked up and configured appropriately, you should be able to run your Next.js project ( `npm run dev` ) and see the `meta` tags on the DOM under the `head` element.

![https://res.cloudinary.com/braydoncoyer/image/upload/v1640024175/dynamic_og_constructued_url_ls7uma.png](https://res.cloudinary.com/braydoncoyer/image/upload/v1640024175/dynamic_og_constructued_url_ls7uma.png)

Look for the `og:image` tag, copy the URL and paste it in a new tab. If everything works, you should see your new dynamic Open Graph image that will appear on social media outlets!

### Using online tools to validate the Open Graph images

Once your application is published, grab the full article slug and paste it into the textbox on [socialsharepreview.com](http://socialsharepreview.com) - a tool that validates that your meta tags are correctly configured for the web.

![https://res.cloudinary.com/braydoncoyer/image/upload/v1640024270/dynamic_og_check_preview_lkzcxh.png](https://res.cloudinary.com/braydoncoyer/image/upload/v1640024270/dynamic_og_check_preview_lkzcxh.png)

## Conclusion

And with that - you've now created a system that dynamically creates Open Graph images for social outlets using Cloudinary and Next.js! 

If you've made it this far and completed this article, I'd love for you to reach out to me on [Twitter](https://twitter.com/BraydonCoyer) and send me a link to your blog or website so I can see the Open Graph images at work!  

### References

- [How to build a personalized image social sharing app with Cloudinary and Next.js](https://www.contentful.com/blog/2021/09/08/personalized-image-social-sharing-with-cloudinary-nextjs/)
- [Automated Open Graph Images with 11ty and Cloudinary](https://www.juanfernandes.uk/blog/automated-open-graph-images-with-11ty-and-cloudinary/)
- [Dynamic Open Graph Images](https://urre.me/writings/dynamic-open-graph-images/)