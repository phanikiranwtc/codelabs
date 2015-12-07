# Responsive design and development for tabs

## Overview
This CodeLab will introduce Designing of Responsive Applications with Ext JS5. With the new tablet support in Ext JS 5 comes “responsiveConfig”, a powerful new feature for making applications respond dynamically to changes in screen size or orientation.

### What you'll learn?
1. What is responsiveness?

2. What is responsiveConfig?

3. Which configs can be responsive

4. How it works?

5. How to make a component responsive?

6. Example for making a component responsive

7. How to make a class responsive?

8. Example for making a class responsive

9. What are rules?

10. What is responsiveFormulas?

Let’s look at each point in detail

**1. What is responsiveness?**

Responsiveness is nothing but responding dynamically to changes in screen size or orientation.

**2. What is responsiveConfig?**
	
It's a powerful new feature for making applications respond dynamically to changes in screen size or orientation.

The responsiveConfig is simply an object with keys that represent conditions under which certain configs will be applied.

ResponsiveConfig is a config, consists of keys that represents the conditions.

It also allow you to dynamically change component configs based on display rules.

Since not all components need to respond to dynamic conditions, responsiveConfig is implemented as a mixin as well as a plugin to allow you to add this functionality to the classes or instances that need it which can be enabled by two ways.

Ext.plugin.Responsive:  Adds responsive capabilities to an Ext.Component
Ext.mixin.Responsive: Adds responsive capabilities to any other class

**3. Which configs can be responsive?**

For a config to participate as responsiveConfig it must have a setter method.

**4. How it works?**

Internally, the framework monitors the viewport for resize and orientation change,
and it re-evaluates all of the responsive rules whenever either orientation change or resize occurs.

In this process if any rule is matched then it will call respective config's setters method.

**5. How to make a component responsive:**

Ext JS Components do not have responsive features enabled by default, so to make a Component responsive you’ll need to use the responsive plugin.

We can either make all the instances responsive by adding responsive plugin to the class body
or can enable the responsiveness for a single Component instance by adding to the instance config.

If you add the responsive plugin to your Component config, your Component will gain a “responsiveConfig” configuration option

**6. Example for making a component responsive :**
This example will give you brief explanation about responsiveness.
Where west regoin hides when the width is less than 500.
https://fiddle.sencha.com/#fiddle/125o


**7. How to make a class responsive:**

If we want to make some classes responsive other than Ext.Component then we can make use of mixin “Ext.mixin.Responsive”

“Ext.mixin.Responsive” provides user with a responsiveConfig that allows the class to conditionally control config properties.

Assume that we have a situation where we are maintaining the tabPosition( whether top/left ) application level and based on the tabPosition we are setting the behavior of other childs.


Then in this scenario we can make use of responsiveness concept by making the class (which listens for the update of  tabPosition )as responsive. 

**8. Example for making a class responsive :**

Ext.define('MyClass', {
        mixins: ['Ext.mixin.Responsive'],
        config: {
                tabPosition: null
        },
        responsiveConfig: {
                wide: {
                        tabPosition: 'top'
                },
                tall: {
                        tabPosition: 'left'
                }
        },
        constructor: function(config) {
                this.initConfig(config);
        },
        updateTabPosition: function(tabPosition) {
                console.log(tabPosition); // logs "left" or "top" as screen shape changes between wide and tall
        }

});


**9. What are rules? :**
Each key, or “rule”, in the responsiveConfig object is a simple JavaScript expression. 
The following variables are available for use in responsiveConfig rules:


‘landscape’ – True if the device orientation is landscape (always ‘true’ on desktop devices)
‘portrait’ – True if the device orientation is portrait (always ‘false’ on desktop devices)
‘tall’ – True if ‘width’ is less than ‘height’ regardless of device type
‘wide’ – True if ‘width’ is greater than ‘height’ regardless of device type
‘width’ – The width of the viewport
‘height’ – The height of the viewport

We can combine these variables in a variety of ways to create complex responsive rules.


Eg:
responsiveConfig: {
        'width < 768 && tall': {
                visible: true
        },
        'width >= 768': {
                visible: false
        }
}
   
**10. What is responsiveFormulas :**
 It is common when using responsiveConfig to have recurring expressions that make for complex configurations. 

Using responsiveFormulas allows you to cut down on this repetition by adding new properties to the "scope" for the rules in a responsiveConfig.
Eg :
  responsiveFormulas: {
            smallView: 'width < 500’,
			mediumView: 'width >= 500 && width < 800 ',
			largeView: 'width >= 800 ',
			customFunction : function(context) {
			/**This is the function where we can add our logics
                  *where as context object holds  the various
 *context values
                 **/
 			}
  		}
      With the above declaration, any `responsiveConfig` can now use these value as below:
responsiveConfig: {
        'smallView': {
                hidden: true
        },
        'mediumView': {
                hidden: false,
	     region: ‘north’

   },
   'largeView': {
         hidden: false,
	     region: ‘west’

     }

}
In the above example setting the region of a component and visibility based on the responsive rule criteria.     
  

### What you'll need?
1. [Sencha Cmd 5/6](https://www.sencha.com/products/sencha-cmd/)
2. [Sencha ExtJS 5.x or any latest ext SDK](https://www.sencha.com/products/extjs/#overview)
3. Google Chrome Browser
4. [XAMPP - Web Server](https://www.apachefriends.org/index.html)

## Environment Setup
1. Install Sencha Cmd
2. Extract Sencha Ext JS SDK
3. Install XAMPP - this is optional if you intend to use the Web Server built into Sencha Cmd

## Creating the app
In this section, let us create the starter project for our responsive application.

### Generating project using Sencha Cmd
1. Create a folder - `myfirstresponsiveapp`
2. Open Command Prompt or Terminal
3. Change directory to `myfirstresponsiveapp`
4. Run the following Sencha Cmd to generate a sample universal application:
```
sencha -sdk /path/to/ExtSDK generate app MyApp ./myfirstresponsiveapp
```
5. Understand generated folders/files/code

### Run the default generated project

