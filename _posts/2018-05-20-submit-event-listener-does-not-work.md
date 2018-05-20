---
---

When you want to submit a form programmatically the event submit does not get triggered. Here is how you can still trigger the submit event.

I use this when I have another element than a submit button in a form. Let's say we have this form:

```html
<form>
  <input name="email" type="email" placeholder="Enter your email">
  <img class="submit-image" src="..." alt="Submit">
</form>
```

I would love to trigger the submit with a click on the image.

This does not work:

```js
const form = document.querySelector('form')
form.addEventListener('submit', (event) => {
  event.preventDefault()
  console.log('We are never getting here unless your click a submit button in the form')
})

const image = document.querySelector('submit-image')
image.addEventListener('click', function() {
  form.submit()
})
```

But fortunately we can use [`document.createEvent`](https://developer.mozilla.org/en-US/docs/Web/API/Document/createEvent):

```js
const form = document.querySelector('form')
form.addEventListener('submit', (event) => {
  event.preventDefault()
  console.log('We are never getting here unless your click a submit button in the form')
})

const image = document.querySelector('submit-image')
image.addEventListener('click', function(clickEvent) {
  const domEvent = document.createEvent('Event')
  domEvent.initEvent('submit', false, true)
  clickEvent.target.closest('form').dispatchEvent(domEvent)
})
```
