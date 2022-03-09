# Library Management System
## Introduction
Library management system is useful for the librarian to keep track of all the books. He can
search the book in the library. This system helps the librarian to track the books that are being
issued to the students or staff.

### Tasks
- Add new book
- Update book
- Delete book
- Issue book
- Return book
- Global book search

### Artifacts
- Flow Chart
- Report
- Source Code
### Importing Modules
```
import datetime
import pandas as pd

df = pd.read_csv('Book1.csv')
today = datetime.date.today()
time_to_return = datetime.timedelta(days=14)  # 14 days to return
return_time = today + time_to_return
structure = pd.DataFrame(df)
pd.set_option('display.max_columns', None)
pd.set_option('display.width', 1000)
```

Here we are importing 2 modules “datetime” and “pandas”. Datetime is used for handling time usage and getting the current time from the user. Then we import a .csv file and store the current time in ‘today’ variable. The ‘time_to_return’ variable adds 14 days as a result date whenever it is called. ‘pd.set_option’ is used to display the terminal correctly and prevent it from overlapping.

## Functions
### Add Book
```def add_book():
    global structure
    global today
    book = input("Enter new book: ")
    row = input("Enter book placement(row,column): ")
    newbook = {'Book Name': book, 'Entry Date': today, 'Availability': 'Yes',  'Issue Date': 'None', 'Issued to': 'None',
               'Return Date': 'None', 'Extra Charges': 0, 'Placement': row}
    structure = structure.append(newbook, ignore_index=True)
```

The add_book() function adds a new book in the DataFrame. Global variables are used so that any variable outside the function can be used inside the function as well. Then we take two inputs from the user for the book name and the placement of the book. Then using a Dictionary in which keys are column headers of a DataFrame, we append the entered data into the DataFrame.

### Issue Book
```def issue_book():
    global structure
    global df
    print('-------------------------------------------------')
    print(structure)
    print('-------------------------------------------------')
    print('')
    book = input("What book do you want to issue?: ")
    if structure.loc[structure['Availability'] == 'No']:
        print("--")
    issue_person = input("Who do you want to issue the book to: ")
    if structure['Book Name'].str.contains(book).any():
        structure.set_index('Book Name', inplace=True)
        structure.loc[book, 'Issue Date'] = today
        structure.loc[book, 'Availability'] = 'No'
        structure.loc[book, 'Issued To'] = issue_person
        structure.loc[book, 'Return Date'] = return_time
        structure.reset_index(inplace=True)
        print('-------------------------------------------------')
        print('Book issued Successfully')
        print('-------------------------------------------------')

    else:
        print('-------------------------------------------------')
        print("Wrong Input, Enter book index again!")
        print('-------------------------------------------------')
        issue_book()
```

The issue_book() function is used to issue a book to a person. It takes two inputs: one for the book name and the other for the person name it is issued to.

### Return Book
```def return_book():
    global structure
    returned_book = input("Enter book name to return: ")
    if structure['Book Name'].str.contains(returned_book).any():
        structure.set_index('Book Name', inplace=True)
        structure.loc[returned_book, 'Availability'] = 'Yes'
        structure.loc[returned_book, 'Issue Date'] = 'None'
        structure.loc[returned_book, 'Return Date'] = 'None'
        structure.loc[returned_book, 'Issued To'] = 'None'
        structure.reset_index(inplace=True)
    else:
        print('-------------------------------------------------')
        print('Invalid Book')
        print('-------------------------------------------------')
        print('Enter Again')
        return_book()
```

The return_book() function returns an issued book and sets all the columns to default values.

### Delete Book
```def delete_book():
    global structure
    delete = input("Which book do you want to delete?: ")
    if structure['Book Name'].str.contains(delete).any():
        structure.set_index('Book Name', inplace=True)
        structure = structure.drop(delete)
        structure.reset_index(inplace=True)
    else:
        print("Invalid Entry")
        print("Enter Again")
        delete_book()
```

The delete_book() function deletes a book from the DataFrame by deleting the row using drop function. Then we set the index to “Book Name” again so that we can take input from the user in a string.

### View Books Issued to a User
```def user_issued():
    user = input("Enter user to view: ")
    global structure
    if structure['Issued To'].str.contains(user).any():  # Checks if the user is in the 'Issued To' box
        check = structure[structure['Issued To'] == user]
        print(check)
    else:
        print('No book issues to specifies person')
        print('')
        print('Enter again')
        user_issued()
```

The user_issued() function checks if the user is in the ‘Issued to’ column of the DataFrame and shows all the books issued to that user.

### Status of a Book
```def book_issued():
    book = input("Enter Book to view: ")
    global structure
    if structure['Book Name'].str.contains(book).any():
        check = structure[structure['Book Name'] == book]
        print(check)
    else:
        print('-------------------------------------------------')
        print('No book with the name', book, 'found in the database')
        print('-------------------------------------------------')
        print('Enter Again')
        book_issued()
```

The book_issued() function checks if the entered book is in the DataFrame. If it is, then it shows the status of the book e.g. it’s availability, the user this book is issued to, placement of the book and entry date.

#### Note:
> *Else Statements at the end of each function is used to call the function again if an invalid entry is entered.*

## Home Screen
This the main code of the program where you are first welcomed by a welcome message and then using the while True infinite loop, we take inputs from user to choose. Each choice has its own function that it calls and if the choice entered is invalid, then the loop runs again.

### Welcome Screen
```print('-------------------------------------------------')
print('-                    -')
print('-  Welcome to Library Management System  -')
print('-                    -')
print('-------------------------------------------------')
print('')
```

As you can here, You are first welcomed by this message. Then we run a while True infinite loop.

### While True Loop
```while True:

    print("""           Enter 'A' to add a new book
           Enter 'I' to issue a book
           Enter 'U' to view issued books to the user
           Enter 'R' to return a book
           Enter 'V' to view available books
           Enter 'B' to view status of a book
           Enter 'D' to view database
           Enter 'X' to delete a book
           Enter 'Q' to exit
           Enter 'S' to save file""")

    print('-------------------------------------------------')
    choice = input("What would you like to do: ")
    print('-------------------------------------------------')
    if choice == 'A':
        add_book()
        print(structure)
        print('-------------------------------------------------')
    elif choice == 'I':
        issue_book()
        print('-------------------------------------------------')
    elif choice == 'U':
        user_issued()
        print('-------------------------------------------------')
    elif choice == 'R':
        return_book()
        print('-------------------------------------------------')
    elif choice == 'B':
        
        book_issued()
        print('-------------------------------------------------')
    elif choice == 'D':
        print(structure)
        print('-------------------------------------------------')
    elif choice == 'X':
        delete_book()
        print('-------------------------------------------------')
    elif choice == 'V':
        available()
        print('-------------------------------------------------')
```

Here what choice ‘D’ does is, it prints the whole DataFrame with all the books and information in it.
```elif choice == 'S':
        structure.to_csv('Book1.csv', index=False)
        count = count + 1
    elif choice == 'Q':
        if count == 0:
            print("You have not saved your work. Do you want to exit anyways?:")
            decision = input(">")
            if decision == 'Yes':
                break
            else:
                continue
        else:
            break
    else:
        print('-------------------------------------------------')
        print('Invalid choice, Choose again')
        print('-------------------------------------------------')

print("Good Bye")
```

At the end, if the user wants to quit, the count variable is used to check if the user has saved the work or not. When user calls the save function, the count is added by 1 and the program breaks out of the loop and exits.

### Our Motive
> ***The reason behind this code is that we wanted to create a system where you can manage and add data in it and upon looking at the library management system, we found it to be very interesting. We wanted to design a program where you can have all your data in a single frame, save the new entered data and is also capable of performing all the other library management system related functions.***

## Raw Code
``` import datetime
import pandas as pd

df = pd.read_csv('Book1.csv')
today = datetime.date.today()
time_to_return = datetime.timedelta(days=14)  # 14 days to return
return_time = today + time_to_return
structure = pd.DataFrame(df)
pd.set_option('display.max_columns', None)
pd.set_option('display.width', 1000)
count = 0


def add_book():
    global structure
    global today
    book = input("Enter new book: ")
    row = input("Enter book placement(row,column): ")
    newbook = {'Book Name': book, 'Entry Date': today, 'Availability': 'Yes', 'Issue Date': 'None', 'Issued to': 'None',
               'Return Date': 'None', 'Extra Charges': 0, 'Placement': row}
    structure = structure.append(newbook, ignore_index=True)


def issue_book():
    global structure
    global df
    print('------------------------------')
    print(structure)
    print('------------------------------')
    print('')
    book = input("What book do you want to issue?: ")
    issue_person = input("Who do you want to issue the book to: ")
    if structure['Book Name'].str.contains(book).any():
        structure.set_index('Book Name', inplace=True)
        structure.loc[book, 'Issue Date'] = today
        structure.loc[book, 'Availability'] = 'No'
        structure.loc[book, 'Issued To'] = issue_person
        structure.loc[book, 'Return Date'] = return_time
        structure.reset_index(inplace=True)
        print('------------------------------')
        print('Book issued Successfully')
        print('------------------------------')

    else:
        print('------------------------------')
        print("Wrong Input, Enter book index again!")
        print('------------------------------')
        issue_book()


def user_issued():
    user = input("Enter user to view: ")
    global structure
    if structure['Issued To'].str.contains(user).any():  # Checks if the user is in the 'Issued To' box
        check = structure[structure['Issued To'] == user]
        print(check)
    else:
        print('No book issues to specifies person')
        print('')
        print('Enter again')
        user_issued()


def book_issued():
    book = input("Enter Book to view: ")
    global structure
    if structure['Book Name'].str.contains(book).any():
        check = structure[structure['Book Name'] == book]
        print(check)
    else:
        print('------------------------------')
        print('No book with the name', book, 'found in the database')
        print('------------------------------')
        print('Enter Again')
        book_issued()


def return_book():
    global structure
    returned_book = input("Enter book name to return: ")
    if structure['Book Name'].str.contains(returned_book).any():
        structure.set_index('Book Name', inplace=True)
        structure.loc[returned_book, 'Availability'] = 'Yes'
        structure.loc[returned_book, 'Issue Date'] = 'None'
        structure.loc[returned_book, 'Return Date'] = 'None'
        structure.loc[returned_book, 'Issued To'] = 'None'
        structure.reset_index(inplace=True)
    else:
        print('------------------------------')
        print('Invalid Book')
        print('------------------------------')
        print('Enter Again')
        return_book()


def available():
    global structure
    print(structure[structure['Availability'] == 'Yes'])


def delete_book():
    global structure
    delete = input("Which book do you want to delete?: ")
    if structure['Book Name'].str.contains(delete).any():
        structure.set_index('Book Name', inplace=True)
        structure = structure.drop(delete)
        structure.reset_index(inplace=True)
    else:
        print("Invalid Entry")
        print("Enter Again")
        delete_book()


print('-------------------------------------------------')
print('-                    -')
print('-  Welcome to Library Management System  -')
print('-                    -')
print('-------------------------------------------------')
print('')

while True:

    print("""           Enter 'A' to add a new book
           Enter 'I' to issue a book
           Enter 'U' to view issued books to the user
           Enter 'R' to return a book
           Enter 'V' to view available books
           Enter 'B' to view status of a book
           Enter 'D' to view database
           Enter 'X' to delete a book
           Enter 'Q' to exit
           Enter 'S' to save file""")

    print('-------------------------------------------------')
    choice = input("What would you like to do: ")
    print('-------------------------------------------------')
    if choice == 'A':
        add_book()
        print(structure)
        print('-------------------------------------------------')
    elif choice == 'I':
        issue_book()
        print('-------------------------------------------------')
    elif choice == 'U':
        user_issued()
        print('-------------------------------------------------')
    elif choice == 'R':
        return_book()
        print('-------------------------------------------------')
    elif choice == 'B':
        
        book_issued()
        print('-------------------------------------------------')
    elif choice == 'D':
        print(structure)
        print('-------------------------------------------------')
    elif choice == 'X':
        delete_book()
        print('-------------------------------------------------')
    elif choice == 'V':
        available()
        print('-------------------------------------------------')
    elif choice == 'S':
        structure.to_csv('Book1.csv', index=False)
        count = count + 1
    elif choice == 'Q':
        if count == 0:
            print("You have not saved your work. Do you want to exit anyways?:")
            decision = input(">")
            if decision == 'Yes':
                break
            else:
                continue
        else:
            break
    else:
        print('-------------------------------------------------')
        print('Invalid choice, Choose again')
        print('-------------------------------------------------')

print("Good Bye")
```


![Flow Chart LMS_1](https://user-images.githubusercontent.com/72499151/157419274-9ecbf25c-e9c7-45b3-b26e-7af4ce6f8c4f.jpg)
![FLow lms_1](https://user-images.githubusercontent.com/72499151/157419223-40bf362a-ccfa-4a22-a363-58e22ca6deb1.jpg)
![Flow Chart LMS_2](https://user-images.githubusercontent.com/72499151/157419286-5a2ddd98-dfea-465f-a448-b7158cf8d147.jpg)
