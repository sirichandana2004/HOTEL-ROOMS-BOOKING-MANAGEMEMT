# HOTEL-ROOMS-BOOKING-MANAGEMEMT
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_ROOMS 100
#define MAX_NAME_LENGTH 100

// Structure to store room details
typedef struct {
    int roomNumber;
    char accommodationType[20];
    int isBooked;
    int numGuests;
    float hoursSpent;
    float ratePerHour;
    float totalBill;
} Room;

// Structure to store guest details
typedef struct {
    char name[MAX_NAME_LENGTH];
    int roomNumber;
    int numGuests;
    float hoursSpent;
} Guest;

// Function to display room availability
void displayAvailableRooms(Room rooms[], int totalRooms) {
    printf("\nAvailable Rooms:\n");
    for (int i = 0; i < totalRooms; i++) {
        if (!rooms[i].isBooked) {
            printf("Room Number: %d, Type: %s\n", rooms[i].roomNumber, rooms[i].accommodationType);
        }
    }
}

// Function to book a room
void bookRoom(Room rooms[], int totalRooms) {
    int roomNum, guests;
    float hours;
    printf("\nEnter Room Number to Book: ");
    scanf("%d", &roomNum);

    for (int i = 0; i < totalRooms; i++) {
        if (rooms[i].roomNumber == roomNum && !rooms[i].isBooked) {
            printf("Enter number of guests: ");
            scanf("%d", &guests);
            printf("Enter number of hours stayed: ");
            scanf("%f", &hours);

            rooms[i].isBooked = 1;
            rooms[i].numGuests = guests;
            rooms[i].hoursSpent = hours;
            rooms[i].totalBill = hours * rooms[i].ratePerHour;

            printf("Room %d booked successfully!\n", roomNum);
            printf("Total Bill: $%.2f\n", rooms[i].totalBill);
            return;
        }
    }
    printf("Room not available or already booked!\n");
}

// Function to calculate the total bill
float calculateTotalBill(Room room) {
    return room.hoursSpent * room.ratePerHour;
}

// Function to store records in file
void storeRecords(Room rooms[], int totalRooms) {
    FILE *file = fopen("hotel_records.txt", "w");
    if (file == NULL) {
        printf("Error opening file!\n");
        return;
    }

    for (int i = 0; i < totalRooms; i++) {
        if (rooms[i].isBooked) {
            fprintf(file, "Room %d | Type: %s | Guests: %d | Hours: %.2f | Total Bill: %.2f\n",
                    rooms[i].roomNumber, rooms[i].accommodationType, rooms[i].numGuests,
                    rooms[i].hoursSpent, rooms[i].totalBill);
        }
    }
    fclose(file);
    printf("Records stored successfully in 'hotel_records.txt'.\n");
}

int main() {
    Room rooms[MAX_ROOMS];
    int totalRooms = 3;

    // Initialize some room data (this can be dynamically created or read from a file)
    rooms[0] = (Room) {101, "Single", 0, 0, 0, 50.0, 0.0};
    rooms[1] = (Room) {102, "Double", 0, 0, 0, 75.0, 0.0};
    rooms[2] = (Room) {103, "Suite", 0, 0, 0, 120.0, 0.0};

    int choice;
    do {
        printf("\nHotel Room Booking System\n");
        printf("1. Display Available Rooms\n");
        printf("2. Book a Room\n");
        printf("3. Store Records\n");
        printf("4. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                displayAvailableRooms(rooms, totalRooms);
                break;
            case 2:
                bookRoom(rooms, totalRooms);
                break;
            case 3:
                storeRecords(rooms, totalRooms);
                break;
            case 4:
                printf("Exiting the system.\n");
                break;
            default:
                printf("Invalid choice. Try again.\n");
        }
    } while (choice != 4);

    return 0;
}
