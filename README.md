
student managment system using python

analyzing and improving a simple Student Management System written in Python.

## Author 
by: Idan Musama Mwaba
## Documentation

[Documentation](https://docs.google.com/document/d/1hl3EgxR3dGWw-jhmV-xwTngdU6wQyCfKPMi0wA-WaRA/edit?usp=drive_link)


## how to run
Save the Code to a File
Save the improved code into a Python file, for example, student_management_system.py.## Original code
class Student:
    def __init__(self, id, name, age, major):
        self.id = id
        self.name = name
        self.age = age
        self.major = major

    def update_student(self, name=None, age=None, major=None):
        if name:
            self.name = name
        if age:
            self.age = age
        if major:
            self.major = major

    def display_student(self):
        print(f"ID: {self.id}, Name: {self.name}, Age: {self.age}, Major: {self.major}")

class StudentDatabase:
    def __init__(self):
        self.students = []

    def add_student(self, student):
        self.students.append(student)

    def remove_student(self, student_id):
        for student in self.students:
            if student.id == student_id:
                self.students.remove(student)
                break

    def display_all_students(self):
        for student in self.students:
            student.display_student()

class StudentManagementSystem:
    def __init__(self):
        self.database = StudentDatabase()

    def add_new_student(self, id, name, age, major):
        student = Student(id, name, age, major)
        self.database.add_student(student)

    def delete_student(self, student_id):
        self.database.remove_student(student_id)

    def update_student_info(self, student_id, name=None, age=None, major=None):
        for student in self.database.students:
            if student.id == student_id:
                student.update_student(name, age, major)

    def show_all_students(self):
        self.database.display_all_students()

# Example Usage
system = StudentManagementSystem()
system.add_new_student(1, "John Doe", 20, "Computer Science")
system.add_new_student(2, "Jane Smith", 22, "Mathematics")
system.show_all_students()
system.update_student_info(1, name="Johnathan Doe")
system.show_all_students()
system.delete_student(2)
system.show_all_students()## Refactored code
class Student:
    def __init__(self, id, name, age, major):
        self.id = id
        self.name = name
        self.age = age
        self.major = major

class StudentInfo:
    def __init__(self, student):
        self.student = student

    def display(self):
        print(f"ID: {self.student.id}, Name: {self.student.name}, Age: {self.student.age}, Major: {self.student.major}")

class StudentDatabase:
    def __init__(self):
        self.students = []

    def add_student(self, student):
        if any(s.id == student.id for s in self.students):
            print(f"Student with ID {student.id} already exists.")
        else:
            self.students.append(student)
            print(f"Student with ID {student.id} added successfully.")

    def remove_student(self, student_id):
        for student in self.students:
            if student.id == student_id:
                self.students.remove(student)
                print(f"Student with ID {student_id} removed successfully.")
                return
        print(f"No student found with ID {student_id}.")

    def display_all_students(self):
        if not self.students:
            print("No students to display.")
        else:
            for student in self.students:
                StudentInfo(student).display()

class StudentManagementSystem:
    def __init__(self):
        self.database = StudentDatabase()

    def add_new_student(self, id, name, age, major):
        student = Student(id, name, age, major)
        self.database.add_student(student)

    def delete_student(self, student_id):
        self.database.remove_student(student_id)

    def update_student_info(self, student_id, name=None, age=None, major=None):
        for student in self.database.students:
            if student.id == student_id:
                if name is not None:
                    student.name = name
                if age is not None:
                    student.age = age
                if major is not None:
                    student.major = major
                print(f"Student with ID {student_id} updated successfully.")
                return
        print(f"No student found with ID {student_id}.")

    def show_all_students(self):
        self.database.display_all_students()

    def get_validated_input(self, prompt, data_type=str, allow_empty=False):
        while True:
            try:
                value = input(prompt).strip()
                if not allow_empty and not value:
                    raise ValueError("Input cannot be empty.")
                if data_type is not str:
                    value = data_type(value)
                return value
            except ValueError as e:
                print(f"Invalid input. {e}")

    def display_menu(self):
        print("\nStudent Management System")
        print("1. Add Student")
        print("2. Delete Student")
        print("3. Update Student Information")
        print("4. View All Students")
        print("5. Exit")

    def run(self):
        while True:
            self.display_menu()
            choice = self.get_validated_input("Enter your choice (1-5): ", int)
            if choice == 1:
                id = self.get_validated_input("Enter student ID: ", int)
                name = self.get_validated_input("Enter student name: ", str)
                age = self.get_validated_input("Enter student age: ", int)
                major = self.get_validated_input("Enter student major: ", str)
                self.add_new_student(id, name, age, major)
            elif choice == 2:
                student_id = self.get_validated_input("Enter student ID to delete: ", int)
                self.delete_student(student_id)
            elif choice == 3:
                student_id = self.get_validated_input("Enter student ID to update: ", int)
                name = self.get_validated_input("Enter new name (leave blank to keep current): ", str, allow_empty=True)
                age = self.get_validated_input("Enter new age (leave blank to keep current): ", int, allow_empty=True)
                major = self.get_validated_input("Enter new major (leave blank to keep current): ", str, allow_empty=True)
                self.update_student_info(student_id, name=name or None, age=age if age else None, major=major or None)
            elif choice == 4:
                self.show_all_students()
            elif choice == 5:
                print("Exiting...")
                break
            else:
                print("Invalid choice. Please select a number between 1 and 5.")

# Example Usage
if __name__ == "__main__":
    system = StudentManagementSystem()
    system.run()

##  Changes Made
1. Code Organization and Separation of Concerns
Version 1:
The Student class is responsible for student data and some behavior (like updating).
The StudentDatabase class handles storage and manipulation of students.
The StudentManagementSystem class manages user interactions with the system.
Version 2:
Introduces the StudentInfo class, which is responsible for displaying student information. This separates the display logic from the Student class, which now only holds data.
This separation of concerns enhances code clarity and maintainability.
2. Input Validation and User Interaction
Version 1:
There is no built-in input validation or user interaction beyond basic method calls and print statements.
Version 2:
Introduces get_validated_input for user input validation. This function handles various data types and ensures inputs are correct before proceeding.
Adds a display_menu method and a run method to manage user interaction via a command-line menu, making the system more user-friendly and interactive.
3. Error Handling and Feedback
Version 1:
Basic functionality without explicit user feedback on operations like adding, updating, or removing students.
Version 2:
Provides feedback messages for actions such as adding, removing, and updating students. This improves user experience by giving clear indications of the success or failure of their actions.
Handles cases where students are not found for removal or updating, informing the user appropriately.
4. Student Update Logic
Version 1:
The update_student method in the Student class updates student attributes based on provided parameters, but does not provide feedback on success or failure.
Version 2:
The update_student_info method in StudentManagementSystem includes checks for None values and provides feedback if a student is updated or not found, which makes the update process more robust and user-friendly.
5. Functionality Extensions
Version 1:
Basic functionality for adding, removing, updating, and displaying students.
Version 2:
Extends functionality with the ability to handle empty inputs during updates, thus offering more flexibility and user control.
6. Code Robustness and Maintainability
Version 1:
Straightforward and functional but less modular and flexible.
Version 2:
Modularized into distinct classes and methods, improving maintainability and scalability. Adding new features or modifying existing ones becomes easier due to the separation of concerns.
7. Feedback Messages
Version 1:
Does not include feedback messages for user actions.
Version 2:
Includes informative messages for various actions (e.g., student added, removed, updated), which helps in tracking the system’s state and improves the user experience.
8. Code Execution Entry Point
Version 1:
Executes commands through direct method calls.
Version 2:
Uses a run method with a command-line interface for better interaction, making it more suitable for real-world usage and testing.