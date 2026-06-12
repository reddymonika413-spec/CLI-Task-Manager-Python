import json
import os

FILE = "tasks.json"


def load_tasks():
    if os.path.exists(FILE):
        with open(FILE, "r") as f:
            return json.load(f)
    return []


def save_tasks(tasks):
    with open(FILE, "w") as f:
        json.dump(tasks, f, indent=4)


def show_tasks(tasks):
    if not tasks:
        print("\nNo tasks available.")
        return

    print("\nYour Tasks:")
    for i, task in enumerate(tasks, start=1):
        status = "✓" if task["done"] else " "
        print(f"{i}. [{status}] {task['task']}")


def add_task(tasks):
    name = input("\nEnter task: ")

    tasks.append({
        "task": name,
        "done": False
    })

    save_tasks(tasks)
    print("Task added successfully!")


def complete_task(tasks):
    show_tasks(tasks)

    try:
        num = int(input("\nEnter task number to complete: "))

        tasks[num - 1]["done"] = True
        save_tasks(tasks)

        print("Task completed!")

    except:
        print("Invalid task number.")


def delete_task(tasks):
    show_tasks(tasks)

    try:
        num = int(input("\nEnter task number to delete: "))

        removed = tasks.pop(num - 1)
        save_tasks(tasks)

        print(f"Deleted: {removed['task']}")

    except:
        print("Invalid task number.")


def menu():
    tasks = load_tasks()

    while True:

        print("\n====== TASK MANAGER ======")
        print("1. Add Task")
        print("2. View Tasks")
        print("3. Complete Task")
        print("4. Delete Task")
        print("5. Exit")

        choice = input("\nChoose option: ")

        if choice == "1":
            add_task(tasks)

        elif choice == "2":
            show_tasks(tasks)

        elif choice == "3":
            complete_task(tasks)

        elif choice == "4":
            delete_task(tasks)

        elif choice == "5":
            print("Goodbye!")
            break

        else:
            print("Invalid choice!")


menu()