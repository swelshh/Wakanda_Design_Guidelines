# General Javascript

### Use Strict

We will "use strict" everywhere we can.

_Exceptions_

* The js files for defining the model (ie inside Project/ModelFolder)



### JSLint
Every js file must pass JSLint...  sort of.  

_Exceptions_

* We do want to use the 'tolerate misordered definitions' option, so you actually need to comment out "use strict" when running jslint.  Its usually best to run it with and without "use strict" to make sure you catch everytyhing.

* Some of the Wakanda templates (eg the js files for components) will not pass jslint, and these are locked areas we cannot change...  so don't worry about it (maybe we'll get jshint up and running)



### Javascript Style
We will generally follow the [Naming Conventions](http://doc.wakanda.org/WakandaStudio0/help/Title/en/page1812.html) described in the Wakanda documentation.  We will also generally use [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwldrn/idiomatic.js/) as a style guide for our javascript.

 _Exceptions_
 
 * we will use the default tab spacing and any other defaults built in to Wakanda rather than changing prefs
 * we generally won't pad inner parenthesis with spaces like this ( "no", "spaces" ), but instead will do it like this ("no", "spaces").  See part 2D.

We will use double quotes for strings instead of single quotes.

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

You should add one or more comment lines just above the functional subunits of your code (which should also be separated with white space). The idea is for another programmer to be able to read the comments of your method like a book. The header area and declarations are the table of contents, and the comments at the top of each block of code are the story. People should be able to read the comments to understand what's going on and not have to look at any code.

It is also important to add plenty of comments about complex data objects (like BLOBS), and any other complex concepts in the function which would be hard to understand if you were to look at the code later.

A comment should always have a blank line above it (except the main header comment at the top of the page).

_Err on the side of too many comments rather than too few._




### Equality
We never use == or !=, only use === or !==, unless you have a really good reason and you specify the reason in comments.


### HTML and CSS Style
Generally use [Code Guide by @mdo](http://mdo.github.io/code-guide/) as a style guide for our html and css.










# Wakanda JS


### Javascript General

Maintainable Javascript book says: "It’s always best to split application logic from any event handler, because the same logic may need to be triggered by different actions in the future."


### Naming Conventions
We don't care about giving the widgets on a page a meaningful ID, but we do want to give them a meaningful name in our code (see examples in the module templates below).

Here are the suffixes we will use:

* Grid = Grid (eg dayGrid)
* Button = Btn
* Text Input = Fld
* Rich Text = Text
* Component = Comp

When working with a datastore class entity, generally name it the same as the class but with the first char lowercase:

* myClass = ds.MyClass.find(1);

When working with a datastore class collection, same rule as an entity but suffix with Coll:

* myClassColl = ds.MyClass.find("id > 5");


### Wakanda General

* The "Initial Query" property of all data sources should always be turned off



### Async
Any Wakanda command that can be run async should be run async unless there is a good reason not to.  If you do not run async, there should be a comment explaining why.


### Example async call for save

```javascript
//function that makes an async call
function save() {
	sources.className.save(async_save);
}
	
//function to handle callback from the server
function async_save(event) {
	if (WAKL.err.async_thereWasntAnError(event)) {
		doStuff();
	}
}
```


### Example async call for query (with userData)

```javascript
//function that makes an async call
function query(date) {
	var param1 = "value";
	sources.className.query("something = :1", async_query, {params:[param1], 
			userData: {info: "if we need anything in the callback"}});
}

//function to handle callback from the server
function async_query(event) {
	if (WAKL.err.async_thereWasntAnError(event)) {
		var infoWeNeed = event.userData.info;
		doStuff(infoWeNeed);
	}
}
```


### CSJS Module Template (non-component)

```javascript
//-------------------------------------------------------------------------
//Description of the module
//-------------------------------------------------------------------------
CC.moduleName = (function () {
"use strict"

	//pull any widgets, datasources into meaningfully named variables and
	//declare any other variables
	var meaningfullName1 = $$("widgetID1"),
		meaningfullName2 = $$("widgetID2"),
		somethingSource = sources.something,
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


### CSJS Module Template (component)

A few notes on datasources in components:

* All datasources should be local unless there is a good reason to be global (and if global it should be documented as to why)
* Always turn off the "Initial Query" property

```Javascript
/** 
 * @fileOverview Code for the [name] component
 * @author Welsh Harris
 * @created 1/09/2014
 */
 
(function Component (id) {
"use strict";

// Add the code that needs to be shared between components here

function constructor (id) {

	// @region beginComponentDeclaration
	var $comp = this;
	this.name = 'campSelDlg';
	// @endregion

	this.load = function (data) {

	// @region namespaceDeclaration
	// @endregion

	//-------------------------------------------------------------------------
	//component API
	//-------------------------------------------------------------------------
	var cs = $comp.sources,
		cw = $comp.widgets,
		exampleBtn = cw.button1;
	
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
	function getEntity() {
		return cs.dataSource.getCurrentElement();
	}
	
	//-------------------------------------------------------------------------
	// eventHandlers
	//-------------------------------------------------------------------------
	
	//add event listeners
	WAF.addListener(exampleBtn, "click", function(event) {
		doSomething();
	});
		
	//-------------------------------------------------------------------------
	//on load
	//-------------------------------------------------------------------------
	
	//load data and nested components
	
	//-------------------------------------------------------------------------
	//public API
	//-------------------------------------------------------------------------
	$comp.availableOutside = availableOutside;
	
	// @region eventManager
	// @endregion

	};


}
return constructor;
})();
```



