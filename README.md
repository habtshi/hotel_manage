# hotel_manage
#include <iostream>
#include <string>
#include <vector>
using namespace std;

class Date {
public:
    int day;
    int month;
    int year;
    Date(int d = 0, int m = 0, int y = 0) : day(d), month(m), year(y) {}
    void display() const {
        cout << day << "/" << month << "/" << year;
    }
};

class HotelRoom {
private:
    int roomNumber;
    bool isBooked;
    Date bookingDate;
    bool isVip;
    double bookingCost;
    string guestName;
public:
    HotelRoom(int roomNumber, bool isVip, double bookingCost)
        : roomNumber(roomNumber), isBooked(false), isVip(isVip), bookingCost(bookingCost) {}

    void bookRoom(Date date, const string& guestName) {
        if (!isBooked) {
            isBooked = true;
            bookingDate = date;
            this->guestName = guestName;
            cout << "Room " << roomNumber << (isVip? " (VIP)" : "")
                 << " has been booked for ";
            bookingDate.display();
            cout << " by " << guestName << "." << endl;
        } else {
            cout << "Room " << roomNumber << (isVip? " (VIP)" : "") << " is already booked." << endl;
        }
    }

    void cancelBooking() {
        if (isBooked) {
            isBooked = false;
            guestName.clear();
            cout << "Booking for room " << roomNumber << (isVip? " (VIP)" : "") << " has been cancelled." << endl;
        } else {
            cout << "Room " << roomNumber << (isVip? " (VIP)" : "") << " is not booked." << endl;
        }
    }

    bool getBookingStatus() const {
        return isBooked;
    }

    Date getBookingDate() const {
        return bookingDate;
    }

    bool isVipRoom() const {
        return isVip;
    }

    double getBookingCost() const {
        return bookingCost;
    }

    string getGuestName() const {
        return guestName;
    }

    int getRoomNumber() const {
        return roomNumber;
    }
};

class Hotel {
private:
    vector<HotelRoom> rooms;

public:
    void addRoom(int roomNumber, bool isVip, double bookingCost) {
        rooms.emplace_back(roomNumber, isVip, bookingCost);
        cout << "Room " << roomNumber << (isVip? " (VIP)" : "") << " added to the hotel." << endl;
    }

    void bookRoom(int roomNumber, Date date, const string& guestName) {
        for (auto& room : rooms) {
            if (room.getRoomNumber() == roomNumber) {
                room.bookRoom(date, guestName);
                return;
            }
        }
        cout << "Invalid room number." << endl;
    }

    void cancelBooking(int roomNumber) {
        for (auto& room : rooms) {
            if (room.getRoomNumber() == roomNumber) {
                room.cancelBooking();
                return;
            }
        }
        cout << "Invalid room number." << endl;
    }

    void displayRooms() const {
        for (const auto& room : rooms) {
            cout << "Room " << room.getRoomNumber() << ": ";
            if (room.getBookingStatus()) {
                cout << "Booked for ";
                room.getBookingDate().display();
                cout << " by " << room.getGuestName() << " ";
            } else {
                cout << "Available ";
            }
            if (room.isVipRoom()) {
                cout << "(VIP)";
            }
            cout << " - Cost: $" << room.getBookingCost() << endl;
        }
    }

    vector<HotelRoom>& getRooms() {
        return rooms;
    }
};

class Manager {
private:
    Hotel hotel;

public:
    void addRoom(int roomNumber, bool isVip, double bookingCost) {
        hotel.addRoom(roomNumber, isVip, bookingCost);
    }

    void bookRoom(int roomNumber, Date date, const string& guestName) {
        hotel.bookRoom(roomNumber, date, guestName);
    }

    void cancelBooking(int roomNumber) {
        hotel.cancelBooking(roomNumber);
    }

    void displayRooms() const {
        hotel.displayRooms();
    }
};

int main() {
    Manager manager;

    int choice;
    int roomNumber;
    Date date;
    string guestName;
    bool isVip;
    double bookingCost;

    do {
        cout << "\n1. Add a room" << endl;
        cout << "2. Book a room" << endl;
        cout << "3. Cancel a booking" << endl;
        cout << "4. Display rooms" << endl;
        cout << "5. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter room number: ";
                cin >> roomNumber;
                cout << "Is it a VIP room? (1 for Yes, 0 for No): ";
                cin >> isVip;
                cout << "Enter booking cost: ";
                cin >> bookingCost;
                manager.addRoom(roomNumber, isVip, bookingCost);
                break;
            case 2:
                cout << "Enter room number: ";
                cin >> roomNumber;
                cout << "Enter booking date (dd mm yyyy): ";
                cin >> date.day >> date.month >> date.year;
                cout << "Enter guest name: ";
                cin.ignore();
                getline(cin, guestName);
                manager.bookRoom(roomNumber, date, guestName);
                break;
            case 3:
                cout << "Enter room number: ";
                cin >> roomNumber;
                manager.cancelBooking(roomNumber);
                break;
            case 4:
                manager.displayRooms();
                break;
            case 5:
                cout << "Exiting..." << endl;
                break;
            default:
                cout << "Invalid choice." << endl;
        }
    
    } while (choice != 5);

    return 0;
}
