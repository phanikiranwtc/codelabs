# Responsive design and development for Desktop & Tablet

## Overview
This CodeLab introduces Designing of Responsive Application with Ext JS 5 / Ext JS 6. With the new tablet support in Ext JS 5, responsive design became important. Hence, it provided tools to make an application respond to changes in screen size or orientation, making development of cross-device applications a lot easier. 

### What you'll learn?
1. What is responsiveness?

2. What is responsiveConfig?

3. Which configs can be responsive

4. How does it work?

5. How to make a component responsive?

6. How to make a class responsive?

7. What are rules?

8. What is responsiveFormulas?

### What you'll need?
1. [Sencha Cmd 6.x](https://www.sencha.com/products/sencha-cmd/download)
2. [Sencha Ext JS 6.x SDK](https://www.sencha.com/products/extjs/#overview)
3. [Google Chrome Browser](https://www.google.com/intl/en/chrome/)
4. Text Editor or IDE
5. [XAMPP - Web Server](https://www.apachefriends.org/index.html) - optional

## Environment Setup
**1. Install Sencha Cmd**

   * Open terminal window and type the command `sencha which`. If Cmd is installed, it will show the Sencha Cmd version installed on
     your system. If it is `6.0.1` or above then it is good otherwise,

   	* Type the command `sencha upgrade`. If older version of Cmd is installed, it will start downloading the latest copy. If it
   	  is not installed, it will show you the `command not found` message.  
   	* In case, Cmd is not installed or your firewall prevents you from running the command `sencha upgrade`, you can install the
   	  latest copy of Cmd by downloading from `http://www.sencha.com/products/sencha-cmd/download`. Choose the option that
   	  includes Java JRE.
   	* After it has been installed by either way, run the command `sencha which` from terminal window. It should now show the
          latest installed Cmd version.
  
**2. Obtain Sencha Ext JS SDK**

 * Downlaod Ext JS 6 SDK from URL `https://www.sencha.com/products/extjs/#overview`.
 * After downloading, unzip the Ext JS SDK at the system root folder.
 * Change the SDK folder's name to  `ext-6`.
	
It should be as follows:
 * Windows: C:/ext-6
 * Mac/UNIX: ~/ext-6
 
**3. Install Google Chrome**

   * Download and install Chrome from `https://www.google.com/intl/en/chrome/`.
   
**4. Obtain Text Editor or IDE**

   * You can choose the text editor or IDE of your choice.

**5. Install XAMPP**

   * This is optional as Sencha libraries work with any server. 
   * For this session, we will use the Sencha Cmd built-in jetty server.
	
**6. Start the Web server**

   * Start the jetty server by opening the terminal window and typing the following command at `c:/`
     
     `sencha web start`
     
   * After starting the jetty server, you should see the following in terminal window:
     
     ![Jetty Server started](https://github.com/walkingtree/codelabs/blob/master/sencha/images/JettyServer.png)
     
   * Leave the terminal window open and running.
     
**7. Test the Server and Ext JS install**

   * Test the install by running URL `localhost:1841` from the browser.
   * You should see the following page: 
   
    ![Ext extracted](https://github.com/walkingtree/codelabs/blob/master/sencha/images/ExtExtracted.png)

   * Now click on `ext-6`, you should see the `Welcome to Ext JS 6.0` page.
   
    ![Welcome to Ext JS](https://github.com/walkingtree/codelabs/blob/master/sencha/images/ExtWelcomePage.png)

**NOTE:** If it does not work then you need to review the install instructions and ensure that the server is running.


## Get the starter project
In this section, let us download and extract the starter project for our responsive application.

1. Download `myfirstresponsiveapp` from [here](https://github.com/walkingtree/codelabs/tree/master/sencha/downloads). Unzip it and place it at `c:/`.	
2. The extracted folder `myfirstresponsiveapp` contains the starter code for classic version of `MyApp`. It must have the following
   folder structure:
   
   ```javascript
	c:/
		myfirstresponsiveapp
			app
			resources
  ``` 	

3. Now open command line and navigate to `c:/ext-6` folder.
4. Generate the `MyApp` application by copying and pasting the following command from `c:/ext-6`:

  ```
   sencha generate app -classic -starter=false MyApp c:/myfirstresponsiveapp
  ```
  
  This would generate the infrastructure for a classic Ext JS application. The `-starter=false` parameter means starter code is not
  created because you already placed your starter code in the `myfirstresponsive` folder.
  
5. In your editor, open file `c:/myfirstresponsiveapp/app.json` and around `line no. 286` in `CSS[]` array, add the following code to
   include the CSS resources: 

   ```json
   {
            "path": "resources/Stylesheet.css",
            "bootstrap": false
   }
   ```
   
6. Save your work.   

7. The first time an application is created, you need to initialize the microloader and create the CSS. To do that, from the terminal
   window, navigate to `c:/myfirstresponsiveapp` and run the following command:

   ```
   sencha app build development
   ```

8. Open browser and run the application by giving URL `localhost:1841/myfirstresponsiveapp`. You should see the following screen:

   ![Responsive Application Screen 1](https://github.com/walkingtree/codelabs/blob/master/sencha/images/Respnsiveapp1.png)
   
### Understand the generated files and folders
* `app/view/Banner.js` - Docked top banner with image and title.
* `app/view/PostFilter.js` - Docked at the top, it has a combo box to select posts by category.
* `app/model/Feed.js` - Model for the combo box store.
* `app/view/posts/Tabpanel.js` - Container in the center region for Thumbnail and List views.
* `app/view/posts/View.js` - Data view to show posts, selected by category, as thumbnails.
* `app/view/posts/Grid.js` - Grid view to show posts, selected by category, as list.
* `app/view/model/Post.js` - Model for the Grid and Data View store.
* `app/view/post/Detail.js` - Detail Panel for showing and editing selected post's details.
* `app/view/post/DetailController.js` - Controller to handle the Edit button click event.
* `app/view/main/Main.js` - Viewport with border layout. 
* `app/view/main/MainController.js` - Main Controller to handle selection of posts by category.
* `app/view/main/MainModel.js` - Main Model with Feeds and Posts store for Combo box and Data View and Grid 
                                 respectively.


## Enhance the Detail Panel
In this section, let us enhance the Detail Panel so that it shows details of the selected post as follows. Later, we will work on this Detail Panel to make it responsive.

![Responsive Application Screen 2](https://github.com/walkingtree/codelabs/blob/master/sencha/images/Responsiveapp2.png)

### Show details of selected post

1. The Detail Panel shows details as a nicely formatted template. For this, open class `MyApp.view.post.Detail` and add the following code to it: 

  ```javascript
	tpl: [
		'<tpl if="this.isData(values)">',
		'<table><tr><td>',
		'<div class="category font12">{category}</div>',
		'<hr>',
		'<div style = "font-size: 22px; margin: 15px; line-height: 1.5; height: 87px"><b>{title}</b></div>',
		'</br>',
		'<img class="wp-thumbnail" src="http://www.gravatar.com/avatar/{email}"  alt="author"  width="90px" height="90px";>',
		'<p class="published-footer font14" style="position: relative;top: 30px;">published on <span class="font14">{[this.formatDate(values)]}</span></p>',
		'</br>',
		'</br>',
		'</br>',
		'<div style = "margin: 15px;height: 150px" class="lineht18 font14">{excerpt}</div>',
		'</td></tr></table>',
		'</tpl>', {
		isData: function(data) {
			return !Ext.Object.isEmpty(data);
		},
		formatDate: function(values) {
			var date = Ext.util.Format.date(values.date, 'jS M,  Y');
			return date;
		}
		}
	]
  ```
  
2. The `tpl` config will require the `data` property. For this, we will bind the `data` property to `post` property of the
   `MainModel`, as it holds reference to the selected post. So, add the following code to the Detail Panel:

   ```javascript
	bind: {
		data: {
			bindTo: '{post}',
			deep: true
		}
	}
   ``` 
 	
3. Save your work and refresh the application. Select a post from either of the two views. It should show the selected post's details
   in the Detail Panel.

### Handle the Edit button 
We want that when User clicks the Edit button, it should show a pop up Window. The Window should have a Form loaded with the selected post. The Form should have the Save and Cancel buttons to commit or reject the changes made by the User. Have a look at this screenshot. 

  ![Responsive Application Screen 3](https://github.com/walkingtree/codelabs/blob/master/sencha/images/responsiveapp3.png)

1. Open command line and navigate to `myfirstresponsiveapp` project folder. Enter the following command to generate the Window view
   alongwith its Model and Controller: 

   ```
   sencha generate view post.edit.Window
   ```
   
2. The Window has `form` as a child item, which is bound to the selected post. The Form has two fields - `Title` and `Summary`. It
   also has the `Save` and `Cancel` buttons to commit or reject the changes. For this, open class `MyApp.view.post.edit.Window` and
   replace its content with the following code: 

   ```javascript
	Ext.define("MyApp.view.post.edit.Window", {
		extend: "Ext.window.Window",
		requires: [
			"MyApp.view.post.edit.WindowController",
			"MyApp.view.post.edit.WindowModel"
		],
		xtype: 'posteditwindow',
		controller: "post-edit-window",
		viewModel: {
			type: "post-edit-window"
		},
		resizable: false,
		bodyPadding: 8,
		modal: true,
		layout: 'fit',
		items: [{
			xtype: 'form',
			reference: 'form',
			modelValidation: true,
			layout: 'form',
			defaults: {
				labelWidth: '15%',
				xtype: 'textareafield',
				anchor: '100%'
			},
			items: [{
				fieldLabel: 'Title',
				name: 'title',
				bind: {
					value: '{post.title}'
				}	
			}, {
				fieldLabel: 'Summary',
				name: 'summary',
				bind: {
					value: '{post.excerpt}'
				}
			}],
			buttons: [{
				text: 'Save',
				formBind: true,
				handler: 'onSaveClick'
			}, {
				text: 'Cancel',
				handler: 'onCancelClick'
			}]
		}]
	});
  ```

3. Let us now define handlers `onSaveClick` and `onCancelClick` configured for the `Save` and `Cancel` buttons. For this, open class     `MyApp.view.post.edit.WindowController` and add the following code: 

   ```javascript
	onSaveClick: function(button) {
		var post = this.getViewModel().get('post');
		if(post){
			this.getViewModel().get('post').commit();
			this.getView().close();
		} else {
			this.getView().close(); 
		}
	},
	onCancelClick: function(button) {
		var post = this.getViewModel().get('post');
		if(post){
			post.reject();
			this.getView().close();
		} else {
			this.getView().close();
		}
	}
  ```
  
4. Now let us create a Window on `Edit` button click. The handler `onEditClick` has been configured for the `Edit` button. Let us
   define it in the Detail Controller. For this, open class `MyApp.view.post.DetailController` and add the following code:

   ```javascript
	onEditClick: function(button) {
		this.getView().add(Ext.create({
			xtype: 'posteditwindow',
			title: 'Edit Post',
			autoShow: true
		}));
	}
  ```
  
5. Since, in the above code, we are creating an instance of xtype `posteditwindow`, let us require this class in
   `MyApp.view.post.DetailController`. So open this class add require class `MyApp.view.post.edit.Window`.

6. Save your work and refresh the application. Select a post and then click on the Edit button. You should see a Window in which the
   form is loaded with the selected post. Make changes to the title or summary fields and click the Save button. You should be able to
   see changes reflected in the Grid, Data View and Detail Panel. Similarly, if you make changes and click the Cancel button, you
   should not see the changes in any of the three views.  


## What is responsiveness?
Responsiveness is responding dynamically to changes in screen size or orientation. This lets an application adapt to any screen size or orientation, without getting crumpled or disoriented.

Examples: 

	[http://www.businessinsider.com/sun-iphone-app-2012-7?IR=T]

	[http://www.theheinekencompany.com/]s


## What is responsiveConfig?
`responsiveConfig` is a powerful new feature introduced in Ext JS 5, for making applications respond dynamically to changes in screen size or orientation. 

It is simply an object with keys. These keys represent conditions or rules, based on which, certain configs get applied. This allows us to conditionally control the config properties. 

For example,

```javascript
	responsiveConfig: {
		wide: {
		        tabPosition: 'left'
		},
		tall: {
		        tabPosition: 'top'
		}
	}
```

Since not all components need to respond to dynamic conditions, `responsiveConfig` can be implemented as a mixin or a plugin, to allow you to add this functionality to classes or instances. Following two classes are responsible for this: 

	```
	Ext.mixin.Responsive
	
	Ext.plugin.Responsive  
	```

## Which configs can be responsive?
For a config to participate as responsiveConfig, it must have a setter method. In the following example, we can use `tabPosition` as a responsiveConfig because tab panel has a `setTabPosition()` method.

```javascript
	responsiveConfig: {
		wide: {
			tabPosition: 'left'
		},
		tall: {
			tabPosition: 'top'
		}
	}
```

## How does it work?
Internally, the framework monitors the viewport for resize and orientation change. If it finds any such change, it re-evaluates all the responsive rules, specified in `responsiveConfig`. If any rule matches, it calls the respective config's setter method.

## How to make a component instance responsive?
Ext JS components are not responsive by default but, we can make a component instance responsive by adding the `responsive` plugin to the instance config in the `items` array. This will give it the `responsiveConfig` configuration option.

For example, let us consider this responsive application on Sencha Fiddle in which the west region hides, when the screen width is less than 500px.

	[https://fiddle.sencha.com/#fiddle/125o]


## How to make all instances of a component responsive?
Apart from `responsive` plugin, Ext JS 5 also provides `Ext.mixin.Responsive`. This mixin, when mixed into a class, provides the `responsiveConfig`, using which a class can conditionally control the config properties.

As this mixin is added at the class level, all instances of the class will gain the responsive behaviour.

For example, based on the screen height if we want to change the layout of the panel at application level, then we can write a code as follows:

```javascript
	Ext.define('MyApp.view.MyPanel', {
		extend: 'Ext.panel.Panel',
		mixin: 'Ext.mixin.Responsive',
		responsiveConfig: {
			'height < 500': {
				layout: 'hbox'
			},
			'height >= 500': {
				layout: 'vbox'
			}
		},
		defaults: {
		xtype: 'button',
		margin: 5
		},
		items: [{
			text: 'Button1'
			}, {
			text: 'Button2'
			}, {
			text: 'Button3'
		}]
	});
```

Let us take another example. Assume that we want to show a pop up window loaded with form data but we want to change the window dimensions  based on whether the device is in landscape or portrait mode. Simultaneously, we also want to change some styles for the form fields.

For this, since Window is a top level class, we can add a mixin to it for making it responsive. But, as `form` is a child and is also not a class authored by us, we can make it responsive by adding `responsive` plugin to it.

``` javascript
	Ext.define('MyApp.view.post.edit.Window', {
	    extend: 'Ext.window.Window',
	    mixins: ['Ext.mixin.Responsive'],
	    xtype: 'posteditwindow',
	    layout: 'fit',
	    responsiveConfig: {
	       landscape: {
	            width: 400,
	            height: 300
	        },
	        portrait: {
	            width: 250,
	            height: 400
	        }
	    },
	    items: [{
	        xtype: 'form',
	        plugins: 'responsive',
	        responsiveConfig: { 
	            landscape: {
	            	fieldDefaults:{
	                    fieldStyle: {
	                        fontSize: '14px',
	                        lineHeight: '1.5'
	                    },
	                    labelClsExtra: 'font14',
	                    grow: false
	           	}
	            },	
	            portrait: {
	                fieldDefaults:{
	                    fieldStyle: {
	                        fontSize: '22px',
	                        lineHeight: '1.8'
	                    },
	                    labelClsExtra: 'font22',
	                    grow: true,
	                    growMax: 150
	                }
	            }    
	        },
	        items: [{
	            xtype: 'textareafield', 	
	            fieldLabel: 'Title',
	            name: 'title'
	            }, {
	            xtype: 'textareafield',
	            fieldLabel: 'Summary',
	            name: 'summary'
	        }],
	        buttons: [{
	            text: 'Save',
	            handler: 'onSaveClick'
	        }, {
	            text: 'Cancel',
	            handler: 'onCancelClick'
	        }]
	    }]
	});
```

## Testing
You can test a responsive application by any of the following three ways:
* Resizing your browser
* Using device metrics
* Adding a browser plugin from the URL:
	* `https://chrome.google.com/webstore/detail/browser-resize/pnmdcoaajafdppfpioijldebfbpogopn`
	

## Let us implement the learnt concepts
As some users will be using a tablet and view the application in either landscape or portrait mode, let us make it responsive depending on the orientation of the browser window. 

### Make the Detail Panel responsive
Refresh the application and open Developers Tools. Click on the Device Mode icon and select iPad. Test the application in landscape and portrait mode. Since the portrait mode has less screen width and the Detail Panel is occupying a lot of it, the Thumbnail View and the List View do not show properly. So, let us move the Detail Panel to the south region when the application is run on a tablet in portrait mode.

Our other requirement is to make the Detail panel collapsible in portrait mode so that the user can collapse it by choice to have a better look of the Thumbnail and List views.

![Region Change](https://github.com/walkingtree/codelabs/blob/master/sencha/images/DetailPanel1.png)

Let us implement this.

1. The east Detail Panel is created in the Main view. To make it responsive, we can either modify the `Detail` class to use the
   `Ext.mixin.Responsive mixin`, or we can modify the main panel's config to use the mixin plugin. Here, we will use the mixin.
2. Open class `MyApp.view.post.Detail` and add the mixin responsive after `requires` array.

   ```
   mixins: ['Ext.mixin.Responsive']
   ```  
3. For changing the region and collapsible property, add the following code:

  ```javascript
	responsiveConfig: {
		wide: {
			region: 'east',
			collapsible: false
		},
		tall: {
			region: 'south',
			collapsible: true
		}
	}
  ```
  
4. Save your work and refresh the application. Test the application for iPad in portrait mode. You should see the collapsible Detail
   Panel in the south region.
  

### Make it more responsive
When the Detail Panel comes to the south, we want it to lay out as a two column of data. 

![Two Column Layout](https://github.com/walkingtree/codelabs/blob/master/sencha/images/DetailPanel2.png)

1. To do this, modify the template to use a one or two column table. Replace the `tpl` config od `Detail.js` with the following code.
   Note that the second column is used only when the window is in portrait mode.

   ```javascript
	tpl: [
	        '<tpl if="this.isData(values)">',
	        '<table><tr><td>',
	        '<div class="category font12">{category}</div>',
	        '<hr>',
	        '<div style = "font-size: 22px; margin: 15px; line-height: 1.5; height: 87px"><b>{title}</b></div>',
	        '</br>',
	        '<tpl if="this.isPortrait()">',
	        '</td><td>',
	        '</tpl>',
	        '<img class="wp-thumbnail" src="http://www.gravatar.com/avatar/{email}"  alt="author"  width="90px" height="90px";>',
	        '<tpl if="!this.isPortrait()">',
	        '<p class="published-footer" style="position: relative;top: 30px;">published on <span class="font14">{[this.formatDate(values)]}</span></p>',
	        '<tpl else>',
	        '<p class="published-footer font22" style="position: relative;top: 30px;">published on <span class="font18"><b>{[this.formatDate(values)]}</b></span></p>',
	        '</tpl>',
	        '</br>',
	        '</br>',
	        '</br>',
	        '<tpl if="!this.isPortrait()">',
	        '<div style = "margin: 15px; font-size: 14px;height:150px" class="lineht18">{excerpt}</div>',
	        '<tpl else>',
	        '<div style = "margin: 15px;height: 150px" class="lineht18 font22">{excerpt}</div>',
	        '</tpl>',
	        '</td></tr></table>',
	        '</tpl>', {
	            isData: function(data) {
	                return !Ext.Object.isEmpty(data);
	            },
	            formatDate: function(values) {
	                var date = Ext.util.Format.date(values.date, 'jS M,  Y');
	                return date;
	            },
	            isPortrait: function() {
	                return Ext.dom.Element.getOrientation() === 'portrait';
	            }
	        }
	    ]
   ```

2. Save your work and refresh the app. Test it in portrait and landscape mode. 

### Handle the re-rendering of template on region change
Notice that it shows the same one or two column layout even when the device mode is changed. The problem is that the template is bound to post changing and, in this case, the post is not changing — whether the Detail Panel is at the east or south, the post being viewed is the same. So, we need to add code that re-renders the template by running setData() again. For this,

1. Add this method to the Detail Panel:

   ```javascript
	setRegion: function(region) {
		this.expand();
		this.callParent(arguments);
		this.setData(this.getData());
	}
  ```
  
  The responsive config is setting the `region` property so it will run the `setRegion()` method internally. The new method is just
  overriding the `Ext.Component.setRegion()` method. It runs `this.setData(this.getData());` to re-run the template.

2. Save your changes, refresh the browser window, then change the window size. You should see two columns, or one, depending on the
   orientation.

  
## What are rules? 
Each key or rule or condition in the responsiveConfig object is a simple JavaScript expression. Based on the screen size or orientation, these rules are dynamically evaluated. Whichever rule matches, its associated configs are applied.

The following variables are available for use in responsiveConfig rules:

* **landscape** – True if the device orientation is landscape (always `true` on desktop devices).
* **portrait** – True if the device orientation is portrait (always `false` on desktop devices).
* **tall** – True if `width <  height` regardless of device type.
* **wide** – True if `width > height` regardless of device type.
* **width** – The width of the viewport in pixels.
* **height** – The height of the viewport in pixels.
* **platform** - An object containing various booleans describing the platform. The properties of this object are also available
                 implicitly (without 'platform' prefix) but using it is more self-documenting and may be useful to resolve anmbiguity,
                 incase `responsiveFormulas` overlaps or hides any of these properties.  

For example,

```javascript
	responsiveConfig: {
		wide: {
			region: 'east'
		},
		tall: {
			region: 'south'
		}
	}
```

Another example,

```javascript
	responsiveConfig: {
		desktop: {
			title: 'Some long descriptive title'
		},
		!desktop: {
			title: 'Some short title'
		}
	}
```

Yet another example,

```javascript
	Ext.create({
		xtype: 'viewport',
		layout: 'border',
		items: [{
			xtype: 'tabpanel',
			region: 'center',
			plugins: 'responsive',
			responsiveConfig: {
				'width < 800': {
					 tabPosition: 'top',
              				 tabRotation: 0
				},
				'width >= 800': {
					 tabPosition: 'left',
              				 tabRotation: 2
				}
			}
		},{
		       xtype: 'panel',
		       region: 'east',
		       width: 150,
        	       title: 'Details'
    		}]
	});
```

We can also combine these variables in a variety of ways to create complex responsive rules.

For example,

```javascript
	responsiveConfig: {
		'width < 600 && tall': {
			hidden: true,
			visible: false
		},
		'width >= 600': {
			hidden: false,
                    	visible: true
		}
	}
```

Another example,

```javascript
	responsiveConfig: {
		'desktop || width > 800': {
			region: 'west'
		},
		'!(desktop || width > 800)': {
			region: 'north'
		}
	}
```

## What is responsiveFormulas
`responsiveConfig` can sometimes have complex recurring javascript expressions which can make the whole responsiveConfig object very  object. In such a scenario, we can use `responsiveFormulas` which allows to cut down on this repetition by adding new properties to the "scope" for the rules in a `responsiveConfig`.
 
For example, in the code given below, we are setting the region and visibility of a component based on the responsive rule.   

```javascript
	responsiveFormulas: {
		smallView: "width < 500",
		mediumView: "width >= 500 && width < 800 ",
		largeView: "width >= 800 ",
		customFunction : function(context) {
			/**This is the function where we can add our logic
			*where as context object holds  the various
			*context values
			**/
		},
		isTouchDevice: function(context) {
			// Example of custom function
			return Ext.feature.has.Touch;
		}	
	}
```

With the above declaration, any `responsiveConfig` can now use these value as shown below:

```javascript
	responsiveConfig: {
		smallView: {
			hidden: true
		},
		mediumView: {
			hidden: false,
			region: "north"
		},
		largeView: {
			hidden: false,
			region: "west"
		},
		isTouchDevice: {
			interactions: [{
				type: 'panzoom'
			}]
		}	
	}
```

## Let us implement the learnt concepts
Since we have more screen width in landscape mode as compared to portrait mode, let's change the Window dimensions based on screen orientation. 

###### Landscape mode

![Window in landscape mode](https://github.com/walkingtree/codelabs/blob/master/sencha/images/WindowLandscape.png)

###### Portrait mode

![Window in portrait mode](https://github.com/walkingtree/codelabs/blob/master/sencha/images/WindowPortraitpng.png)

      
### Make the Window responsive
As Window is a top level class, we will use the responsive mixin rather than the plugin. 

Later, we will also make the form fields responsive for which we will need the same rules again. So, rather than repeating the rules, why not to describe some responsiveFormulas!

1. Open class `MyApp.view.post.edit.Window` in editor, and add the responsive mixin.

   ```javascript
         mixins: ['Ext.mixin.Responsive']
   ```      

2. We want to test the app in landscape and portrait mode on a tablet. We can use the rules `wide` and `tall` but let us be more
   specific here and create our own rules. In the following code, we have defined custom functions / responsive formulas to check the
   dimensions of device and we paln to use them in responsiveConfig later. So, copy and paste this code in `Window.js` class.

   ```javascript
	responsiveFormulas: {
		isPortrait : function(context) {
			if(context.height > 1000 && context.width <800)
				return true;
			else 
				return false;
		},
		isLandscape : function(context) {
			if(context.width > 1000 && context.height < 800)
				return true;
			else 
				return false;
		}
	}
   ```
   
3. Now, let us change the Window dimensions depending on device orientation. For this, add the following code which calls the
   pre-defined responsive formulas and based on their truth value, sets the configs for Window.

   ```javascript
	responsiveConfig: {
		isLandscape: {
			width: 700,
			height: 350
		},
		isPortrait: {
			width: 400,
			height: 400
		}
	}
   ```
  
4. Save your work and refresh the app. Select a post and click the Edit button. You should see the Window changing its dimensions
   according to change in device orientation.


### Make the form fields responsive
We want that with change in Window dimensions, the Form textareafield should also grow accordingly. Since, `xtype: 'form'` is in the Window class hierarchy and is not a class authored by us,we would use the responsive plugin here.

1. Add the following code to form config object:

   ```javascript
         `plugins: 'responsive'`
   ```

2. `responsiveFormulas` have already been defined at the Window class level, so let us use them in the `responsiveConfig` for form by
   adding the following code:

   ```javascript
         responsiveConfig: {
            isLandscape: {
                 fieldDefaults:{
                    grow: true,
                    growMax: 150
                }
            },
            isPortrait: {
                fieldDefaults:{
                    grow: true,
                    growMax: 200
                }
            }    
        }
   ```        
      
3. Save your work and refresh the app. Test the app in portrait and landscape mode. You should see the teaxtareafields grow
   differently with change in Window dimensions.

## Just for fun
Let us make the tab panel responsive too. When the window is tall, we'll put the tabs on the left and rotate the text. When the window is wide, we'll put the tabs on top. Here is a screenshot of tabpanel viewed in portrait mode.

![Tab Panel Responsive](https://github.com/walkingtree/codelabs/blob/master/sencha/images/TabpanelResponsive.png)

1. Since the tab panel is a top-level class, we can use the `mixins:['Ext.mixin.Responsive']` rather than the plugin. So, open class
   `MyApp.view.posts.TabPanel` and add the mixin.

2. Add the `responsiveFormulas`

   ```javascript
	responsiveFormulas: {
		isPortrait: function(context) {
			if (context.height > 1000 && context.width < 800)
				return true;
			else
				return false;
		},
		isLandscape: function(context) {
			if (context.width > 1000 && context.height < 800)
				return true;
			else
				return false;
		}
	}
   ```	

3. Now add a `responsiveConfig`. Have it set `tabPosition` to `top` when `isLandscape()` returns true, or `left` when `isPortrait()`
   returns true. Also, set `tabRotation` to `0` (horizontal) when `isLandscape()` returns true, or `2` (counter-clockwise) when
   `isPortrait()` returns true.

   ```javascript
	responsiveConfig: {
		isLandscape: {
			tabPosition: 'top',
			tabRotation: 0
		},
		isPortrait: {
			tabPosition: 'left',
			tabRotation: 2
		}
	}
  ```

3. Save and refresh the application. You should see the tabbar move to left and tabs rotated when the application is run on tablet in
   portrait mode.
  

## Let us recap it together
* Responsiveness is responding dynamically to changes in screen size or orientation.

* `responsiveConfig` is the config that contains the code to make applications respond dynamically to changes in screen size or
   orientation. 

* `responsiveConfig` is simply an object with keys, which represent conditions or rules. Based on the truth value of these conditions,   certain configs get applied, allowing us to conditionally control the config properties. 

* For a config to participate as responsiveConfig, it must have a setter method. For example, we can use `tabPosition` as a
  responsiveConfig because tab panel has a `setTabPosition()` method. For example: 
	
          ```javascript
		  responsiveConfig: {
		        wide: {
		              tabPosition: 'top'
		       },
		        tall: {
		              tabPosition: 'left'
		        }
		    }
          ```

* `responsiveConfig` can be implemented as a mixin or a plugin for adding this functionality either to classes or instances.

* The plugin is added as follows. The class instance to which it is added becomes responsive.

	```javascript
		plugins: 'responsive'
	```
	
* The mixin is added as follows. As it is added at class level, all instances of that class will gain the responsive behaviour. 

	```javascript
		mixins: ['Ext.mixin.Responsive']
	```	

* Following rules are available but these can be any valid javascript expressions:

	* **landscape** – True if the device orientation is landscape (always `true` on desktop devices).
	* **portrait** – True if the device orientation is portrait (always `false` on desktop devices).
	* **tall** – True if `width <  height` regardless of device type.
	* **wide** – True if `width > height` regardless of device type.
	* **width** – The width of the viewport in pixels.
	* **height** – The height of the viewport in pixels.
	* **platform** - An object containing various booleans describing the platform. The properties of this object are also
	                 available implicitly (without 'platform' prefix) but using it is more self-documenting and may be useful to
                         resolve anmbiguity, incase `responsiveFormulas` overlaps or hides any of these properties.  

*  `responsiveFormulas` allow to cut down on repetition of rules in responsiveConfig, by adding new properties to the "scope" for the
   rules in a responsiveConfig. For example, once the following responsiveFormulas is defined, it can be used in any responsiveConfig.

	```javascript
		responsiveFormulas: {
			smallView: "width < 500",
			mediumView: "width >= 500 && width < 800 ",
			largeView: "width >= 800 ",
			isTouchDevice: function(context) {
				return Ext.feature.has.Touch;
			}	
		}
	```
	
* Responsive applications can be tested by any of the following ways:
	* Resizing the browser
	* Using device metrics
	* Adding a browser plugin from the URL:
		* `https://chrome.google.com/webstore/detail/browser-resize/pnmdcoaajafdppfpioijldebfbpogopn`
		
## Summary
The idea of responsive design was already very popular in the web design world. With the introduction of touch / mobile support in Ext JS 5, it became important to provide this functionality for Ext JS applications also. Hence, a new responsive mixin and plugin were introduced, which provide the responsiveConfig to conditionally control the config properties and make components respond to changes in screen size or orientation. This makes development of cross-device applications in Ext JS a simple task.


