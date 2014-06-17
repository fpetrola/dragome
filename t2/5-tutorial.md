
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

A proxy to the service is obtained using a service factory, passing PersonService interface:
``` Java
PersonService personService= serviceFactory.createSyncService(PersonService.class);
```

A proxy to the service is obtained using a service factory, passing PersonService interface:
``` Java
PersonService personService= serviceFactory.createSyncService(PersonService.class);
```


``` Java
public class PersonCrudPage extends DragomeVisualActivity
{
	PersonService personService= serviceFactory.createSyncService(PersonService.class);
	List<Person> persons= new ArrayList<Person>();

	public void build()
	{
		mainPanel.addChild(new VisualButtonImpl("save-button", v -> personService.savePersons(persons)));
		mainPanel.addChild(new VisualButtonImpl("add-button", v -> showPersons(Arrays.asList(new Person()))));

		showPersons(personService.getPersons());
	}

	private void showPersons(final List<Person> somePersons)
	{
		persons.addAll(somePersons);
		new TemplateRepeater<Person>(somePersons, mainTemplate, "row", this::fillTemplate);
	}

	public void fillTemplate(final Person person, Template itemTemplate)
	{
		final VisualPanel rowPanel= new VisualPanelImpl(itemTemplate);
		mainPanel.addChild(rowPanel);

		rowPanel.addChild(new VisualButtonImpl("delete-button", v -> {
			persons.remove(person);
			rowPanel.getParent().removeChild(rowPanel);
		}));

		ModelBinder<Person> modelBinder= new ModelBinder<Person>(person, rowPanel);
		modelBinder.bindToPanel(new VisualTextFieldImpl<String>("givenName"));
		modelBinder.bindToPanel(new VisualTextFieldImpl<String>("surname"));
		modelBinder.bindToPanel(new VisualComboBoxImpl<String>("nickname", Arrays.asList("Pelusa", "Burrito", "Bocha", "Bruja")));
	}
}
```




