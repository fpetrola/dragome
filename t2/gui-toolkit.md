#Gui Toolkit

##Components

###VisualLabel

Usage example:
``` Java
VisualLabel<String> label= new VisualLabelImpl<String>("label1");
label.setValue("text");
```


###VisualButton

Usage example:
``` Java
VisualButton button= new VisualButtonImpl("button1", v -> System.out.println("Click"));
```


###VisualCheckbox

Usage example:
``` Java
VisualCheckbox checkBox= new VisualCheckboxImpl("check1");
checkBox.setValue(true);
```

###VisualTextField

Usage example:
``` Java
VisualTextField<String> textfield= new VisualTextFieldImpl<String>("textfield1");
textfield.setValue("text");
```


###VisualComboBox

Usage example:
``` Java
VisualComboBox combobox= new VisualComboBoxImpl<String>("nickname", Arrays.asList("Pelusa", "Burrito", "Bocha", "Bruja"))
```


###VisualPanel

Usage example:
``` Java
VisualPanel panel= new VisualPanelImpl("panel1");
panel.addChild(label);
panel.addChild(button);
panel.addChild(textfield);
```


##Binding models


###Binding a textfield

Using ModelBinder:
``` Java
VisualTextFieldImpl<String> textField= new VisualTextFieldImpl<String>("name");
ModelBinder<Person> binder= new ModelBinder<Person>(person);
binder.bindMemberToValueHolder("name", textField, String.class);
```

Using Pectin:
``` Java
VisualTextFieldImpl<String> textField= new VisualTextFieldImpl<String>("name");
AbstractBeanModelProvider<Person> modelProvider= new ReflectionBeanModelProvider<Person>(Person.class);
FieldModel<String> nameFieldModel= new FormModel().fieldOfType(String.class).boundTo(modelProvider, "name");
FormBinder binder= new FormBinder();
binder.bind(nameFieldModel).to(textField);
```

As Dragome Form Bindings is based on GWT Pectin Project [GWT Pectin][1], you can use a lot of its great features.
For more info Take a look at: [GuideBindingToBeans][2]


[1]: https://code.google.com/p/gwt-pectin
[2]: https://code.google.com/p/gwt-pectin/wiki/GuideBindingToBeans


