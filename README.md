# 🚗 Car Parking System Using Object-Oriented Programming in C++

### 🧾 A Mini Project Report

---

## 📘 Abstract

The **Car Parking System** project is developed using **C++** to demonstrate the practical application of **Object-Oriented Programming (OOP)** principles for managing vehicle parking efficiently.  
This system automates the allocation and tracking of parking slots, reducing manual errors and improving efficiency.

The project handles basic operations such as:
- Parking a vehicle  
- Removing a vehicle  
- Displaying parking slot status  

Key OOP principles such as **encapsulation**, **abstraction**, and **class-based modular design** are implemented.  
The system models real-world entities — `Vehicle`, `ParkingSlot`, and `ParkingLot` — as C++ classes, each responsible for specific functionality.  
Using **vectors (STL)** allows dynamic resizing and easy management of parking slots.

This project successfully demonstrates how OOP can be used to design scalable, maintainable, and efficient real-world systems.

---

## 🧩 List of Abbreviations

| Term | Meaning |
|------|----------|
| OOP | Object-Oriented Programming |
| STL | Standard Template Library |
| Vec / Vector | Dynamic array from STL |
| Class | Blueprint for creating objects |
| Object | Instance of a class |
| Encapsulation | Bundling of data and functions |
| Abstraction | Hiding implementation details |
| Inheritance | Deriving new classes from existing ones |
| Polymorphism | Using the same interface for different types |
| Pointer | Variable storing a memory address |
| Menu-driven Program | User interacts through menu options |
| Switch-case | Multi-branching control structure |
| Memory Leak | Unreleased allocated memory |
| `cin`, `cout` | Input/output streams |
| `getline` | Reads entire line input |

---

## 🏗️ 1. Introduction

### Motivation Behind the Project

The **main goal** of this project is to design and implement an **automated car parking management system** using **C++ and OOP concepts**.

In modern urban environments, managing limited parking space efficiently is a growing challenge.  
Manual systems often lead to:
- Mismanagement of slots  
- Congestion and time wastage  
- Difficulty tracking occupied/available spaces  

This project aims to automate the process by dynamically allocating parking slots, maintaining vehicle records, and providing real-time status updates.

### Key Highlights
- Uses **C++ classes** (`Vehicle`, `ParkingSlot`, `ParkingLot`) for modular structure.  
- Demonstrates **OOP principles** like abstraction and encapsulation.  
- **Menu-driven console interface** for user interaction.  
- **Dynamic memory allocation** ensures flexibility and scalability.  
- Can be expanded to include billing, authentication, and hardware integration.  

This project serves both as a **practical urban solution** and as an **educational example** of applying OOP to real-world problems.

---

## 🌍 Impact on Modern Life

| Aspect | Traditional Parking | With This System | Impact |
|--------|--------------------|------------------|--------|
| Slot Allocation | Manual, error-prone | Automated, precise | Faster, accurate |
| Space Utilization | Often inefficient | Dynamic slot management | More vehicles accommodated |
| Record Keeping | Paper-based | Digital and searchable | Better tracking |
| Security | Low | Controlled access | Improved safety |
| Time Required | Long waiting | Quick allocation | Reduced congestion |
| User Experience | Frustrating | Interactive console | Enhanced convenience |
| Error Reduction | Frequent | Automated and consistent | Minimal mistakes |
| Scalability | Hard to expand | Code easily extendable | Supports larger lots |

---

## 💻 Code Implementation

```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

// Vehicle class
class Vehicle {
    string plateNumber;
    string userName;
public:
    Vehicle(const string& plate, const string& user)
        : plateNumber(plate), userName(user) {}
    string getPlate() const { return plateNumber; }
    string getUserName() const { return userName; }
};

// ParkingSlot class
class ParkingSlot {
    Vehicle* vehicle;
public:
    ParkingSlot() : vehicle(nullptr) {}
    bool isEmpty() const { return vehicle == nullptr; }
    void parkVehicle(Vehicle* v) { vehicle = v; }
    void removeVehicle() { vehicle = nullptr; }
    Vehicle* getVehicle() const { return vehicle; }
};

// ParkingLot class using vector
class ParkingLot {
    vector<ParkingSlot> slots;
public:
    ParkingLot(int size) : slots(size) {}

    bool parkVehicle(Vehicle* v) {
        for (auto& slot : slots) {
            if (slot.isEmpty()) {
                slot.parkVehicle(v);
                return true;
            }
        }
        return false;
    }

    bool removeVehicle(const string& plate) {
        for (auto& slot : slots) {
            if (!slot.isEmpty() && slot.getVehicle()->getPlate() == plate) {
                slot.removeVehicle();
                return true;
            }
        }
        return false;
    }

    void displayStatus() const {
        cout << "\nParking Slots Status:\n";
        for (size_t i = 0; i < slots.size(); ++i) {
            cout << "Slot " << i + 1 << ": ";
            if (!slots[i].isEmpty()) {
                cout << "Taken | User: " << slots[i].getVehicle()->getUserName()
                     << " | Plate: " << slots[i].getVehicle()->getPlate() << endl;
            } else {
                cout << "Empty\n";
            }
        }
    }
};

// Main function
int main() {
    ParkingLot lot(10);
    int choice;
    do {
        cout << "\n--- Car Parking System Menu ---\n";
        cout << "1. Park Vehicle\n2. Remove Vehicle\n3. Display Parking Status\n4. Exit\nEnter choice: ";
        cin >> choice;

        switch (choice) {
            case 1: {
                string user, plate;
                cin.ignore();
                cout << "Enter user name: ";
                getline(cin, user);
                cout << "Enter vehicle number plate: ";
                getline(cin, plate);
                Vehicle* newCar = new Vehicle(plate, user);
                if (lot.parkVehicle(newCar))
                    cout << "Vehicle parked successfully.\n";
                else {
                    cout << "Parking lot full. Unable to park vehicle.\n";
                    delete newCar;
                }
                break;
            }
            case 2: {
                string plate;
                cin.ignore();
                cout << "Enter vehicle number plate to remove: ";
                getline(cin, plate);
                if (lot.removeVehicle(plate))
                    cout << "Vehicle removed successfully.\n";
                else
                    cout << "Vehicle not found.\n";
                break;
            }
            case 3:
                lot.displayStatus();
                break;
            case 4:
                cout << "Exiting program.\n";
                break;
            default:
                cout << "Invalid choice. Please try again.\n";
        }
    } while (choice != 4);
    return 0;
}
