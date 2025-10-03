# Smart-Student-Management-System-SSMS

Smart Student Management System (SSMS)
The Smart Student Management System (SSMS) is a comprehensive Java console application designed to manage student, course, and academic data. This project was specifically architected to demonstrate a wide array of Java concepts, ranging from fundamental syntax and OOP principles to advanced features like Generics, Concurrency, Annotations, and File I/O Streams.

It provides a modular, multi-package structure typical of real-world enterprise applications.

✨ Key Features
Student Management: Register and search students with validation checks.

Course Enrollment: Enroll students in predefined courses.

Grading: Add marks, calculate GPA, and assign Enum-based grades (A, B, C, D, F).

Persistence: Student data is automatically saved and loaded from a local students.csv file using robust I/O Streams.

Concurrency: A Daemon Thread handles auto-saving in the background every 10 seconds.

Data Structures: Uses HashMap, ArrayList, HashSet, and a Generic Repository for efficient data handling.

Utilities: Includes mathematical helpers like Factorial and Fibonacci using Recursion.

💡 Java Roadmap Concepts Demonstrated
This project integrates every concept from the provided Java tutorial roadmap:

Category

Concepts Implemented

Object-Oriented Programming (OOP)

Inheritance (Student, Teacher extend User), Abstraction (User abstract class), Interface (Payable), Polymorphism (getSummary()), Encapsulation (private fields, public accessors/mutators).

Data Handling & Structures

Generics (GenericRepository<T, K>, Utils.topN), Collections (List, Map, Set), HashMap, ArrayList, HashSet.

Advanced Features

Annotations (@Author), Threads (AutoSaveThread), Lambda Expressions (used for sorting and stream filtering), Advanced Sorting (Stream.sorted() and custom Comparator via Lambda).

I/O & Persistence

File Handling, I/O Streams (FileReader, FileWriter, BufferedReader, BufferedWriter), Exceptions (custom ValidationException, NotFoundException, standard IOException), try-with-resources.

Core Logic

Recursion (Utils.factorial, Utils.fibonacci), Enum (Role, Grade), Regex (Validator for email/ID format), Type Casting, Date.

📁 Project Package Layout
To run the application, ensure your source files (.java) are placed within the following nested package directories under your project's src folder:

src/
├── com/ssms/
    ├── app/                # Main execution logic
    ├── entities/           # Core model classes (User, Student, Course)
    ├── enums/              # Enum definitions (Role, Grade)
    ├── interfaces/         # Interface contracts (Payable)
    ├── repo/               # Repository layer (GenericRepository, StudentRepository)
    ├── services/           # Business logic layer (StudentService)
    ├── persistence/        # File management and Concurrency (FileManager, AutoSaveThread)
    ├── utils/              # Helper classes (Validator, Utils - Recursion demo)
    ├── exceptions/         # Custom exception classes
    └── annotations/        # Custom annotation definition

🚀 Setup and Running
This is a standard Java console application and requires Java 11 or newer to execute correctly (due to the extensive use of Streams and Lambdas).

1. Compilation
Compile all .java files from your project root or IDE (e.g., IntelliJ, Eclipse, VS Code).

2. Execution
Run the main class from the console:

java com.ssms.app.Main

3. Usage
The application will immediately start the AutoSaveThread in the background and load any existing data from students.csv. You will be presented with a simple command-line menu to interact with the system.

Enter 0 to gracefully stop the auto-save thread and exit the application after a final save.
