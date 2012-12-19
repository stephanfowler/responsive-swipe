responsive-swipe
================

Swipable edition-based page loader, with responsive content loading.

###Example Site

http://metro.co.uk/

###Javascript

Basic setup with a static edition:
```javascript
var mySwipe = $('#pageBody').responsiveSwipe({
	ajaxRegex: '^<div class="pageBodyInner',
	edition: ['/', '/foo', '/bar', '/etc']
});
```

Basic setup with a dynamically switching edition:
```javascript
var afterShow = function (context, pageData, api) {
	// On initial page, or following a click, set the edition to what's passed in via the pageData mechanism
	// Otherwise, ignore the passed in edition, e.g. following a swipe.
	// This allows articles to appear in a sipweable mixed-category editions, 
	// even though they're by default in a category-specific edition. 
	if( pageData.clickType === 'initial' || pageData.clickType === 'link') {
		api.setEdition(pageData.edition);
	}
}

var mySwipe = $('#pageBody').responsiveSwipe({
	ajaxRegex: '^<div class="pageBodyInner',
	afterShow: afterShow
});
```

####HTML
```html
<div id="pageBody">
	<div id="swipeview-slider">
		<div id="swipeview-masterpage-0">
			<!-- Leave empty. First lefthand content will load here -->
		</div>
		<div id="swipeview-masterpage-1">
			<div class="pageBodyInner">
				<!-- Put the initial content in this div -->
			</div>
		</div>
		<div id="swipeview-masterpage-2">
			<!--Leave empty. First righthand content will load here -->
		</di
	</div>
</div>
```

###Configuration options

Values show are defaults:
```javascript
// Callback after a pane is loaded (including hidden panes); use for fancy js-managed rendering.
afterLoad: function(){},

// Callback before any pane is made visible.
beforeShow: function(){},

// Callback after a pane is made visible; use for analytics events, social buttons, etc.
afterShow: function(){},

// Validator regular expression for Ajax responses.
ajaxRegex: '.*',

// Possible values for screen width. For cacheing, the fewer the better.
breakpoints: [481, 768, 1024],

// A list of paths - e.g. ["\/","\/foo\/", "\/bar\/"] - which left/right actions will step through.
// Set the edition using this option, or in afterShow callback function using api.setEdition. The latter method also allows you to change the edition mid-flow .
edition: [],

// Allow ajax+pushState behaviour (requires HTML5 History API support)
enablePjax: true,

// Allow swipe behaviour (requires CSS Transitions support)
enableSwipe: true,

// Reload content on window resize; switches the width metric to window- rather than screen-width; for testing only.
emulator: false,

// Milliseconds until edition should expire, i.e. cache should flush and/or content should reload instead of Ajax'ing. 0 => no expiry.
expiryPeriod: 0,

// CSS selector for anchors that should initiate an ajax+pushState reload.
linkSelector: 'a:not(.no-ajax)',

// The CSS selector for an element containing a data-json attribute with arbitrary data about the page.
pageDataSelector: '.responsive-swipe-meta',

// The name of the query param sent wth Ajax page-fragment requests
queryParam: 'frag_width',

// CSS selector for a spinner/busy indicator
loadingIndicator: undefined,

// The custom swipeview.js lib
swipeViewLib: '/js/responsive-swipe_swipeview.js'
```

###Capabilities detected:

* __HTML5 History API__ - allows changing the URL after an ajax load. Without this, we won't go ahead with ajax page loading.

* __CSS Transitions__ - allows sliding of left/right preloaded panes.

Three capability combinations that are supported are:

* History=false                   : no pjax navigation; (IE < 10, Android native browser, etc)
* History=true, Transitions=false : pjax page navigation; no swiping; (An unlikely case; Transitions was widely implemented before History was)
* History=true, Transitions=true  : pjax page navigation; swiping. (Modern browsers)

You can override these using the enablePjax and enableSwipe options. (You can only disable supported features, not enable unsupported ones!)

###Dependencies

Requires a [forked version](https://github.com/stephanfowler/SwipeView) of [SwipeView](https://github.com/cubiq/SwipeView) (also included in this repo) and jQuery.

###Docs

More coming soon.

