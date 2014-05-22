---
layout: index-en
title: Naming Conventions
---
{% include JB/setup %}
# RAMP Naming Conventions

## Terms
Resource dictionary = a singleton object that only contain read-only fields (in C# or Java, these would be classes that cannot be instantiated and only contain public static fields)

Utilities = a singleton object that contain useful functions but does not contain any fields (in C# or Java, these would be classes that cannot be instantiated and only contain public static functions) 

Singleton object = a singleton object that contains both fields and functions (in C# or Java, these would be classes that cannot be instantiated, have static fields, and static functions). These are the only objects can have states that change over time (e.g. Map can have different extents, Datagrid can have different points, whereas the Resource dictionary never changes, neither do the Utility classes).

|Module Path|	Return Type|	Preferred Arg Alias|	Notes
|Folder: Modules	| | |
|ramp/basemapselector| singleton object| BasemapSelector| 
|ramp/bookmarklink| singleton object| BookmarkLink| 
|ramp/datagrid| singleton object| Datagrid| 
|ramp/datagridClickHandler| singleton object| DatagridClickHandler| 
|ramp/ecdmp| singleton object| Ecdmp| 
|ramp/eventManager| resource dictionary| EventManager| 
|ramp/featureClickHandler| singleton object| FeatureClickHandler| 
|ramp/featureHighlighter| singleton object| FeatureHighlighter| 
|ramp/filterManager| singleton object| FilterManager| 
|ramp/globalStorage| singleton object| GlobalStorage| 
|ramp/graphicExtension| singleton object| GraphicExtension| 
|ramp/map| singleton object| Map| 
|ramp/maptips| singleton object| Maptips| 
|ramp/navigation| singleton object| Navigation| 
|ramp/quickzoom| class| Quickzoom| 
|ramp/ramp| singleton object| Ramp| 
|Folder: Utils| | | |
|utils/array| utilities| UtilArray| 
|utils/decorator| utilities| Decorator| 
|utils/dictionary| utilities| UtilDict| 
|utils/functionMangler| singleton object| FunctionMangler| 
|utils/popupManager| ? (singleton or utilities)| PopupManager| 
|utils/prototype| singleton object| UtilPrototype| 
|utils/url| class| UtilUrl| 
|utils/util| utilities| UtilMisc| Misc = Miscellaenous



Why everything is singleton in RAMP:
-	There is only one map, one navigation widget, one datagrid, etc. There is no point of having an option to create two datagrids or two maps. Even if the datagrid needs a tabbing option, there may be multiple “tab” objects, but there is still only one datagrid. 
-	Keeps code simpler. We no longer need to use “this.” everywhere. Using “this” caused a lot of problems with scope when we’re using anonymous functions (e.g. in publish/subscribe, arrayUtil.forEach, deferred.after). We need to “hitch” (using dojo/lang’s hitch function) the scope onto the function, and sometimes when there is an anonymous function nested in another anonymous function, the scoping gets tricky. Numerous times, a variable or function is unexpectedly undefined due to scoping issues. When everything is singleton, we no longer use “this”, instead we declare all the variables at the top of the file, and due to closure, the variables are always in scope. 
-	Each Utility class only needs one instance of itself, there’s no point of having two instance of the same Utility class.
-	Each Resource class only needs one instance of itself for the same reason. 

How to convert singleton to classes that allow more than one instance:
If we ever do need to convert back to classes that allow multiple objects, the conversion would not be difficult. 
This is an example of a really simple singleton class
`
define(["util/util"],
    function (UtilMisc) {

        var foo;
        return {
            init: function (value) {
                foo = value;
            },

            bar: function () {
                return foo;
            }
        };
    });
`
Usage (assume the singleton class is located at “modules/sample.js”):
`
define(["modules/sample"],
    function (Sample) {

        // … Other Code …
        Sample.init(45);
        Sample.bar(); // Returns 45

    });
`
Now to convert it into a class that can create multiple instances, we just need to define a constructor function and wrap it around our existing code:
`
define([“util/util”],
    function (UtilMisc) { 
        return {
            createNewInstance : function(args) {
     var foo;
     return {
         init : function(value) {
             foo = value;
         },
 
            	         bar : function () {
                            return foo;
                        }
                    };
        };
    });
`

Note the only thing that was added is:
`
return {	     
    createNewInstance : function(args) {
        // Original code here
    }
}
`
You could move the code in the init function into the createNewInstance function:
`
define([“util/util”],
    function (UtilMisc) { 
        return {
            createNewInstance : function(value) {
     var foo = value;
     return {
 
            	         bar : function () {
                            return foo;
                        }
                    };
        };
    });
`
Usage:
`
define([“modules/sample”],
    function (Sample) {
       
	// … Other Code …
	var a = Sample.createNewInstance(45),
                     b = Sample.createNewInstance(50);
	a.bar(); // Returns 45
	b.bar(); // Returns 50
    });
`