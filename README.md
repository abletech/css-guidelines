# Abletech CSS Guidelines

The following are guidlines that should be followed (where possible) so we can keep all of our CSS/SASS easy to read and maintain. They are generally good practice.

***

### Contents 

* [Frameworks](#frameworks)
  * [Bootstrap](#bootstrap) 
  * [Normalize](#normalize) 
  * [Compass](#compass) 
* [Structure](#structure)
  * [Stylesheet Structure](#stylesheet-structure) 
  * [Layout Manifest Contents](#layout-manifest-contents) 
* [Writing CSS and SASS](#best-practices)
  * [Naming Conventions](#naming-conventions) 
  * [How to format CSS](#how-to-format-css) 
  * [How to format SCSS](#how-to-format-scss) 
  * [How to format comments](#how-to-format-comments)   
  * [Writing maintainable SCSS](#writing-maintainable-scss) 
  * [Writing maintainable CSS](#writing-maintainable-css) 
  * [Writing Mixins](#writing-mixins) 
  * [Example of bad CSS](#example-of-bad-css) 
* [Best Practices](#best-practices)


***

### Frameworks

**The following are a list of frameworks that should be included in every project to help with front-end development**

#### Bootstrap

**A front-end framework for faster and easier web development**

Bootstrap can be added to your project by adding the following to your gemfile:

<pre>
gem 'sass-rails', '~> 3.2'
gem 'bootstrap-sass', '~> 2.3.2.1'
</pre>

then importing bootstrap in your manifest stylesheet:

<pre>
@import "bootstrap";
@import "bootstrap-responsive";
</pre>

#### Normalize

**Makes browsers render all elements more consistently and in line with modern standards**

**( Note: This might be included by bootstrap already )**

Normalize can be added to your project by downloading the following file and including in your common styles folder:

[normalize](http://necolas.github.com/normalize.css/2.1.2/normalize.css)

#### Compass

**A framework full of reusable cross-browser patterns**

Compass can be added to your project by adding the following to your gemfile:

<pre>
gem "compass-rails";
</pre>

then importing compass in your manifest stylesheet:

<pre>
@import "compass";
</pre>

***

### Structure

**The following will tell you how to structure your CSS/SASS in your application**

#### Stylesheet Structure

This is generally how your stylesheets directory should look. The layout files are used as an example for what your specific styles would be.

<pre>
/stylesheets
  	layout1.scss
  	layout2.scss
  	layout3.scss
	
	/partials
  		_base.scss
  		_grid.scss
  		_variables.scss
  		_mixins.scss  
  		_typography.scss
  		_button.scss
  		_forms.scss
  		
  	/vendor
  		_plugin.scss
  		    
	/layout1
  		_yourstyles.scss
  		
	/layout2
  		_yourstyles.scss
  		
	/layout3
  		_yourstyles.scss  		
  				
</pre>

When including your stylesheets you should always make sure they come in the following order:

* **Reset** (A common reset to help aid cross-browser differences)
* **Framework** (A framework such as Boostrap which your project is built upon)
* **Vendor** (Vendor styles for plugins etc..)
* **Common** (Common Styles which are used throughout the project)
* **Specific** (Specific styles that are unique to layouts or pages)

#### Layout Manifest Contents

Your application manifest file should look like the following:

<pre>
// import frameworks
@import "bootstrap";
@import "bootstrap-responsive";
@import "compass";

// import vendor
@import "vendor/plugin";

// import variables and mixins
@import "common/variables"
@import "common/mixins"

// import common partials
@import "common/base"
@import "common/grid"
@import "common/typography"
@import "common/button"
@import "common/forms"

// import all layout specific styles
@import "layout1/*/**"
</pre>

Bootstrap is imported into the manifest file as a base for every project. **Make sure you have the [bootstrap-sass](https://github.com/thomas-mcdonald/bootstrap-sass) gem installed.**

The bootstrap-responsive import is optional depending on whether you want a responsive layout or not.

Common partials can be added and removed depending on what you need for each layout. This gives you great control of what styles are being compiled into each layout.

***

### Writing CSS and SASS

#### Naming Conventions

<pre>
.btn-large
</pre>

* Make sure you always use hyphens where a space is needed as opposed to using an underscore
* Use sensible names for the type of element and it's function

#### How to format CSS

<pre>
[class] {
	[property]: [value];
	[property]: [value];
	[property]: [value];
	[property]: [value];
}

[class] {
	[property]: [value];
	[property]: [value];
}
</pre>

* Always make sure each new property is on a new line to improve readability. 
* Make sure there is a space before the value.
* Include a semi-colon at the end of each line. 
* Include a space between each style block

#### How to format SCSS

<pre>
[class] {

	[class] {
		[property]: [value];
	}	
	
}
</pre>

* Use exactly the same formatting as CSS but you can now nest rules inside one another
* Try not to nest past 3 levels deep where possible

#### How to format comments

<pre>
/*
This is a block comment
This is a block comment
This is a block comment
*/

[class] {
	[property]: [value]; /* Inline Comment */
}
</pre>

* Comments should never be compiled into the css on the live site, they are just there for development
* Use comments where needed so other developers can understand what you have done

#### Writing maintainable SCSS

**My variables file**

<pre>

$btn-primary-background: #1e13f7;
$btn-primary-border: #171188;

$btn-secondary-background: #f7472b;
$btn-secondary-border: #571a10;

</pre>

I also used a variables file to maintain color variables for button  

**My buttons file**

<pre>

%btn {
  padding: 5px 20px;
  border-width: 1px;
  border-style: solid;
  @include border-radius(3px); 
}

.btn-primary {
  @extend %btn; 
  background-color: $btn-primary-background;
  border-color: $btn-primary-border;
  &:hover {
    background-color: shade($btn-primary-background, 10%);
    border-color: shade($btn-primary-border, 5%);
  }
}

.btn-secondary {
  @extend %btn; 
  background-color: $btn-secondary-background;
  border-color: $btn-secondary-border;
  &:hover {
    background-color: shade($btn-secondary-background, 10%);
    border-color: shade($btn-secondary-border, 5%);
  }
}

</pre>

Above im using a placeholder for buttons called %btn which includes all the base styles for the button that are common to every button. **Note: This placeholder is not compiled into the css unless it's extended by another class.** 

When I create the primary btn class, I simple extend the placeholder %btn so that it uses these base styles then I build my unique styles on top of that. E.g the background color and border color.

#### Writing maintainable CSS

<pre>
.content-box {
  float: left;
  padding: 10px;
  margin: 10px 0;
  width: 100%;
  border-style: solid;
  border-width: 1px;
  &.red {
    background-color: red;
    border-color: red;
  }
  &.blue {
    background-color: blue;
    border-color: blue;
  }
  &.small {
    height: 300px;
  }
  &.large {
    height: 500px;
  }
}
</pre>

#### Example of bad CSS

<pre>

.content-box-red {
  float: left;
  padding: 10px;
  margin: 10px 0;
  width: 100%;
  background-color: red;
  border: 1px solid red;
}

.content-box-blue {
  float: left;
  padding: 10px;
  margin: 10px 0;
  width: 100%;
  background-color: blue;
  border: 1px solid blue;
}

.content-box-blue-small {
  float: left;
  padding: 10px;
  margin: 10px 0;
  width: 100%;
  background-color: blue;
  border: 1px solid blue;
  height: 300px;
}

.content-box-red-large {
  float: left;
  padding: 10px;
  margin: 10px 0;
  width: 100%;
  background-color: red;
  border: 1px solid red;
  height: 500px;
}

</pre>

The above is a bad example of css, the common styles that each element shares should have been extracted into a seperate class called content-box

#### Writing Mixins

<pre>
$mobile: 320px;
$tablet: 1024px;
$desktop: 1280px;

@mixin respond-to($media) {

  @if $media == mobile {
    @media only screen and (max-width: $mobile) { @content; }
  }
  
  @else if $media == tablet {
    @media only screen and (min-width: $mobile + 1) and (max-width: $tablet - 1) { @content; }
  }
  
  @else if $media == desktop {
    @media only screen and (min-width: $desktop) { @content; }
  }
  
}
</pre>

Above is a mixin used for media queries. The mixin is written so it can be included on any element then you can define the size of the screen you want the mixin to react to.

***

### Best Practices

Below are a few best practices that can be applied when working with SASS or CSS:

* Try to write as little CSS as possible using techniques from OOCSS 
* Be careful writing too specific CSS on general classes as it will cascade down
* Use comments where possible so other developers understand what you have done
* Never use ID's to style elements! Only ever use them if you need javascript to refer to specific elements
* Always adopt the same spacing and naming conventions that the application your working on uses
* Add common styles to common directory and specific styles to the layout/page directory
* Always try to keep selectors as short as possible and less specific

***