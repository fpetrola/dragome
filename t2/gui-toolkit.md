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

###VisualPanel

Usage example:
``` Java
VisualPanel panel= new VisualPanelImpl("panel1");
panel.addChild(label);
panel.addChild(button);
panel.addChild(textfield);
```





