#First Application

## Source code

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
@PageAlias(alias= "helloworld")
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


##How to execute

You can start a servlet container using this maven command:

``` shell
mvn jetty:run
```

To run any Dragome page you need to start it calling a HTML file that includes "dragome.js" file.
``` Html
<script type="text/javascript" src="dragome/dragome.js"></script>
```

In case you want to debug some part in js, you can use a verbose version of js files including instead the following:

``` Html
<script type="text/javascript" src="dragome-resources/dragome-debug.js"></script>
```


Since HelloWorldPage class has an associated @PageAlias annotation with value "helloworld", you can find this page in the following URL:

http://localhost:8080/my-app1/run.html?helloworld






