# library-management-system-in-c


Library Management System
This is a simple command-line library management system written in C. It allows the user to add, remove, and find books in a library, as well as display all books in the library and import/export books from/to a file.


Options
Add book: Allows the user to add a new book to the library.
Remove book: Allows the user to remove a book from the library by ID.
Find book: Allows the user to search for a book in the library by title.
Display all books: Displays a list of all books in the library.
Import books from file: Imports a list of books from a CSV file.
Export books to file: Exports the current list of books to a CSV file.
Quit: Exits the program.



File Format
The import/export functions use a simple CSV file format. Each line of the file represents one book, and the fields are separated by commas. The fields are:

Book ID (long integer)
Title (string, up to 50 characters)
Author (string, up to 50 characters)
Year of publication (integer)
Publisher (string, up to 50 characters)
