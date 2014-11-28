jQuery viewportScale
=============

This simple jQuery plugin (compatible with jQuery 1.7+ and 2.*.x) lets you apply responsive viewport size units (vw, vh, vmax and vmin) with browsers with incomplete CSS3 support. See <em><a href="http://www.w3.org/TR/css3-values" rel="external">W3C: CSS Values and Units Module Level 3</a></em> for more details on these properties.

In <em>fluid design</em>, the width of HTML elements can always be calculated in percent as long as the document body has a known percentage value, although percentage width and height are always relative to their parent element's width and height. However, until recently without Javascript it was impossible to scale an element based on viewport height alone. This technique has become very common in mobile designs, e.g. where a page section is designed to occupy the whole screen height and width, but more content is available by scrolling up or down. Viewport height (vh) units are the answer.

When applied to <em>font-sizes</em>, viewport width (vw) is a lightweight alternative to <strong>fit-text</strong> scripts to ensure text always occupies approximately the same proportion of the screen width and thus take up the same number of lines.

The plugin does not detect browser support for viewport units, as some common browsers such as IOS Safari before version 7 and Internet Explorer before version 10 provide partial support. See <a href="http://caniuse.com/#feat=viewport-units">Can I Use Viewport units</a> for more detailed analysis.

For a practical example please view this simple <a href="http://multifaceted.info/demos/viewport-scale/demo/">demo</a> .

As of November 2014, the latest official version of all major browsers support vw, vh, vmax and vmin. However, if you need to support IE8, IE9 (partial support), Safari < 7 (no support), iOS Safari < 7.1, you may target this plugin only at users of these browsers and let modern browsers use CSS only. For IE, you may use conditional tags to include additional scripts not required for other browsers. For iOS Safari, you may need to use jQuery's deprecated $.browser object (available for jQuery 1.9+ via a plugin) or other third-party browser detection scripts.

<p>A  <a href="http://multifaceted.info/demos/viewport-scale/demo/index.html">simple demo</a> shows you the desired effects</p>

<h2>Correct Viewport Width<h3>

Before the introduction of CSS viewport units, browsers did not consistently distinguish between window width with or without scrollbars. Most mobile browsers and some desktop browsers hide scrollbars when not scrolling and thus report Javascript's window.outerWidth to be exactly 100vw. However, most desktop browsers subtract the scrollbar width from this value. As a result, 100% of window width is always not the same as 100vw. If you wish to ensure exact compatibility with W3C definition of viewport width, please include Tyson Matanich's 
<a href="https://github.com/tysonmatanich/viewportSize">viewportSize</a>. This is not necessary if you require only viewport heights (vh) or do not mind discrepencies in viewport width when scrollbars are always present.

<h2>How it works<h3>

On initial page load the script gauges the correct window height and width (optionally with the aid of viewportSize to gauge correct viewport width without scrollbars). It then translates these into pixel values for the following properties:

<ol>
	<li>width</li>
	<li>height</li>
	<li>top</li>
	<li>bottom</li>
	<li>left</li>
	<li>right</li>
	<li>min-width</li>
	<li>min-height</li>
	<li>max-width</li>
	<li>max-height</li>
	<li>font-size</li>
</ol>

The plugin does not support other common properties, as these can be derived from the above using relative units, e.g. padding and margin in em's are relative the font-size and layout properties in percent are relative to their parent elements.

The plugin will recalculate these values when the browser window is resized or the screen orientation is flipped.

<h3>Sample usage:</h3>

This plugin is best suited for situations where viewport units are only used sparingly for a few strategic layout elements or text that must fit within those elements (a great alternative to Fit-Text).

If you need to target a large set of HTML objects matching one or more CSS paths, the plugin will generate inline markup for each element and will recalculate these values when browser windows are resized and thus potentially consume more resources, especially in older browsers.

For basic usage when applying viewport height or width to CSS height or width properties, you need only add a comma separated string:

<pre>
	/* Exactly 80% of viewport height irrespective of page length
	* most common use case, translates to 
	* #my-element { height: 80vh; }
	*/
	$('#my-element').viewportScale('80vh');
</pre>

<pre>
	/* Exactly 50% of viewport height and 25% of viewport width 
	* Translates to:
	* #my-element { height: 50vh; width: 25vw; }
	*/
	$('#my-element').viewportScale('50vh,25vw');
</pre>

For other supported combinations, use object notation:

<pre>
	/* Height, width and font-size scaled to match viewport width only 
	* Translates to:
	* #my-element { max-height: 30vw; width: 20vw; font-size: 2vw; }
	*/
	$('#my-element').viewportScale({
		'max-height': '30vw',
		 width: '20vw',
		'font-size': '2vw'
	});
</pre>

<h4>HTML Snippet:</h4>
<pre>
&lt;section class="half-width-height"&gt;
	&lt;p&gt; Scalable text 2% of viewport height or width, whichever is the largest&lt;/p&gt;
&lt;/section&gt;

&lt;section class="third-width-fixed-aspect"&gt;
	&lt;p&gt; Fixed aspect ratio relative to screen width only. &lt;/p&gt;
&lt;/section&gt;

</pre>

<h4>Stylesheet:</h4>
<pre>
section.half-width-height {
  position: relative;
  width: 50vw; /* viewport width, not supported in IE8*/
  height: 50vh; /* viewport height, not supported in IE8*/
	border: dashed 3px #999999;
	background-color: #ffff66;
	margin: 5%;
}

section.half-width-height p {
  font-size: 2vmax;
}

section.third-width-fixed-aspect {
  position: relative;
  width: 33.333vw; /* viewport width, not supported in IE8*/
  height: 33.333vh; /* viewport height, not supported in IE8*/
	border: dashed 3px #999999;
	background-color: #9999ff;
	margin: 5%;
}

</pre>

<h4>Javascript in jQuery context:</h4>
<pre>
	
/* Optionally include and instantiate viewportSize to add window.viewportSize.getWidth() method */

viewportSize.getWidth();
	
/* simple set up for only height and width using vh and vw units respectively */
$('section.half-width-height').viewportScale('50vh,50vw');

/* For other units and combinations use object notation */
$('section.half-width-height p').viewportScale({
	'font-size': '2vmax'
});

/* For other units and combinations use object notation */
$('section.third-width-fixed-aspect').viewportScale({
	'width': '33.333vw',
	'height': '33.333vw',
});

</pre>

<h4>Resize Events</h4>

The plugin responds to window resizing and changes in screen orientation. If you using this plugin within a resize event handler, you may want to disable this feature. just add false as the second parameter.

<pre>
	
			$('#my-container').viewPortScale({'font-size': '3vw'}, false);
	
</pre>

