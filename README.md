responsive-swipe
================

Swipable edition-based page loader, with responsive content loading. Developed for and deployed on [Metro](http://metro.co.uk/).

####HTML
```html

<!-- 
HTML header etc and static (non-swipeable) area. 
-->
<div id="myResponsiveSwipe">
	<div id="swipeview-slider">
		<div id="swipeview-masterpage-0"><!-- Initial "previous page" will be Ajax'd and injected here --></div>
		<div id="swipeview-masterpage-1">
		<!-- 
		The initial content area. 
		e.g. <div class="content">Hello World etc.</div>
	
		IMPORTANT: pages served by urls ending with the query param ?frag_width=...
		MUST consist of this content area ONLY, i.e. omitting all HTML  
		outside of <div class="content">...</div> 
	
		This allows the content areas from adjacent urls in the edition to be preloaded 
		into hidden side panes, in anticipation of being swiped in. 
		-->
		</div>
		<div id="swipeview-masterpage-2"><!-- Initial "next page" will be Ajax'd and injected here --></div>
	</div>
</div>
<!-- 
Static (non-swipeable) area, HTML footer etc. 
-->
```
Use exact id names for ids beginning swipeview-*. The outer element can have an arbitrary ID/class.

Throughout your content area elements, you should prefer class="foo" over id="foo". Three instances of content 
will always exist in the DOM, meaning that id values will likely become non-unique in the documen, causing problems
if you then use document.getElementById etc.

###Page Fragment Serving
All pages must recognize the query param ?frag_width=... and return ONLY their content area when that param is present. 
In the example above this would mean omitting ALL elements outside of the `<div class="content">...</div>`.  

Additionally, `<script>` tags (in particular those that include this lib and its dependencies) must be placed outside
of the content area, and thus NOT be included in  served by urls ending with the `frag_width` query param.

This allows the content areas from adjacent urls in the edition to be preloaded into hidden
side panes - in anticipation of being swiped in - but without each itself loading in the whole js mechanism again.

###Javascript
Basic setup with a static edition:
```javascript
var mySwipe = $('#myResponsiveSwipe').responsiveSwipe({
	edition: ['/', '/foo', '/bar', '/etc']
});
```

Basic setup with a dynamically switching edition:
```javascript
var afterShow = function (context, pageData, api) {
	// On initial page, or following a click, set the edition to what's passed in via the pageData mechanism
	// Otherwise - eg. following a swipe - ignore the passed-in edition.
	if( pageData.clickType === 'initial' || pageData.clickType === 'link') {
		api.setEdition(pageData.edition);
	}
}

var mySwipe = $('#myResponsiveSwipe').responsiveSwipe({
	afterShow: afterShow
});
```

###Configuration options

Values show are defaults:
```javascript
// Callback after a pane is loaded (including hidden panes); use for fancy js-managed rendering.
afterLoad: function(context){},

// Callback before any pane is made visible.
beforeShow: function(){},

// Callback after a pane is made visible; use for analytics events, social buttons, etc.
afterShow: function (context, pageData, api),

// Validator regular expression for Ajax responses.
ajaxRegex: '.*',

// Possible values for screen width. For cacheing, the fewer the better.
breakpoints: [481, 768, 1024],

// A list of page paths - e.g. ["\/","\/foo\/", "\/bar\/"]. Left/right swipe actions will step through these.
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
swipeViewLib: '/js/responsive-swipe_swipeview.js',

// The server's guess of the device width. Set this to zero if you want
// the initial page content to be Ajax reloaded, when the device width exceeds the
// lowest value in the breakpoints option array. 
widthGuess: 1024
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

###Example / Demo

Responsive Swipe was developed for - and is deployed on - http://metro.co.uk/

For a basic usage demo, see https://github.com/andfinally/responsive-swipe-demo

###Docs

More soon.

