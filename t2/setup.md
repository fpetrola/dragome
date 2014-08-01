#Setup

There are no text configuration files in Dragome, everything is managed by some annotations and extending some classes for configuring for example which plugins are enabled.

##Available annotations:

### Type of compiler at class level or method level
@DragomeCompilerSettings(CompilerType.Strict|CompilerType.Standard)

### Page alias (see [hello world example](helloworld-app.md) for more info)
@PageAlias(alias= "{page-name}")

### To specify which class implements a service interface (see [Services](services.md) for more info)
@ServiceImplementation(ConcreteServiceImpl.class)


##For configuring the entire application:


If you want to configure both CallbackEvictor and MethodLogger at the same time you may use this kind of configuration:

``` Java
@DragomeConfiguratorImplementor
public class ExamplesApplicationConfigurator extends CompositeIntrumentationDragomeConfigurator
{
	private CallbackEvictorConfigurator callbackEvictorConfigurator;
	private MethodLoggerConfigurator methodLoggerConfigurator;

	public ExamplesApplicationConfigurator()
	{
		callbackEvictorConfigurator= new CallbackEvictorConfigurator();
		callbackEvictorConfigurator.setEnabled(false);

		methodLoggerConfigurator= new MethodLoggerConfigurator(Person.class.getName());
		methodLoggerConfigurator.setEnabled(true);

		init(callbackEvictorConfigurator, methodLoggerConfigurator);
	}

	public ExecutionHandler getExecutionHandler()
	{
		return callbackEvictorConfigurator.isEnabled() ? callbackEvictorConfigurator.getExecutionHandler() : super.getExecutionHandler();
	}
}
```


## For IDEs and Browsers

Debug capabilities are working only in Chrome browser at current version. Other browsers will be supported in near future.


There is no installation required, neither browser or IDE plugin required.

Avoiding these sad problems...

![alt text](gwt-plugin-chrome.png "gwt-plugin-chrome")

![alt text](gwt-plugin-firefox.png "gwt-plugin-firefox")


