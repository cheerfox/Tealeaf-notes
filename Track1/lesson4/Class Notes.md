##unobtrusive javascript
Unobtrusice javascript means we grab javascript code out out our html code to another file and include it in html


```html
<script src="/application.js"></script>
```
`/` means go to the root path

```javascript
	//traverse to all input elements in hit_form
	$(#hit_form input).click(function() {
		alert('hit button is click!!');
		return false;  //stop the action from execute
	});
```

- You can use console in the browser inspector to test the javascript code or jquery
- You can use the XHR subtab in network tab to see if there is an ajax request issued
- ajax process in browser and behind the scene

##DOM



##Need to know
- The different between client side code and server siede code
- what DOM means
- unobtrusive javascript and how to use jQery to manipulate DOM
- What ajax is and why to issue Ajax requests
- Using jQery to issue Ajax request
- handing ajax requset on server side
- handling ajax response in client side
- dealing with re-binding issue when the DOM is changed
