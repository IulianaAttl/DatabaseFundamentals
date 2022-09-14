# DatabaseProjects

### Assignment.accdb
For this assignment, I have decided to design a database management system for a music instrument manufacturing company. This company has stores in multiple locations selling instruments they manufacture. My application gives a view to the managers about their categories of instruments, the instruments themselves, stores, employees, customers, and sales. 

There are five main categories of instruments. Strings, woodwind, brass, percussion, and keyboards. The user of the application can find out the materials in which each instrument is available by searching for the category.

Each instrument must have a unique number by which it can be identified as well as having one of the five categories and belonging to only one store. This application also stores other information about the instruments such as the instrument name, quantity currently in stock, the origin (where the instrument has been manufactured), and the price of each one.

The stores have a unique id by which they can be identified, the address, phone number, the opening and closing times, the manager’s name, and the email of the store.
All employees must have a unique id. They must all work in only one store. There should not be a store without any employees. This application has records of employees' first name, last name, phone number, address, email, date of birth, salary, and their gender.

The instruments company must also be able to keep track of their customers. Each customer has a unique number by which they can be identified, their first name, last name, address, phone number, date of birth, and gender. 

Finally, and most importantly, this application is designed for the owners of the company to see and keep track of their completed sales. Each sale must have a unique identification number. A sale is complete if it has one or more instruments, if it belongs to one and only one store, if it has been managed by one and only one employee and made by one and only one customer. This entity also stores other information such as the date and time the sale has been made and the method of payment used by the customer.

## DDL - Data Definition Language

### CATEGORIES TABLE  
CREATE TABLE Categories(  
CategoryName VARCHAR (50) UNIQUE NOT NULL PRIMARY KEY,  
MaterialsAvailable VARCHAR(50 ) NOT NULL  
);  

### INSTRUMENTS TABLE  
CREATE TABLE Instruments(  
InstrumentID COUNTER PRIMARY KEY UNIQUE NOT NULL,  
CategoryName VARCHAR (50) REFERENCES Categories(CategoryName) NOT NULL,  
StoreID INT NOT NULL,  
InstrumentName VARCHAR (50) NOT NULL,  
QuantityInStock INT NOT NULL DEFAULT 0,  
Origin VARCHAR (50) NOT NULL,  
PriceEach CURRENCY NOT NULL DEFAULT 0.00,  
CONSTRAINT fkStores FOREIGN KEY (StoreID) REFERENCES Stores ON DELETE CASCADE ON UPDATE CASCADE  
);  

### STORES TABLE  
CREATE TABLE Stores(  
StoreID COUNTER PRIMARY KEY UNIQUE NOT NULL,  
Address VARCHAR (50) NOT NULL,  
Phone CHAR (10) NOT NULL UNIQUE,  
OpeningTime TIME NOT NULL,  
ClosingTime TIME NOT NULL,  
Manager VARCHAR (50) NOT NULL,  
Email VARCHAR (50) NOT NULL UNIQUE  
);  

### EMPLOYEES TABLE  
CREATE TABLE Employees(  
EmployeeID COUNTER UNIQUE PRIMARY KEY NOT NULL,  
StoreID INT NOT NULL,  
FirstName VARCHAR (50) NOT NULL,  
LastName VARCHAR (50) NOT NULL,  
Phone CHAR (10) NOT NULL UNIQUE,  
Address VARCHAR(50) NOT NULL,  
Email VARCHAR (50) NOT NULL UNIQUE,  
DOB DATE NOT NULL,  
Salary CURRENCY NOT NULL DEFAULT 0.00,  
Gender CHAR (1) NOT NULL,  
CONSTRAINT fkStores FOREIGN KEY (StoreID) REFERENCES Stores ON DELETE CASCADE ON UPDATE CASCADE,  
CONSTRAINT genderCheck CHECK (Gender = 'M' OR Gender = 'F')  
);  

### CUSTOMERS TABLE  
CREATE TABLE Customers(  
CustomerID COUNTER UNIQUE PRIMARY KEY NOT NULL,  
FirstName VARCHAR(50) NOT NULL,  
LastName VARCHAR(50) NOT NULL,  
Address VARCHAR(50) NOT NULL,  
Phone CHAR(10) NOT NULL UNIQUE,  
DOB DATE NOT NULL,  
Email VARCHAR(50) NOT NULL UNIQUE,  
Gender CHAR(1) NOT NULL  
CONSTRAINT genderCheck CHECK (Gender = 'M' OR Gender = 'F')  
);  

### SALES TABLE  
CREATE TABLE Sales(  
SaleID COUNTER UNIQUE PRIMARY KEY NOT NULL,  
InstrumentID INT NOT NULL,  
CustomerID INT NOT NULL,  
EmployeeID INT NOT NULL,  
StoreID INT NOT NULL,  
Dates DATE NOT NULL,  
Times TIME NOT NULL,  
Method CHAR (4),  
CONSTRAINT fkStores FOREIGN KEY (StoreID) REFERENCES Stores ON DELETE CASCADE ON UPDATE CASCADE,  
CONSTRAINT fkEmployees FOREIGN KEY (EmployeeID) REFERENCES Employees ON DELETE CASCADE ON UPDATE CASCADE,  
CONSTRAINT fkCustomers FOREIGN KEY (CustomerID) REFERENCES Customers ON DELETE CASCADE ON UPDATE CASCADE,  
CONSTRAINT fkInstruments FOREIGN KEY (InstrumentID) REFERENCES Instruments ON DELETE CASCADE ON UPDATE CASCADE,  
CONSTRAINT methodCheck CHECK (Method = 'Cash' OR Method = 'Card')  
);  
