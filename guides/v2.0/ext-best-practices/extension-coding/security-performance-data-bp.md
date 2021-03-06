---
layout: default
group: ext-best-practices
subgroup: 02_Extension-Coding
title: Security, Performance, and Data Handling
menu_title: Security, performance, and data handling
menu_order: 3
github_link: ext-best-practices/extension-coding/security-performance-data-bp.md

---

##{{page.menu_title}}
{:.no_toc}

You should make sure that your extension handles data with care in order to prevent sensitive information from being exposed. Incorrect handling of data requests or class usage can negatively impact your extension and create security vulnerabilities. Consider applying the following best practices to your extension to improve performance and security.

### Content
{:.no_toc}

* Table of Content
{:toc}

---

### Avoid Using Low-Level Functionality
  Be sure not to use low-level functionality that is explicitly prohibited by the framework or specification under which the software is supposed to operate. The use of low-level functionality can violate the specification in unexpected ways that effectively disable built-in protection mechanisms, introduce exploitable inconsistencies, or otherwise expose the functionality to attack.

### Use Wrappers Instead of Superglobal Variables
  Make sure that your Magento application uses Magento wrapper objects, and does not directly use PHP superglobals:
  ```
  $GLOBALS, $_SERVER, $_GET, $_POST, $_FILES, $_COOKIE, $_SESSION, $_REQUEST, $_ENV
  ```
  .

### Use the Correct MySQL Data Types
  MySQL offers a range of numeric, string, and time data types. If you are storing a date, use a DATE or DATETIME field. Using an INTEGER or STRING can make SQL queries more complicated, if not impossible. It is often tempting to invent your own data formats; for example, storing serialized PHP objects in string. Database management may be easier, but MySQL will become a dumb data store and it may lead to problems later.

### Get the Correct Data from the Correct Object
  Be sure to retrieve data from the correct object. For example, get SKU data from the Product instead of the Order object.

### Avoid Raw SQL Queries
  Raw SQL queries can lead to potential security vulnerabilities and database portability issues. Use data adapter capabilities (Varien_Db_Adapter_Pdo_Mysql by default) to build and execute queries and move all data access code to a resource model. Use prepared statements to make sure that queries are safe to execute.

### Use Well-Defined Indexes
  Foreign keys should have indexes. If you're using a field in a WHERE clause of an SQL query you should have an index on it. Such indexes should cover multiple columns based on the queries needed. As a general rule of thumb, indexes should be applied to any column named in the WHERE clause of a SELECT query.

  For example, assume we have a user table with a numeric ID (the primary key) and an email address. During log on, MySQL must locate the correct ID by searching for an email. With an index, MySQL can use a fast search algorithm to locate the email almost instantly. Without an index, MySQL must check every record in sequence until the address is found.

  It's tempting to add indexes to every column, however, they are regenerated during every table INSERT or UPDATE. That can hit  performance; only add indexes when necessary.

### Avoid Using Global Events
  Only on rare occasions would it be necessary to use a global event. You should use frontend or adminhtml to narrow the scope instead.

### Use Magento Data Collections
  Execution of a SQL query is one of the most resource-taxing operations. Running SQL queries in a loop often results in a performance bottleneck. To load the EAV model, several heavy queries are required to execute. As the number of executed queries is multiplied with the number of categories, the result is extremely inefficient and slow code. Instead of loading models in a loop, Magento data collections can help to load a set of models in a very efficient manner.

### Validate Input and Properly Encode or Escape Output
  Remember to always validate data from non-trusted data sources. Sanitizing data coming into your extension and produced by it will improve overall security.

  For example, to prevent XSS vulnerability, avoid creating methods that output non-validated user-supplied data without proper HTML encoding.

### Always Encrypt Sensitive Data or Configurations
  Never store sensitive information in clear text within a resource that might be accessible to another control sphere. This type of information should be encrypted or otherwise protected.
