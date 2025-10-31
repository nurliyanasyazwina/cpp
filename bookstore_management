#include <iostream>
#include <iomanip>
#include <string>
#include <limits>

using namespace std;

struct book{
  string ISBN, title, author;
  double price;
  int stock;
};
struct Sale {
	string ISBN;
	string title;
	int quantity;
	double unitPrice;
	double total;
};

void addBook(book books[], int &bookCount);
void displayBooks(const book books[], int bookCount);
int searchBook(const book books[], int bookCount, string keyword);
void buyBook(book books[], int &bookCount, Sale sales[], int &saleCount);
void showReport(const Sale sales[], int saleCount);
       
   
int main(){
  book books[100];
  int bookCount = 0;

  Sale sales[100]; 
  int saleCount = 0;

    int choice;
    do{
        cout << "\n===== Bookstore Management System =====\n";
        cout << "1. Add New Book\n";
        cout << "2. Display All Books\n";
        cout << "3. Search Book\n";
        cout << "4. Buy Book\n";
        cout << "5. Show Sales Report\n";
        cout << "6. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;
        cin.ignore(numeric_limits<streamsize>::max(), '\n'); 

        switch(choice){
            case 1:
                addBook(books, bookCount);
                break;
            case 2:
                displayBooks(books, bookCount);
                break;
            case 3: {
                string keyword;
                cout << "Enter ISBN or Title to search: ";
                getline(cin, keyword);
                searchBook(books, bookCount, keyword);
                break;
            }
            case 4:
                buyBook(books, bookCount, sales, saleCount);
                break;
            case 5:
                showReport(sales, saleCount);
                break;
            case 6:
                cout << "Exiting the program. Goodbye!\n";
                break;
            default:
                cout << "Invalid choice. Please try again.\n";
        }
    }
    while(choice != 6);

  return 0;
}

void addBook(book books[], int &bookCount) {
    cout << "\n----- Add New Book(s) -----\n";

    int numBooks;
    cout << "How many books to add? ";
    cin >> numBooks;

    // Validate number of books
    if (!cin || numBooks <= 0 || bookCount + numBooks > 100) {
        cin.clear(); // reset error state
        cin >> ws;   
        cout << "Invalid number of books to add.\n";
        return;
    }

    for (int i = 0; i < numBooks; i++) {
        cout << "\nAdding book " << (bookCount + 1) << ":\n";

        cout << "Enter ISBN: ";
        getline(cin >> ws, books[bookCount].ISBN);

        // Check for duplicate ISBN
        bool exists = false;
        for (int j = 0; j < bookCount; j++) {
            if (books[j].ISBN == books[bookCount].ISBN) {
                cout << "Book with this ISBN already exists! Skipping this entry.\n";
                exists = true;
                break;
            }
        }
        if (exists) continue;

        cout << "Enter Title: ";
        getline(cin >> ws, books[bookCount].title);

        cout << "Enter Author: ";
        getline(cin >> ws, books[bookCount].author);

        cout << "Enter Price: ";
        while (!(cin >> books[bookCount].price) || books[bookCount].price <= 0) {
            cin.clear();
            cin >> ws;
            cout << "Invalid price. Enter again: ";
        }

        cout << "Enter Stock Quantity: ";
        while (!(cin >> books[bookCount].stock) || books[bookCount].stock < 0) {
            cin.clear();
            cin >> ws;
            cout << "Invalid stock. Enter again: ";
        }

        cout << "Book successfully added!\n";
        bookCount++;
    }
}


void displayBooks(const book books[], int bookCount){
  cout << "\n-----Book List-----\n";
  if(bookCount == 0){
    cout << "No books in inventory\n";
    return;
  }

  for(int i=0; i<bookCount; i++){
    cout << i+1 << ". " << books[i].title << " by " << books[i].author << endl
    << " ISBN: " << books[i].ISBN
    << " | Price: RM" << books[i].price
    << " | Stock: " << books[i].stock << endl;
  }

}

int searchBook(const book books[], int bookCount, string keyword){
  for(int i=0; i<bookCount; i++){
    if(books[i].ISBN == keyword || books[i].title == keyword){
      cout << "\nBook found: \n";
      cout << "Title: " << books[i].title << endl;
      cout << "Author: " << books[i].author << endl;
      cout << "Price: RM" << books[i].price << endl;
      cout << "Stock: " << books[i].stock << endl;
      return i;
    }
  }

  cout << "Book not found\n";
  return -1;
}

void buyBook(book books[], int &bookCount, Sale sales[], int &saleCount){
    
    cout << "----- Purchase Book -----\n";
    if (bookCount == 0){
      cout << "No books available to purchase.\n";
      return;
      
    }
    string isbn;
  cout << "Enter ISBN of the book to buy: ";
  getline(cin, isbn);
    int index = searchBook(books, bookCount, isbn);
    if (index == -1){ 
      cout << "Cannot process purchase.\n"; 
      return;
      
    }
    int quantity; 
  cout << "Enter quantity to buy: ";
  cin >> quantity;
  cin.ignore();
    if (quantity <= 0){
      cout << "Invalid quantity.\n";
      return;
      
    }
    if (quantity > books[index].stock){
      cout << "Stock not enough! Available: "<<books[index].stock<<"\n";
      return;
      
    }
    double totalPrice = quantity * books[index].price;
    books[index].stock -= quantity;
    cout << "\nPurchase Successful!\n";
    cout << "You bought: " << books[index].title << "\n";
    cout << "Quantity: " << quantity << "\n";
    cout << "Total Price: RM" << fixed << setprecision(2) << totalPrice << "\n";
    cout << "Remaining Stock: " << books[index].stock << "\n";

    Sale newSale;
    newSale.ISBN = books[index].ISBN;
    newSale.title = books[index].title;
    newSale.quantity = quantity;
    newSale.unitPrice = books[index].price;
    newSale.total = totalPrice;
    sales[saleCount++] = newSale;
    
}
void showReport(const Sale sales[], int saleCount) {
    cout << "\n----- Sales Report (Today) -----\n";
    if (saleCount == 0) {
        cout << "No sales recorded.\n";
        return;
    }

    cout << left
         << setw(15) << "ISBN"
         << setw(28) << "Title"
         << right
         << setw(10) << "Qty"
         << setw(12) << "Unit(RM)"
         << setw(12) << "Total(RM)" << "\n";

    cout << setfill('-') << setw(77) << "-" << setfill(' ') << "\n";

    double grandTotal = 0.0;
    for (int i = 0; i < saleCount; ++i) {
        cout << left
             << setw(15) << sales[i].ISBN
             << setw(28) << (sales[i].title.size() > 27 ? sales[i].title.substr(0,27) + "…" : sales[i].title)
             << right
             << setw(10) << sales[i].quantity
             << setw(12) << fixed << setprecision(2) << sales[i].unitPrice
             << setw(12) << fixed << setprecision(2) << sales[i].total
             << "\n";
        grandTotal += sales[i].total;
    }
    cout << setfill('-') << setw(77) << "-" << setfill(' ') << "\n";
    cout << right << setw(49) << "Grand Total: " << setw(12) << fixed << setprecision(2) << grandTotal << "\n";
}
