responsive-swipe
================

Swipable edition-based page loader, with responsive content loading.

###Example Site

http://metro.co.uk/

###Usage

Javascript
```javascript
var mySwipe = $('#pageBody').responsiveSwipe({
	ajaxRegex: '^<div class="pageBodyInner',
	edition: ['/', '/foo', '/bar', '/etc']
});
```

HTML
```html
<div id="pageBody">
	<div id="swipeview-slider">
		<div id="swipeview-masterpage-0">
			<!-- Empty. First lefthand content will load here -->
		</div>
		<div id="swipeview-masterpage-1">
			<div class="pageBodyInner">
				<!-- Initial content here -->
			</div>
		</div>
		<div id="swipeview-masterpage-2">
			<!-- Empty. First righthand content will load here -->
		</div>
	</div>
</div>
```

###Configuration options

Values show are defaults:
```javascript
// Callback after a pane is loaded (including hidden panes); use for fancy js-managed rendering.
afterLoad: function(){ /* */ },

// Callback after a pane is made visible; use for analytics events, social buttons, etc.
afterShow: function(){ /* */ },

// Validator regular expression for Ajax responses.
ajaxRegex: '.*',

// Possible values for screen width. Wrt. cacheing, the fewer the better.
breakpoints: [481, 768, 1024],

// A list of paths - e.g. ["\/","\/foo\/", "\/bar\/"] - which left/right actions will step through.
edition: [],

// Seconds beyond which any action (click, swipe, etc) should trigger a page reload in order to refresh edition. 21600 secs = 6 hours.
expiry: 21600,

// Allow ajax+pushState behaviour (requires HTML5 History API support)
enablePjax: true,

// Allow swipe behaviour (requires CSS Transitions support)
enableSwipe: true,

// Reload content on window resize; switches the width metric to window- rather than screen-width; for testing only.
emulator: false,

// Path of homepage
homePath: '/',

// CSS selector for anchors that should initiate an ajax+pushState reload.
linkSelector: 'a:not(.no-ajax)',

// The CSS selector for an element containing a data-json attribute with arbitrary data about the page.
pageDataSelector: '.responsive-swipe-meta',

// The name of the query param sent by Ajax page-fragment requests
queryParam: 'frag_width',

// CSS selector for a spinner/busy indicator
loadingIndicator: undefined,

// CSS selector for clickable "next" element, for edition nav
arrowNext: undefined,

// CSS selector for clickable "previous" element, for edition nav
arrowPrev: undefined,

// CSS selector for elements to hide as a swipe begins
hideFirst: undefined,

// CSS selector for elements as empty as a swipe begins
emptyFirst:	undefined,

// The custom swipeview.js lib
swipeViewLib: '/js/responsive-swipe_swipeview.js'
```

###Capabilities detected:

History HTML5 API - allows changing the URL after an ajax load. Without this, we won't go ahead with ajax page loading.

Transitions in CSS - allows sliding of left/right preloaded panes.

Three capability combinations that are supported are:
A. History=false                   : no pjax navigation; (IE < 10, Android native browser, etc)
B. History=true, Transitions=false : pjax page navigation; no swiping; (An unlikely case; Transitions was widely implemented before History was)
C. History=true, Transitions=true  : pjax page navigation; swiping. (Modern browsers)

You can override these using the enablePjax and enableSwipe options. (You can only disable supported features, not enable unsupported ones!)

###Dependencies

Requires a [forked version](https://github.com/stephanfowler/SwipeView) of [SwipeView](https://github.com/cubiq/SwipeView) (also included in this repo) and jQuery.

###Docs

More coming soon.

