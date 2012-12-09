# API Collections

The API is made of collections defined by Models with a REST-like interface. What does that mean? Simply, your templates interact with the database natively using 4 methods commonly known by REST conventions:  <a href="/alpha/docs/api/helpers#get">get()</a>, <a href="/alpha/docs/api/helpers#put">put()</a>, <a href="/alpha/docs/api/helpers#post">post()</a> and <a href="/alpha/docs/api/helpers#delete">delete()</a>.

API collections described here are available with the default Forward installation. You may extend these collections or develop new ones by creating <a href="/alpha/docs/api/classes#model">application models</a>.

*Note: The following examples are written in template format. You can test them yourself by creating a file such as <code>app/templates/alpha/test.html</code> and running it at <code>http://your-domain.com/test.html</code>*


<a id="accounts"></a>
### /accounts

Accounts are used to keep track of customers and their relationships.

* **id** &mdash; number, auto
* **name** &mdash; string
* **email** &mdash; string
* **password** &mdash; string
* **roles** &mdash; array
* **ip_address** &mdash; string
* **date_created** &mdash; date/time, auto
* **date_updated** &mdash; date/time, auto
* **first_name** &mdash; string, auto
* **last_name** &mdash; string, auto
* **orders** &mdash; collection, auto
* **billing** &mdash; array, auto
* **shipping** &mdash; array, auto
* **order_count** &mdash; number, auto
* **last_order** &mdash; record, auto
* **credits** &mdash; collection, auto
* **balance** &mdash; number, auto

Example: Create a new account

	{$new = post("/accounts", [
		name => "Jane Doe",
		email => "jane@example.com",
		password => "will get encrypted",
		any_other_field => "value"
	])}
	
	{$new|dump} # Dump the result of post(), useful for testing and debugging
	
Example: Get a single account by e-mail

	{$account = get("/accounts/jane@example.com")}

Example: Update an existing account (reset password)

	{$updated = put("/accounts/jane@example.com", [
		password_reset => "new password"
	])}
	
Example: Search accounts for a name that contains "jane"

	{$found = get("/accounts", [
		search => [name => "jane"]
	])}
	
Example: Delete an existing account

	{$deleted = delete("/accounts/jane@example.com")} # Goes to /trash...


<a id="carts"></a>
### /carts

Carts are used as a staging area and total calculator for orders. Typical usage involves saving a <code>cart_id</code> in the session and fetching it at the beginning of every page request.

* **id** &mdash; number, auto
* **items** &mdash; array
* **order** &mdash; array
* **coupon_code** &mdash; string
* **discounts** &mdash; array
* **order_id** &mdash; number
* **account_id** &mdash; number
* **session_id** &mdash; string
* **date_created** &mdash; date/time, auto
* **date_updated** &mdash; date/time, auto
* **account** &mdash; record, auto
* **shipping_methods** &mdash; array, auto
* **taxes** &mdash; array, auto
* **sub_total** &mdash; number, auto
* **sub_discount** &mdash; number, auto
* **shipping_total** &mdash; number, auto
* **shipping_discount** &mdash; number, auto
* **tax_total** &mdash; number, auto
* **discount_total** &mdash; number, auto
* **grand_total** &mdash; number, auto
* **credit_total** &mdash; number, auto
* **billing_total** &mdash; number, auto
* **product_cost** &mdash; number, auto
* **quantity** &mdash; number, auto
* **weight** &mdash; number, auto
* **abandoned** &mdash; boolean, auto

Example: Get or post a cart (with session)

	{if $session.cart_id}
		{$request.cart = get("/carts/{$session.cart_id}")}
	{else}
		{$request.cart = post("/carts")}
		{$session.cart_id = $request.cart.id}
	{/if}
	
Example: Put an item (product) in an existing cart

	{$request.cart = get("/carts/{$session.cart_id}")}
	...
	
	{if $params.item}
	
		{$request.cart = put($request.cart, [
			item => $params.item
		])}
	{/if}

Example: Put order details in a cart from a checkout form

	{if $params.order}
	
		{$request.cart = put($request.cart, [
			order => $params.order
		])}
	{/if}
	
Example: Put a coupon code in a cart

	{if $params.coupon_code}
	
		{$request.cart = put($request.cart, [
			coupon_code => $params.coupon_code
		])}
	{/if}
	
Example: Convert cart to an order (typically happens at the end of checkout)

	{$order = post("/orders", [cart => $request.cart])}
	

<a id="categories"></a>
### /categories

Categories are used to organize products.

* **id** &mdash; number, auto
* **slug** &mdash; string
* **name** &mdash; string
* **parent_id** &mdash; number
* **description** &mdash; string
* **slug** &mdash; string
* **date_created** &mdash; date/time, auto
* **date_updated** &mdash; date/time, auto
* **parent** &mdash; record, auto
* **children** &mdash; collection, auto
* **child_count** &mdash; number, auto
* **products** &mdash; collection, auto
* **product_count** &mdash; number, auto

Example: Get active products from a particular category (by slug)

	{$mens_shoes = get("/products", [
		category => "mens-shoes",
		is_active => true
	])}
	
Example: Put a product in a category

	{$mens_shoes = get("/categories/mens-shoes")}
	...
	
	{$updated = put("/products/fancy-shoes", [
		categories => [$mens_shoes.id, ...]
	])}


<a id="channels"></a>
### /channels

Channels are containers for content entries. A typical example is a blog (channel) that has articles (entries). A channel may define a set of fields that can be used by an interface to simplify creating/editing entries.

* **id** &mdash; number, auto
* **slug** &mdash; string
* **name** &mdash; string
* **fields** &mdash; array
* **date_created** &mdash; date/time, auto
* **date_updated** &mdash; date/time, auto
* **entries** &mdash; collection, auto
* **entry_count** &mdash; number, auto

Example: Get blog entries

	{foreach get("/channels/blog/entries") as $article}
		...
	{/foreach}


<a id="discounts"></a>
### /discounts

Discounts are used to conditionally modify the totals of carts and orders.

* **id** &mdash; number, auto
* **name** &mdash; string
* **rules** &mdash; array
* **conditions** &mdash; array
* **description** &mdash; string
* **codes** &mdash; array
* **codes_used** &mdash; array
* **code_history** &mdash; array
* **is_valid** &mdash; boolean
* **date_created** &mdash; date/time, auto
* **date_updated** &mdash; date/time, auto
* **orders** &mdash; collection, auto

Example: Apply a discount code to a cart

	{$result = put($request.cart, [coupon_code => "FREESHIP"])}
	
	{if $result.errors}
		{flash error="Invalid coupon code"}
	{else}
		{flash notice="Coupon code applied"}
	{/if}
		

<a id="emails"></a>
### /emails

This collection is typically used to render an email template and send it. It also keeps a log of emails sent.

* **id** &mdash; number, auto
* **type** &mdash; string
* **to** &mdash; string
* **cc** &mdash; string
* **bcc** &mdash; string
* **subject** &mdash; string
* **text** &mdash; string
* **html** &mdash; string
* **date_created** &mdash; date/time, auto

Example: Send an order email (template found in <code>app/templates/emails/order.txt</code> and <code>order.html</code>)
		
		{$result = post("/emails/order", [
			order => get("/orders/12345")
		])}
		
		... or ...
		
		{$order = put("/orders/12345", [":email" => true])}
		
Example: E-mail template <code>app/templates/emails/test.txt</code> with default values

		{$request.to = $account.email}
		{$request.from = "Example <you@example.com>"}
		{$request.subject = "Welcome {$account.first_name}!"}
		
		Thanks for signing up!
		...
		

<a id="entries"></a>
### /entries

Entries are content containers, typically (but not always) related to a channel.

* **id** &mdash; number, auto
* **slug** &mdash; string
* **name** &mdash; string
* **channel_id** &mdash; number
* **date_created** &mdash; date/time, auto
* **date_updated** &mdash; date/time, auto
* **channel** &mdash; record, auto



<a id="orders"></a>
### /orders

* **id** &mdash; number, auto
* **account_id** &mdash; number
* **name** &mdash; string
* **email** &mdash; string
* **phone** &mdash; string
* **shipping** &mdash; array
* **billing** &mdash; array
* **schedule** &mdash; array
* **sub_total** &mdash; number
* **sub_discount** &mdash; number
* **tax_total** &mdash; number
* **shipping_total** &mdash; number
* **shipping_discount** &mdash; number
* **discount_total** &mdash; number
* **grand_total** &mdash; number
* **credit_total** &mdash; number
* **billing_total** &mdash; number
* **product_cost** &mdash; number
* **shipping_cost** &mdash; number
* **coupon_code** &mdash; string
* **error** &mdash; string
* **parent_id** &mdash; number
* **prev_id** &mdash; number
* **next_id** &mdash; number
* **date_created** &mdash; date/time, auto
* **date_updated** &mdash; date/time, auto
* **date_scheduled** &mdash; date/time, auto
* **date_shipped** &mdash; date/time, auto
* **date_cancelled** &mdash; date/time, auto
* **date_returned** &mdash; date/time, auto
* **status** &mdash; string, auto
* **account** &mdash; collection, auto
* **discounts** &mdash; collection, auto
* **discount_ids** &mdash; array, auto
* **payments** &mdash; collection, auto
* **payment_total** &mdash; number, auto
* **payment_balance** &mdash; number, auto
* **items** &mdash; collection, auto
* **item_ids** &mdash; array, auto
* **item_out_of_stock** &mdash; collection, auto
* **shipments** &mdash; collection, auto
* **cart** &mdash; record, auto



<a id="payments"></a>
### /payments

* **id** &mdash; number, auto
* **ref** &mdash; string
* **account_id** &mdash; number
* **order_id** &mdash; number
* **method** &mdash; string
* **action** &mdash; string
* **amount** &mdash; number
* **status** &mdash; string
* **reason** &mdash; string
* **date_created** &mdash; date/time, auto
* **date_updated** &mdash; date/time, auto
* **account** &mdash; record, auto
* **order** &mdash; record, auto



<a id="products"></a>
### /products

* **id** &mdash; number, auto
* **slug** &mdash; string
* **name** &mdash; string
* **price** &mdash; string
* **cost** &mdash; string
* **sku** &mdash; string
* **weight** &mdash; number
* **description** &mdash; string
* **category_ids** &mdash; array
* **variants** &mdash; array
* **items** &mdash; array
* **stock** &mdash; array
* **images** &mdash; array
* **is_active** &mdash; boolean
* **date_created** &mdash; date/time, auto
* **date_updated** &mdash; date/time, auto
* **categories** &mdash; array, auto



<a id="settings"></a>
### /settings

Settings are currently configured in <code>config/app.yml</code>. In the future, there will be a settings management page in the Admin UI.


<a id="shipments"></a>
### /shipments

* **id** &mdash; number, auto
* **order_id** &mdash; number
* **name** &mdash; string
* **phone** &mdash; string
* **address** &mdash; string
* **city** &mdash; string
* **state** &mdash; string
* **zip** &mdash; string
* **country** &mdash; string
* **method** &mdash; string
* **tracking** &mdash; string
* **date_created** &mdash; date/time, auto
* **date_updated** &mdash; date/time, auto
* **items** &mdash; collection, auto
* **order** &mdash; record, auto
* **carrier** &mdash; string, auto

### /trash

Used as an archive of "deleted" records.