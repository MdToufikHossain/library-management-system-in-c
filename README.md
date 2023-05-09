#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_BOOKS 1000

struct book {
    long id;
    char title[50];
    char author[50];
    int year;
    char publisher[50];
};

struct Library {
    struct book books[MAX_BOOKS];
    int num_books;
};

// Function to find a book in the library by its title
void findBook(struct Library* library, char title[50]) {
    int found = 0;
    for (int i = 0; i < library->num_books; i++) {
        if (strcmp(library->books[i].title, title) == 0) {
            printf("Book details:\n");
            printf("ID: %ld\n", library->books[i].id);
            printf("Title: %s\n", library->books[i].title);
            printf("Author: %s\n", library->books[i].author);
            printf("Year: %d\n", library->books[i].year);
            printf("Publisher: %s\n", library->books[i].publisher);
            found = 1;
            break;
        }
    }
    if (!found) {
        printf("Book not found!\n");
    }
}

// Function to import books from a file into the library
void import_books(struct Library* library, char* filename) {
    FILE* file = fopen(filename, "r");
    if (file == NULL) {
        printf("Error: could not open file '%s' for reading\n", filename);
        return;
    }

    char line[256];
    int num_books = 0;
    while (fgets(line, sizeof(line), file) != NULL) {
        sscanf(line, "%ld,%[^,],%[^,],%d,%[^,\n]", &library->books[num_books].id, library->books[num_books].title, library->books[num_books].author, &library->books[num_books].year, library->books[num_books].publisher);
        num_books++;
    }
    library->num_books = num_books;
    printf("%d books imported from file '%s'\n", num_books, filename);

    fclose(file);
}

// Function to export books from the library to a file
void export_books(struct Library* library, char* filename) {
    FILE* file = fopen(filename, "w");
    if (file == NULL) {
        printf("Error: could not open file '%s' for writing\n", filename);
        return;
    }

    for (int i = 0; i < library->num_books; i++) {
        fprintf(file, "%ld,%s,%s,%d,%s\n", library->books[i].id, library->books[i].title, library->books[i].author, library->books[i].year, library->books[i].publisher);
    }
    fclose(file);
    printf("Books exported to file '%s'\n", filename);
}

// Function to add a book to the library
void add_book(struct Library* library) {
    struct book new_book;
    printf("Enter book information:\n");
    printf("ID: ");
    scanf("%ld", &new_book.id);
    printf("Title: ");
    scanf("%s", new_book.title);
    printf("Author: ");
    scanf("%s", new_book.author);
    printf("Year: ");
    scanf("%d", &new_book.year);
    printf("Publisher: ");
    scanf("%s", new_book.publisher);
    library->books[library->num_books++] = new_book;
    printf("Book added to library.\n");
}
// Function to remove a book from the library
void remove_book(struct Library* library, long id) {
int found = 0;
for (int i = 0; i < library->num_books; i++) {
if (library->books[i].id == id) {
for (int j = i; j < library->num_books - 1; j++) {
library->books[j] = library->books[j+1];
}
library->num_books--;
found = 1;
printf("Book removed from library.\n");
break;
}
}
if (!found) {
printf("Book not found!\n");
}
}

// Function to display all books in the library
void display_all_books(struct Library* library) {
printf("Library contains %d books:\n", library->num_books);
for (int i = 0; i < library->num_books; i++) {
printf("Book %d:\n", i+1);
printf("ID: %ld\n", library->books[i].id);
printf("Title: %s\n", library->books[i].title);
printf("Author: %s\n", library->books[i].author);
printf("Year: %d\n", library->books[i].year);
printf("Publisher: %s\n", library->books[i].publisher);
}
}

// Function to display a menu of options and return the user's choice
int display_menu() {
int choice;
printf("Library Management System\n");
printf("1. Add book\n");
printf("2. Remove book\n");
printf("3. Find book\n");
printf("4. Display all books\n");
printf("5. Import books from file\n");
printf("6. Export books to file\n");
printf("7. Quit\n");
printf("Enter your choice (1-7): ");
scanf("%d", &choice);
return choice;
}

int main() {
struct Library library;
library.num_books = 0;
int choice;
do {
    choice = display_menu();
    switch (choice) {
        case 1:
            add_book(&library);
            break;
        case 2:
            long id;
            printf("Enter book ID: ");
            scanf("%ld", &id);
            remove_book(&library, id);
            break;
        case 3:
            char title[50];
            printf("Enter book title: ");
            scanf("%s", title);
            findBook(&library, title);
            break;
        case 4:
            display_all_books(&library);
            break;
        case 5:
            char filename_import[50];
            printf("Enter filename to import from: ");
            scanf("%s", filename_import);
            import_books(&library, filename_import);
            break;
        case 6:
            char filename_export[50];
            printf("Enter filename to export to: ");
            scanf("%s", filename_export);
            export_books(&library, filename_export);
            break;
        case 7:
            printf("Goodbye!\n");
            break;
        default:
            printf("Invalid choice. Please enter a number between 1 and 7.\n");
            break;
    }
} while (choice != 7);

return 0;
}

