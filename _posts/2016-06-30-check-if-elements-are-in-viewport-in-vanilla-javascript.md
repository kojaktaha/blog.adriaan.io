---
---

With *CSS* you can do a lot of cool stuff like `transform` and `transition`. But you will still need *JavaScript* to add and remove classes to elements. In the code example you will see how to add classes to elements which are in the current viewport. It will **not** cover how to remove them later.

Let's say you have some sections on your webpage and you want to show them with a nice `transition` when they become visible for the user.

You will need this *SCSS* with a `visible` class:

```scss
section.fly-in {
  transition: 1s ease-in-out;
  transform: translate(200px, 0);

  &.visible {
    transform: translate(0, 0);
  }
}
```

To add the class `visible` we use some *JavaScript*:

```js
const sections = document.querySelectorAll('section.fly-in');

window.onscroll = function() {
  // Don't run the rest of the code if every section is already visible
  if (!document.querySelectorAll('section.fly-in:not(.visible)')) return;
  
  // Run this code for every section in sections
  for (const section of sections) {
    if (section.getBoundingClientRect().top <= window.innerHeight * 0.75 && section.getBoundingClientRect().top > 0) {
      section.classList.add('visible');
    }
  }
};
```

Note: this code is ES6, so maybe you may have to compile it to ES5 if you run it in a browser.
