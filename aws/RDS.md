# Relational database

A relational database stores data in the form of tables.  
Each table has:  
rows  
columns  

And tables can be connected to each other using relationships.  

Examples:  
MySQL  
PostgreSQL  
Oracle  
SQL Server  

---

# Non-relational database

A non-relational database does not strictly store data in table-to-table relational form.  
It can store data as:  
documents  
key-value pairs  

---

## 1. Relational database in simple words

Suppose you have an ecommerce app.  
You may have separate tables like:  

### Users table

user_id  
name  
email  
1  
Ram  
ram@gmail.com  

### Orders table

order_id  
user_id  
product  
101  
1  
Laptop  

Here:  
Users and Orders are related  
user_id connects both tables  

That is why it is called relational database.  

### Main idea

Data is stored in structured tables, and relationships are maintained between tables.

---

## 2. Non-relational database in simple words

In a non-relational database like MongoDB, the same data may look like this in one document:

```json
{
 "name": "Ram",
 "email": "ram@gmail.com",
 "orders": [
   {
     "order_id": 101,
     "product": "Laptop"
   }
 ]
}
