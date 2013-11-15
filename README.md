	/* Get parameters from url for redirect back to where user came from */
  
	jQuery( document ).ready(function( $ ) {
		document.getElementById("urlRedirect").value=getQueryVariable("url");
	});
