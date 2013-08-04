# Plans

```javascript
//function that makes an async call
function save() {
	sources.className.save(async_save);
}
	
//function to handle callback from the server
function async_save(event) {
	if ( CC.page.thereWasntAnError (event) ) {
		doStuff();
	}
}
```