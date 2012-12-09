# Template Guide

A template defines the user interface portion of an app. You may have several templates in one project, and therefore many different interfaces or web sites in a single app. Giving templates the power to direct request logic is one of the key design aspects of Forward.

This approach deviates from the practice of keeping templates logic-free and maintaining request flow in Controllers coupled with procedural Models.

At the core is a lightweight MVC framework, and as such Controllers may be used in combination with Views to form templates, but there are many benefits to combining these components in a single file. This guide will primarily demonstrate the combined template approach.

<a id="syntax"></a>
## Syntax

There are two modes of syntax in template files.

#### Compiled templates

Compiled templates can represent various output formats and use more expressive template syntax, which is based on the <a href="http://www.smarty.net/docs/en/">Smarty 3</a> template engine. Smarty 3 was released in late 2011, and was chosen for its improved syntax and speed compared to Smarty 2.

To use compiled syntax in a template file, give it any extension related to the output of the file, such as `.html` or `.json`. All extensions other than `.php` are compiled by default.

Example: app/templates/your-template/search.html

	{$products = get("/products", [
		search => $params.q
	])}
	
	<h1>Found {pluralize "{$products.count} items"} matching {$params.q|escape}
	
Example: app/templates/your-template/products.json

	{"/categories/featured/products"|get:$params|json_encode}


#### Raw (PHP) templates

PHP templates allow you to skip the compilation process and write plain PHP. This is most useful for scripts with no output, which may be intended for the command line, or special logic that other templates can extend.

To use PHP syntax in a template file, give it the `.php` extension.

Example: app/templates/your-template/search.php

	<?php
	$products = get("/products", array(
		'search' => $params->q
	));
	?>
	
	Found <?= pluralize("{$products['count']} items"); ?>
		matching <?= htmlspecialchars($params->q); ?>
		

<a id="routing"></a>
## Routing

When a web request is received, the framework will match the target template by <a href="install#configuration">domain route</a>, and then fine a matching template file based on the web request URI.

Routes can be explicitly configured in `config/app.yml`, but the preferred routing method is by convention.

Example config/app.yml:

	...
	domain_routes:
	  admin: admin.example.com
	  example: (www.)?example.com
	...

Example request:

	GET http://example.com/products.html
	
	# Router looks for files in this order:
	app/templates/example/products/index.html
	app/templates/example/products.html
	app/templates/example/404.html
	
There is no limit to nested template files.

#### Page Arguments

You may pass arguments as part of a the request URI:

	GET http://example.com/blog/example-blog.html
	
	# Router looks for files in this order:
	app/templates/example/blog/example-blog/index.html
	app/templates/example/blog/example-blog.html
	app/templates/example/blog/index.html
	app/templates/example/index/blog.html
	app/templates/example/blog.html
	app/templates/example/404.html

To capture arguments left on the end of the current request, use the `args` helper:

	{args $arg1 $arg2 $arg3 ...}
	
Or, get arguments from the `$request` variable:

	{$arg1 = $request.args.0}
	{$arg1 = $request.args.1}
	{$arg3 = $request.args.2}
	...
	
Make sure to `urlencode` page arguments before using them in any API requests:

	{args $blog_slug}

	{if $blog = get("/channels/blogs/entries/{$blog_slug|urlencode}")}
	
		{render "blog/entry" for=$blog}
		{return true}
	
	{/if}
	
	...
	
		
<a id="formats"></a>
## Formats

Output format is defined by the request URI, and defaults to `html` if not specified:

	GET http://example.com/products
	
	# Default html extension
	app/templates/example/products.html
	
Use another extension to represent a different format:

	GET http://example.com/products.json
	
	# Route with explicit JSON extension
	app/templates/example/products.json
	
Use the `php` extension to bypass the template engine completely:

	GET http://example.com/products.php
	
	# Raw PHP script
	app/templates/example/products.php


<a id="layouts"></a>
## Layouts

Layouts contain markup that surround template views. The concept is nearly identical to that of similar MVC frameworks.

The default layout file for any `html` template is `app/templates/example/layouts/default.html`.

Framework views are rendered before layouts, and content is passed to the layout in a variable named `$content_for_layout`.

	<!doctype html>
	<html>
		<head>
			...
		</head>
		<body>
			...
			{$content_for_layout}
			...
		</body>
	</html>
	
Another benefit of the order of view rendering is that you can pass arguments from view to layout in the `$request` variable.

Example view:

	{$request.page_title = "Page specific title"}
	
Example layout:

	<!doctype html>
	<html>
		<head>
			<title>{$request.page_title|default:"Default title"}</title>
		</head>
		...
	</html>		

The same convention of using `$request` applies between views also.


<a id="errors"></a>
## Errors

Error handling is built into the API, but there are helpers and standards for dealing with errors in a template.

If an API method returns an error, details will be present in format of `$result.errors`:

	{if $request.post}
	
		{post [email => $params.email] in "/accounts" $result}
	
		{if !$result.errors}
	
			{redirect "/success"}
		
		{/if}
	{/if}
	
	<form method="post" action="">
		<div class="field">
			<label>E-mail address <span class="error">{$result.errors.email}</span></label>
			<input type="email" name="email" value="bad email value" />
		</div>
		<button type="submit">Create account</button>
	</form>


<a id="examples"></a>
## Examples

The best examples currently are in the `admin` and `alpha` templates that come with the code base.

More template examples and tutorials coming soon.

Want to write a tutorial? That's great! E-mail <a href="mailto:contribute@getfwd.com">contribute@getfwd.com</a> with your ideas and we'll help you put the pieces together.