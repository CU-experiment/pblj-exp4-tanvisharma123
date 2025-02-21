Easy Level: Employee Management using ArrayList
This program allows users to add, update, remove, and search for employees using an ArrayList.

java
Copy
Edit
import java.util.*;

class Employee {
    int id;
    String name;
    double salary;

    public Employee(int id, String name, double salary) {
        this.id = id;
        this.name = name;
        this.salary = salary;
    }

    public String toString() {
        return "ID: " + id + ", Name: " + name + ", Salary: " + salary;
    }
}

public class EmployeeManager {
    public static void main(String[] args) {
        ArrayList<Employee> employees = new ArrayList<>();
        Scanner sc = new Scanner(System.in);
        int choice;

        do {
            System.out.println("\n1. Add Employee\n2. Update Employee\n3. Remove Employee\n4. Search Employee\n5. Display All\n6. Exit");
            System.out.print("Enter your choice: ");
            choice = sc.nextInt();

            switch (choice) {
                case 1:
                    System.out.print("Enter ID, Name, and Salary: ");
                    employees.add(new Employee(sc.nextInt(), sc.next(), sc.nextDouble()));
                    break;
                case 2:
                    System.out.print("Enter Employee ID to Update: ");
                    int updateId = sc.nextInt();
                    for (Employee emp : employees) {
                        if (emp.id == updateId) {
                            System.out.print("Enter new Name and Salary: ");
                            emp.name = sc.next();
                            emp.salary = sc.nextDouble();
                            break;
                        }
                    }
                    break;
                case 3:
                    System.out.print("Enter Employee ID to Remove: ");
                    int removeId = sc.nextInt();
                    employees.removeIf(emp -> emp.id == removeId);
                    break;
                case 4:
                    System.out.print("Enter Employee ID to Search: ");
                    int searchId = sc.nextInt();
                    for (Employee emp : employees) {
                        if (emp.id == searchId) {
                            System.out.println(emp);
                        }
                    }
                    break;
                case 5:
                    for (Employee emp : employees) {
                        System.out.println(emp);
                    }
                    break;
            }
        } while (choice != 6);

        sc.close();
    }
}
Medium Level: Storing and Finding Cards Using Collections
This program uses a HashMap to store playing cards and allows users to find all cards of a specific suit.

java
Copy
Edit
import java.util.*;

public class CardCollection {
    public static void main(String[] args) {
        HashMap<String, List<String>> cards = new HashMap<>();

        // Adding cards
        addCard(cards, "Hearts", "Ace");
        addCard(cards, "Hearts", "King");
        addCard(cards, "Spades", "Queen");
        addCard(cards, "Diamonds", "Jack");
        addCard(cards, "Clubs", "10");

        Scanner sc = new Scanner(System.in);
        System.out.print("Enter suit to find cards: ");
        String suit = sc.nextLine();

        if (cards.containsKey(suit)) {
            System.out.println("Cards in " + suit + ": " + cards.get(suit));
        } else {
            System.out.println("No cards found in " + suit);
        }

        sc.close();
    }

    public static void addCard(HashMap<String, List<String>> map, String suit, String card) {
        map.putIfAbsent(suit, new ArrayList<>());
        map.get(suit).add(card);
    }
}
Hard Level: Ticket Booking System with Synchronized Threads
This program uses multithreading and synchronization to prevent double booking.

java
Copy
Edit
import java.util.concurrent.locks.ReentrantLock;

class TicketBookingSystem {
    private int availableSeats = 10;
    private final ReentrantLock lock = new ReentrantLock();

    public void bookTicket(String passenger, int priority) {
        lock.lock();
        try {
            if (availableSeats > 0) {
                System.out.println(passenger + " booked a seat. Seats left: " + (--availableSeats));
            } else {
                System.out.println("No seats left for " + passenger);
            }
        } finally {
            lock.unlock();
        }
    }
}

class Passenger extends Thread {
    private TicketBookingSystem system;
    private String name;

    public Passenger(TicketBookingSystem system, String name, int priority) {
        super(name);
        this.system = system;
        this.setPriority(priority);
    }

    public void run() {
        system.bookTicket(getName(), getPriority());
    }
}

public class TicketBookingMain {
    public static void main(String[] args) {
        TicketBookingSystem system = new TicketBookingSystem();

        Passenger p1 = new Passenger(system, "VIP-1", Thread.MAX_PRIORITY);
        Passenger p2 = new Passenger(system, "VIP-2", Thread.MAX_PRIORITY);
        Passenger p3 = new Passenger(system, "User-1", Thread.NORM_PRIORITY);
        Passenger p4 = new Passenger(system, "User-2", Thread.NORM_PRIORITY);
        Passenger p5 = new Passenger(system, "User-3", Thread.MIN_PRIORITY);

        p1.start();
        p2.start();
        p3.start();
        p4.start();
        p5.start();
    }
}
