# 6thDevelop a menu driven Program in C for the following operations on Circular QUEUE
of Characters (Array Implementation of Queue with maximum size MAX)
a. Insert an
Element on to Circular QUEUE 
b. Delete an Element from Circular QUEUE 
c. Demonstrate Overflow and Underflow situations on Circular QUEUE
d. Display the status of Circular QUEUE 
e. Exit Support the program with appropriate functions for each of the
above operations.



#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

#define MAX_SIZE 5

// Circular Queue structure
struct CircularQueue {
    char items[MAX_SIZE];
    int front, rear;
};

// Function prototypes
void initializeQueue(struct CircularQueue *q);
bool isFull(struct CircularQueue *q);
bool isEmpty(struct CircularQueue *q);
void enqueue(struct CircularQueue *q, char value);
char dequeue(struct CircularQueue *q);
void display(struct CircularQueue *q);

int main() {
    struct CircularQueue queue;
    initializeQueue(&queue);

    char choice, value;

    do {
        printf("\nCircular Queue Operations Menu:\n");
        printf("a. Insert an element\n");
        printf("b. Delete an element\n");
        printf("c. Demonstrate Overflow and Underflow situations\n");
        printf("d. Display the status of the Circular Queue\n");
        printf("e. Exit\n");
        printf("Enter your choice: ");
        scanf(" %c", &choice);

        switch (choice) {
            case 'a':
                printf("Enter element to insert: ");
                scanf(" %c", &value);
                if (!isFull(&queue))
                    enqueue(&queue, value);
                else
                    printf("Circular Queue Overflow! Cannot insert element.\n");
                break;

            case 'b':
                if (!isEmpty(&queue))
                    printf("Deleted element: %c\n", dequeue(&queue));
                else
                    printf("Circular Queue Underflow! Cannot delete element.\n");
                break;

            case 'c':
                // Demonstrate overflow and underflow by performing operations on full and empty queues
                printf("Demonstrating Overflow and Underflow situations:\n");
                initializeQueue(&queue); // Reset queue
                enqueue(&queue, 'A');
                enqueue(&queue, 'B');
                enqueue(&queue, 'C');
                enqueue(&queue, 'D');
                enqueue(&queue, 'E'); // Queue is now full
                display(&queue); // Display full queue
                printf("Attempting to insert element into full queue...\n");
                enqueue(&queue, 'F'); // Attempting to insert into full queue
                dequeue(&queue); // Remove an element
                dequeue(&queue); // Remove an element
                dequeue(&queue); // Remove an element
                dequeue(&queue); // Remove an element
                dequeue(&queue); // Remove the last element, making the queue empty
                display(&queue); // Display empty queue
                printf("Attempting to delete element from empty queue...\n");
                dequeue(&queue); // Attempting to delete from empty queue
                break;

            case 'd':
                display(&queue);
                break;

            case 'e':
                printf("Exiting program.\n");
                break;

            default:
                printf("Invalid choice! Please enter a valid option.\n");
        }
    } while (choice != 'e');

    return 0;
}

// Function to initialize circular queue
void initializeQueue(struct CircularQueue *q) {
    q->front = q->rear = -1;
}

// Function to check if the circular queue is full
bool isFull(struct CircularQueue *q) {
    return (q->front == 0 && q->rear == MAX_SIZE - 1) || (q->rear == (q->front - 1) % (MAX_SIZE - 1));
}

// Function to check if the circular queue is empty
bool isEmpty(struct CircularQueue *q) {
    return q->front == -1;
}

// Function to insert an element into the circular queue
void enqueue(struct CircularQueue *q, char value) {
    if (isFull(q)) {
        printf("Circular Queue Overflow! Cannot insert element.\n");
        return;
    }

    if (q->front == -1)
        q->front = 0;
    q->rear = (q->rear + 1) % MAX_SIZE;
    q->items[q->rear] = value;
}

// Function to delete an element from the circular queue
char dequeue(struct CircularQueue *q) {
    char deletedElement;
    if (isEmpty(q)) {
        printf("Circular Queue Underflow! Cannot delete element.\n");
        return '\0'; // Return null character if queue is empty
    }

    deletedElement = q->items[q->front];
    if (q->front == q->rear)
        initializeQueue(q);
    else
        q->front = (q->front + 1) % MAX_SIZE;
    
    return deletedElement;
}

// Function to display the circular queue
void display(struct CircularQueue *q) {
    if (isEmpty(q)) {
        printf("Circular Queue is empty.\n");
        return;
    }

    printf("Circular Queue elements:\n");
    if (q->front <= q->rear) {
        for (int i = q->front; i <= q->rear; i++)
            printf("%c ", q->items[i]);
    } else {
        for (int i = q->front; i < MAX_SIZE; i++)
            printf("%c ", q->items[i]);
        for (int i = 0; i <= q->rear; i++)
            printf("%c ", q->items[i]);
    }
    printf("\n");
}
