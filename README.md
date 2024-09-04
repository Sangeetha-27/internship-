import java.util.ArrayList;
import java.util.HashMap;
import java.util.Scanner;

class Course {
    private String courseCode;
    private String title;
    private String description;
    private int capacity;
    private String schedule;
    private ArrayList<String> registeredStudents;

    public Course(String courseCode, String title, String description, int capacity, String schedule) {
        this.courseCode = courseCode;
        this.title = title;
        this.description = description;
        this.capacity = capacity;
        this.schedule = schedule;
        this.registeredStudents = new ArrayList<>();
    }

    public String getCourseCode() {
        return courseCode;
    }

    public String getTitle() {
        return title;
    }

    public String getDescription() {
        return description;
    }

    public int getCapacity() {
        return capacity;
    }

    public String getSchedule() {
        return schedule;
    }

    public int getAvailableSlots() {
        return capacity - registeredStudents.size();
    }

    public boolean registerStudent(String studentId) {
        if (registeredStudents.size() < capacity) {
            registeredStudents.add(studentId);
            return true;
        }
        return false;
    }

    public boolean removeStudent(String studentId) {
        return registeredStudents.remove(studentId);
    }

    public ArrayList<String> getRegisteredStudents() {
        return registeredStudents;
    }
}

class Student {
    private String studentId;
    private String name;
    private ArrayList<Course> registeredCourses;

    public Student(String studentId, String name) {
        this.studentId = studentId;
        this.name = name;
        this.registeredCourses = new ArrayList<>();
    }

    public String getStudentId() {
        return studentId;
    }

    public String getName() {
        return name;
    }

    public ArrayList<Course> getRegisteredCourses() {
        return registeredCourses;
    }

    public void registerCourse(Course course) {
        if (course.registerStudent(studentId)) {
            registeredCourses.add(course);
            System.out.println("Successfully registered for course: " + course.getTitle());
        } else {
            System.out.println("Registration failed. The course is full.");
        }
    }

    public void dropCourse(Course course) {
        if (registeredCourses.remove(course)) {
            course.removeStudent(studentId);
            System.out.println("Successfully dropped course: " + course.getTitle());
        } else {
            System.out.println("You are not registered for this course.");
        }
    }
}

class CourseDatabase {
    private HashMap<String, Course> courses;

    public CourseDatabase() {
        this.courses = new HashMap<>();
    }

    public void addCourse(Course course) {
        courses.put(course.getCourseCode(), course);
    }

    public Course getCourse(String courseCode) {
        return courses.get(courseCode);
    }

    public void displayAvailableCourses() {
        System.out.println("\n--- Available Courses ---");
        for (Course course : courses.values()) {
            System.out.println("Course Code: " + course.getCourseCode());
            System.out.println("Title: " + course.getTitle());
            System.out.println("Description: " + course.getDescription());
            System.out.println("Schedule: " + course.getSchedule());
            System.out.println("Available Slots: " + course.getAvailableSlots() + "/" + course.getCapacity());
            System.out.println();
        }
    }
}

class StudentDatabase {
    private HashMap<String, Student> students;

    public StudentDatabase() {
        this.students = new HashMap<>();
    }

    public void addStudent(Student student) {
        students.put(student.getStudentId(), student);
    }

    public Student getStudent(String studentId) {
        return students.get(studentId);
    }
}

public class Main {
    public static void main(String[] args) {
        // Initialize course database
        CourseDatabase courseDatabase = new CourseDatabase();
        courseDatabase.addCourse(new Course("CS101", "Introduction to Computer Science", "Basic concepts of computer science.", 30, "Mon & Wed 10:00-11:30"));
        courseDatabase.addCourse(new Course("MATH101", "Calculus I", "Introduction to calculus.", 25, "Tue & Thu 09:00-10:30"));
        courseDatabase.addCourse(new Course("PHYS101", "Physics I", "Fundamental concepts of physics.", 20, "Mon & Wed 12:00-13:30"));

        // Initialize student database
        StudentDatabase studentDatabase = new StudentDatabase();
        studentDatabase.addStudent(new Student("S001", "Alice"));
        studentDatabase.addStudent(new Student("S002", "Bob"));

        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.println("\n--- Course Registration System ---");
            System.out.println("1. View Available Courses");
            System.out.println("2. Register for a Course");
            System.out.println("3. Drop a Course");
            System.out.println("4. View Registered Courses");
            System.out.println("5. Exit");
            System.out.print("Choose an option: ");
            int option = scanner.nextInt();

            switch (option) {
                case 1:
                    courseDatabase.displayAvailableCourses();
                    break;
                case 2:
                    System.out.print("Enter your Student ID: ");
                    String studentId = scanner.next();
                    Student student = studentDatabase.getStudent(studentId);
                    if (student != null) {
                        courseDatabase.displayAvailableCourses();
                        System.out.print("Enter the Course Code to register: ");
                        String courseCode = scanner.next();
                        Course course = courseDatabase.getCourse(courseCode);
                        if (course != null) {
                            student.registerCourse(course);
                        } else {
                            System.out.println("Invalid course code.");
                        }
                    } else {
                        System.out.println("Invalid Student ID.");
                    }
                    break;
                case 3:
                    System.out.print("Enter your Student ID: ");
                    studentId = scanner.next();
                    student = studentDatabase.getStudent(studentId);
                    if (student != null) {
                        System.out.println("\n--- Registered Courses ---");
                        for (Course c : student.getRegisteredCourses()) {
                            System.out.println("Course Code: " + c.getCourseCode() + ", Title: " + c.getTitle());
                        }
                        System.out.print("Enter the Course Code to drop: ");
                        String courseCode = scanner.next();
                        Course course = courseDatabase.getCourse(courseCode);
                        if (course != null) {
                            student.dropCourse(course);
                        } else {
                            System.out.println("Invalid course code.");
                        }
                    } else {
                        System.out.println("Invalid Student ID.");
                    }
                    break;
                case 4:
                    System.out.print("Enter your Student ID: ");
                    studentId = scanner.next();
                    student = studentDatabase.getStudent(studentId);
                    if (student != null) {
                        System.out.println("\n--- Registered Courses ---");
                        for (Course c : student.getRegisteredCourses()) {
                            System.out.println("Course Code: " + c.getCourseCode());
                            System.out.println("Title: " + c.getTitle());
                            System.out.println("Description: " + c.getDescription());
                            System.out.println("Schedule: " + c.getSchedule());
                            System.out.println();
                        }
                    } else {
                        System.out.println("Invalid Student ID.");
                    }
                    break;
                case 5:
                    System.out.println("Exiting the system. Goodbye!");
                    scanner.close();
                    System.exit(0);
                default:
                    System.out.println("Invalid option. Please try again.");
            }
        }
    }
}
