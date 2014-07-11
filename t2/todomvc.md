``` Java
public class TodosPage extends DragomeVisualActivity
{
	public void build()
	{
		ComponentBuilder<TodoManager> componentBuilder= new ComponentBuilder<TodoManager>(mainPanel, new TodoManagerImpl("/", new LocalStorage()));
		TodoManager todoManager= componentBuilder.getModel();

		componentBuilder.bindTemplate("new-todo").as(VisualTextField.class).toProperty(TodoManager::getNewTodo, TodoManager::setNewTodo).onKeyUp(() -> todoManager.addTodo(), KEY_ENTER).build();
		ComponentBuilder<TodoManager> mainSectionBuilder= componentBuilder.bindTemplate("main-section").as(VisualPanel.class).builderFromHere();
		VisualCheckbox allChecked= mainSectionBuilder.bindTemplate("toggle-all").as(VisualCheckbox.class).toProperty(TodoManager::isAllChecked, TodoManager::setAllChecked).onClick(v -> todoManager.markAll(!todoManager.isAllChecked())).build();
		mainSectionBuilder.show(allChecked).when(() -> !todoManager.getTodos().isEmpty());
		mainSectionBuilder.show(mainSectionBuilder.panel()).when(() -> !todoManager.getTodos().isEmpty());

		mainSectionBuilder.bindTemplate("completed-todo").as(VisualPanel.class).toListProperty(TodoManager::getTodos).filter(TodoManager::getStatusFilter).repeat((Todo todo, ComponentBuilder<Todo> icb) -> {
			icb.bindTemplate("todo-input").as(VisualTextField.class).toProperty(Todo::getTitle, Todo::setTitle).onKeyUp(() -> todoManager.doneEditing(todo), KEY_ESC, KEY_ENTER).onBlur(v -> todoManager.doneEditing(todo)).build();
			icb.bindTemplate("title").as(VisualLabel.class).toProperty(Todo::getTitle, Todo::setTitle).onDoubleClick(v -> todoManager.editTodo(todo)).build();
			icb.bindTemplate("completed").as(VisualCheckbox.class).toProperty(Todo::isCompleted, Todo::setCompleted).build();
			icb.bindTemplate("destroy").as(VisualButton.class).onClick(v -> todoManager.removeTodo(todo)).build();
			icb.styleWith("completed").when(todo::isCompleted);
			icb.styleWith("editing").when(() -> todo == todoManager.getEditedTodo());
		});

		ComponentBuilder<TodoManager> footerBuilder= componentBuilder.bindTemplate("footer-section").as(VisualPanel.class).builderFromHere();
		footerBuilder.showWhen(() -> !todoManager.getTodos().isEmpty());
		footerBuilder.bindTemplate("items-count").as(VisualLabel.class).toProperty(TodoManager::getRemainingCount, TodoManager::setRemainingCount).build();
		footerBuilder.bindTemplate("items-label").as(VisualLabel.class).to(() -> todoManager.getRemainingCount() == 1 ? "item" : "items").build();

		Stream.of("/", "/active", "/completed").forEach(location -> {
			VisualLink link= footerBuilder.bindTemplate("filter:" + location).as(VisualLink.class).onClick(v1 -> todoManager.setLocation(location)).build();
			footerBuilder.style(link).with("selected").when(() -> todoManager.getLocation().equals(location));
		});

		ComponentBuilder<TodoManager> clearCompletedBuilder= footerBuilder.bindTemplate("clear-completed").as(VisualPanel.class).onClick(v2 -> todoManager.clearCompletedTodos()).builderFromHere();
		clearCompletedBuilder.bindTemplate("clear-completed-number").as(VisualLabel.class).toProperty(TodoManager::getCompletedCount, TodoManager::setCompletedCount).build();
		clearCompletedBuilder.showWhen(() -> todoManager.getCompletedCount() > 0);
	}
}
```
