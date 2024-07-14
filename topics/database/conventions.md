# Naming

-   **Singular form**. Both tables and columns.
-   Be consistent! Doesn't matter if you use camelCase or snake_case. Use whatever the front-end uses.
-   Avoid abbreviations or prefixes.
-   Use unique names that cannot collude with SQL/RDBMS reserved words (avoid name, order, percent...) **or** use a trailing underscore.
-   Do not use the table name followd by “id” (e.g. client_id) as your PK. id is more than enough and everyone will understand.
-   Never use capital letters in your table or field names. Ever.
