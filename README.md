# Purpose

This code may be used in many situations, but I first used it to redirect customers back to the product page they were previewing before they signed in. This particular Shopify site required users to log in before they add anything to their carts. See the 'Log in' link in the following image:

![alt text](https://github.com/lukehatfield/shopifyRedirect/blob/master/images/login.jpg "Login link")

# Shopify Redirect Implementation


	/* Get parameters from url for redirect back to where user came from */
  
	jQuery( document ).ready(function( $ ) {
		document.getElementById("urlRedirect").value=getQueryVariable("url");
	});

