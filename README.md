# Simpler-Sidebar-Evergreen
[![Flattr Button](https://button.flattr.com/flattr-badge-large.png)](https://flattr.com/submit/auto?user_id=dcdeiv&url=https%3A%2F%2Fgithub.com%2Fdcdeiv%2Fsimpler-sidebar)

A version of dcdeiv's Simpler Sidebar that uses CSS3 transitions rather than
jQuery animations.

* Simpler-Sidebar-Evergreen is forked from [Simpler-Sidebar](http://dcdeiv.github.io/simpler-sidebar).

[![NPM](https://nodei.co/npm/simpler-sidebar-evergreen.png)](https://nodei.co/npm/simpler-sidebar-evergreen/)


#### Why this fork??
* Simpler sidebar works well, and has a nice simple API, but jQuery animations
are slow and painful.
* CSS3 transitions (especially opacity and transform transitions) are smooth on
all devices.
* Note that a dependency on the 'transitionend' event means this version of
simpler-sidebar requires IE10+ and Safari 6.0+(all other browsers are
evergreen). You can probably support earlier with a transitionend polyfill.

## Getting Started
Download the [production version][min] of the [development version][max].

[min]: https://raw.github.com/dcdeiv/simpler-sidebar/master/dist/simpler-sidebar.min.js
[max]: https://raw.github.com/dcdeiv/simpler-sidebar/master/dist/simpler-sidebar.js

Simpler-Sidebar is also available via **NPM** and **Bower**:

* `bower install simpler-sidebar-evergreen`.
* `npm install simpler-sidebar-evergreen`.

This fork is simpler than Simple-Sidebar because you won't need to do much more than this:

```html
<div id="navbar">
	<!--
	#navbar is positioned fixed.

	It does not matter what element #toggle-sidebar is, give it a selector (in this example #toggle-sidebar).
	-->
	<span id="toggle-sidebar" class="button icon"></span>
</div>

<div id="sidebar">
    <!--
    Simpler-Sidebar will handle #sidebar's position.

    To let the content of your sidebar overflow, especially when you have a lot of content in it, you have to add a "wrapper" that wraps all content.
    -->
    <div id="sidebar-wrapper" class="sidebar-wrapper">
        <!--
        Links below are just an example. Give each clickable element, for example links, a class to trigger the closing animation.
        -->
        <a class="close-sidebar" href="#">Link</a>
        <a class="close-sidebar" href="#">Link</a>
        <a class="close-sidebar" href="#">Link</a>
        <a class="close-sidebar" href="#">Link</a>
    </div>
</div>
```

If you add the sidebar-wrapper (and you should), remember to give it this style attributes:

```css
.sidebar-wrapper {
    position: relative;
    height: 100%;
    overflow: auto;
}
```

At the bottom of the web page, just before the `</body>` tag, include the **jQuery** library.

```html
<script src="https://ajax.googleapis.com/ajax/libs/jquery/2.1.4/jquery.min.js"></script>
<script src="simpler-sidebar/dist/jquery.simpler-sidebar.min.js"></script>
```

Call the Simpler-Sidebar plugin function and fill it with the options you need. Here is an example of some required options.

```html
<script>
    $(document).ready(function() {
        $('#sidebar').simplerSidebar({
            opener: '#toggle-sidebar',
            sidebar: {
                align: 'left', //or 'right' - This option can be ignored, the sidebar will automatically align to right.
                width: 300, //You can ignore this option, the sidebar will automatically size itself to 300px.
                closingLinks: '.close-sidebar' // If you ignore this option, the plugin will look for all links and this can be buggy. Choose a class for every object inside the sidebar that once clicked will close the sidebar.
            }
        });
    });
</script>
```

## OPTIONS
This is a full list of options.
You can override the single option by using the plugin API or directly in the function.

### How to use the public access to plugin options:
The base API is `$.fn.simplerSidebar.settings`. Check [Options List](#options-list) out to see the full list of available APIs.

```javascript
$.fn.simplerSidebar.opener = '#toggle-sidebar';
$.fn.simplerSidebar.attr = 'simplersidebar';
$.fn.simplerSidebar.top = 42;
$.fn.simplerSidebar.animation.duration = '0.5s;
$.fn.simplerSidebar.animation.duration = 'ease-in-out';
$.fn.simplerSidebar.sidebar.align = 'left';
$.fn.simplerSidebar.sidebar.width = 300;
$.fn.simplerSidebar.sidebar.gap = 64;
$.fn.simplerSidebar.sidebar.closingLinks = '.close-sidebar';
$.fn.simplerSidebar.sidebar.css.zIndex = 3000;
$.fn.simplerSidebar.mask.display = true;
$.fn.simplerSidebar.mask.css.backgroundColor = 'black';
$.fn.simplerSidebar.mask.opacity = 0.5;
$.fn.simplerSidebar.mask.css.filter = 'Alpha(opacity=50)';

$( '#sidebar' ).simplerSidebar();
```
Overriding multiple options can be buggy, especially when you try to override `sidebar`, the plugin will crash.

```javascript
$.fn.simplerSidebar.settings.mask.css = {
	//your style
};
```

### Options List
* **opener**: selector for the button/icon that will trigger the animation.
* **attr**: is the `data-*` attribute that makes the plugin works. If `simplersidebar` is somehow causing you issues, you can change it.
* **top**: is the `position-top` of the entire plugin. You can choose whatever number you want (better if you choose it according to the navbar's height) or let it be 0 by ignoring it.
* **animation**:
  * **duration**: a duration string for the animation, in seconds, as per CSS3 spec (e.g. '0.3s').
  * **easing**: A CSS3 easing function or shorthand property string. E.g. 'ease-in-out' or 'cubic-bezier(0,0,1,1)'
* **sidebar**:
  * **align**: default is `undefined` which means that is aligned to *right*. If you want to align it to left, write `left`.
  * **width**: the max width of the sidebar, this option is default to 300, please change it as you please.
  * **gap**: the gap is the space between the left margin of the sidebar and the left side of the window (and viceversa). It is useful so that the user can click that space to close the sidebar.
  * **closingLinks**: links or elements that close the sidebar. I suggest to choose a class and give it to all links and other elements such as icons, banner, images, etc, that are links or that are supposed to be clicked. By default it is `a` so every link in the sidebar will close the sidebar. You can use multiple selectors too but, avoid using nested selector otherwise the function will be triggered twice. For example you can select `'a, .close-sidebar'` but if an element is `<a class=".close-sidebar">` the animation will be triggered twice.
  * **css**: here you can store all css, anyway I suggest not to add more css attributes to the one below.
    * **zIndex**: by default is is 3000 but you have to change it to the higher z-index number in your css plus 1.
* **mask**:
  * **display**: `true` or `false`. `false` will remove this option.
  * **opacity**: by default is 0.5.
  * **css**: here you can store all css attributes to give the mask div. However I suggest to do it in your css file except for these below. You can call this div by its data attribute for example: `[data-simplersidebar="mask"]`.
    * **backgroundColor**: the color of the mask. By default is `'black'`.
    * **filter**: IE opacity 0.5 = 50 and so on: `'Alpha(opacity=50)'`.

## Release History
* **v1.5.1** (2015-10-21) - Fork to simpler-sidebar-evergreen, use CSS3
transitions for the sidebar and mask elements.
* **v1.4.0** (2015-08-19) - Fix resize issue [#7](https://github.com/dcdeiv/simpler-sidebar/issues/7).
* **v1.3.4** (2015-07-08) - Enhancement in the README.md, package.json, and bower.json files.
* **v1.3.3** (2015-07-02) -
  * Add Grunt. Simpler-Sidebar files are moved to `dist/` and renamed to *jquery.simpler-sidebar.js* and *jquery.simpler-sidebar.min.js*.
  * Fix *sidebar.closingLinks* and *sidebar.align*.
* **v1.2.3** (2015-06-23) - Fix animations functions.
* **v1.2.2** (2015-06-16) - Add jQuery as dependency of NPM and Bower ([#3](https://github.com/dcdeiv/simpler-sidebar/pull/3))
* **v1.2.0** (2015-05-18) - Add support to AJAX and *mask.display*, change *dataName* to *attr*;
* **v1.1.1** (2015-05-15) - Add support to *left sidebar*.
* **v1.0.0** (2015-05-14) -
  * Stop supporting *subwrapper*.
  * Animate only the sidebar and not the entire page.
  * Support only for the right sidebar.
