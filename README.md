#LIBRARY MANAGEMENT SYSTEM
import json
import os

class Library:
    def __init__(self, book_list, name, filename="library_data.json"):
        self.filename = filename
        self.name = name
        self.books_list = []
        self.lend_dict = {}
        self.load_data(book_list)

    def load_data(self, default_books):
        if os.path.exists(self.filename):
            try:
                with open(self.filename, "r") as file:
                    data = json.load(file)
                    self.books_list = data.get("books", [])
                    self.lend_dict = data.get("lends", {})
            except:
                self.books_list = default_books
                self.lend_dict = {}
        else:
            self.books_list = default_books
            self.lend_dict = {}
            self.save_data()

    def save_data(self):
        with open(self.filename, "w") as file:
            json.dump({
                "books": self.books_list,
                "lends": self.lend_dict
            }, file, indent=4)

    def display_books(self):
        print(f"\nBooks available in {self.name}:")
        available = [book for book in self.books_list if book not in self.lend_dict]
        if not available:
            print("No books are currently available.")
        else:
            for book in available:
                print(f"- {book}")

    def lend_book(self, user, book):
        book = book.strip().title()
        user = user.strip().title()
        if book not in self.books_list:
            print("This book is not available in the library.")
        elif book in self.lend_dict:
            print(f"Sorry, this book is already issued to {self.lend_dict[book]}.")
        else:
            self.lend_dict[book] = user
            self.save_data()
            print("Book issued successfully.")

    def add_book(self, book):
        book = book.strip().title()
        if book in self.books_list:
            print("This book already exists in the library.")
        else:
            self.books_list.append(book)
            self.save_data()
            print("Book added successfully.")

    def remove_book(self, book):
        book = book.strip().title()
        if book in self.lend_dict:
            print(f"Cannot remove this book as it is currently issued to {self.lend_dict[book]}.")
        elif book in self.books_list:
            self.books_list.remove(book)
            self.save_data()
            print("Book removed successfully.")
        else:
            print("This book does not exist in the library.")

    def return_book(self, book):
        book = book.strip().title()
        if book in self.lend_dict:
            self.lend_dict.pop(book)
            self.save_data()
            print("Book returned successfully.")
        else:
            print("This book was not issued from this library.")

    def display_lent_books(self):
        print(f"\nCurrently lent books in {self.name}:")
        if not self.lend_dict:
            print("No books are currently lent out.")
        else:
            for book, user in self.lend_dict.items():
                print(f"- {book} (Issued to: {user})")

    def my_books(self, user):
        user = user.strip().title()
        my = [book for book, u in self.lend_dict.items() if u == user]
        print(f"\nBooks borrowed by {user}:")
        if my:
            for book in my:
                print(f"- {book}")
        else:
            print("You have not borrowed any books.")

if __name__ == "__main__":
    main = Library(
        ['Python', 'C++ Basics', 'SQLAlchemy', 'Web Development', 'Algorithms by CLRS'],
        "the programming library"
    )
    while True:
        print(f"\nWelcome to {main.name}")
        print("1. Display Books")
        print("2. Lend a Book")
        print("3. Add a Book to the Library")
        print("4. Remove a Book from the Library")
        print("5. Return a Book")
        print("6. Display Lent Books")
        print("7. My Borrowed Books")
        print("8. Exit")
        choice = input("Enter your choice (1-8): ")
        if choice == '1':
            main.display_books()
        elif choice == '2':
            main.display_books()
            print("\nChoose a book from the list above.")
            book = input("Enter book name: ")
            user = input("Enter your name: ")
            main.lend_book(user, book)
        elif choice == '3':
            book = input("Enter book name to add: ")
            main.add_book(book)
        elif choice == '4':
            book = input("Enter book name to remove: ")
            main.remove_book(book)
        elif choice == '5':
            book = input("Enter book name to return: ")
            main.return_book(book)
        elif choice == '6':
            main.display_lent_books()
        elif choice == '7':
            user = input("Enter your name: ")
            main.my_books(user)
        elif choice == '8':
            print("Thanks for using the Library Management System!")
            break
        else:
            print("Invalid choice. Please try again.")
