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

We created this database in SQL Server Management Studio (SSMS). By correctly connecting the tables, we generated the following Entity-Relationship Diagram (ERD): 
![ERD Schema](https://github.com/KhalidEltaweel/Gravity-Bookstore-Project/blob/main/ERD/OLTP%20ERD/SSMS%20Screenshot.png)

For a clearer view of the ERD diagram, you can download the attached `.drawio` file and open it on [this Website][def2].

[def]: https://github.com/bbrumm/databasestar/tree/main/sample_databases/sample_db_gravity/gravity_sqlserver

## Data Warehouse
### Data Modeling:
- **Star Schema:**
![DWH Schema](https://github.com/KhalidEltaweel/Gravity-Bookstore-Project/blob/main/DWH%20model/data_model.jpg)

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

##### Dim_status Table:
 **Fields:**
 - `status_SK` (Primary Key)
 - `order_status_id` (business Key)
 - `order_status`


#### Schema Architecture:

- Each table is connected to the **Fact_book** table via foreign keys, forming a **star schema**.
- The **Fact_book** table is positioned at the center, surrounded by dimension tables (**Dim_shipping, Dim_Book, Dim_Customer, Dim_Date**).
- This star schema architecture facilitates efficient querying and data analysis, a common practice in Data Warehousing.

####  Why This Approach is Optimal:

Alignment with Analytical Use Cases:
The star schema is specifically designed for analytical workloads, such as aggregating sales data, analyzing trends, and generating reports. It simplifies complex queries and allows for fast data retrieval, which is critical for business intelligence and decision-making.

Centralized Fact Table:
The fact_book_sales table acts as the single source of truth for transactional data (e.g., sales orders). This centralization ensures consistency and simplifies the process of joining data with dimensions for analysis.

Dimension Tables for Context:
Dimension tables (dim_book, dim_customer, dim_shipping, etc.) provide the necessary context for the facts. For example, you can easily analyze sales by book, customer, or shipping method by joining the fact table with the relevant dimension.

Scalability for Large Datasets:
The schema is designed to handle large volumes of data efficiently. The fact table stores transactional data, while dimension tables store descriptive attributes, reducing redundancy and improving storage efficiency.

Historical Data Tracking:
The inclusion of start_date, end_date, and is_current fields in dimension tables (e.g., dim_customer, dim_shipping) allows you to track changes over time (slowly changing dimensions). This is essential for historical analysis and trend identification.

Benefits of the Star Schema for Your Model:
Improved Query Performance:
Star schemas are optimized for read-heavy operations. Queries often involve simple joins between the fact table and dimension tables, which are typically smaller and indexed, resulting in faster query execution.

Ease of Use for Analysts:
The schema is intuitive and easy to understand, making it accessible for analysts and business users who may not have deep technical expertise. This reduces the learning curve and speeds up report generation.

Flexibility in Analysis:
You can easily slice and dice data across multiple dimensions (e.g., sales by year, customer, book, or shipping method). This flexibility is crucial for answering a wide range of business questions.

Reduced Data Redundancy:
Dimension tables are normalized, reducing redundancy and ensuring consistent data storage. For example, customer details are stored once in dim_customer and referenced in the fact table via a foreign key.

Support for Aggregation:
The schema is well-suited for aggregating data (e.g., total sales, average order value, sales by region), which is a common requirement in analytical workloads.
Advantages of the Star Schema

Simplicity:
The structure is easy to design, implement, and maintain compared to more complex schemas like snowflake or normalized schemas.
Fast Query Performance:
Optimized for read-heavy operations, making it ideal for analytical queries.

Scalability:
Can handle large datasets efficiently, especially when combined with modern data warehouse technologies (e.g., columnar storage, partitioning).
Business-Friendly:

The schema aligns well with how business users think about data (e.g., sales by customer, book, or time period).
Historical Analysis:
Supports slowly changing dimensions (SCDs), enabling historical tracking and trend analysis.
Disadvantages of the Star Schema
Data Redundancy in Dimensions:

While dimension tables are normalized, they can still contain redundant data if not carefully managed. For example, if a book’s author appears in multiple rows, there could be duplication.
Limited Flexibility for Complex Relationships:
Star schemas are not ideal for modeling complex relationships between dimensions. If your use case requires multi-level hierarchies or many-to-many relationships, a snowflake schema might be more appropriate.

Storage Overhead:
The denormalized nature of the fact table can lead to large storage requirements, especially for high-volume transactional data.
Maintenance Challenges:
Managing slowly changing dimensions (SCDs) can be complex, particularly when tracking historical changes over time.
Not Ideal for Transactional Systems:
Star schemas are designed for analytical workloads, not transactional systems. If your use case involves frequent updates or complex transactions, a normalized schema would be more suitable.

When to Use the Star Schema:
1-Your primary goal is analytical reporting and business intelligence.
2-You need fast query performance for aggregations and joins.
3-Your data relationships are relatively simple and hierarchical.
4-You want a schema that is easy for business users to understand and use.

Avoid it when:
Your use case involves complex, multi-level relationships between dimensions.
You need to support frequent updates or transactional workloads.
Storage efficiency is a critical concern, and you cannot tolerate redundancy.
--------------------------------------------------------------------------------
Conclusion:
Your star schema is an excellent choice for a book sales analysis model. It provides the simplicity, performance, and flexibility needed for analytical workloads while supporting historical data tracking and scalability. However, be mindful of its limitations, such as potential data redundancy and challenges with complex relationships. By leveraging modern data warehouse technologies (e.g., columnar storage, partitioning, and indexing), you can further optimize the performance and scalability of your star schema.
