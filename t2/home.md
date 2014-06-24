#What is Dragome?
Dragome is an open source tool for creating web applications in pure Java language.  
Based on bytecode to javascript compilation, you can execute applications written in Java directly on browsers.  
You can use your favorite IDE, your favorite Java frameworks and tools because Dragome is totally transparent.  


[*Learn how to code Dragome apps in 5 minutes*DRAGOME 5' TUTORIAL][4]

##Transparency
* 100% transparent development: Java code runs on browser with no special modifications
* Share code between server and client side
* You can use [Dynamic Proxies][1]
* You can use [Java Reflection API][2]
* Compilation based on bytecode: You can make use of all bytecode instrumentation tools features
* Get rid of callbacks: Make async calls with no callbacks! How? See [Callback Evictor][3] Tool!
* Neither browser or IDE plugin, nor configuration or installation required
* Very fast web development
* Full Java stack web applications
* Debug your code in your favorite Java IDE
* Javascript native interface
* Very powerful template engine



##Start working with Dragome right now!
Maven archetype
```shell
mvn archetype:generate -DarchetypeGroupId=com.dragome -DarchetypeArtifactId=simple-webapp-archetype -DarchetypeVersion=1.0 -DgroupId={your-package-name} -DartifactId={your-app-name}
```


##How is programming web apps with Dragome?
**pure Java! pure HTML! runs as js inside browser!**


##Take a look to the following source code:
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
  [4]: 5-tutorial.md
