{{ noparse }}

The category pages are the hub of your products and are where users are able to browse through and find items relevant to their needs. They are important sections to any store and we have attempted to add as much information as may be required, before getting started you should acquaint yourselves with the [concepts](/documentation/themeing/concepts/) we use throughout these sections.

### Basics {#basics}

In your theme you will need to create the following document:

	views/modules/firesale/category.php

This will overload the default view we have created and allow you to take full-advantage of all the objects we have provided to the page.

### Objects {#objects}

Objects are groups of data that have been passed to the page to be used directly with lex, requiring almost no programming and certainly no PHP knowledge. They can be called almost anywhere on the page quickly and simply, containing all of the information you will need to fill out your pages with all the content you can!

#### Overall {#overall}

The overall category object contains all of the data provided by the user for the given category and is accessible entirely through lex tags under the category.* prefix. A full list of these tags that can be used are below:

Tag | Type | Description
--- | ---- | -----------
{{ category.id }} | Single | The ID of the current category
{{ category.title }} | Single | The title of the current category
{{ category.slug }} | Single | The URL slug of the current category
{{ category.description }} | Single | The description of the current category
{{ category.images }} | Object | Images for category, more details [here](#images)
{{ pagination.links }} | Single | Pagination list for the current category


#### Products {#products}

In this document to display the products we have provided a complete product object, rather than just the basics, to allow for the complete customisation of your pages we provide all of the information about them, again all accessible via lex tags. To begin simply start your loop as follows:

	{{ products }}
		Product object accessible here
	{{ /products }}

Within these tags these are some of the more common items you will have access to, the complete listing can be found on the product page documentation [here](/documentation/themeing/products). Unlike the root category data, the product object does not have to be prefixed.

Tag | Type | Description
--- | ---- | -----------
{{ id }} | Single | The product ID
{{ code }} | Single | The product code
{{ title }} | Single | The product title
{{ slug }} | Single | The product slug, used for URLs
{{ rrp }} & {{ rrp_tax }} | Single | The products retail price, with and without tax respectively
{{ price }} & {{ price_tax }} | Single | The products current price, with and without tax
{{ description }} | Single | The complete description for a product
{{ snippet }} | Single | The description limited to 25 words and without any HTML
{{ image }} | Single | The primary image ID for the product

### Images {#images}

Added in version 1.1 you can add images to your categories, these are then passed onto the page and all category objects that you pull out either via plugin or directly. As with products we have integrated this system with Files and you can alter them within the files administration section as well as reorder and delete images directly from the category in question. The table below is a quick reference of a number of the items found in the object but for a complete listing see the [Files](http://docs.pyrocms.com/2.1/manual/plugins/files) documentation.

Tag | Type | Description
--- | ---- | -----------
{{ name }} | Single | The images title, set within files, defaults to the image name
{{ description }} | Single | The description, as set within files
{{ id }} | Single | The image ID, used as a reference in the URL
{{ width }} | Single | The width of the original image
{{ height }} | Single | The height of the original image

To display images on your category pages you can use the following tags:

	{{ category.images }}
		<img src="{{ url:site }}files/{{ id }}/{{ width }}/{{ height }}" alt="{{ name }}" />
	{{ /category.images }}

> Alternatively to use just the primary image for a product you can use {{ category.images.0.KEY }}

### Example {#example}

Below is an example (taken from our default views) on how to use the more basic elements on the product object and how to create a loop to display them. By default we pass 15 products to the category page, but this can be changed from within the settings depending on your requirements.

	{{ if products }}
		{{ products }}
			<article>
			{{ if ! image  }}
				<div class="no_image_180"></div>
			{{ else }}
				<a href="{{ firesale:url route='product' id=id }}"><img src="{{ url:site }}files/thumb/{{ image }}/180/180" alt="{{ title }}" /></a>
			{{ endif }}
				<section class="price-round medium"><span class="rrp">{{ if rrp > price }}{{ rrp }}{{ endif }}</span><span class="price">{{ price_formatted }}</span></section>
				&lt;header&gt;
					<h3><a href="{{ firesale:url route='product' id=id }}">{{ title }}</a></h3>
					<em>{{ code }}</em>
				&lt;/header&gt;
				<p class="description">{{ snippet }}</p>
				<footer>
					<a href="{{ firesale:url route='cart' }}insert/{{ id }}/1" class="basket"><span class="icon"></span>Add to Basket</a>
				</footer>
				<br class="clear" />
			</article>
		{{ /products }}
	{{ else }}
		<center><h3>No Products Found!</h3></center>
	{{ endif }}

In the example you will note that the entire loop is wrapped in a conditional tag, this is the easiest method to display a message for an empty category and gives you a chance to show a nice message to the user rather than just leaving them looking at a blank page.

For the URLs you will see the use of the {{ firesale:url }} plugin, this is a new system introduced in 1.1 to allow you to dynamically change all of your URLs from within the administration panel. By using these tags, rather than hard-coding them, any changes you make in the back-end will be instantly updated and effective on the front, meaning no extra work on your, or the designers part.

### Extras {#extras}

Along with all of the normal information you may require for the category page, we've added a few extras that can be deployed in your theme to give an extra level of customisation. These can all be accessed with plugin tags or via hard-coded URLs, your choice.

#### Sorting {#sorting}

You can allow your users to sort the category items by a number of methods (and expanding on these is really easy if you want more) and the currently included items are listed below. When a user changes their preference it will be stored in a cookie so we remember their choice throughout the browsing experience and can preselect it for them on a range of pages.

* Title, A-Z and Z-A
* Price, Low to High and High to Low
* Model (Product Code), A-Z and Z-A

> If you would like to see more in the core just send us a suggestion on GitHub and we'll do our best to get them in for you.

To use these in your layout you can use the following plugin tags:

	{{ ordering }}
		{{ key }}
		{{ title }}
	{{ /ordering }}

The key and value items are all you need to display and link to the ordering options, below is an example from our default views on how to achieve this and includes the tags to display the currently selected value.

	<div id="listing-sort" class="switcher">
		<span>{{ order.title }}</span>
		<ul>
		{{ ordering }}
			<li><a href="{{ firesale:url route='category-custom' }}order/{{ key }}">{{ title }}</a></li>
		{{ /ordering }}
		</ul>
	</div>

With a little css this turns into a great dropdown to do your selection and will redirect them back to the page with the cookie set and the order being respected.

#### Styling {#styling}

More and more eCommerce stores are letting users decide how to display listings with a traditional grid format switching to list view. To allow you to do this we've added a small function to the core to track whatever style you want to use. Doing this is very easy and you can use something like the code below to perform it, whatever you pass will be cleaned and stored in the cookie so we can remember it and then passed back to the script for you to apply to your elements or parent and style it how you like.

	<a href="{{ firesale:url route='category-custom' }}style/grid" class="grid{{ if layout == 'grid' }} selected{{ endif }}"><span class="icon"></span>&lt;?php echo lang('firesale:categories:grid'); ?&gt;</a>
	<a href="{{ firesale:url route='category-custom' }}style/list" class="list{{ if layout == 'list' }} selected{{ endif }}"><span class="icon"></span>&lt;?php echo lang('firesale:categories:list'); ?&gt;</a>

And then you can add the class to your parent container like so:

	<section class="{{ layout }}">
	{{ products }}
		...
	{{ /products }}
	</section>

> The user will be redirected back to the current page with the new style in place, however because all it will effectively change is the class, you can set the cookie with JavaScript and then change the class without the need for a page reload!

{{ /noparse }}