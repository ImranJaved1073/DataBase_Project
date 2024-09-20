# SuperMarket Inventory Management System

Efficient management of a supermarket is vital for ensuring smooth operations, customer satisfaction, and profitability. In the modern era, technological advancements have significantly transformed the retail landscape, yet many supermarkets grapple with outdated management systems that hinder their ability to meet evolving consumer demands and streamline internal processes.
## Table of Contents
1. [Introduction](#introduction)
2. [ERD Model](#erd-model)
3. [Attributes of Entities](#attributes-of-entities)
4. [Connectivity Table](#connectivity-table)
5. [ERD Diagram](#erd-diagram)
6. [Relational Schema (Normalization)](#relational-schema-normalization)
7. [Relations Description](#relations-description)
8. [SQL Statements for Table Creation](#sql-statements-for-table-creation)
9. [Designing Views](#designing-views)
10. [Relational Data Model (Dependency Diagram)](#relational-data-model-dependency-diagram)
11. [SQL Statements for Common Report](#sql-statements-for-common-report)
12. [Demonstrating Functions, Procedures, and Triggers](#demonstrating-functions-procedures-and-triggers)

## Introduction
Efficient management of a supermarket is vital for ensuring smooth operations, customer satisfaction, and profitability. In the modern era, technological advancements have significantly transformed the retail landscape, yet many supermarkets grapple with outdated management systems that hinder their ability to meet evolving consumer demands and streamline internal processes.
## ERD Model
Details the Entity-Relationship Diagram for the system.

## Attributes of Entities
Lists and describes the attributes of each entity in the system.

## Connectivity Table
Explains the relationships and cardinality between different entities.

## ERD Diagram
Provides the graphical representation of the ERD.
![image](https://github.com/user-attachments/assets/c8a3bfe8-380e-4263-a92b-ee4fd32f1901)


## Relational Schema (Normalization)
Describes the normalization process and the resulting relational schema.

## 1. First Normal Form (1NF):
For a relation to be in first normal form (1NF), there must not exist any multi-valued attribute within the relation. In the given relation there exist multi value attributes so the relation is not in 1NF. Now we have to remove multi value attributes in another relation.

- Product (Product_id, Product_name, Product_description, Quantity, UnitPrice, SalePrice, Category_id, Category_name)

- StockEntry (Supplier_id, Product_id, inDate, Quantity, Supplier_Name, Supplier_Address, Supplier_Contact, Supplier_CNIC)
      Product_id is foreign key refers to Product

- Order (Product_id, order_no, Customer_id, customer_name, Customer_Address, Customer_Contact, customer_CNIC, Cashier_id, Cashier _Name, Cashier _Address, Cashier _Contact, Cashier _CNIC, password, Total_Bill, orderDate, Order_Qty, status, Total_Price, order_price)
Product_id is foreign key refers to Product


## 2. Second Normal Form (2NF):
A table is in 2nd Normal Form (2NF) if it is in 1st Normal Form (1NF) and there must not be any partial dependencies, means that for each non-key attribute, its value must be dependent only on the entire candidate key and not on a part of it.

In above StockEntry relation Supplier_id which is part of composite primary key seperately identifying Supplier_Name, Supplier_Address, Supplier_Contact and Supplier_CNIC so we create sepearte relation Supplier and make Supplier_id fk in above relation and similarly for
Order Relation.

- Product (Product_id, Product_name, Product_description, Quantity, UnitPrice, SalePrice, Category_id, Category_name)

- StockEntry (Supplier_id, Product_id, inDate, Quantity)
Product_id and Supplier_id are foreign keys refers to Product and Supplier respectively

- Supplier (Supplier_id, Supplier_Name, Supplier_Address, Supplier_Contact, Supplier_CNIC)

- Order (order_no, Customer_id, customer_name, Customer_Address, Customer_Contact, customer_CNIC, Cashier_id, Cashier _Name, Cashier _Address, Cashier _Contact, Cashier _CNIC, password, Total_Bill, orderDate, status)

- OrderDetail (Product_id, order_no, Order_Qty, Total_Price, order_price)
Product_id and Order_no are foreign keys refers to Product and Order respectively


## 3. Third Normal Form (3NF):
For a table to be in third normal form, it must be in 2nd normal form and there must not exist any transitive functional dependency in the relation which states that:
Non-prime attributes → non-prime attributes OR A →B and B →C
In Product relation Product_id →Category_id and Category_id →Category_name
In Order relation Order_no →Customer_id and Customer_id →Customer_name, address, cnic, etc.
Also Order_no →Cashier_id and Cashier_id →Cashier_name, address, cnic, etc.
Hence seperating these entities.

- Product (Product_id, Product_name, Product_description, Quantity, UnitPrice, SalePrice, Category_id)
Category_id is foreign key refers to Category

- Category (Category_id, Category_name)

- StockEntry (Supplier_id, Product_id, inDate, Quantity)
Product_id and Supplier_id are foreign keys refers to Product and Supplier respectively

- Supplier (Supplier_id, Supplier_Name, Supplier_Address, Supplier_Contact, Supplier_CNIC)

- Order (order_no, Customer_id, Cashier_id, Total_Bill, orderDate, status)
Customer_id, Cashier_id are foreign keys refers to Customer, Cashier

- Customer (Customer_id, customer_name, Customer_Address, Customer_Contact, customer_CNIC)
- Cashier (Cashier_id, Cashier _Name, Cashier _Address, Cashier _Contact, Cashier _CNIC, password)

- OrderDetail (Product_id, order_no, Order_Qty, Total_Price, order_price)
Product_id and Order_no are foreign keys refers to Product and Order respectively


## Relations Description
Details the relations and constraints applied to the database tables.

## SQL Statements for Table Creation
Provides the SQL statements used to create the database tables.

### Employee
      create table Employee (
                employee_id number(10) primary key,
                name varchar2(50) not null,
                address varchar(250),
                contact varchar(20) not null,
                cnic varchar2(15)
            );

### Supplier
      create table supplier (
    supplier_id number(10) primary key,
    name varchar2(50) not null,
    address varchar(250),
    contact varchar(20) not null,
    cnic varchar2(15)
    );

### Category
      create table category (
          cat_id number(10) primary key,
          cat_name varchar(30) not null
      );

### Product
      CREATE TABLE Product (
          Product_id NUMBER(10) PRIMARY KEY,
          Product_name VARCHAR2(50) not null,
          Product_desc   VARCHAR2(150),
          Quantity number(10) not null,
          UnitPrice number(10, 2),
          SalePrice number(10, 2) not null,
          Cat_id number(10),
          Supplier_id number not null,
          FOREIGN KEY (Cat_id) REFERENCES Category(Cat_id),
          FOREIGN KEY (Supplier_id) REFERENCES Supplier(Supplier_id)
      );

### StocksEntry
      create table stocksEntry (
          product_id number(10) not null,
          quantity int not null,
          indate date default sysdate,
          supplier_id  number(10) not null,
          FOREIGN KEY (product_id) REFERENCES Product(Product_id),
          FOREIGN KEY (Supplier_id) REFERENCES Supplier(Supplier_id),
          PRIMARY KEY (product_id,Supplier_id,indate));	

### Cashier
      create table cashier (
          cashier_id number(10) primary key,
          foreign key (cashier_id) references Employee(employee_id),
          password varchar(25) not null
      );

### Orders
      create table Orders (
          order_no number(10) primary key,
          customer_id number(10),
          cashier_id number(10),
          total_bill number(10, 2),
          orderdate date default sysdate,
          status varchar2(15),
          constraint order_cusID_fk foreign key (customer_id) references customer(customer_id),
          constraint order_casID_fk foreign key (cashier_id) references cashier(cashier_id),
          constraint order_status_ck check(status IN('Pending','Paid','Cancelled')),
          constraint order_totalbill_ck check(total_bill >= 0)
      );

### Orderdetail
      create table orderdetail (
          order_no number(10) not null,
          product_id number(10) not null,
          quantity number(10) not null,
          discount number(10,2),
          total_price number(10,2),
          price number(10,2),
          foreign key (order_no) references orders(order_no),
          foreign key (product_id) references product(product_id),
          primary key (order_no, product_id),
          constraint trans_qty_ck check(quantity >= 0),
          constraint trans_price_ck check(price >= 0),
          constraint trans_disc_ck check(discount >=0 AND discount < 0.5*price)
      );

### customer
      create table customer (
          customer_id number (10) primary key,
          name varchar2(50) not null,
          address varchar(250),
          contact varchar(20) not null,
          cnic varchar2(15)
      );



## Designing Views
Describes the views created for the system for better data representation.

## Relational Data Model (Dependency Diagram)
Shows the dependency diagram for the relational data model.

## SQL Statements for Common Report
Provides SQL statements used for generating common reports.

## Demonstrating Functions, Procedures, and Triggers
Describes the functions, procedures, and triggers implemented in the database.
