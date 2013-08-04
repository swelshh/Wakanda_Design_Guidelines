# General Javascript

### Use Strict

We will "use strict" everywhere we can.

_Exceptions_

* The js files for defining the model (ie inside Project/ModelFolder)



### JSLint
Every js file must pass JSLint.  We do want to allow 



### Javascript Style
We will generally use [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwldrn/idiomatic.js/) as a style guide for our javascript.

 _Exceptions_
 
 * we will use the default tab spacing and any other defaults built in to Wakanda rather than changing prefs
 * we generally won't pad inner parenthesis with spaces like this ( "no", "spaces" ), but instead will do it like this ("no", "spaces").  See part 2D.



### Comments
Every JS file must have a header comment that looks like this:

```Javascript
/** 
 * @fileOverview description of what's in this js file
 * @author Welsh Harris
 * @created 07/20/2013
 */
```

If we have a specific license for the code (eg MIT), the header will look like this:

```Javascript
/** 
 * @fileOverview description of what's in this js file
 * @author Welsh Harris
 * @created 07/20/2013
 *
 * @name AppName
 * @copyright (c) 2013 CoreBits DataWorks LLC
 * @license Released under the MIT license (including in distribution in <MIT LICENSE.txt>
 */
```

You should add one or more comment lines just above the functional subunits of your code (which should also be separated with white space). The idea is for another programmer to be able to read the comments of your method like a book. The header area and declarations are the table of contents, and the comments at the top of each block of code are the story.

It is also important to add plenty of comments about complex data objects (like BLOBS), and any other complex concepts in the function which would be hard to understand if you were to look at the code later.

_Err on the side of too many comments rather than too few._









# CSJS

### CSJS Module Template

```javascript
//-------------------------------------------------------------------------
//Description of the module
//-------------------------------------------------------------------------
CC.moduleName = (function () {
"use strict"

	//pull any widgets into meaningfully named variables and
	//declare any other variables
	var meaningfullName1 = $$("widgetID1"),
		meaningfullName2 = $$("widgetID2"),
		whateverElse = {};
	
	//init
	function init() {
		
		//add event listeners if there are any
		WAF.addListener(meaningfullName1, "click", function(event) {
			doSomething();
		});
		
		//anything else we need to do to init

	}

	//define private functions
	function doSomething() {
		//code
	}
	
	//define public functions in the same way (but add to public api below)
	function availableOutside() {
		//code
	}
	
	//make accessor functions if the outside world needs access
	//to anything in our module
	function getMeaningfullName1() {
		return getMeaningfullName1.getValue();
	}
	
	//--------------------
	//public API
	//--------------------
	return {
		init: init,
		availableOutside: availableOutside,
		getMeaningfullName1: getMeaningfullName1;
	};
	
}());
```



### Example async call for save

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



### Example async call for query (with userData)

```javascript
//function that makes an async call
function query(date) {
	var param1 = "value";
	sources.className.query("something = :1", async_query, {params:[param1], userData: {info: "if we need anything in the callback"}});
}

//function to handle callback from the server
function async_query(event) {
	if ( CC.page.thereWasntAnError (event) ) {
		var infoWeNeed = event.userData.info;
		doStuff(infoWeNeed);
	}
}
```