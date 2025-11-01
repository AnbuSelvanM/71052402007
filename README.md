#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct Student {
    int studentID;
    char name[50];
    int roomNumber;
    char hostelBlock[10];
    struct Student *next;
} Student;

Student *head = NULL;

// Function prototypes
Student* createStudent(int id, char name[], int room, char block[]);
void addStudent(int id, char name[], int room, char block[]);
void removeStudent(int id);
void searchByName(char name[]);
void searchByRoom(int room);
void displayAllotments();
void displayBlockWise(char block[]);
void reverseDisplay(Student *node);
Student* cloneList(Student *source);
void countStudentsPerBlock();

// Create a new student node
Student* createStudent(int id, char name[], int room, char block[]) {
    Student *newStudent = (Student*) malloc(sizeof(Student));
    newStudent->studentID = id;
    strcpy(newStudent->name, name);
    newStudent->roomNumber = room;
    strcpy(newStudent->hostelBlock, block);
    newStudent->next = NULL;
    return newStudent;
}

// Add new allotment
void addStudent(int id, char name[], int room, char block[]) {
    Student *newStudent = createStudent(id, name, room, block);
    if (head == NULL) {
        head = newStudent;
    } else {
        Student *temp = head;
        while (temp->next != NULL)
            temp = temp->next;
        temp->next = newStudent;
    }
    printf("Student added successfully!\n");
}

// Remove student by ID
void removeStudent(int id) {
    if (head == NULL) {
        printf("List is empty!\n");
        return;
    }
    Student *temp = head, *prev = NULL;
    while (temp != NULL && temp->studentID != id) {
        prev = temp;
        temp = temp->next;
    }
    if (temp == NULL) {
        printf("Student with ID %d not found!\n", id);
        return;
    }
    if (prev == NULL)
        head = head->next;
    else
        prev->next = temp->next;
    free(temp);
    printf("Student removed successfully!\n");
}

// Search by name
void searchByName(char name[]) {
    Student *temp = head;
    int found = 0;
    while (temp != NULL) {
        if (strcmp(temp->name, name) == 0) {
            printf("\nFound: ID:%d | Name:%s | Room:%d | Block:%s\n",
                   temp->studentID, temp->name, temp->roomNumber, temp->hostelBlock);
            found = 1;
        }
        temp = temp->next;
    }
    if (!found) printf("No student found with name %s\n", name);
}

// Search by room number
void searchByRoom(int room) {
    Student *temp = head;
    int found = 0;
    while (temp != NULL) {
        if (temp->roomNumber == room) {
            printf("\nFound: ID:%d | Name:%s | Room:%d | Block:%s\n",
                   temp->studentID, temp->name, temp->roomNumber, temp->hostelBlock);
            found = 1;
        }
        temp = temp->next;
    }
    if (!found) printf("No student found in room %d\n", room);
}

// Display all allotments
void displayAllotments() {
    if (head == NULL) {
        printf("No allotments to display.\n");
        return;
    }
    Student *temp = head;
    printf("\n--- Hostel Allotments ---\n");
    while (temp != NULL) {
        printf("ID:%d | Name:%s | Room:%d | Block:%s\n",
               temp->studentID, temp->name, temp->roomNumber, temp->hostelBlock);
        temp = temp->next;
    }
}

// Display allotments block-wise
void displayBlockWise(char block[]) {
    Student *temp = head;
    int found = 0;
    printf("\n--- Students in Block %s ---\n", block);
    while (temp != NULL) {
        if (strcmp(temp->hostelBlock, block) == 0) {
            printf("ID:%d | Name:%s | Room:%d\n",
                   temp->studentID, temp->name, temp->roomNumber);
            found = 1;
        }
        temp = temp->next;
    }
    if (!found) printf("No students in block %s.\n", block);
}

// Display list in reverse order
void reverseDisplay(Student *node) {
    if (node == NULL)
        return;
    reverseDisplay(node->next);
    printf("ID:%d | Name:%s | Room:%d | Block:%s\n",
           node->studentID, node->name, node->roomNumber, node->hostelBlock);
}

// Clone the list
Student* cloneList(Student *source) {
    if (source == NULL)
        return NULL;
    Student *newNode = createStudent(source->studentID, source->name,
                                     source->roomNumber, source->hostelBlock);
    newNode->next = cloneList(source->next);
    return newNode;
}

// Count students per block
void countStudentsPerBlock() {
    Student *temp = head;
    char blocks[100][10];
    int count[100], n = 0, i;
    
    while (temp != NULL) {
        int found = 0;
        for (i = 0; i < n; i++) {
            if (strcmp(blocks[i], temp->hostelBlock) == 0) {
                count[i]++;
                found = 1;
                break;
            }
        }
        if (!found) {
            strcpy(blocks[n], temp->hostelBlock);
            count[n] = 1;
            n++;
        }
        temp = temp->next;
    }

    printf("\n--- Students per Block ---\n");
    for (i = 0; i < n; i++)
        printf("Block %s: %d students\n", blocks[i], count[i]);
}

// Main menu
int main() {
    int choice, id, room;
    char name[50], block[10];
    Student *clonedList = NULL;

    do {
        printf("\n==== Student Hostel Allotment System ====\n");
        printf("1. Add new allotment\n");
        printf("2. Remove student from hostel\n");
        printf("3. Search by name\n");
        printf("4. Search by room number\n");
        printf("5. Display all allotments\n");
        printf("6. Display allotments block-wise\n");
        printf("7. Reverse display\n");
        printf("8. Clone list\n");
        printf("9. Count students per block\n");
        printf("0. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);
        getchar(); // consume newline

        switch (choice) {
            case 1:
                printf("Enter Student ID: ");
                scanf("%d", &id);
                printf("Enter Name: ");
                scanf(" %[^\n]", name);
                printf("Enter Room Number: ");
                scanf("%d", &room);
                printf("Enter Hostel Block: ");
                scanf(" %s", block);
                addStudent(id, name, room, block);
                break;
            case 2:
                printf("Enter Student ID to remove: ");
                scanf("%d", &id);
                removeStudent(id);
                break;
            case 3:
                printf("Enter name to search: ");
                scanf(" %[^\n]", name);
                searchByName(name);
                break;
            case 4:
                printf("Enter room number to search: ");
                scanf("%d", &room);
                searchByRoom(room);
                break;
            case 5:
                displayAllotments();
                break;
            case 6:
                printf("Enter block name: ");
                scanf(" %s", block);
                displayBlockWise(block);
                break;
            case 7:
                printf("\n--- Reverse Display ---\n");
                reverseDisplay(head);
                break;
            case 8:
                clonedList = cloneList(head);
                printf("List cloned successfully!\n");
                break;
            case 9:
                countStudentsPerBlock();
                break;
            case 0:
                printf("Exiting program...\n");
                break;
            default:
                printf("Invalid choice!\n");
        }
    } while (choice != 0);

    return 0;
}
