#What is Dragome?
Dragome is an open source tool for creating web applications in pure Java language.  
Based on bytecode to javascript compilation, you can execute applications written in Java directly on browsers.  
You can use your favorite IDE, your favorite Java frameworks and tools because Dragome is totally transparent.  


[Learn how to code Dragome apps in 2 minutes][4]

----------

##Why Dragome
* Code everything in Java (server side and client side), it will be transformed to javascript automatically.
* Higher level of abstraction for GUI development using components.
* High performance for generated client side code.
* Java based development process and code reuse.
* Use the excellent debugging support offered by the mature Java IDEs.
* Make use of built-in IDE support to refactor Java code, improve your agile development.
* Use the IDE syntax highlighting, error checking, code completion shortcuts, fix proposals, etc
* You can create highly responsive web applications with heavy lifting on the client-side and reduced chattiness with the server-side.
* You will minimize contact with HTML files. Managed by graphics designers not developers, Dragome uses exactly the same files that designers are editing, just plain HTML files.
* UI Updates can be deployed by non developer roles because code is totally decoupled from HTML files.
* Use continuation in your development: you can pause your program and continue it whenever you need.
* Unit testing integration: you can also run your Junit tests on a browser.


###Top features
* 100% transparent development: Java code runs on browser with no special modifications
* Share code between server and client side
* Java 8 ready! Build your pages using lambda expressions, default methods, streams
* You can use [Dynamic Proxies][1] and [Java Reflection API][2]
* Compilation based on bytecode: You can make use of all bytecode instrumentation tools features
* Get rid of callbacks: Make async calls with no callbacks! How? See [Callback Evictor][3] Tool!
* Neither browser or IDE plugin, nor configuration or installation required
* Full Java stack web applications
* Debug your code in your favorite Java IDE
* Javascript native interface
* Very powerful template engine
* ... see more at [Features][5]


Also see [Dragome Todos](todos.md) for more info about the future

----------






###Start working with Dragome right now!
Maven archetype
```shell
mvn archetype:generate -DarchetypeGroupId=com.dragome -DarchetypeArtifactId=simple-webapp-archetype -DarchetypeVersion=1.0 -DgroupId={your-package-name} -DartifactId={your-app-name}
```

----------

###How to setup

[Setup your application](app-setup.md)

----------

###How is programming web apps with Dragome?
**pure Java! pure HTML! runs as js inside browser!**

----------

###Take a look at the following source code:

For more details see [Hello World Application][6]

**Service definition**
``` Java
public interface HelloWorldService
{
	public abstract String getGreetingsFor(String name);
}
```

**Service implementation**
``` Java
public class HelloWolrdServiceImpl implements HelloWorldService
{
	public String getGreetingsFor(String name)
	{
		return "Hello " + name + "! (" + new Date() + ")";
	}
}
```

**Service consumer - GUI**
``` Java
public class HelloWorldPage extends DragomeVisualActivity
{
	HelloWorldService helloWorldService= serviceFactory.createSyncService(HelloWorldService.class);

	public void build()
	{
		final VisualLabel<String> label= new VisualLabelImpl<String>("message");
		final VisualButton button= new VisualButtonImpl("button", v -> label.setValue(helloWorldService.getGreetingsFor("World")));
		mainPanel.addChild(label);
		mainPanel.addChild(button);
	}
}
```

**HTML**
``` Html
<html>
<head>
<script type="text/javascript" src="dragome/dragome.js"></script>
</head>

<body>
	Message: <b data-template="message">text</b> <br>
	<button data-template="button">Say hello!</button>
</body>

</html>
```


  [1]: http://docs.oracle.com/javase/7/docs/api/java/lang/reflect/Proxy.html
  [2]: http://docs.oracle.com/javase/tutorial/reflect/
  [3]: callback-evictor.md
  [4]: two-minutes-tutorial.md#DRAGOME%202'%20TUTORIAL
  [5]: features.md
  [6]: helloworld-app.md

