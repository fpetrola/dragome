##Services

There are some parts of your application that need to be located at server side, such as database access, ORM interactions, heavy load proccesses, etc. 

For this purpose a services mechanism is provided, that helps you to communicate both sides with almost no effort. 
It's very simple, you need to create a Interface for your service, and then create a concrete class that implements it, just this.

Dragome will automatically resolve which is the implementor of your service, will create a proxy that serialize the data, sends the petition to server, deserialize data, and execute the desired method of concrete class. Then if there is a result it will transport it back to the browser using the same automatic mechanism.


Let's see an example:

###Service Interface
``` Java
public interface HelloWorldService
{
	public abstract String getGreetingsFor(String name);
}
```

###Service Implementor
``` Java
public class HelloWolrdServiceImpl implements HelloWorldService
{
	public String getGreetingsFor(String name)
	{
		return "Hello " + name + "! (" + new Date() + ")";
	}
}
```
###Service instantation
``` Java
HelloWorldService helloWorldService= serviceFactory.createSyncService(HelloWorldService.class);
```


This HelloWorldService 

