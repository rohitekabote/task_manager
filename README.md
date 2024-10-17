# Task Manager CLI Application

## Description
A simple command-line interface (CLI) task manager application where users can add, view, delete, and complete tasks. Tasks are saved and loaded from a JSON file (`tasks.json`).

## Features
- **Add a Task**: Allows you to add a new task to the task list.
- **View Tasks**: Displays all tasks, showing their status (completed or not).
- **Delete a Task**: Removes a task by its ID.
- **Mark a Task as Completed**: Updates the task status to completed.
- **Save and Load Tasks**: Saves tasks to and loads tasks from `tasks.json` file.

## Requirements
- Python 3.x installed on your system.

## Installation
1. Creating a new directory for the project, e.g., task_manager
2. Inside the directory, creating a Python file named task_manager.py
3. Open the project folder in your terminal or VS Code.


## Running the Application
To run the application, use the following command in your terminal:
```bash
python task_manager.py
```

## Usage
Once the application is running, you will see a menu with the following options:
1. Add Task
2. View Tasks
3. Delete Task
4. Complete Task
5. Save & Exit

Follow the on-screen prompts to manage your tasks effectively.

## Notes
- The application saves tasks in a file called `tasks.json` in the same directory. Ensure you have write permissions in this folder.
- If no tasks are found, the application will inform you accordingly.

## Author
[Rohit Ekabote]  
[Contact: 8197811904]

## CODE 
import json
import os

# Define Task structure
class Task:
    def __init__(self, task_id, title, completed=False):
        self.id = task_id
        self.title = title
        self.completed = completed

    def to_dict(self):
        return {'id': self.id, 'title': self.title, 'completed': self.completed}

    @classmethod
    def from_dict(cls, task_dict):
        return cls(task_dict['id'], task_dict['title'], task_dict['completed'])


# Initialize an empty task list
tasks = []
next_task_id = 1

# Function to Add Task
def add_task(title):
    global next_task_id
    task = Task(next_task_id, title)
    tasks.append(task)
    next_task_id += 1
    print(f"Task '{title}' added successfully.")

# Function to View Tasks
def view_tasks():
    if not tasks:
        print("No tasks available.")
    else:
        for task in tasks:
            status = "Completed" if task.completed else "Incomplete"
            print(f"[{task.id}] {task.title} - {status}")

# Function to Delete Task
def delete_task(task_id):
    global tasks
    tasks = [task for task in tasks if task.id != task_id]
    print(f"Task {task_id} deleted successfully.")

# Function to Mark Task as Complete
def complete_task(task_id):
    for task in tasks:
        if task.id == task_id:
            task.completed = True
            print(f"Task {task_id} marked as complete.")
            return
    print(f"Task {task_id} not found.")

# Function to Save Tasks to JSON file
def save_tasks(filename='tasks.json'):
    with open(filename, 'w') as file:
        json.dump([task.to_dict() for task in tasks], file)
    print("Tasks saved to file.")

# Function to Load Tasks from JSON file
def load_tasks(filename='tasks.json'):
    global tasks, next_task_id
    if os.path.exists(filename):
        with open(filename, 'r') as file:
            tasks_data = json.load(file)
            tasks = [Task.from_dict(task) for task in tasks_data]
            if tasks:
                next_task_id = max(task.id for task in tasks) + 1
    print("Tasks loaded from file.")

# Login Credentials 
def login():
    print("Please log in to access the Task Manager.")
    
    email = input("Enter your email: ").strip()
    password = input("Enter your password: ").strip()
    
    if email == DUMMY_EMAIL and password == DUMMY_PASSWORD:
        print("Login successful!\n")
        return True
    else:
        print("Invalid email or password. Please try again.\n")
        return False

# Command Line Interface (CLI) for Task Management
def main():
    # User must log in first
    logged_in = False
    while not logged_in:
        logged_in = login()
    
    load_tasks()
    
    while True:
        print("\nTask Manager CLI")
        print("1. Add Task")
        print("2. View Tasks")
        print("3. Delete Task")
        print("4. Mark Task as Complete")
        print("5. Save & Exit")
        
        choice = input("Choose an option: ").strip()

        if choice == '1':
            title = input("Enter task title: ").strip()
            add_task(title)
        elif choice == '2':
            view_tasks()
        elif choice == '3':
            task_id = int(input("Enter task ID to delete: ").strip())
            delete_task(task_id)
        elif choice == '4':
            task_id = int(input("Enter task ID to mark complete: ").strip())
            complete_task(task_id)
        elif choice == '5':
            save_tasks()
            break
        else:
            print("Invalid option. Please try again.")

if __name__ == "__main__":
    main()
