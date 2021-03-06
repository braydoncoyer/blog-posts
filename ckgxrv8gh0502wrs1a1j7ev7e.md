## Make A Not-So-Spooky Ghost with HTML & CSS

Get in the Halloween spirit (ha 👻) by making this not-so-spooky ghost with some HTML and a few lines of CSS!

Open up your favorite IDE or follow along on CodePen!

Here's the final result: 

<iframe height="565" style="width: 100%;" scrolling="no" title="Shy Ghost - Halloween 2020" src="https://codepen.io/braydoncoyer/embed/ZEOxvdj?height=265&theme-id=dark&default-tab=css,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/braydoncoyer/pen/ZEOxvdj'>Shy Ghost - Halloween 2020</a> by Braydon Coyer
  (<a href='https://codepen.io/braydoncoyer'>@braydoncoyer</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>


### The Markup

Before I begin, I like to consider the end-result in terms of shapes, and how I'm going to structure the markup in a way I can utilize CSS to get the results I want. 

Let's think it through:
* The ghost has a capsule-like body
* The ghost has two round eyes
* The ghost has two round dimples
* The ghost has some circles at the bottom to give the effect of a wavy-bottom of the body
* A shadow is present at the bottom of the ghost to aid in the hovering effect


Each of these items can be translated to `divs` with appropriate class names applied.


```
<div class="ghost">
      <div class="ghost__eyes"></div>
      <div class="ghost__dimples"></div>
      <div class="ghost__feet">
        <div class="ghost__feet-foot"></div>
        <div class="ghost__feet-foot"></div>
        <div class="ghost__feet-foot"></div>
        <div class="ghost__feet-foot"></div>
      </div>
</div>
<div class="shadow"></div>
``` 

Now that we've created the structure (markup) of the ghost, let's start styling.


### The Ghost Body

Let's first define some color variables that we'll use throughout the article.

NOTE: I'm using SCSS, but you can use regular CSS variables, too.

```
$background: #00034b;
$white: #ffffff;
$grey: #dbdbdb;
$pink: #ffbeff;
$shadow: #000232;

``` 

Now that we have the colors defined, let's work on the ghost body. 

```
.ghost {
  // 1
  position: relative;
  width: 150px;
  height: 225px;
  background: $white;
  // 2
  box-shadow: -17px 0px 0px $grey inset, 0 0 50px #5939db;
  // 3 
  border-radius: 100px 100px 0 0;
}

```

Let's consider the styles above:
1. Even at this early stage, we can assume that we're going to need to position elements absolutely in the future, so we set the ghost's body as the anchor point. This will help us later.
2. Did you know you can use multiple box-shadows on the same element? Well, now you do! The box shadow is doing two things. First, it's giving us the grey inner shadow inside the ghost body (which is why we set it to `inset`). Second, we can use it to give us a nice glow around the ghost. 
3. How do we get the capsule-like design without effecting the bottom of the element? The `border-radius` property can take optional arguments so you can change the radius of different sides of the element. 

With these styles applied, we get the following result:


![Screen Shot 2020-10-30 at 2.12.34 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1604085162776/nw9zuFvck.png)

### The Eyes

Let's now look at (ha 👀 ) the eyes of the ghost. 

Because I'm using SCSS to style, I have the ability to nest my styles.

```
&__eyes {
    // 1
    display: flex;
    justify-content: space-around;
    margin: 0 auto;
    padding: 70px 0 0;
    width: 90px;
    height: 20px;

    // 2
    &:before,
    &:after {
      content: "";
      display: block;
      width: 15px;
      height: 25px;
      background: $background;
      border-radius: 50%;
    }
  }
```

A few things to note about the styles above: 
1. I've decided to use the `ghost__eyes` div as the container for both eyes. This means I'll position this container appropriately inside the ghost body.
2. What about the actual eyes? Instead of created a div for each individual eye, I've decided to use Pseudo-Elements and style them appropriately. This helps reduce unneeded markup. 

We've got some eyes now!


![Screen Shot 2020-10-30 at 2.23.08 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1604085796412/bG6LWFBWQ.png)


### The Dimples

Let's take the same approach with the dimples and use Pseudo-Elements for the dimples.

```
&__dimples {
    display: flex;
    justify-content: space-around;
    margin: 0 auto;
    padding: 35px 0 0;
    width: 130px;
    height: 20px;

    &:before,
    &:after {
      content: "";
      display: block;
      width: 15px;
      height: 15px;
      background: $pink;
      border-radius: 50%;
    }
  }

```


![Screen Shot 2020-10-30 at 2.25.44 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1604085955198/FkRLwW032.png)


### The Feet

We're making some good progress! Let's now make the bottom of the ghost by creating a few circles and positioning them correctly.

```
&__feet {
    width: 100%;
    // 1
    position: absolute;
    bottom: -13px;
    // 2
    display: flex;
    justify-content: space-between;
 
    &-foot {
      // 3
      width: 25%;
      height: 26px;
      border-radius: 50%;
      background: $white;

      // 4
      &:last-child {
        background-image: linear-gradient(to right, $white 55%, $grey 45%);
      }
    }
  }

```


1. Remember how we set `position: relative` up above in the ghost body? Here's they payoff. We want to place these feet at the bottom of the ghost body, so `position: absolute` is our friend here.
2. The feet need to be side-by-side and flexbox is one way to achieve that.
3. Because there are 4 feet total, each foot should take up 25% of the given width.
4. Finally, we target the last foot and use `linear-gradient()` to change the background color from `white` to `grey` in the middle of the element. This continues the illusion of the inner shadow that we set in the ghost body.



![Screen Shot 2020-10-30 at 2.42.16 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1604086945123/61hn_hlDK.png)


### The Shadow

Our little ghost is pretty much done! Let's quickly style the shadow!
There's nothing fancy going on here, we apply the color, size and position correctly.

```
.shadow {
  background: $shadow;
  width: 150px;
  height: 40px;
  margin-top: 50px;
  border-radius: 50%;
}
```


![Screen Shot 2020-10-30 at 2.44.03 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1604087050083/j1ow6G5HI.png)


### Animation

To wrap up, let's bring this little guy to life by using CSS animations!

Back up where we styled the ghost body, include an `animation` property. We'll create the actual keyframes animation next.

```
.ghost {
  ...
  animation: float 2s infinite
}
```

Next, define a keyframes animation called `float`. At the beginning and end of the animation (0% and 100%), the ghost should be at the initial starting position. Half way through the animation (50%), we want the ghost to be positioned slightly above the starting position. 

```
@keyframes float {
  0%,
  100% {
    transform: translateY(0);
  }

  50% {
    transform: translateY(-15px);
  }
}
```

Great! The ghost now floats up and down and repeats infinitely! 


To make the floating illusion more convincing, we can apply another animation on the shadow itself! 

Add an `animation` property to the styles on the shadow.

```
.shadow {
  ...
  animation: shadow 2s infinite;
}
```

And finally, define a keyframes animation that changes the scale of the shadow element.

```
@keyframes shadow {
  0%,
  100% {
    transform: scale(1);
  }

  50% {
    transform: scale(0.9);
  }
}
```


### Conclusion

And there we go! CSS Art is always so much fun to make and it's a great way to challenge yourself and learn new things.

Thanks for reading! If you liked this article and want more content like this, read some of my [other articles](https://blog.braydoncoyer.dev/) , [subscribe to my newsletter](https://braydoncoyer.dev/newsletter/) and make sure to follow me on [Twitter](https://twitter.com/BraydonCoyer)!

Happy Halloween! 

<iframe height="565" style="width: 100%;" scrolling="no" title="Shy Ghost - Halloween 2020" src="https://codepen.io/braydoncoyer/embed/ZEOxvdj?height=265&theme-id=dark&default-tab=css,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/braydoncoyer/pen/ZEOxvdj'>Shy Ghost - Halloween 2020</a> by Braydon Coyer
  (<a href='https://codepen.io/braydoncoyer'>@braydoncoyer</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>



