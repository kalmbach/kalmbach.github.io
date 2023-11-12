# Dark/Light Modes
I love to read my blog and other websites in dark mode, but every once I need
the light mode to shock my senses and enjoy the forgotten youth of my eyes.

I figured it would be nice to have a toggle at the end of the page to switch
from Dark to Light modes easily, so we did. 

# How it works?
I thought you would never ask :)

To pull this off we need to:
1. Prepare the Document
2. Add the CSS
3. Add the JS

## Preparing the Document

To switch modes, we will basically swap the background and the foreground colors
on the entire site, we will starting by adding a data attribute to the **html**
element that will hold the default mode.

```html
<html data-theme="dark">
</html>
```

We also need a visual element the user can click to perform the action on demand.
A button will do.

```html
  <button type="button" aria-label="Light Theme" data-theme-toggle>
    Light Theme
  </button>
```

The **data-theme-toggle** attribute will serve as a selector later.

The initial status of the button will show the option to change to **Light Theme**.

## Adding the CSS

In the past we would be use different classes to define different styles
for the site, and then switch classes on the **html** document. Not anymore.

Introducing [CSS Custom Properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties)!

We can define **CSS Variables** to hold the colors and then reuse them throughout the document, and the beauty of 
it is that we can redefine the same set of such properties tied to a selector.

```css
[data-theme="light"] {
  --color-bg: white;
  --color-fg: black;
}

[data-theme="dark"] {
  --color-bg: black;
  --color-fg: white;
}

body {
  color: var(--color-fg);
  background-color: var(--color-bg);
}
```
Remember the **data-theme** attribute? Yes! Is the one we defined in the **<html>** element.
As it is now, you can manually change the value of that attribute to see how the site modes look.
```javascript
document.querySelector('html').setAttribute('data-theme', 'light');
document.querySelector('html').setAttribute('data-theme', 'dark');
```

## Adding the Javascript

With the previous changes we already have all the structure to show different modes on the site.
What we need now is to provide a dynamic way to the visitor to switch them at will.

First we will find out what is the user's system preference by matching media queries with 
[window.matchMedia](https://developer.mozilla.org/en-US/docs/Web/API/Window/matchMedia)

```javascript
window.matchMedia('(prefers-color-scheme: dark)')
window.matchMedia('(prefers-color-scheme: light)')
```

Once we have the user preference, we will save it on the browser **localStorage**
```javascript
localStorage.saveItem('theme', 'dark')
```

In the next visit we will check if there is a saved preference in the **localStorage**, if not
we will use the system's preference.

To make all this easier, we will load a set of handy functions

```javascript
// Get Theme Preference
function getTheme() {
  const mediaSettingDark = window.matchMedia(
    '(prefers-color-scheme: dark)'
  );

  const mediaSetting = mediaSettingDark.matches ? 'dark' : 'light';
  const storedSetting = localStorage.getItem('theme');

  return storedSetting || mediaSetting;
}

// Set Theme Preference and reflect that change in the button
function setTheme(theme) {
  document.querySelector('html').setAttribute('data-theme', theme);
  localStorage.setItem('theme', theme);

  const label = theme === 'dark' ? 'Light Theme' : 'Dark Theme';
  const button = document.querySelector('[data-theme-toggle]');

  if (button) {
    button.innerText = label;
    button.setAttribute('aria-label', label);
  }
}

// Toggle Themes
function toggleTheme() {
  const theme = getTheme() === 'dark' ? 'light' : 'dark';
  setTheme(theme);
}

// Set Preferred Theme
function setCurrentTheme() {
  setTheme(getTheme());
}
```

Now all we need to do is to add an event listener to the button to
react to the user's click.

```javascript
const button = document.querySelector('[data-theme-toggle]');
button.addEventListener('click', toggleTheme);
```

Ready! Now the button is working.
But, we also want it to remember the visitor's preference, so we will set the
theme whent the document is loaded.

```javascript
<script>
  setCurrentTheme();
</script>
```

Nice and easy!
