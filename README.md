# Project
#include <iostream>
#include <string>
#include <vector>
#include <fstream>
#include <ctime>
#include <cmath>

Enum BookStatus { Available, Borrowed };

// Book Class
Class Book {
Public:
    Int bookID;
    Std::string title;
    Std::string author;
    Std::string genre;
    BookStatus status;

    Book(int id, std::string t, std::string a, std::string g)
        : bookID(id), title(t), author(a), genre(g), status(Available) {}

    Void displayBook() {
        Std::cout << “ID: “ << bookID << “, Title: “ << title
                  << “, Author: “ << author << “, Genre: “ << genre
                  << “, Status: “ << (status == Available ? “Available” : “Borrowed”) << “\n”;
    }

    Void updateStatus(BookStatus newStatus) {
        Status = newStatus;
    }
};

// Member Class
Class Member {
Public:
    Int memberID;
    Std::string name;
    Std::string address;
    Std::string phoneNumber;

    Member(int id, std::string n, std::string addr, std::string phone)
        : memberID(id), name(n), address(addr), phoneNumber(phone) {}

    Void displayMember() {
        Std::cout << “ID: “ << memberID << “, Name: “ << name
                  << “, Address: “ << address << “, Phone: “ << phoneNumber << “\n”;
    }
};

// Transaction Class
Class Transaction {
Public:
    Int transactionID;
    Int memberID;
    Int bookID;
    Std::time_t borrowDate;
    Std::time_t returnDate;

    Transaction(int tID, int mID, int bID, std::time_t bDate)
        : transactionID(tID), memberID(mID), bookID(bID), borrowDate(bDate), returnDate(0) {}

    Void returnBook(std::time_t rDate) {
        returnDate = rDate;
    }

    Double calculateOverdueFee() {
        If (returnDate == 0) return 0; // Not yet returned
        Double daysOverdue = difftime(returnDate, borrowDate) / (60 * 60 * 24);
        Return (daysOverdue > 14) ? (daysOverdue – 14) * 1.0 : 0; // 1.0 fee per day over 14 days
    }
};

// File Handling for Books
Void saveBooksToFile(const std::vector<Book>& books) {
    Std::ofstream outFile(“books.txt”);
    For (const auto& book : books) {
        outFile << book.bookID << “ “ << book.title << “ “ << book.author << “ “ 
                << book.genre << “ “ << book.status << “\n”;
    }
    outFile.close();
}

Void loadBooksFromFile(std::vector<Book>& books) {
    Std::ifstream inFile(“books.txt”);
    While (inFile) {
        Int id;
        Std::string title, author, genre;
        Int status;
        inFile >> id >> title >> author >> genre >> status;
        if (inFile) {
            books.push_back(Book(id, title, author, genre));
            books.back().updateStatus(static_cast<BookStatus>(status));
        }
    }
    inFile.close();
}

// File Handling for Members
Void saveMembersToFile(const std::vector<Member>& members) {
    Std::ofstream outFile(“members.txt”);
    For (const auto& member : members) {
        outFile << member.memberID << “ “ << member.name << “ “ << member.address << “ “ 
                << member.phoneNumber << “\n”;
    }
    outFile.close();
}

Void loadMembersFromFile(std::vector<Member>& members) {
    Std::ifstream inFile(“members.txt”);
    While (inFile) {
        Int id;
        Std::string name, address, phone;
        inFile >> id >> name >> address >> phone;
        if (inFile) {
            members.push_back(Member(id, name, address, phone));
        }
    }
    inFile.close();
}

// File Handling for Transactions
Void saveTransactionsToFile(const std::vector<Transaction>& transactions) {
    Std::ofstream outFile(“transactions.txt”);
    For (const auto& transaction : transactions) {
        outFile << transaction.transactionID << “ “ << transaction.memberID 
                << “ “ << transaction.bookID << “ “ << transaction.borrowDate << “ “
                << transaction.returnDate << “\n”;
    }
    outFile.close();
}

Void loadTransactionsFromFile(std::vector<Transaction>& transactions) {
    Std::ifstream inFile(“transactions.txt”);
    While (inFile) {
        Int tID, mID, bID;
        Std::time_t bDate, rDate;
        inFile >> tID >> mID >> bID >> bDate >> rDate;
        if (inFile) {
            transactions.push_back(Transaction(tID, mID, bID, bDate));
            transactions.back().returnBook(rDate);
        }
    }
    inFile.close();
}

// Adding Books
Void addBook(std::vector<Book>& books, int id, std::string title, std::string author, std::string genre) {
    Books.push_back(Book(id, title, author, genre));
    saveBooksToFile(books);
}

// View All Books
Void viewAllBooks(const std::vector<Book>& books) {
    For (const auto& book : books) {
        Book.displayBook();
    }
}

// Borrow Book
Void borrowBook(std::vector<Book>& books, std::vector<Transaction>& transactions, int bookID, int memberID) {
    For (auto& book : books) {
        If (book.bookID == bookID && book.status == Available) {
            Std::time_t currentTime = std::time(nullptr);
            Book.updateStatus(Borrowed);
            Transactions.push_back(Transaction(transactions.size() + 1, memberID, bookID, currentTime));
            saveBooksToFile(books);
            std::cout << “Book borrowed successfully.\n”;
            return;
        }
    }
    Std::cout << “Book is not available.\n”;
}

// Return Book
Void returnBook(std::vector<Book>& books, std::vector<Transaction>& transactions, int transactionID) {
    For (auto& transaction : transactions) {
        If (transaction.transactionID == transactionID) {
            Std::time_t currentTime = std::time(nullptr);
            Transaction.returnBook(currentTime);
            For (auto& book : books) {
                If (book.bookID == transaction.bookID) {
                    Book.updateStatus(Available);
                    saveBooksToFile(books);
                    std::cout << “Book returned successfully.\n”;
                    return;
                }
            }
        }
    }
    Std::cout << “Transaction not found.\n”;
}

// Reports
Void generateReport(const std::vector<Book>& books, const std::vector<Transaction>& transactions) {
    Std::cout << “Available Books:\n”;
    For (const auto& book : books) {
        If (book.status == Available) {
            Book.displayBook();
        }
    }

    Std::cout << “\nBooks Currently Borrowed:\n”;
    For (const auto& book : books) {
        If (book.status == Borrowed) {
            Book.displayBook();
        }
    }

    Std::cout << “\nOverdue Members:\n”;
    For (const auto& transaction : transactions) {
        If (transaction.calculateOverdueFee() > 0) {
            Std::cout << “Member ID: “ << transaction.memberID
                      << “, Transaction ID: “ << transaction.transactionID
                      << “, Overdue Fee: $” << transaction.calculateOverdueFee() << “\n”;
        }
    }
}

Int main() {
    Std::vector<Book> books;
    Std::vector<Member> members;
    Std::vector<Transaction> transactions;

    // Load data from files
    loadBooksFromFile(books);
    loadMembersFromFile(members);
    loadTransactionsFromFile(transactions);

    // Menu loop
    Int choice;
    Do {
        Std::cout << “\nLibrary Management System\n”;
        Std::cout << “1. Add Book\n”;
        Std::cout << “2. View All Books\n”;
        Std::cout << “3. Borrow Book\n”;
        Std::cout << “4. Return Book\n”;
        Std::cout << “5. Generate Report\n”;
        Std::cout << “0. Exit\n”;
        Std::cout << “Enter your choice: “;
        Std::cin >> choice;

        Switch (choice) {
            Case 1: {
                Int id;
                Std::string title, author, genre;
                Std::cout << “Enter Book ID: “;
                Std::cin >> id;
                Std::cout << “Enter Title: “;
                Std::cin.ignore();  // To ignore newline left by std::cin
                Std::getline(std::cin, title);
                Std::cout << “Enter Author: “;
                Std::getline(std::cin, author);
                Std::cout << “Enter Genre: “;
                Std::getline(std::cin, genre);
                addBook(books, id, title, author, genre);
                break;
            }
            Case 2:
                viewAllBooks(books);
                break;
            case 3:


