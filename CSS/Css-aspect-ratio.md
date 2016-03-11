#title:  Css aspect ratio
##date:  2016-03-10 22:50:32
##sub-folder:  CSS

Kind of a nifty trick for maintaining aspect ratios. If you're setting up, let's say, a featured image that needs to keep a certain aspect ratio, one way to do it might be to set up an image as a background-image on an element and then set the height. So it would be something like this

_______________________
|                     |
|       BOXED         |
|       IMAGE         |
|                     |
|_____________________|
----text underneath ----

````css
.boxed-image {
	width: 100%;
	height: 500px;
	// or maybe max-height: 500px;
}
````

The problem with using height here is that it will ALWAYS keep things at this height, even when the container / viewport shrinks. A better approach is to set the height to 0 and use the padding-bottom property to set an asepct ratio. This is the same trick that's used for responsive iframe embeds. But it lets you set an aspect ratio once, which then applies universally and responsively.

Like so:

````css
.boxed-image {
	width: 100%;
	height: 0;
	padding-bottom: 56.625%; // 16 x 9
}
````