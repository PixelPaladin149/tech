# tech
sprinttech
import json
import os

class Task:
    def __init__(self, title, description, due_date):
        self.title = title
        self.description = description
        self.due_date = due_date
        self.completed = False

    def __str__(self):
        status = "Completed" if self.completed else "Not completed"
        return f"Title: {self.title}\nDescription: {self.description}\nDue date: {self.due_date}\nStatus: {status}"

class TodoList:
    def __init__(self, filename="tasks.json"):
        self.filename = filename
        self.tasks = []
        self.load_tasks()

    def load_tasks(self):
        if os.path.exists(self.filename):
            with open(self.filename, "r") as file:
                tasks_data = json.load(file)
                self.tasks = [Task(**data) for data in tasks_data]
        else:
            self.tasks = []

    def save_tasks(self):
        with open(self.filename, "w") as file:
            json.dump([task.__dict__ for task in self.tasks], file, indent=4)

    def add_task(self, title, description, due_date):
        new_task = Task(title, description, due_date)
        self.tasks.append(new_task)
        self.save_tasks()

    def remove_task(self, title):
        self.tasks = [task for task in self.tasks if task.title != title]
        self.save_tasks()

    def edit_task(self, old_title, new_title, new_description, new_due_date):
        for task in self.tasks:
            if task.title == old_title:
                task.title = new_title
                task.description = new_description
                task.due_date = new_due_date
                self.save_tasks()
                break

    def mark_task_completed(self, title):
        for task in self.tasks:
            if task.title == title:
                task.completed = True
                self.save_tasks()
                break

    def display_tasks(self):
        if not self.tasks:
            print("No tasks found.")
        else:
            for task in self.tasks:
                print(task)
                print("-" * 30)

def main():
    todo_list = TodoList()

    while True:
        print("\nTodo List Menu")
        print("1. Add task")
        print("2. Remove task")
        print("3. Edit task")
        print("4. Mark task as completed")
        print("5. View tasks")
        print("6. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            title = input("Enter task title: ")
            description = input("Enter task description: ")
            due_date = input("Enter due date (YYYY-MM-DD): ")
            todo_list.add_task(title, description, due_date)
        elif choice == "2":
            title = input("Enter the title of the task to remove: ")
            todo_list.remove_task(title)
        elif choice == "3":
            old_title = input("Enter the title of the task to edit: ")
            new_title = input("Enter new task title: ")
            new_description = input("Enter new task description: ")
            new_due_date = input("Enter new due date (YYYY-MM-DD): ")
            todo_list.edit_task(old_title, new_title, new_description, new_due_date)
        elif choice == "4":
            title = input("Enter the title of the task to mark as completed: ")
            todo_list.mark_task_completed(title)
        elif choice == "5":
            todo_list.display_tasks()
        elif choice == "6":
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
