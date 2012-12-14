# Helpers

Helpers are simple functions that can be used anywhere in your application. Those described here are available with the default Forward installation.

<a id="get"></a>
### get()

GET request.

	{get $result from "/collection" [field => "value"]}
	...
	{$result = get("/collection", [field => "value"])}


<a id="put"></a>
### put()

PUT request.

	{put [field => "value"] in "/collection" $result}
	...
	{$result = put("/collection", [field => "value"])}


<a id="post"></a>
### post()

POST request.

	{post [field => "value"] in "/collection" $result}
	...
	{$result = post("/collection", [field => "value"])}
	
	
<a id="delete"></a>
### delete()

DELETE request.

	{delete "/collection/id"}
	...
	{$result = delete("/collection/id")}
	

<a id="age"></a>
### age()

Returns the age of a date/time.

	{$order.date_created|age} # 27 minutes ago



<a id="age_date"></a>
### age_date()

Returns the age of a date/time, or the date if it's outside of 'today'.

	{$account.date_created|age_date} # 23 hours ago
	
	{$product.date_created|age_date} # Dec 25 2012



<a id="bind"></a>
### bind()

Bind callback to an event. Typically used in a plugin to modify the result of a model or framework event.

	bind('products', 'get', function ($event)
	{
		if ($event['data']['search'])
		{
			return my_custom_search($event['data']['search']);
		}
	});



<a id="camelize"></a>
### camelize()

Camelize a string.

	{"long-name-for-example"|camelize} # LongNameForExample



<a id="countries"></a>
### countries ()

Returns a complete list of countries available for display and validation purposes.

	{foreach countries() as $country}
		{$country.code}
		{$country.name}
		{$country.states}
	{/foreach}
	
	{$us = "US"|countries}
	{$us.name} # United States
	


<a id="dispatch"></a>
### dispatch()

Dispatch a new request.

	{dispatch "/any/url"}
	


<a id="dump"></a>
### dump()

Dump a variable. Useful for testing and debugging.

	{"/channels/blog/entries"|get|dump}
	


<a id="flash"></a>
### flash()

Set a message that will persist through a redirect.

	{flash error="Uh oh, something went wrong" redirect="/somewhere"}
	
	{flash notice="Account saved!" refresh=true}
	
	{flash warning="There have been {$x} login attempts"}



<a id="hyphenate"></a>
### hyphenate()

Hyphenate a string.

	{"Long Name For Example"|hyphenate} # long-name-for-example



<a id="image"></a>
### image()

Get a relative image path. Also creates and caches new image files according to parameters.

	{get $product from "/products/{$slug}"}
	<img src="{image for=$product width=200 height=150 padded=true}" />
	...
	{get $order from "/orders/{$id}"}
	{foreach $order.items as $item}
		{if $src = image([type => product, id => $item.id, width => 150])}
			<img src="{$src}" />
		{/if}
	{/foreach}



<a id="in"></a>
### in()

Determine if value A is contained in value B.

	{$values = [a, b, c]}
	
	{if a|in:$values} # true
	{if x|in:$values} # false
	
	{$value = "Hello World"}
	
	{if "Hello"|in:$value} # true
	{if "Goodbye"|in:$value} # false



<a id="markdown"></a>
### markdown()

Markdown parser. Based on PHP Markdown by Michel Fortin.

	{$blog.content|markdown}



<a id="merge"></a>
### merge()

Merge two indexed arrays recursively.

	{$set1 = [a => [b => c], x => y]}
	{$set2 = [a => [b => d]]}
	
	{$result = $set1|merge:$set2} # [a => [b => d], x => y]



<a id="money"></a>
### money()

Format number as localized money string.

	{$price = 10}
	
	{$price|money} # $10.00
	
	{(-$price)|money:true} # ($10.00)



<a id="orderby"></a>
### orderby()

Order an array by index. Default ascending. Prefix with "!" for descending.

	{foreach $users|orderby:"name" as $user}
		...
	{/foreach}



<a id="pluralize"></a>
### pluralize()

Pluralize a string. Converts a word to english plural form.
	
	{pluralize "{$items|count} items"} # 1 item
	
	{pluralize "{$items|count} items"} # 10 items
	
	{pluralize "Person"} # People
	
	{pluralize word="Category" if_many=$categories} # Categories



<a id="redirect"></a>
### redirect()

Redirect a request.

	{redirect "/relative/url"}
	
	{redirect "https://example.com/absolute/url"}



<a id="refresh"></a>
### refresh()

Refresh the current request.

	{refresh}



<a id="render"></a>
### render()

Render a view.

	{render "relative/view/path"}
	
	{render "/absolute/view/path.html"}
	


<a id="singularize"></a>
### singularize()

Singularize a string. Converts a word to english singular form.

	{singularize "people"} # person



<a id="spacify"></a>
### spacify()

Spacify a string.

	{"long-name-for-example"|spacify} # Long Name For Example



<a id="total"></a>
### total()

Format a total value as a string with precision. To format localized money string, use <a href="#money">money()</a> instead.

	{$price = 10}
	
	{$price|total} # 10.00



<a id="trigger"></a>
### trigger()

Trigger callback for an event. Returns the first argument as a result after callback(s).

	$value1 = trigger('target', 'eventname', $value1, $value2[, $value3 ...]);



<a id="underscore"></a>
### underscore()

Underscore a string.

	{"LongNameForExample"|underscore} # long_name_for_example



