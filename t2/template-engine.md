#Template Engine

HTML templates are based in pure HTML files.  
It is not required to be XHTML o well formed HTML, any HTML will work if it can be parsed by a browser.  
No special tags are required, and sub templates (placeholders) are identified using custom data attribute "data-template" which is a standard mechanism.  
For each element containing data-template attribute, template engine creates an instance of Template class following the same structure of HTML.
``` HTML
<html>
<body>
	<div data-template="parent">
		<span data-template="first-child">first</span>
		<span data-template="second-child">first</span>
	</div>
</body>
</html>
```
With this HTML, Template Engine will create a Template instance with name "parent", containing two children that are also instances of Template called "first-child" and "second-child":
``` Java
Template parent= getTemplate("parent");
Template firstChild= parent.getChild("first-child");
Template secondChild= parent.getChild("second-child");
```
In most cases you can create new components just combining existing ones, but in case you need to create a low level component that requires a particular HTML rendering you can access directly to DOM.  
For that, "Template.getContent" method will return this associated element, the one marked with "data-template" attribute.  
Dragome use standard W3C objects to represent all HTML related stuff.
``` Java
org.w3c.dom.Element element= parent.getContent();
element.setAttribute("class", "selected");
```

