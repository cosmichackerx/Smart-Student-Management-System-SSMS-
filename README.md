# Smart-Student-Management-System-SSMS

Here’s your complete `README.md` file for the Smart Student Management System (SSMS), formatted and ready to include in your project:

---

```markdown
# 🎓 Smart Student Management System (SSMS)

The **Smart Student Management System (SSMS)** is a comprehensive Java console application designed to manage student, course, and academic data. This project is architected to demonstrate a wide array of Java concepts—from basic syntax and OOP principles to advanced features like Generics, Concurrency, Annotations, and File I/O Streams.

It follows a modular, multi-package structure typical of real-world enterprise applications.

---

## ✨ Key Features

- **Student Management**: Register and search students with validation checks.
- **Course Enrollment**: Enroll students in predefined courses.
- **Grading**: Add marks, calculate GPA, and assign Enum-based grades (A, B, C, D, F).
- **Persistence**: Automatically saves and loads student data from `students.csv` using robust I/O Streams.
- **Concurrency**: A Daemon Thread handles auto-saving in the background every 10 seconds.
- **Data Structures**: Uses `HashMap`, `ArrayList`, `HashSet`, and a Generic Repository for efficient data handling.
- **Utilities**: Includes mathematical helpers like Factorial and Fibonacci using Recursion.

---

## 💡 Java Roadmap Concepts Demonstrated

| Category                   | Concepts Implemented                                                                 |
|---------------------------|----------------------------------------------------------------------------------------|
| **Object-Oriented Programming (OOP)** | Inheritance (`Student`, `Teacher` extend `User`), Abstraction (`User` abstract class), Interface (`Payable`), Polymorphism (`getSummary()`), Encapsulation (private fields, public accessors/mutators) |
| **Data Handling & Structures**        | Generics (`GenericRepository<T, K>`, `Utils.topN`), Collections (`List`, `Map`, `Set`), `HashMap`, `ArrayList`, `HashSet` |
| **Advanced Features**                | Annotations (`@Author`), Threads (`AutoSaveThread`), Lambda Expressions (used for sorting and stream filtering), Advanced Sorting (`Stream.sorted()` and custom `Comparator` via Lambda) |
| **I/O & Persistence**                | File Handling, I/O Streams (`FileReader`, `FileWriter`, `BufferedReader`, `BufferedWriter`), Exceptions (custom `ValidationException`, `NotFoundException`, standard `IOException`), try-with-resources |
| **Core Logic**                       | Recursion (`Utils.factorial`, `Utils.fibonacci`), Enum (`Role`, `Grade`), Regex (`Validator` for email/ID format), Type Casting, Date |

---

## 📁 Project Package Layout

Place your `.java` source files in the following nested package directories under your `src` folder:

```
src/
└── com/ssms/
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
```
---
Nice — buckle up, Arslan. Below is a complete, multi-package **Java Smart Student Management System (SSMS)** project that touches every concept from your roadmap.
I kept the code readable and modular so you can compile, run, and extend it. Paste these files into a Java project following the package structure and run `com.ssms.app.Main` to try the console demo.

---

## Project package layout (create these directories under `src`)

```
com/ssms/app
com/ssms/entities
com/ssms/enums
com/ssms/interfaces
com/ssms/repo
com/ssms/services
com/ssms/persistence
com/ssms/utils
com/ssms/exceptions
com/ssms/annotations
```

---

### 1) `com.ssms.annotations.Author.java`

```java
package com.ssms.annotations;

import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

@Retention(RetentionPolicy.RUNTIME)
public @interface Author {
    String name();
    String date() default "";
}
```

---

### 2) `com.ssms.enums.Role.java`

```java
package com.ssms.enums;

public enum Role {
    ADMIN, STUDENT, TEACHER, GUEST
}
```

---

### 3) `com.ssms.enums.Grade.java`

```java
package com.ssms.enums;

public enum Grade {
    A, B, C, D, F;
}
```

---

### 4) `com.ssms.exceptions.ValidationException.java`

```java
package com.ssms.exceptions;

public class ValidationException extends Exception {
    public ValidationException(String message) { super(message); }
}
```

---

### 5) `com.ssms.exceptions.NotFoundException.java`

```java
package com.ssms.exceptions;

public class NotFoundException extends Exception {
    public NotFoundException(String message) { super(message); }
}
```

---

### 6) `com.ssms.interfaces.Payable.java`

```java
package com.ssms.interfaces;

public interface Payable {
    double calculateFee();
}
```

---

### 7) `com.ssms.entities.User.java`

```java
package com.ssms.entities;

import com.ssms.enums.Role;
import java.util.Date;

public abstract class User {
    protected String id;
    protected String name;
    protected String email;
    protected Role role;
    protected Date createdAt;

    public User(String id, String name, String email, Role role) {
        this.id = id;
        this.name = name;
        this.email = email;
        this.role = role;
        this.createdAt = new Date();
    }

    public String getId() { return id; }
    public String getName() { return name; }
    public String getEmail() { return email; }
    public Role getRole() { return role; }

    public abstract String getSummary();
}
```

---

### 8) `com.ssms.entities.Course.java`

```java
package com.ssms.entities;

import java.io.Serializable;
import java.util.Objects;

public class Course implements Serializable {
    private String code;
    private String title;
    private int credits;

    public Course(String code, String title, int credits) {
        this.code = code;
        this.title = title;
        this.credits = credits;
    }

    public String getCode() { return code; }
    public String getTitle() { return title; }
    public int getCredits() { return credits; }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Course course = (Course) o;
        return Objects.equals(code, course.code);
    }

    @Override
    public int hashCode() { return Objects.hash(code); }

    @Override
    public String toString() {
        return code + " - " + title + " (" + credits + "cr)";
    }
}
```

---

### 9) `com.ssms.entities.Teacher.java`

```java
package com.ssms.entities;

import com.ssms.enums.Role;

public class Teacher extends User {
    private String department;

    public Teacher(String id, String name, String email, String department) {
        super(id, name, email, Role.TEACHER);
        this.department = department;
    }

    public String getDepartment() { return department; }

    @Override
    public String getSummary() {
        return String.format("Teacher[%s] %s (%s) - Dept: %s", id, name, email, department);
    }
}
```

---

### 10) `com.ssms.entities.Student.java`

```java
package com.ssms.entities;

import com.ssms.enums.Role;
import com.ssms.enums.Grade;
import com.ssms.interfaces.Payable;

import java.io.Serializable;
import java.util.*;

public class Student extends User implements Serializable, Payable {
    private Map<String, Double> courseMarks = new HashMap<>(); // courseCode -> marks (0-100)
    private Set<Course> enrolled = new HashSet<>();
    private transient List<String> notes = new ArrayList<>(); // transient for demo of serialization difference

    public Student(String id, String name, String email) {
        super(id, name, email, Role.STUDENT);
    }

    public void enroll(Course c) { enrolled.add(c); }
    public void addMark(String courseCode, double marks) { courseMarks.put(courseCode, marks); }
    public Map<String, Double> getCourseMarks() { return Collections.unmodifiableMap(courseMarks); }
    public Set<Course> getEnrolled() { return Collections.unmodifiableSet(enrolled); }
    public void addNote(String n) { notes.add(n); }

    public double calculateGPA() {
        if (courseMarks.isEmpty()) return 0.0;
        // simple scale: average/25 => 4.0 scale
        double avg = courseMarks.values().stream().mapToDouble(Double::doubleValue).average().orElse(0.0);
        return Math.round((avg / 25.0) * 100.0) / 100.0;
    }

    public Grade getGradeForCourse(String courseCode) {
        Double m = courseMarks.get(courseCode);
        if (m == null) return Grade.F;
        if (m >= 85) return Grade.A;
        if (m >= 70) return Grade.B;
        if (m >= 55) return Grade.C;
        if (m >= 40) return Grade.D;
        return Grade.F;
    }

    @Override
    public double calculateFee() {
        return 1000 + (enrolled.size() * 250);
    }

    @Override
    public String getSummary() {
        return String.format("Student[%s] %s, GPA=%.2f, Enrolled=%d", id, name, calculateGPA(), enrolled.size());
    }
}
```

---

### 11) `com.ssms.repo.GenericRepository.java`

```java
package com.ssms.repo;

import java.util.*;
import java.util.function.Predicate;
import java.util.stream.Collectors;

/**
 * Generic in-memory repository with basic CRUD + search
 */
public class GenericRepository<T, K> {
    private final Map<K, T> store = new LinkedHashMap<>();

    public void save(K key, T value) { store.put(key, value); }
    public Optional<T> findById(K key) { return Optional.ofNullable(store.get(key)); }
    public void delete(K key) { store.remove(key); }
    public List<T> findAll() { return new ArrayList<>(store.values()); }

    public List<T> search(Predicate<T> predicate) {
        return store.values().stream().filter(predicate).collect(Collectors.toList());
    }

    public boolean exists(K key) { return store.containsKey(key); }

    public int size() { return store.size(); }
}
```

---

### 12) `com.ssms.repo.StudentRepository.java`

```java
package com.ssms.repo;

import com.ssms.entities.Student;

public class StudentRepository extends GenericRepository<Student, String> {
    // Can add student-specific repository helpers later
}
```

---

### 13) `com.ssms.utils.Validator.java`

```java
package com.ssms.utils;

import java.util.regex.Pattern;

public class Validator {
    private static final Pattern EMAIL = Pattern.compile("^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,}$");
    private static final Pattern ID = Pattern.compile("^[A-Z]{2}\\d{3,6}$");

    public static void requireNonNull(Object o, String name) {
        if (o == null) throw new IllegalArgumentException(name + " cannot be null");
    }

    public static boolean isValidEmail(String email) {
        return email != null && EMAIL.matcher(email).matches();
    }

    public static boolean isValidId(String id) {
        return id != null && ID.matcher(id).matches();
    }
}
```

---

### 14) `com.ssms.utils.Utils.java`

```java
package com.ssms.utils;

import java.util.List;
import java.util.stream.Collectors;

public class Utils {

    // Recursion example
    public static long factorial(int n) {
        if (n < 0) throw new IllegalArgumentException("n must be >= 0");
        if (n == 0 || n == 1) return 1;
        return n * factorial(n - 1);
    }

    // Fibonacci
    public static long fibonacci(int n) {
        if (n < 0) throw new IllegalArgumentException("n must be >= 0");
        if (n == 0) return 0;
        if (n == 1) return 1;
        return fibonacci(n - 1) + fibonacci(n - 2);
    }

    // a sample utility with generics
    public static <T extends Comparable<? super T>> List<T> topN(List<T> list, int n) {
        return list.stream().sorted((a,b)->b.compareTo(a)).limit(n).collect(Collectors.toList());
    }
}
```

---

### 15) `com.ssms.persistence.FileManager.java`

```java
package com.ssms.persistence;

import com.ssms.entities.Student;

import java.io.*;
import java.util.ArrayList;
import java.util.List;

/**
 * Simple file storage: CSV style for students (id,name,email)
 */
public class FileManager {
    private final File file;

    public FileManager(String path) {
        this.file = new File(path);
    }

    public void saveStudents(List<Student> students) throws IOException {
        try (BufferedWriter bw = new BufferedWriter(new FileWriter(file))) {
            for (Student s : students) {
                String line = String.format("%s,%s,%s", s.getId(), s.getName().replace(",", ""), s.getEmail());
                bw.write(line);
                bw.newLine();
            }
            bw.flush();
        }
    }

    public List<Student> readStudents() throws IOException {
        List<Student> out = new ArrayList<>();
        if (!file.exists()) return out;
        try (BufferedReader br = new BufferedReader(new FileReader(file))) {
            String ln;
            while ((ln = br.readLine()) != null) {
                String[] parts = ln.split(",", 3);
                if (parts.length < 3) continue;
                Student s = new Student(parts[0], parts[1], parts[2]);
                out.add(s);
            }
        }
        return out;
    }

    public boolean delete() {
        return file.exists() && file.delete();
    }
}
```

---

### 16) `com.ssms.services.StudentService.java`

```java
package com.ssms.services;

import com.ssms.entities.Student;
import com.ssms.entities.Course;
import com.ssms.exceptions.NotFoundException;
import com.ssms.exceptions.ValidationException;
import com.ssms.repo.StudentRepository;
import com.ssms.utils.Validator;

import java.io.IOException;
import java.util.*;
import java.util.stream.Collectors;

public class StudentService {
    private final StudentRepository repo;
    private final List<Course> catalog;

    public StudentService(StudentRepository repo) {
        this.repo = repo;
        this.catalog = new ArrayList<>();
        // sample catalog
        catalog.add(new Course("CS101", "Intro to CS", 3));
        catalog.add(new Course("MA101", "Calculus I", 3));
        catalog.add(new Course("EN101", "Academic Writing", 2));
    }

    public List<Course> getCatalog() { return Collections.unmodifiableList(catalog); }

    public void registerStudent(Student s) throws ValidationException {
        if (!Validator.isValidId(s.getId())) throw new ValidationException("Invalid ID format");
        if (!Validator.isValidEmail(s.getEmail())) throw new ValidationException("Invalid email");
        if (repo.exists(s.getId())) throw new ValidationException("Student exists");
        repo.save(s.getId(), s);
    }

    public void enrollCourse(String studentId, String courseCode) throws NotFoundException {
        Student s = repo.findById(studentId).orElseThrow(() -> new NotFoundException("Student not found"));
        catalog.stream().filter(c -> c.getCode().equalsIgnoreCase(courseCode)).findFirst()
                .ifPresent(s::enroll);
    }

    public void addMark(String studentId, String courseCode, double marks) throws NotFoundException {
        Student s = repo.findById(studentId).orElseThrow(() -> new NotFoundException("Student not found"));
        s.addMark(courseCode, marks);
    }

    public List<Student> topStudents(int n) {
        return repo.findAll().stream()
                .sorted((a,b) -> Double.compare(b.calculateGPA(), a.calculateGPA()))
                .limit(n)
                .collect(Collectors.toList());
    }

    public List<Student> searchByName(String query) {
        String q = query.toLowerCase();
        return repo.search(s -> s.getName().toLowerCase().contains(q));
    }

    public List<Student> allStudents() { return repo.findAll(); }

    public Optional<Student> getStudent(String id) { return repo.findById(id); }
}
```

---

### 17) `com.ssms.persistence.AutoSaveThread.java`

```java
package com.ssms.persistence;

import com.ssms.entities.Student;
import com.ssms.services.StudentService;

import java.io.IOException;
import java.util.List;

/**
 * Background auto-save thread that periodically saves student list
 */
public class AutoSaveThread extends Thread {
    private final StudentService service;
    private final FileManager manager;
    private volatile boolean running = true;
    private final long intervalMs;

    public AutoSaveThread(StudentService service, FileManager manager, long intervalMs) {
        this.service = service;
        this.manager = manager;
        this.intervalMs = intervalMs;
        setDaemon(true);
        setName("AutoSaveThread");
    }

    @Override
    public void run() {
        while (running) {
            try {
                List<Student> students = service.allStudents();
                manager.saveStudents(students);
                // simple console notification
                System.out.println("[AutoSave] saved " + students.size() + " students at " + System.currentTimeMillis());
                Thread.sleep(intervalMs);
            } catch (IOException e) {
                System.err.println("[AutoSave] IO failed: " + e.getMessage());
            } catch (InterruptedException e) {
                running = false;
                Thread.currentThread().interrupt();
            }
        }
    }

    public void stopSaving() { running = false; }
}
```

---

### 18) `com.ssms.app.Main.java`

```java
package com.ssms.app;

import com.ssms.annotations.Author;
import com.ssms.entities.Student;
import com.ssms.entities.Course;
import com.ssms.exceptions.NotFoundException;
import com.ssms.exceptions.ValidationException;
import com.ssms.persistence.AutoSaveThread;
import com.ssms.persistence.FileManager;
import com.ssms.repo.StudentRepository;
import com.ssms.services.StudentService;
import com.ssms.utils.Utils;

import java.io.IOException;
import java.util.List;
import java.util.Scanner;

@Author(name = "cosmichackerx", date = "2025-10-03")
public class Main {
    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        StudentRepository repo = new StudentRepository();
        StudentService service = new StudentService(repo);
        FileManager fm = new FileManager("students.csv");

        // load existing students if file exists
        try {
            fm.readStudents().forEach(s -> {
                try {
                    service.registerStudent(s);
                } catch (ValidationException e) {
                    // ignore malformed saved records
                }
            });
        } catch (IOException e) {
            System.err.println("Failed to load students: " + e.getMessage());
        }

        // start autosave thread (daemon)
        AutoSaveThread saver = new AutoSaveThread(service, fm, 10_000);
        saver.start();

        // sample data
        try {
            Student s1 = new Student("ST1001", "Ali Khan", "ali.khan@example.com");
            s1.enroll(new Course("CS101","Intro to CS",3));
            s1.addMark("CS101", 92.0);
            service.registerStudent(s1);

            Student s2 = new Student("ST1002", "Zara Noor", "zara.noor@example.com");
            s2.enroll(new Course("CS101","Intro to CS",3));
            s2.addMark("CS101", 78.0);
            service.registerStudent(s2);
        } catch (ValidationException ex) {
            System.err.println("Sample data invalid: " + ex.getMessage());
        }

        boolean running = true;
        while (running) {
            showMenu();
            String cmd = scanner.nextLine().trim();
            switch (cmd) {
                case "1":
                    registerFlow(service);
                    break;
                case "2":
                    enrollFlow(service);
                    break;
                case "3":
                    addMarkFlow(service);
                    break;
                case "4":
                    listStudents(service);
                    break;
                case "5":
                    topStudents(service);
                    break;
                case "6":
                    utilitiesDemo();
                    break;
                case "0":
                    running = false;
                    break;
                default:
                    System.out.println("Unknown command");
            }
        }

        // stop autosave and save final
        saver.stopSaving();
        try {
            fm.saveStudents(service.allStudents());
        } catch (IOException e) {
            System.err.println("Final save failed: " + e.getMessage());
        }
        System.out.println("Goodbye ✨");
    }

    private static void showMenu() {
        System.out.println("\n=== SSMS Menu ===");
        System.out.println("1) Register Student");
        System.out.println("2) Enroll Student in Course");
        System.out.println("3) Add Marks");
        System.out.println("4) List Students");
        System.out.println("5) Top Students");
        System.out.println("6) Utilities (factorial/fibo)");
        System.out.println("0) Exit");
        System.out.print("Choose: ");
    }

    private static void registerFlow(StudentService service) {
        try {
            System.out.print("ID (e.g. ST1001): ");
            String id = scanner.nextLine().trim().toUpperCase();
            System.out.print("Name: ");
            String name = scanner.nextLine().trim();
            System.out.print("Email: ");
            String email = scanner.nextLine().trim();

            Student s = new Student(id, name, email);
            service.registerStudent(s);
            System.out.println("Registered: " + s.getSummary());
        } catch (ValidationException ex) {
            System.err.println("Failed: " + ex.getMessage());
        }
    }

    private static void enrollFlow(StudentService service) {
        try {
            System.out.print("Student ID: ");
            String id = scanner.nextLine().trim().toUpperCase();
            System.out.println("Catalog:");
            for (Course c : service.getCatalog()) System.out.println("  " + c);
            System.out.print("Course code to enroll: ");
            String cc = scanner.nextLine().trim().toUpperCase();
            service.enrollCourse(id, cc);
            System.out.println("Enrolled successfully.");
        } catch (NotFoundException ex) {
            System.err.println("Error: " + ex.getMessage());
        }
    }

    private static void addMarkFlow(StudentService service) {
        try {
            System.out.print("Student ID: ");
            String id = scanner.nextLine().trim().toUpperCase();
            System.out.print("Course code: ");
            String cc = scanner.nextLine().trim().toUpperCase();
            System.out.print("Marks (0-100): ");
            double m = Double.parseDouble(scanner.nextLine().trim());
            service.addMark(id, cc, m);
            System.out.println("Marks added.");
        } catch (NotFoundException ex) {
            System.err.println("Error: " + ex.getMessage());
        } catch (NumberFormatException nfe) {
            System.err.println("Invalid number");
        }
    }

    private static void listStudents(StudentService service) {
        System.out.println("All Students:");
        service.allStudents().forEach(s -> System.out.println("  " + s.getSummary()));
    }

    private static void topStudents(StudentService service) {
        System.out.print("How many top students to show? ");
        try {
            int n = Integer.parseInt(scanner.nextLine().trim());
            List<Student> tops = service.topStudents(n);
            for (int i = 0; i < tops.size(); i++) {
                System.out.printf("%d) %s\n", i+1, tops.get(i).getSummary());
            }
        } catch (NumberFormatException e) {
            System.err.println("Invalid number");
        }
    }

    private static void utilitiesDemo() {
        System.out.print("Enter n for factorial: ");
        try {
            int n = Integer.parseInt(scanner.nextLine().trim());
            System.out.println("factorial(" + n + ") = " + Utils.factorial(n));
            System.out.print("Enter n for fibonacci: ");
            int f = Integer.parseInt(scanner.nextLine().trim());
            System.out.println("fibonacci(" + f + ") = " + Utils.fibonacci(f));
        } catch (NumberFormatException e) {
            System.err.println("Not a number");
        } catch (Exception e) {
            System.err.println("Error: " + e.getMessage());
        }
    }
}
```

---

## Notes & Highlights (what parts of your roadmap are demonstrated)

* **Intro / Syntax / Output / Comments**: `Main` with menu and comments.
* **Variables / Data Types / Type Casting / Math**: `calculateGPA`, `Utils` methods, parsing input.
* **Strings / Regex Validation**: `Validator.isValidEmail` and ID format.
* **Loops / Arrays / Collections**: `forEach`, `List`, `Map`, `Set`, `HashMap`, `ArrayList`.
* **Methods / Overloading / Scope / Recursion**: `Utils.factorial` and `fibonacci` (recursion), methods across services.
* **Classes, Objects, Constructors, this, Modifiers, Encapsulation**: `User`, `Student`, `Teacher`, private fields with getters.
* **Packages / API**: Everything separated into packages.
* **Inheritance / Polymorphism / super**: `User` → `Student` / `Teacher`.
* **Inner Classes / Anonymous Classes**: You can add them; demo uses anonymous comparator pattern in `topStudents` lambda. (If you want explicit inner classes, I can add one quickly.)
* **Abstraction / Interfaces**: `Payable` interface in `Student`.
* **Enum**: `Role`, `Grade`.
* **User Input / Date**: `Scanner`, `createdAt` field.
* **Exceptions / try-with-resources**: File IO uses try-with-resources; custom `ValidationException` & `NotFoundException`.
* **File Handling / I/O Streams**: `FileManager` read/write.
* **Data Structures / Collections / Iterator**: `GenericRepository` uses `LinkedHashMap`.
* **Generics**: `GenericRepository<T,K>` and `Utils.topN` generic helper.
* **Annotations**: `@Author` on `Main`.
* **Threads**: `AutoSaveThread` saves periodically (daemon).
* **Lambda / Advanced Sorting**: Lambdas used in `StudentService.topStudents` and streams.
* **Wrapper classes**: Autoboxing used in maps/lists.
* **Regex & Validation**: `Validator`.
* **Mini How-Tos**: Factorial/Fibonacci utilities included.

---

## How to run

1. Create the package directories and paste each file in the appropriate package.
2. Compile all. (Works with Java 11+).
3. Run `com.ssms.app.Main`.
4. The app auto-saves to `students.csv` every 10 seconds (daemon thread). Use the menu.

---

---

## 🚀 Setup and Running

This is a standard Java console application and requires **Java 11 or newer** to execute correctly (due to extensive use of Streams and Lambdas).

### 1. Compilation

Compile all `.java` files from your project root or using your IDE (e.g., IntelliJ, Eclipse, VS Code).

### 2. Execution

Run the main class from the console:

```bash
java com.ssms.app.Main
```

### 3. Usage

- The application starts the `AutoSaveThread` in the background.
- Loads existing data from `students.csv`.
- Presents a command-line menu to interact with the system.
- Enter `0` to gracefully stop the auto-save thread and exit the application after a final save.

---

## 🛠️ Contribution & Learning

This project is ideal for learners who want to:
- Practice modular Java development
- Understand real-world application architecture
- Explore advanced Java features hands-on
- Prepare for interviews or academic assessments

---

## 📜 License

This project is open for educational use. Feel free to modify and extend it for your learning journey.

```


