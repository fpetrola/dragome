``` Java
public class TodosPage extends DragomeVisualActivity
{
	TodoController todoController= new TodoController("/", new LocalStorage());

	public void build()
	{
		ComponentBuilder<TodoController> componentBuilder= new ComponentBuilder<TodoController>(mainPanel, todoController);
		componentBuilder.bindTemplate("new-todo").as(VisualTextField.class).withProperty(TodoController::getNewTodo, TodoController::setNewTodo).onKey(this::addTodo).build();

		ComponentBuilder<TodoController> mainSectionBuilder= componentBuilder.bindTemplate("main-section").as(VisualPanel.class).builderFromHere();
		VisualComponent allChecked= mainSectionBuilder.bindTemplate("toggle-all").as(VisualCheckbox.class).withProperty(TodoController::isAllChecked, TodoController::setAllChecked).build();
		mainSectionBuilder.show(allChecked).when(() -> !todoController.todos.isEmpty());
		mainSectionBuilder.show(mainSectionBuilder.mainComponent()).when(() -> !todoController.getTodos().isEmpty());

		mainSectionBuilder.bindTemplate("completed-todo").as(VisualPanel.class).withListModel(TodoController::getTodos).filter(TodoController::getStatusFilter).repeat((Todo todo, ComponentBuilder<Todo> icb) -> {

			icb.bindTemplate("todo-input").as(VisualTextField.class).withProperty(Todo::getTitle, Todo::setTitle).onKey(code -> editing(code, todo)).build();
			icb.bindTemplate("title").as(VisualLabel.class).withProperty(Todo::getTitle, Todo::setTitle).onDoubleClick(v -> todoController.editTodo(todo)).build();
			icb.bindTemplate("completed").as(VisualCheckbox.class).withProperty(Todo::isCompleted, Todo::setCompleted).onClick(v -> todo.setCompleted(!todo.isCompleted())).build();
			icb.bindTemplate("destroy").as(VisualButton.class).onClick(v -> todoController.removeTodo(todo)).build();

			VisualComponent component= icb.mainComponent();
			icb.style(component).with("completed").when(todo::isCompleted);
			icb.style(component).with("editing").when(() -> todo == todoController.editedTodo);

		});

		createFooterPanel(componentBuilder);
	}

	private void createFooterPanel(ComponentBuilder<TodoController> mainComponentBuilder)
	{
		ComponentBuilder<TodoController> footerBuilder= mainComponentBuilder.bindTemplate("footer-section").as(VisualPanel.class).builderFromHere();
		footerBuilder.show(footerBuilder.mainComponent()).when(() -> !todoController.getTodos().isEmpty());
		footerBuilder.bindTemplate("items-count").as(VisualLabel.class).withProperty(TodoController::getRemainingCount, TodoController::setRemainingCount).build();
		footerBuilder.bindTemplate("items-label").as(VisualLabel.class).with(() -> todoController.getRemainingCount() == 1 ? "item" : "items").build();

		Stream.of("/", "/active", "/completed").forEach(location -> {
			VisualComponent component= footerBuilder.bindTemplate("filter:" + location).as(VisualLink.class).onClick(v -> todoController.setLocation(location)).build();
			footerBuilder.style(component).with("selected").when(() -> todoController.getLocation().equals(location));
		});

		ComponentBuilder<TodoController> clearCompletedBuilder= footerBuilder.bindTemplate("clear-completed").as(VisualPanel.class).onClick(v -> todoController.clearCompletedTodos()).builderFromHere();
		clearCompletedBuilder.bindTemplate("clear-completed-number").as(VisualLabel.class).withProperty(TodoController::getCompletedCount, TodoController::setCompletedCount).build();

		clearCompletedBuilder.show(clearCompletedBuilder.mainComponent()).when(() -> todoController.getCompletedCount() > 0);
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

