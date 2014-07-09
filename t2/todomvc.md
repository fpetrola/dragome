``` Java
public class TodosPage extends DragomeVisualActivity
{
	TodoController todoController= new TodoController("/", new LocalStorage());

	public void build()
	{
		ComponentBuilder<TodoController> componentBuilder= new ComponentBuilder<TodoController>(mainPanel, todoController);
		componentBuilder.bindTemplate("new-todo").toComponent(VisualTextField.class).withModel(TodoController::getNewTodo, TodoController::setNewTodo).addKeyListener(this::addTodo).build();

		ComponentBuilder<TodoController> mainSectionBuilder= componentBuilder.bindTemplate("main-section").toComponent(VisualPanel.class).builderFromHere();
		VisualComponent allChecked= mainSectionBuilder.bindTemplate("toggle-all").toComponent(VisualCheckbox.class).withModel(TodoController::isAllChecked, TodoController::setAllChecked).build().component();
		mainSectionBuilder.show(allChecked).when(() -> !todoController.todos.isEmpty());
		mainSectionBuilder.show(mainSectionBuilder.mainComponent()).when(() -> !todoController.getTodos().isEmpty());

		mainSectionBuilder.bindTemplate("completed-todo").toComponent(VisualPanel.class).withListModel(TodoController::getTodos).filter(TodoController::getStatusFilter).repeatUsing((Todo todo, ComponentBuilder<Todo> icb) -> {

			icb.bindTemplate("todo-input").toComponent(VisualTextField.class).withModel(Todo::getTitle, Todo::setTitle).addKeyListener(code -> editing(code, todo)).build();
			icb.bindTemplate("title").toComponent(VisualLabel.class).withModel(Todo::getTitle, Todo::setTitle).addDoubleClickListener(v -> todoController.editTodo(todo)).build();
			icb.bindTemplate("completed").toComponent(VisualCheckbox.class).withModel(Todo::isCompleted, Todo::setCompleted).addClickListener(v -> todo.setCompleted(!todo.isCompleted())).build();
			icb.bindTemplate("destroy").toComponent(VisualButton.class).addClickListener(v -> todoController.removeTodo(todo)).build();

			VisualComponent component= icb.mainComponent();
			icb.style(component).with("completed").when(todo::isCompleted);
			icb.style(component).with("editing").when(() -> todo == todoController.editedTodo);

		});

		createFooterPanel(componentBuilder);
	}

	private void createFooterPanel(ComponentBuilder<TodoController> mainComponentBuilder)
	{
		ComponentBuilder<TodoController> footerBuilder= mainComponentBuilder.bindTemplate("footer-section").toComponent(VisualPanel.class).builderFromHere();
		footerBuilder.show(footerBuilder.mainComponent()).when(() -> !todoController.getTodos().isEmpty());
		footerBuilder.bindTemplate("items-count").toComponent(VisualLabel.class).withModel(TodoController::getRemainingCount, TodoController::setRemainingCount).build();
		footerBuilder.bindTemplate("items-label").toComponent(VisualLabel.class).with(() -> todoController.getRemainingCount() == 1 ? "item" : "items").build();

		Stream.of("/", "/active", "/completed").forEach(location -> {
			VisualComponent component= footerBuilder.bindTemplate("filter:" + location).toComponent(VisualLink.class).addClickListener(v -> todoController.setLocation(location)).build().component();
			footerBuilder.style(component).with("selected").when(() -> todoController.getLocation().equals(location));
		});

		ComponentBuilder<TodoController> clearCompletedBuilder= footerBuilder.bindTemplate("clear-completed").toComponent(VisualPanel.class).addClickListener(v -> todoController.clearCompletedTodos()).builderFromHere();
		clearCompletedBuilder.bindTemplate("clear-completed-number").toComponent(VisualLabel.class).withModel(TodoController::getCompletedCount, TodoController::setCompletedCount).build();

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
}```

