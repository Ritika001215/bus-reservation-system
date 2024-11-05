# bus-reservation-system
This C++ bus reserving application simplifies automates the booking and management of bus tickets, which helps both the transport company and customers.
It allows user to book, cancel and view available seat in real time while maintaining accurate records of bus schedules, passenger detail and seat availability.
#include <iostream>
#include <iomanip>
#include <string>

using namespace std;

class Bus {
    string bus_number;
    string bus_driver;
    string bus_arrival;
    string bus_depart;
    string b_from;
    string b_to;
    string bus_seat[8][4];

public:
    void addNewBus();
    void makeReservation(Bus buses[], int busIndex);
    void cancelReservation(Bus buses[], int busIndex);
    void clearSeats();
    void viewBusDetails(int busIndex);
    void showAvailableBuses(Bus buses[], int busCount);
    void showSeatAvailability(int busIndex);
    void displayLine(char ch);
};

void Bus::addNewBus() {
    cout << "Enter Bus Number: ";
    cin >> bus_number;

    cout << "\nEnter Driver's Name: ";
    cin >> bus_driver;

    cout << "\nArrival Time: ";
    cin >> bus_arrival;

    cout << "\nDeparture Time: ";
    cin >> bus_depart;

    cout << "\nFrom: ";
    cin >> b_from;

    cout << "\nTo: ";
    cin >> b_to;

    clearSeats();
    cout << "\nNew Bus Added Successfully.\n";
}

void Bus::makeReservation(Bus buses[], int busIndex) {
    buses[busIndex].showSeatAvailability(busIndex);

    cout << "Enter Seat Number for Reservation: ";
    int seatNumber;
    cin >> seatNumber;

    if (seatNumber < 1 || seatNumber > 32) {
        cout << "Invalid seat number. Please enter a number between 1 and 32.\n";
        return;
    }

    int row = (seatNumber - 1) / 4;
    int col = (seatNumber - 1) % 4;

    if (buses[busIndex].bus_seat[row][col] == "Empty") {
        cout << "Enter Passenger's Name: ";
        cin >> buses[busIndex].bus_seat[row][col];
        cout << "Reservation Successful!\n";
    } else {
        cout << "The seat is already reserved. Please choose another seat.\n";
    }
}
void Bus::cancelReservation(Bus buses[], int busIndex) {
    buses[busIndex].showSeatAvailability(busIndex);

    cout << "Enter Seat Number to Cancel Reservation: ";
    int seatNumber;
    cin >> seatNumber;

    if (seatNumber < 1 || seatNumber > 32) {
        cout << "Invalid seat number. Please enter a number between 1 and 32.\n";
        return;
    }

    int row = (seatNumber - 1) / 4;
    int col = (seatNumber - 1) % 4;

    if (buses[busIndex].bus_seat[row][col] != "Empty") {
        cout << "Reservation for seat " << seatNumber << " is cancelled.\n";
        buses[busIndex].bus_seat[row][col] = "Empty"; // Clear the reservation
    } else {
        cout << "The seat is already empty. No reservation to cancel.\n";
    }
}
void Bus::clearSeats() {
    for (int x = 0; x < 8; x++) {
        for (int z = 0; z < 4; z++) {
            bus_seat[x][z] = "Empty";
        }
    }
}

void Bus::viewBusDetails(int busIndex) {
    displayLine('*');
    cout << "Bus Number: \t" << bus_number
         << "\nDriver: \t" << bus_driver << "\t\tArrival Time: \t"
         << bus_arrival << "\tDeparture Time:" << bus_depart
         << "\nFrom: \t\t" << b_from << "\t\tTo: \t\t" << b_to << "\n";
    displayLine('*');

    showSeatAvailability(busIndex);  // Display seat availability for the specific bus
}

void Bus::showAvailableBuses(Bus buses[], int busCount) {
    displayLine('*');
    cout << "Available Buses:\n";
    for (int i = 0; i < busCount; i++) {
        if (!buses[i].bus_number.empty()) {
            cout << "Bus Number: " << buses[i].bus_number << "\tDriver: " << buses[i].bus_driver
                 << "\tFrom: " << buses[i].b_from << "\tTo: " << buses[i].b_to << "\n";
        }
    }
    displayLine('_');
}

void Bus::showSeatAvailability(int busIndex) {
    cout << "Seat Availability for Bus Number: " << bus_number << "\n";
    int seatNumber = 1;
    int availableSeats = 0;

    for (int x = 0; x < 8; x++) {
        for (int z = 0; z < 4; z++) {
            cout << setw(5) << left << seatNumber << ".";
            cout << setw(10) << left << bus_seat[x][z];
            if (bus_seat[x][z] == "Empty") {
                availableSeats++;
            }
            seatNumber++;
        }
        cout << "\n";
    }

    cout << "\nThere are " << availableSeats << " seats available in Bus Number: " << bus_number << "\n";
}

void Bus::displayLine(char ch) {
    for (int x = 80; x > 0; x--) {
        cout << ch;
    }
    cout << '\n';
}

int main() {
    const int MAX_BUSES = 5;
    Bus buses[MAX_BUSES];
    int busCount = 0;
    int choice;

    while (true) {
        cout << "\n\n";
        cout << "\t\t***SIMPLE BUS RESERVATION SYSTEM***";
        cout << "\n\n";
        cout << "\t\t\t1. Add New Bus\n\t\t\t"
             << "2. Make Reservation\n\t\t\t"
             << " 3. Cancel Reservation\n\t\t\t"
             << "4. View Reservation\n\t\t\t"
             << "5. Show Available Buses\n\t\t\t"
             << "6. Exit";
        cout << "\n\t\t\tEnter your Choice: ";

        cin >> choice;

        switch (choice) {
        case 1:
            if (busCount < MAX_BUSES) {
                buses[busCount].addNewBus();
                busCount++;
            } else {
                cout << "Maximum bus limit reached. Cannot add more buses.\n";
            }
            break;

        case 2:
            int busIndex;
            cout << "Enter Bus Index (0 to " << busCount - 1 << "): ";
            cin >> busIndex;
            if (busIndex >= 0 && busIndex < busCount) {
                buses[busIndex].makeReservation(buses, busIndex);
            } else {
                cout << "Invalid Bus Index.\n";
            }
            break;
         case 3:
            cout << "Enter Bus Index (0 to " << busCount - 1 << "): ";
            cin >> busIndex;
            if (busIndex >= 0 && busIndex < busCount) {
                buses[busIndex].cancelReservation(buses, busIndex);
            } else {
                cout << "Invalid Bus Index.\n";
            }
            break;
        case 4:
            cout << "Enter Bus Index (0 to " << busCount - 1 << "): ";
            cin >> busIndex;
            if (busIndex >= 0 && busIndex < busCount) {
                buses[busIndex].viewBusDetails(busIndex);
            } else {
                cout << "Invalid Bus Index.\n";
            }
            break;

        case 5:
            buses[0].showAvailableBuses(buses, busCount);
            break;

        case 6:
            cout << "Exiting the program. Thank you!\n";
            return 0;

        default:
            cout << "Invalid choice. Please try again.\n";
        }
    }

    return 0;
}
