# Shopify Redirect after Login

This code may be used in many situations, but I first used it to redirect customers back to the product page they were previewing after they signed in. The Shopify site I was working on required users to log in before they add anything to their carts. See the 'Log in' link in the following image:

![Login link](https://github.com/lukehatfield/shopifyRedirect/raw/master/images/login.jpg "Login link")

This link replaces the 'Add to Cart' button when the visitor is not logged in. The code behind this link looks like this:

	{% if customer %}
  	...
	{% else %}
		<p>Please <a href="/account/login?url={{ theme_role | theme_url }}">Log in</a> or {{ 'Register' | customer_register_link }} before you proceed.</p>
	{% endif %}
              
Notice the url in the href. It includes a query-like syntax, with a question mark preceeding the 'key' name of, in this case, 'url'. Then the equal sign designates the value is to follow. Here, the value is dynamically produced, and it is the url to the current product page the visitor is on.

Once clicked, the visitor is sent to the login page, but with some additional information: namely, the url where she came from. The image below shows the url at the login page with the additional info.

![Login url](https://github.com/lukehatfield/shopifyRedirect/raw/master/images/login-.jpg "Login url")


	/* Get parameters from url for redirect back to where user came from */
  
	jQuery( document ).ready(function( $ ) {
		document.getElementById("urlRedirect").value=getQueryVariable("url");
	});

