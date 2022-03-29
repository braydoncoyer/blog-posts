## The Powerful CSS not Selector

The `:not()` CSS selector is a powerful addition to the pseudo-class toolbelt, allowing you to select elements that are omitted by its argument.

## A basic :not() CSS Selector Example

Here’s an example. I have a few classes set up - one applies base styles for all buttons, one sets the styles of a primary button, and another determines what a primary disabled button should look like.


>📢 I’m using SCSS in the example below to gain the benefit of class nesting and variables, but the application of the `:not()` selector is the same.


```scss
.button {
  border: none;
  padding: 1rem 2rem;
  border-radius: 0.5rem;
  cursor: pointer;
	margin-top: 1rem;
}

.button--primary {
  background: $button--primary;
  color: white;
}

.button--disabled {
  background: $button--disabled;
  cursor: auto;
}
```

%[https://codepen.io/braydoncoyer/pen/KKZmNJa]

In order to align with accessibility, it’s important that the background of the button changes when in `hover` state. That’s simple enough; here’s the change.

```scss
.button--primary:hover {
  background: $button-primary-hover;
}
```

But, after adding the `:hover` selector, we run into a problem. Try hovering over the disabled button and notice that the background changes as if we were hovering over an active primary button.

%[https://codepen.io/braydoncoyer/pen/dyJWOBr]

How do we get around this? The `:not()` selector makes this an easy fix, allowing the change to only affect primary buttons that are *not* disabled!

```scss
.button--primary:hover:not(.button--disabled) {
  background: $button-primary-hover;
}
```

%[https://codepen.io/braydoncoyer/pen/JjMNbgm]


>📢 Instead of using a class to determine if the button is disabled, I could have opted to use the `:disabled` attribute. I think the examples above are a bit easier to follow.


## Browser Compatibility for the :not Selector

Thankfully, the `:not()` selector is supported by most major browsers. 

Check out [caniuse.com](https://caniuse.com/css-not-sel-list) to see the exceptions.

## Conclusion

In this article, we briefly discussed the `:not()` selector and saw a real-world example. A variety of options open up when using this selector - what applications can you think of?