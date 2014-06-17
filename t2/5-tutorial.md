
#5' Dragome Tutorial

Let's create a simple crud using Dragome.

We will make use of some tools provided by Dragome SDK: 
* service creation
* visual components
* model binding
* template engine
* template repeaters 

First of all we create a simple activity for building our page extending DragomeVisualActivity:

``` Java
public class PersonCrudPage extends DragomeVisualActivity
```

We create a proxy to PersonService using a service factory provided in dragome visual activity:

``` Java
PersonService personService= serviceFactory.createSyncService(PersonService.class);
```



