-   SQL Server Instance = The running process of SQL Server, which manages databases.
-   Database = A collection of data inside an instance.
-   Schema = A logical namespace inside a database to organize objects.

---

-   SQL Server (the program) runs instances.
-   Each instance can manage multiple databases.
-   Each database can have multiple schemas.

---

| **Concept**             | **Explanation**                                                                             |
| ----------------------- | ------------------------------------------------------------------------------------------- |
| **SQL Server (RDBMS)**  | The software you install that allows you to create, manage, and query relational databases. |
| **SQL Server Instance** | A running process of SQL Server that can manage one or more databases.                      |
| **Database**            | A collection of tables, views, and objects managed by an instance.                          |
| **Schema**              | A logical namespace inside a database to organize tables and other objects.                 |

```
SQL Server (RDBMS)
└── Instance: MSSQLSERVER
    ├── Database: ERP_DB
    │   ├── Schema: hr
    │   │   ├── Table: hr.employees
    │   ├── Schema: finance
    │       ├── Table: finance.invoices
    ├── Database: Retail_DB
        ├── Schema: store
            ├── Table: store.products
```

| **RDBMS**      | **Instance**        | **Schemas**         |
| -------------- | ------------------- | ------------------- |
| **SQL Server** | Yes (Named/Default) | Yes                 |
| **PostgreSQL** | Yes (Cluster-based) | Yes                 |
| **MySQL**      | No (Single process) | No (Databases only) |
