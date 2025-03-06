# Book-Store-Management-System
Command Line Interface (CLI) project using Python for a Book Store Management System.
import json
import os

# File to store book data
BOOKS_FILE = "books.json"

def load_books():
    """Load books from the JSON file."""
    if not os.path.exists(BOOKS_FILE):
        return []
    with open(BOOKS_FILE, "r") as file:
        return json.load(file)

def save_books(books):
    """Save books to the JSON file."""
    with open(BOOKS_FILE, "w") as file:
        json.dump(books, file, indent=4)

def add_book():
    """Add a new book to the store."""
    books = load_books()
    
    title = input("Enter book title: ").strip()
    author = input("Enter author name: ").strip()
    isbn = input("Enter ISBN: ").strip()
    genre = input("Enter genre: ").strip()
    price = input("Enter price: ").strip()
    quantity = input("Enter quantity: ").strip()
    
    # Validate price and quantity
    try:
        price = float(price)
        quantity = int(quantity)
        if price < 0 or quantity < 0:
            raise ValueError
    except ValueError:
        print("Invalid price or quantity. Must be positive numbers.")
        return
    
    # Check for duplicate ISBN
    for book in books:
        if book["isbn"] == isbn:
            print("A book with this ISBN already exists.")
            return
    
    # Add book to list
    books.append({
        "title": title,
        "author": author,
        "isbn": isbn,
        "genre": genre,
        "price": price,
        "quantity": quantity
    })
    
    save_books(books)
    print("Book added successfully!")

def view_books():
    """Display all books in the store."""
    books = load_books()
    if not books:
        print("No books available.")
        return
    
    print("\nAvailable Books:")
    for book in books:
        print(f"Title: {book['title']}, Author: {book['author']}, ISBN: {book['isbn']}, Genre: {book['genre']}, Price: ${book['price']}, Stock: {book['quantity']}")

def remove_book():
    """Remove a book by ISBN."""
    books = load_books()
    isbn = input("Enter ISBN of the book to remove: ").strip()
    
    for book in books:
        if book["isbn"] == isbn:
            books.remove(book)
            save_books(books)
            print("Book removed successfully!")
            return
    print("Book not found.")

def main():
    """Main menu for the book store management system."""
    while True:
        print("\nBook Store Management System")
        print("1. Add Book")
        print("2. View Books")
        print("3. Remove Book")
        print("4. Exit")
        
        choice = input("Enter your choice: ")
        if choice == "1":
            add_book()
        elif choice == "2":
            view_books()
        elif choice == "3":
            remove_book()
        elif choice == "4":
            print("Exiting...")
            break
        else:
            print("Invalid choice. Try again.")

if __name__ == "__main__":
    main()
