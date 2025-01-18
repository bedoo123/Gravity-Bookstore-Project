# End-to-End Data Warehouse Solution

## OLTP Database: Gravity Bookstore

Gravity Bookstore is an online transactional processing (OLTP) database designed for a fictional bookstore. It captures and manages detailed information about books, customers, and sales transactions. We will be utilizing this database for our project.
You can find the database

### Tables Description

- **book**: A list of all books available in the store.
- **book_author**: Stores the authors for each book, which is a many-to-many relationship.
- **author**: A list of all authors.
- **book_language**: A list of possible languages of books.
- **publisher**: A list of publishers for books.
- **customer**: A list of the customers of the Gravity Bookstore.
- **customer_address**: A list of addresses for customers, as a customer can have more than one address, and an address has more than one customer.
- **address_status**: A list of statuses for an address, because addresses can be current or old.
- **address**: A list of addresses in the system.
- **country**: A list of countries that addresses are in.
- **cust_order**: A list of orders placed by customers.
- **order_line**: A list of books that are a part of each order.
- **shipping_method**: The possible shipping methods for an order.
- **order_history**: The history of an order, such as ordered, cancelled, delivered.
- **order_status**: The possible statuses of an order.

### Tables Contents

1. **book**
   - **Primary Key**: `book_id`
   - **Attributes**: `title`, `isbn13`, `language_id`, `num_pages`, `publication_date`, `publisher_id`.

2. **book_author**
   - **Composite Primary Key**: `book_id`, `author_id`

3. **Author Table**
   - **Primary Key**: `author_id`
   - **Attributes**: `title`, `author_name`

4. **book_language**
   - **Primary Key**: `language_id`
   - **Attributes**: `title`, `language_code`, `language_name`.

5. **Publisher**
   - **Primary Key**: `publisher_id`
   - **Attributes**: `title`, `publisher_name`.

6. **Customer**
   - **Primary Key**: `customer_id`
   - **Attributes**: `title`, `first_name`, `last_name`, `email`

7. **customer_address**
   - **Primary Key**: `customer_id`, `address_id`
   - **Attributes**: `status_id`

8. **address_status**
   - **Primary Key**: `status_id`
   - **Attributes**: `address_status`

9. **address**
   - **Primary Key**: `address_id`
   - **Attributes**: `street_number`, `street_name`, `city`, `country_id`

10. **country**
    - **Primary Key**: `country_id`
    - **Attributes**: `country_name`

11. **cust_order**
    - **Primary Key**: `order_id`
    - **Attributes**: `order_date`, `customer_id`, `shipping_method_id`, `des_address_id`

12. **order_line**
    - **Primary Key**: `line_id`
    - **Attributes**: `order_id`, `book_id`, `price`

13. **shipping_method**
    - **Primary Key**: `method_id`
    - **Attributes**: `method_name`, `cost`

14. **order_history**
    - **Primary Key**: `history_id`
    - **Attributes**: `order_id`, `status_id`, `status_date`

15. **order_status**
    - **Primary Key**: `status_id`
    - **Attributes**: `status_value`

### ERD

We created this database in SQL Server Management Studio (SSMS). By correctly connecting the tables, we generated the following Entity-Relationship Diagram (ERD): (https://github.com/KhalidEltaweel/Gravity-Bookstore-Project/blob/main/Task%201/OLTP%20ERD/SSMS%20Screenshot.png)

For a clearer view of the ERD diagram, you can download the attached `.drawio` file and open it on [this Website][def2].

[def]: https://github.com/bbrumm/databasestar/tree/main/sample_databases/sample_db_gravity/gravity_sqlserver

## Data Warehouse
### Data Modeling:
- **Star Schema:**
![DWH Schema](https://github.com/KhalidEltaweel/Gravity-Bookstore-Project/blob/main/DWH%20model/star_schema.jpg)

### Schema and Tables:
#### Tables Overview:

##### Fact_book Table:
**Fields:**
   - `order_id_SK` (Primary Key)
   - `order_id` (Business Key)
   - `book_id` (Foreign Key)
   - `customer_id` (Foreign Key)
   - `shipping_id` (Foreign Key)
   - `date_id` (Foreign Key)
   - `order_status`(dd)
   - `order_status`(Foreign Key)
   - `shipped_date`(Foreign Key)
   - `delivery_date`(Foreign Key)
   - `total price` (measure)
   - `shipping_cost` (measure)
   - `quantity_sold` (measure)


##### Dim_shipping Table:
 **Fields:**
   - `shipping_SK` (Primary Key)
   - `shipping_bk` (Business Key)
   - `shipping_method`
   - `start_date`
   - `end_date`
   - `is_current`

##### Dim_Book Table:
 **Fields:**
   - `book_id_SK` (Primary Key)
   - `book_id_BK` (Business Key)
   - `isbn13`
   - `title`
   - `publication_date`
   - `num_pages`
   - `language_name`
   - `publisher_name`
   - `author_name`
   - `price`
   - `start_date`
   - `end_date`
   - `is_current`

##### Dim_Customer Table:
 **Fields:**
   - `customer_id_SK` (Primary Key)
   - `customer_id_BK` (Business Key)
   - `first_name`
   - `last_name`
   - `email`
   - `country`
   - `street_name`
   - `city`
   - `address_status`
   - `start_date`
   - `end_date`
   - `is_current`

##### Dim_Date Table:
 **Fields:**
   - `date_Id_SK` (Primary Key)
   - `year`
   - `month`
   - `day`
   - `quarter`
   - `dayofweek`


#### Schema Architecture:

- Each table is connected to the **Fact_book** table via foreign keys, forming a **star schema**.
- The **Fact_book** table is positioned at the center, surrounded by dimension tables (**Dim_shipping, Dim_Book, Dim_Customer, Dim_Date**).
- This star schema architecture facilitates efficient querying and data analysis, a common practice in Data Warehousing.
