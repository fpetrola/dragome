#Setup

There are no text configuration files in Dragome, everything is managed by some annotations and extending some classes for configuring for example which plugins are enabled.

##Available annotations:

### Type of compiler at class level or method level
@DragomeCompilerSettings(CompilerType.Strict|CompilerType.Standard)

### Page alias (see [hello world example](helloworld-app.md) for more info)
@PageAlias(alias= "{page-name}")

### To specify which class implements a service interface
@ServiceImplementation(ConcreteServiceImpl.class)




DragomeConfigurator
Callback evictor config
Configuracion de serializador por servicio




There is no installation required, neither browser or IDE plugin required.


Avoiding these sad problems...

![alt text](gwt-plugin-chrome.png "gwt-plugin-chrome")

![alt text](gwt-plugin-firefox.png "gwt-plugin-firefox")


