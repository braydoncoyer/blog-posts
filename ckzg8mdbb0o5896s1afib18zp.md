## Tailwind Gradients - How to Make a Glowing Gradient Background

Gradients have become extremely popular on the web. In fact, it’s unusual to see a product website without a gradient to add a splash of color and contrast all of the negative space. 

Subtlety can go a long way - and in this brief article, I’ll show you how to achieve a powerful glowing gradient effect with Tailwind. 

Here’s what we’ll be building:

<iframe height="565" style="width: 100%;" scrolling="no" title="Glowing Gradients with Tailwind CSS" src="https://codepen.io/braydoncoyer/embed/LYzwoLm?default-tab=result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/braydoncoyer/pen/LYzwoLm">
  Glowing Gradients with Tailwind CSS</a> by Braydon Coyer (<a href="https://codepen.io/braydoncoyer">@braydoncoyer</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>



## Tailwind CSS Gradients

Before we dive into the implementation, it’s important to understand how gradients work in Tailwind. Tailwind added support for gradients in its second major release, providing several new utility classes to add color stops to achieve a gradient background on an element.

In a very basic linear example, you must define the starting color and the ending color, using the `from-{color}` and `to-{color}` utilities respectively.  Here’s an example.

```html
<div class="h-14 bg-gradient-to-r from-cyan-500 to-blue-500"></div>
```


![basic Tailwind gradient](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/69ue76do14zbx5kp00pf.png)

### Define the gradient direction

You may have noticed an extra utility class in the example above (`bg-gradient-to-r`). In addition to the color stops, it’s important to define the direction of the linear gradient on the targeted element. 

There are several utility classes available to define the Tailwind gradient direction:

- `bg-gradient-to-t`
- `bg-gradient-to-tr`
- `bg-gradient-to-r`
- `bg-gradient-to-br`
- `bg-gradient-to-b`
- `bg-gradient-to-bl`
- `bg-gradient-to-l`
- `bg-gradient-to-tl`

It’s also worth noting that you can apply the direction and color of the gradient conditionally. 

For example, if you wanted to change the direction of the gradient on element hover, you may use the following utility classes:

```html
<div class="bg-gradient-to-r hover:bg-gradient-to-l from-purple-500 to-pink-500"></div>
```

### Tailwind Gradients with more than two color stops

It’s possible to add an additional color stop to Tailwind linear gradients. You can achieve this using the `via-{color}` utilities. Elements with this utility applied will fade seamlessly between the first, second and ending color stops.

```html
<div class="h-24 bg-gradient-to-r from-indigo-500 via-purple-500 to-pink-500"></div>
```


![gradient with more than two steps](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/awd12j0rjrpm4ckdh0n6.png)

## Setting Up Your Environment to Create a Glowing Gradient

Now that we have a basic understanding of how Tailwind handles gradients, we can turn our attention to the implementation. If you’re wanting to add this glowing gradient effect in a side-project, chances are you have a React, Vue or Angular repository already spun up and standing by. If you haven’t installed Tailwind in your framework of choice, check out the [official spin-up guides here](https://tailwindcss.com/docs/installation/framework-guides).

If you’re just tinkering and want to try new things, the Tailwind team has their own web-based editor with the library configured and ready-to-go. I recommend trying [Tailwind Play](https://play.tailwindcss.com/) and following along with this tutorial. [CodePen](https://codepen.io/trending) is another great option.

## Creating the Background and Constraints with Tailwind CSS

Before we start working on the card and glowing gradient, let’s create the background and add in some width constraints so that the card doesn’t take up the entire screen. You can tweak this to your needs. The code below adds a subtle gray background, uses Flexbox to center the child `div` and applies a max-width.

For future steps, all markup will be wrapped inside these two container `div` elements.

```html
<div class="min-h-screen bg-gray-50 flex flex-col justify-center relative overflow-hidden sm:py-12">
  <div class="max-w-7xl mx-auto">
	<!-- All markup will go here -->
  </div>
</div>
```

## Create the Card with Basic Utility Classes

Next, create a `div` element representing the card itself. Apply some *X* and *Y* padding, make the background white, round the corners and apply some Flexbox utility classes for what’s to come.

```html
<div class="relative px-7 py-6 bg-white ring-1 ring-gray-900/5 rounded-lg leading-none flex items-top justify-start space-x-6">
   
</div>
```

### Add Card Content

Now that the container of the card has been established, we can fill it out with some content. The example I’ve created uses an icon on the left, and the card information on the right.

Give the icon a size and optionally add a color using the `text-{color}` utility class. 

The actual text content of the card is placed within a containing `div` , breaking free from the parent’s flex layout and changing it back to the default display value of `block`. 

The containing `div` has a `space-y-2` utility applied to it which will add margin between the child elements, providing some subtle negative space. 

```html
<div class="relative px-7 py-6 bg-white ring-1 ring-gray-900/5 rounded-lg leading-none flex items-top justify-start space-x-6">
	<!-- card content -->
  <svg class="w-8 h-8 text-purple-600" fill="none" viewBox="0 0 24 24">
    <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M6.75 6.75C6.75 5.64543 7.64543 4.75 8.75 4.75H15.25C16.3546 4.75 17.25 5.64543 17.25 6.75V19.25L12 14.75L6.75 19.25V6.75Z"></path>
  </svg>
  <div class="space-y-2">
    <p class="text-slate-800">Learn how to make a glowing gradient background!</p>
    <a href="#" class="block text-indigo-400 group-hover:text-slate-800 transition duration-200">Read Article →</a>
  </div>
</div>
```

> 👉 NOTE: The card container does not need to be a flex element if you don’t want content side-by-side.


![The card with content](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/3ac44c40uim5106v16ze.png)

With all this in place, we’re ready to add the gradient! 

## Create the Glowing Gradient with Tailwind Utility Classes

Making the glowing gradient is shockingly simple. In fact, we really only need one additional element. 

Inside of the first container `div` used to apply a max-width, create an additional `div` at the same hierarchy level as the `div` used for the card. Apply the following class names to the new element:

```html
<div class="min-h-screen bg-gray-50 flex flex-col justify-center relative overflow-hidden sm:py-12">
  <div class="max-w-7xl mx-auto">
		
		<!-- New Div -->
		<div class="absolute -inset-1 bg-gradient-to-r from-purple-600 to-pink-600 rounded-lg blur opacity-25"></div>
 
		<div class="relative px-7 py-6 bg-white ring-1 ring-gray-900/5 rounded-lg leading-none flex items-top justify-start space-x-6">
		   <svg class="w-8 h-8 text-purple-600" fill="none" viewBox="0 0 24 24">
         <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M6.75 6.75C6.75 5.64543 7.64543 4.75 8.75 4.75H15.25C16.3546 4.75 17.25 5.64543 17.25 6.75V19.25L12 14.75L6.75 19.25V6.75Z"></path>
       </svg>
       <div class="space-y-2">
         <p class="text-slate-800">Learn how to make a glowing gradient background!</p>
         <a href="#" class="block text-indigo-400 group-hover:text-slate-800 transition duration-200">Read Article →</a>
       </div>
    </div>
  </div>
</div>
```

The magic happens with the `-inset-1` utility class that Tailwind provides, anchoring absolutely positioned elements against the edges of the nearest positioned parent. Since the `inset` utility class is prefixed with a negative value, the element will be positioned outside of the top, right, left and bottom of the parent element.


![The gradient background isn't applied to the card](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/k8ee70smhua8c4h66hdx.png)

Of course, since we haven’t set a reference point for this element, it doesn’t give us an expected result. To fix this, add the `relative` class to the parent `div`.

```html
<div class="relative max-w-7xl mx-auto">
		<div class="absolute -inset-1 bg-gradient-to-r from-purple-600 to-pink-600 rounded-lg blur opacity-25"></div>
 
		<div class="relative px-7 py-6 bg-white ring-1 ring-gray-900/5 rounded-lg leading-none flex items-top justify-start space-x-6">
		   <svg class="w-8 h-8 text-purple-600" fill="none" viewBox="0 0 24 24">
         <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M6.75 6.75C6.75 5.64543 7.64543 4.75 8.75 4.75H15.25C16.3546 4.75 17.25 5.64543 17.25 6.75V19.25L12 14.75L6.75 19.25V6.75Z"></path>
       </svg>
       <div class="space-y-2">
         <p class="text-slate-800">Learn how to make a glowing gradient background!</p>
         <a href="#" class="block text-indigo-400 group-hover:text-slate-800 transition duration-200">Read Article →</a>
       </div>
    </div>
  </div>
```


![The gradient background has been fixed](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/vw0l11jdbtpf1dzmzerr.png)

Feel free to tweak the gradient colors, gradient direction, blur and opacity to your liking!

### Implementing the Gradient Hover State in Tailwind with the Group Utility and Hover Modifier

In its current state, the gradient is noticeable behind the card but it may be nice to increase the opacity of the gradient when the user hovers over the card. Thankfully, Tailwind makes this an easy task with the help of the `hover` modifier. 

Traditionally, the `hover` modifier activates when your mouse overlaps with the element. However, what if we want our gradient to change when the mouse is over any part of the card, not just the gradient? 

This is where the `group` utility class comes into play. The Tailwind `group` utility class allows styles to be applied based on the state of some parent element. The target element can change with the `group-{modifier}` utility. 

In our example, we want to apply the `group` class to the parent card `div`, and then set `group-hover:opacity-100` modifier on the gradient itself. 

```html
<div class="group relative max-w-7xl mx-auto">
 <div class="absolute -inset-1 bg-gradient-to-r from-purple-600 to-pink-600 rounded-lg blur opacity-25 group-hover:opacity-100 transition duration-1000 group-hover:duration-200"></div>
 <div class="px-7 py-6 bg-white ring-1 ring-gray-900/5 rounded-lg leading-none flex items-top justify-start space-x-6">
   <svg class="w-8 h-8 text-purple-600" fill="none" viewBox="0 0 24 24">
     <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M6.75 6.75C6.75 5.64543 7.64543 4.75 8.75 4.75H15.25C16.3546 4.75 17.25 5.64543 17.25 6.75V19.25L12 14.75L6.75 19.25V6.75Z"></path>
   </svg>
   <div class="space-y-2">
     <p class="text-slate-800">Learn how to make a glowing gradient background!</p>
     <a href="#" class="block text-indigo-400 group-hover:text-slate-800 transition duration-200">Read Article →</a>
   </div>
 </div>
</div>
```

Now, whenever the mouse is over any part of the card, the gradient’s opacity will increase to 100%. The effect is a bit jarring, though - let’s add a nice transition to allow the opacity to gradually increase and decrease.

To do this, simply add `transition duration-1000` to the gradient `div` signaling Tailwind to make the transition 1 second long. Optionally, if you wanted the increase in opacity to be quicker, you can add the `group-hover:duration-200` utility class to the `div` as well. 

With that we now have replicated the popular glowing gradient effect with Tailwind CSS! 

Here’s the final result! 

<iframe height="565" style="width: 100%;" scrolling="no" title="Glowing Gradients with Tailwind CSS" src="https://codepen.io/braydoncoyer/embed/LYzwoLm?default-tab=result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/braydoncoyer/pen/LYzwoLm">
  Glowing Gradients with Tailwind CSS</a> by Braydon Coyer (<a href="https://codepen.io/braydoncoyer">@braydoncoyer</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>

## A Word on Safari Support

If you test your changes in Safari, the result may look a bit strange and unexpected. Because of key support missing in Safari, you may need to ensure [Autoprefixer is installed and configured in your project](https://tailwindcss.com/docs/using-with-preprocessors#using-post-css-as-your-preprocessor). 

## Conclusion

Thanks for reading! If you enjoyed this article, consider signing up for [my newsletter](https://www.getrevue.co/profile/braydoncoyer) so you don't miss out on helpful content like this!