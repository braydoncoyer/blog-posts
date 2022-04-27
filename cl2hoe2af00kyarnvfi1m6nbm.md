## Enable Autocomplete for Tailwind CSS in VSCode

If you’ve recently found Tailwind CSS, you’re no doubt amazed at how quickly you can write CSS! But you may be wondering if there is a way to add autocomplete in VSCode so you can write Tailwind CSS even more quickly and more efficiently?

**Absolutely! The Tailwind Labs team has developed and released an official plugin that adds autocomplete to your VSCode environment, and it only takes a few clicks to enable! The Autocomplete extension allows you to quickly apply appropriate utility class names to your markup, removing the guess work and providing relevant information at a glance. Furthermore, the plugin also supports linting, hover previews and syntax highlighting! Let’s quickly add autocomplete to VSCode!** 

## Install the Tailwind CSS VSCode Plugin

Adding autocomplete to VSCode for Tailwind is easier than you think. Head to the Visual Studio Code Marketplace and search for [Tailwind CSS IntelliSense](https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss). Alternatively, you can open VSCode and click on the Extensions tab in the sidebar and search for the plugin. 

Once you’ve found the plugin in VSCode, click the Install button to install the plugin globally in your workspace. From here, the plugin should ***automagically*** ✨ work so long as you have Tailwind installed and a `tailwind.config.js` or `tailwind.config.cjs` file in your repository. If you aren’t seeing the changes take affect, you may need to restart VSCode and try again.

Unlike some other VSCode extensions for Tailwind, this plugin is developed and maintained by the Tailwind team! You can expect this extension to be updated as new major versions of Tailwind are released with new features!

## Benefits of Autocomplete

Writing CSS with Tailwind is already insanely effective, but it can be even better if you use VSCode with the autocomplete plugin! With autocomplete added in VSCode for Tailwind, you can start to type in a utility class and a pop up suggests a few options to choose from. This is extremely helpful for a few reasons:

- You may not need to fully type the utility class. Simply start to type the class name and the intelligent autocomplete will suggest a few options. You can use the arrow keys to navigate up and down in the suggestion list, and hit Enter or Return on your keyboard to add the class name into the markup. It’s super quick!
- If you can’t remember which Tailwind class to use, the autocomplete can help you narrow down and find the correct utility.
- The autocomplete can be very useful, especially when selecting color-based utility classes! Not only can you quickly scan through the color palette, but the autocomplete also gives you a preview of the color in the list!

## Identify Tailwind CSS Errors with Linting

Using Tailwind out-of-the-box provides a fast way to write CSS in your application. But the real power of Tailwind begins to shine when you extend the library to use your own design system. Supplying your own color palette, extending the default typography settings and even using the `@apply` feature can help set your site above others made with the library and provide a custom solution to fit any development need. 

As you extend the library, you run the risk of configuration code that produces errors at compile time. Without the Tailwind VSCode extension installed, you may not know that an error has occurred until the site is built. 

Thankfully, the plugin helps guard against error and small mistakes by providing real-time linting, allowing you to not only quickly identify when something is wrong and should be changed, but also get an idea of how to make the code compliant. 

For example, when using `@apply` and stringing together class names in an external `.css` file, I occasionally use a text-color that I have not defined in my `tailwind.config.js` file. If (and when) these small mistakes occur, the Tailwind VSCode plugin puts a red squiggly underneath the offender, making it easy for me to see the error and quickly fix before saving the file and beginning a refresh of the development server.  

## Use Hover Previews to Peek at The Underlying CSS

Tailwind CSS is a CSS framework, allowing you to write your styles without leaving your markup. Unlike other CSS frameworks like Bootstrap where you can learn Bootstrap-specific class names, Tailwind suggests that the user understands CSS as a prerequisite. Generally, Tailwind won’t aid developers write CSS faster if they don’t understand CSS in the first place. 

However, with the Tailwind CSS autocomplete plugin installed in VSCode, it is possible to peek into the underlying CSS implementation. **Simply hover over a utility class in your markup and the extension will provide a window showing the actual CSS happening behind the scenes.** 

Why is this helpful?

First, having the ability to look at the CSS behind the utility class can be helpful in better understanding CSS and proper syntax. Second, for some non-utility Tailwind classes like the `prose` class, you can peek at all of the CSS rules and statements that are applied to the code. This may not be super useful in your day-to-day operations, but I’ve already run into a few situations where this has helped me solve an issue or quickly see which CSS unit is applied to the elements on the DOM.

![https://res.cloudinary.com/braydoncoyer/image/upload/v1651069540/prose_f8wbn2.png](https://res.cloudinary.com/braydoncoyer/image/upload/v1651069540/prose_f8wbn2.png)

## What about JetBrains IDEs?

Can you get autocomplete with Tailwind CSS if VSCode isn’t your IDE of choice? Absolutely! 

First, make sure you have Tailwind installed in your project and a `tailwind.config.js` file at the root of your repository. Next, open the **Settings/Preferences | Plugins** page and enable the **CSS** and **Tailwind CSS** plugins.

That’s it! WebStorm will now provide autocomplete for Tailwind classes in your markup! Additionally, Webstorm will also allow you to preview the resulting CSS if you hover over a Tailwind class, and the IDE can provide code completion based on the customization made through the `tailwind.config.js` file (if you add new colors, or extend the default typography settings, for example). 

## Conclusion

Tailwind makes writing CSS a breeze. It’s lightning quick! But with the aid of the Tailwind CSS VSCode autocomplete feature, not only is it faster to find utility classes you want to add to your markup, but you also get the added benefit of a plugin that bundles linting for real-time error identification and hover previews for when you want to glance at the code beneath the class name. Furthermore, you can have peace of mind knowing that the plugin will never become stale or outdated because the Tailwind CSS team are behind the extension and will continue to push updates as new features are released!