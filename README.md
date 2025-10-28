# oriented-programming
BIT1103
# info.py

# --- Data Structures ---

# Books dictionary: ISBN → book details
books = {
    "001": {
        "title": "1984",
        "author": "George Orwell",
        "genre": "Fiction",
        "total_copies": 3,
        "available_copies": 3
    }
}

# Members list: each member = dictionary
members = [
    {"member_id": 1, "name": "Isha", "email": "isha@email.com", "borrowed_books": []}
]

# Genres tuple: fixed values
genres = ("Fiction", "Non-Fiction", "Fantasy", "Sci-Fi", "Mystery")

# --- Core Functions (CRUD + Borrow/Return) ---

def add_book(isbn, title, author, genre, total_copies):
    """Add a new book to the library."""
    if isbn not in books and genre in genres:
        books[isbn] = {
            "title": title,
            "author": author,
            "genre": genre,
            "total_copies": total_copies,
            "available_copies": total_copies
        }
        print(f"Book '{title}' added successfully.")
    else:
        print("Book already exists or invalid genre.")

def add_member(member_id, name, email):
    """Add a new library member."""
    for m in members:
        if m["member_id"] == member_id:
            print("Member ID already exists!")
            return
    members.append({"member_id": member_id, "name": name, "email": email, "borrowed_books": []})
    print(f"Member '{name}' added successfully.")

def search_book(keyword):
    """Search for a book by title or author."""
    found = False
    for isbn, details in books.items():
        if keyword.lower() in details["title"].lower() or keyword.lower() in details["author"].lower():
            print(f"Found: {details['title']} by {details['author']} ({details['genre']})")
            found = True
    if not found:
        print("No matching books found.")

def update_book(isbn, new_title=None, new_author=None, new_genre=None, new_total=None):
    """Update book details."""
    if isbn in books:
        if new_title: books[isbn]["title"] = new_title
        if new_author: books[isbn]["author"] = new_author
        if new_genre and new_genre in genres: books[isbn]["genre"] = new_genre
        if new_total:
            diff = new_total - books[isbn]["total_copies"]
            books[isbn]["total_copies"] = new_total
            books[isbn]["available_copies"] += diff
        print(f"Book '{isbn}' updated successfully.")
    else:
        print("Book not found.")

def delete_book(isbn):
    """Delete a book if no copies are borrowed."""
    if isbn in books:
        if books[isbn]["available_copies"] == books[isbn]["total_copies"]:
            del books[isbn]
            print(f"Book '{isbn}' deleted successfully.")
        else:
            print(" Cannot delete — some copies are borrowed.")
    else:
        print("Book not found.")

def borrow_book(member_id, isbn):
    """Borrow a book if available and member hasn’t exceeded 3 books."""
    for member in members:
        if member["member_id"] == member_id:
            if len(member["borrowed_books"]) >= 3:
                print(" Cannot borrow more than 3 books.")
                return
            if isbn in books and books[isbn]["available_copies"] > 0:
                books[isbn]["available_copies"] -= 1
                member["borrowed_books"].append(isbn)
                print(f" {member['name']} borrowed '{books[isbn]['title']}'.")
                return
            else:
                print("Book not available.")
                return
    print("Member not found.")

def return_book(member_id, isbn):
    """Return a borrowed book."""
    for member in members:
        if member["member_id"] == member_id:
            if isbn in member["borrowed_books"]:
                member["borrowed_books"].remove(isbn)
                books[isbn]["available_copies"] += 1
                print(f" {member['name']} returned '{books[isbn]['title']}'.")
                return
            else:
                print(" Book not borrowed by member.")
                return
    print("Member not found.")

   #UML Diagram
   ![image alt](https://github.com/Olatunde2025/oriented-programming/blob/main/UML%20Diagram%20-%20Library%20System.png)
