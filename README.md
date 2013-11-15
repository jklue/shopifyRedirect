# Shopify Redirect after Login
## The product page

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

![Login url](https://github.com/lukehatfield/shopifyRedirect/raw/master/images/login-page.jpg "Login url")

This image shows we came from the begonia product page, and it will use this extra info to send us back there when we finish logging in.

## The login page

The key to this redirect action is identified in the Shopify post which answers the question, ["Can my customers be redirected to a new page when they login to their account?"](http://docs.shopify.com/support/your-store/customers/can-my-customers-be-redirected-to-a-new-page-when-they-login-to-their-account). In step 4 at the bottom of the post, it shows how to redirect a customer after a successful login. (If this redirect feature was available for redirecting after a customer registers, I would have used it there too, but for now it seems like it's just available for the login page.)

Basically the article says to look for the following line of code:

	{% form 'customer_login' %}

**Immediately after** this line, we can add a hidden input type with a name of "checkout_url" and a value with the url of the page to which we redirect. This code will not work if the form tag does not immediately preceed it. See generic code from the Shopify page below:

	<input type="hidden" name="checkout_url" value="/pages/special-page" />

The instructions you are reading just takes this one step further, and dynamically changes the value attribute of that hidden input element.

Because we are dynamically changing the value attribute of this input element, the value could be left blank, but I found it useful to place "/collections/all" as a placeholder in case someone finds this page without coming from a product page. If the value attribute was blank, there would be an error after a successful login. See updated code below:

	<input id="urlRedirect" type="hidden" name="checkout_url" value="/collections/all" />

## The javascript

I used the jQuery .ready function to wait until the page loads, then used plain old javascript to select the input element, and target the value attribute. Insert the following line of code in a <script> tag at the bottom of the login.liquid page.

	/* Once the page loads, select the elmement with the id of 'urlRedirect' and change the value to the value of the 'url' key in the address bar */
  
	jQuery( document ).ready(function( $ ) {
		document.getElementById("urlRedirect").value=getQueryVariable("url");
	});
	
You can see that the block of code above calls the function, "getQueryVariable" and passes the string, "url", which identifies the key for the key-value pair we are looking for in the url. The function getQueryVariable was written by Chris Coyier and is from this [post](http://css-tricks.com/snippets/javascript/get-url-variables/) on his CSS-tricks.com website.
	
	/* Get parameters from url for redirect back to where user came from */
	/* From http://css-tricks.com/snippets/javascript/get-url-variables/ */
  function getQueryVariable(variable)
  {
         var query = window.location.search.substring(1);
         var vars = query.split("&");
         for (var i=0;i<vars.length;i++) {
                 var pair = vars[i].split("=");
                 if(pair[0] == variable){return pair[1];}
         }
         return("/collections/all");
  }

