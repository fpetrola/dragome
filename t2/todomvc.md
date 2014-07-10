``` Java
public class TodosPage extends DragomeVisualActivity
{
	TodoController todoController= new TodoController("/", new LocalStorage());

	public void build()
	{
		ComponentBuilder<TodoController> componentBuilder= new ComponentBuilder<TodoController>(mainPanel, todoController);
		componentBuilder.bindTemplate("new-todo").as(VisualTextField.class).toProperty(TodoController::getNewTodo, TodoController::setNewTodo).onKey(this::addTodo).build();

		ComponentBuilder<TodoController> mainSectionBuilder= componentBuilder.bindTemplate("main-section").as(VisualPanel.class).builderFromHere();
		VisualCheckbox allChecked= mainSectionBuilder.bindTemplate("toggle-all").as(VisualCheckbox.class).toProperty(TodoController::isAllChecked, TodoController::setAllChecked).build();
		mainSectionBuilder.show(allChecked).when(() -> !todoController.getTodos().isEmpty());
		mainSectionBuilder.show(mainSectionBuilder.panel()).when(() -> !todoController.getTodos().isEmpty());

		mainSectionBuilder.bindTemplate("completed-todo").as(VisualPanel.class).toListProperty(TodoController::getTodos).filter(TodoController::getStatusFilter).repeat((Todo todo, ComponentBuilder<Todo> icb) -> {

			icb.bindTemplate("todo-input").as(VisualTextField.class).toProperty(Todo::getTitle, Todo::setTitle).onKey(code -> editing(code, todo)).build();
			icb.bindTemplate("title").as(VisualLabel.class).toProperty(Todo::getTitle, Todo::setTitle).onDoubleClick(v -> todoController.editTodo(todo)).build();
			icb.bindTemplate("completed").as(VisualCheckbox.class).toProperty(Todo::isCompleted, Todo::setCompleted).onClick(v -> todo.setCompleted(!todo.isCompleted())).build();
			icb.bindTemplate("destroy").as(VisualButton.class).onClick(v -> todoController.removeTodo(todo)).build();

			VisualPanel component= icb.panel();
			icb.style(component).with("completed").when(todo::isCompleted);
			icb.style(component).with("editing").when(() -> todo == todoController.editedTodo);

		});

		createFooterPanel(componentBuilder);
	}

	private void createFooterPanel(ComponentBuilder<TodoController> mainComponentBuilder)
	{
		ComponentBuilder<TodoController> footerBuilder= mainComponentBuilder.bindTemplate("footer-section").as(VisualPanel.class).builderFromHere();
		footerBuilder.show(footerBuilder.panel()).when(() -> !todoController.getTodos().isEmpty());
		footerBuilder.bindTemplate("items-count").as(VisualLabel.class).toProperty(TodoController::getRemainingCount, TodoController::setRemainingCount).build();
		footerBuilder.bindTemplate("items-label").as(VisualLabel.class).to(() -> todoController.getRemainingCount() == 1 ? "item" : "items").build();

		Stream.of("/", "/active", "/completed").forEach(location -> {
			VisualLink link= footerBuilder.bindTemplate("filter:" + location).as(VisualLink.class).onClick(v -> todoController.setLocation(location)).build();
			footerBuilder.style(link).with("selected").when(() -> todoController.getLocation().equals(location));
		});

		ComponentBuilder<TodoController> clearCompletedBuilder= footerBuilder.bindTemplate("clear-completed").as(VisualPanel.class).onClick(v -> todoController.clearCompletedTodos()).builderFromHere();
		clearCompletedBuilder.bindTemplate("clear-completed-number").as(VisualLabel.class).toProperty(TodoController::getCompletedCount, TodoController::setCompletedCount).build();

		clearCompletedBuilder.show(clearCompletedBuilder.panel()).when(() -> todoController.getCompletedCount() > 0);
	}

	private void addTodo(int code)
	{
		if (code == KeyUpListener.KEY_ENTER)
			todoController.addTodo();
	}

	public void editing(int keyCode, Todo todo)
	{
		if (keyCode == KeyUpListener.KEY_ESC)
			todoController.doneEditing(todo);
	}
}
```

