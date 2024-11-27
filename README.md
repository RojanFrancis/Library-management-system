#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Define the structure for a book
struct Book {
    int id;
    char title[100];
    char author[100];
    struct Book* next;
};

// Function prototypes
struct Book* createBook(int id, char* title, char* author);
void addBook(struct Book** head, int id, char* title, char* author);
void displayBooks(struct Book* head);
struct Book* searchBook(struct Book* head, char* title);
void deleteBook(struct Book** head, int id);

int main() {
    struct Book* library = NULL;
    int choice, id;
    char title[100], author[100];

    while (1) {
        printf("\nLibrary Management System\n");
        printf("1. Add Book\n");
        printf("2. Display All Books\n");
        printf("3. Search for a Book\n");
        printf("4. Delete a Book\n");
        printf("5. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter Book ID: ");
                scanf("%d", &id);
                printf("Enter Book Title: ");
                getchar(); // Clear newline character from buffer
                fgets(title, 100, stdin);
                title[strcspn(title, "\n")] = '\0'; // Remove newline character
                printf("Enter Author Name: ");
                fgets(author, 100, stdin);
                author[strcspn(author, "\n")] = '\0'; // Remove newline character
                addBook(&library, id, title, author);
                break;

            case 2:
                displayBooks(library);
                break;

            case 3:
                printf("Enter Book Title to Search: ");
                getchar();
                fgets(title, 100, stdin);
                title[strcspn(title, "\n")] = '\0';
                struct Book* found = searchBook(library, title);
                if (found) {
                    printf("Book Found: ID=%d, Title=%s, Author=%s\n", found->id, found->title, found->author);
                } else {
                    printf("Book not found.\n");
                }
                break;

            case 4:
                printf("Enter Book ID to Delete: ");
                scanf("%d", &id);
                deleteBook(&library, id);
                break;

            case 5:
                printf("Exiting...\n");
                return 0;

            default:
                printf("Invalid choice. Please try again.\n");
        }
    }
}

// Function to create a new book
struct Book* createBook(int id, char* title, char* author) {
    struct Book* newBook = (struct Book*)malloc(sizeof(struct Book));
    newBook->id = id;
    strcpy(newBook->title, title);
    strcpy(newBook->author, author);
    newBook->next = NULL;
    return newBook;
}

// Function to add a new book to the library
void addBook(struct Book** head, int id, char* title, char* author) {
    struct Book* newBook = createBook(id, title, author);
    newBook->next = *head;
    *head = newBook;
    printf("Book added successfully.\n");
}

// Function to display all books in the library
void displayBooks(struct Book* head) {
    if (!head) {
        printf("No books in the library.\n");
        return;
    }
    printf("\nBooks in the Library:\n");
    while (head) {
        printf("ID: %d, Title: %s, Author: %s\n", head->id, head->title, head->author);
        head = head->next;
    }
}

// Function to search for a book by title
struct Book* searchBook(struct Book* head, char* title) {
    while (head) {
        if (strcmp(head->title, title) == 0) {
            return head;
        }
        head = head->next;
    }
    return NULL;
}

// Function to delete a book by ID
void deleteBook(struct Book** head, int id) {
    struct Book* temp = *head;
    struct Book* prev = NULL;

    while (temp && temp->id != id) {
        prev = temp;
        temp = temp->next;
    }

    if (!temp) {
        printf("Book with ID %d not found.\n", id);
        return;
    }

    if (prev) {
        prev->next = temp->next;
    } else {
        *head = temp->next;
    }

    free(temp);
    printf("Book deleted successfully.\n");
}
