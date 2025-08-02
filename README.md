# Book-Store-Analysis-SQL-Project

## Project Overview
This project demonstrates SQL skills in database design, data import/export, and complex query writing with joins, aggregations, and filtering. The queries range from basic retrievals to advanced analytics, providing insights into sales performance and inventory management. It serves as a practical example of how to analyze e-commerce data for business intelligence purposes.

## Objectives

1. **Set up a book sales database**: Create and populate a books sales database with the provided sales data.
2. SQL querying for business insights (sales trends, inventory, customer behavior).
3. Advanced analytics (best-selling books, top customers, stock management).

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `onlinebookstoredb`.
- **Table Creation**: Three key tables: `Books`, `Customers` , and `Orders`. are created to store the sales data.


```sql
CREATE DATABASE onlinebookstoredb;

CREATE TABLE Books (
    Book_ID SERIAL PRIMARY KEY,
    Title VARCHAR (100),
    Author VARCHAR (100),
    Genre VARCHAR (50),
    Published_Year INT,
    Price NUMERIC (10, 2),
    Stock INT
);

CREATE TABLE Customers (
    Customer_ID SERIAL PRIMARY KEY,
    Name VARCHAR (100),
    Email VARCHAR(100),
    Phone VARCHAR (15),
    City VARCHAR (50),
    Country VARCHAR (150)
);

CREATE TABLE Orders (
    Order_ID SERIAL PRIMARY KEY,
    Customer_ID INT REFERENCES Customers (Customer_ID),
    Book_ID INT REFERENCES Books (Book_ID),
    Order_Date DATE,
    Quantity INT,
    Total_Amount NUMERIC (10, 2)
);
```

### 2. Data Importing

```sql
SELECT
    *
FROM
    Books;

COPY Books (
    Book_ID,
    Title,
    Author,
    Genre,
    Published_Year,
    Price,
    Stock
)
FROM
    'D:\sql project\Books. csv' CSV HEADER;


SELECT
    *
FROM
    Customers;

COPY Customers (Customer_ID, Name, Email, Phone, City, Country)
FROM
    'D:\sql project\Customers. csv' CSV HEADER;


SELECT
    *
FROM
    Orders;

FROM
    'D:\sql project\Orders. csv' COPY Orders (
        Order_ID,
        Customer_ID,
        Book_ID,
        Order_Date,
        Quantity,
        Total_Amount
    ) CSV HEADER;

```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

```sql
--1) Retrieve all books in the 'fiction' genre:
SELECT
    *
FROM
    Books
Where
    Genre = 'Fiction';

--2) Find books published after the year 1950:
SELECT
    *
FROM
    Books
WHERE
    Published_year > 1950;

--3) List all the customers from canada:
SELECT
    *
FROM
    Customers
WHERE
    country = 'Canada';

--4) Show orders placed in november 2023:
SELECT
    *
FROM
    Orders
Where
    order_date BETWEEN '2023-11-01'
    AND '2023-11-30';

--5)Retrieve all stock of books available:
SELECT
    SUM(stock) AS Total_Stock
FROM
    Books;

--6) Find the details of most expensive book:
SELECT
    *
FROM
    Books
ORDER BY
    Price DESC
LIMIT
    1;

--7) Show all customers who ordered more then 1 quantity of a book:
SELECT
    *
FROM
    Orders
WHERE
    quantity > 1;

--8) List all genres available in the books table:
SELECT
    DISTINCT genre
FROM
    Books;

--Advance questions:
--1) RETRIEVE THE TOTAL NUMBER OF BOOKS SOLD FOR EACH GENRE:
SELECT
    *
FROM
    Orders;

SELECT
    b.Genre,
    SUM (o.Quantity) AS Total_Books_Sold
FROM
    Orders o
    JOIN Books b ON o.book_id = b.book_id
GROUP BY
    b.Genre;

--2) Find the average price of books in the 'Fantacy' genre:
SELECT
    AVG(price) AS Average_Price
FROM
    Books
WHERE
    Genre = 'Fantasy';

--3) List customers who hve placed t least 2 orders:
SELECT
    customer_id,
    COUNT(Order_id) AS ORDERS_COUNT
FROM
    orders
GROUP BY
    customer_id
HAVING
    COUNT(Order_id) >= 2;

-- 4) Find the most frequently ordered book: 
SELECT
    o.Book_id,
    b.title,
    COUNT (O.order_id) AS ORDER_COUNT
FROM
    orders o
    JOIN books b ON o.book_id = b.book_id
GROUP BY
    o.book_id,
    b.title
ORDER BY
    ORDER_COUNT DESC
LIMIT
    1;

-- 5) Retrieve the total quantity of books sold by each author: 
SELECT
    b.author,
    SUM(o.quantity) AS Total_Books_Sold
FROM
    orders o
    JOIN books b ON o.book_id = b.book_id
GROUP BY
    b.Author;

-- 6) List the cities where customers who spent over $30 are located:
SELECT
    DISTINCT C.city,
    total_amount
FROM
    orders o
    JOIN customers c ON o.customer_id = c.customer_id
WHERE
    o.total_amount > 30;

-- 7) Find the customer who spent the most on orders:
SELECT
    c.customer_id,
    c.name,
    SUM(o.total_amount) AS Total_Spent
FROM
    orders o
    JOIN customers c ON o.customer_id = c.customer_id
GROUP BY
    C.customer_id,
    c.name
ORDER BY
    Total_spent Desc
LIMIT
    1;

--8) Calculate the stock remaining after fulfilling all orders: 
SELECT
    b.book_id,
    b.title,
    b.stock,
    COALESCE (SUM (o.quantity), 0) AS Order_quantity,
    b.stock - COALESCE (SUM (o.quantity), 0) AS Remaining_Quantity
FROM
    books b
    LEFT JOIN orders o ON b.book_id = o.book_id
GROUP BY
    b.book_id
ORDER BY
    b.book_id;

```

## Key Findings
- Sales Trends: Identifies top genres and authors by sales volume.
- Customer Behavior: Highlights frequent buyers and high-spending customers.
- Stock Control: Tracks remaining inventory after order fulfillment.
- Data Quality: Includes checks for NULL values and data consistency.


## Conclusion

This project demonstrates end-to-end SQL database management, from schema design to business intelligence. It serves as a learning resource for SQL beginners and a foundation for scaling an e-commerce bookstore.

## Report Generated By: - [Shanza Waheed]

Follow me on social media
- **YouTube**: (https://www.youtube.com/@Shanza-Waheed)
- **Instagram**: (https://www.instagram.com/shanza_waheed_)
