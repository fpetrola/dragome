#Template Engine

HTML templates are based in pure HTML files.  
No special tags are required, and placeholders (subtemplates) are identified using a custom data attribute "data-template".
For each element containing data-template attribute, template engine will be creating an instance of Template class following the same structure of HTML.
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
With this Template Engine will create a Template instance with name "parent", containing two children that are also instances of Template called "first-child" and "second-child":
``` Java
Template parent= getTemplate("parent");
Template firstChild= parent.getChild("first-child");
Template secondChild= parent.getChild("second-child");
```
In HTML templates "Template.getContent" method will return this associated element, the one marked with "data-template" attribute.
Dragome use standard W3C objects to represent all HTML related stuff.
``` Java
org.w3c.dom.Element element= parent.getContent();
element.setAttribute("class", "selected");
```

