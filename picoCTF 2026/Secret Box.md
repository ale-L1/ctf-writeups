# Secret Box

**Category**: Web exploitation

**Vulnerability**: SQL injection

## Overview
Secret Box is a web application that allows registered users to create and manage private secret notes. During the challenge, a SQL injection vulnerability was identified in the functionality used to create new notes.

By interacting with the application and analyzing the database errors it returned, it was possible to identify PostgreSQL as the underlying DBMS and understand the structure of the SQL query being executed.

The vulnerability allowed an attacker to inject SQL expressions, making it possible to enumerate database metadata, identify relevant tables and columns, and retrieve secret notes belonging to the administrator, including the flag.

---

### Reconnaissance
#### Initial Reconnaissance
As soon as we start the challenge instance, we're greeted by a page where we can either log in or sign up. After registering and logging in, we're redirected to our personal vault, where we can create secret notes.

By clicking on "Create Secret", we can test a simple SQL injection payload and observe how the web application behaves. 




The application returns a detailed database error, providing useful information about the SQL query being executed.

From the error message, we can infer that the application is performing an INSERT operation. By examining the error more closely, we can also identify PostgreSQL as the underlying DBMS.

#### Identifying the SQL Injection
Let's see whether we can inject a SELECT statement into the existing query. We can use PostgreSQL's string concatenation operator, ||, and a single quote (') to escape the original string literal:

``` 
'|| (SELECT version()) ||'
```

The application creates a note containing the database version. This confirms that our SQL injection works and validates our earlier assumption that the application uses PostgreSQL.

### Exploitation
#### Database Enumeration

Now that we've confirmed the vulnerability, let's gather information about the database structure.

We can start by querying `information_schema.tables`, which provides metadata about the tables accessible through the information schema.

A straightforward query such as `SELECT * FROM information_schema.tables` would return multiple rows and columns. However, our injection context only allows us to display a single resulting value in the note.

To work around this limitation, we can use PostgreSQL's `string_agg()` function to combine multiple results into a single string. By concatenating the schema and table names, we can identify which tables are available:
```
'|| (SELECT string_agg(table_schema || '.' || table_name, ', ') FROM information_schema.tables) ||'
```

The query returns a list of tables, including many standard PostgreSQL tables. Two particularly interesting results stand out: public.secrets and public.users. The first may contain the secret notes, while the second may provide information about the application's users.

#### Enumerating Table Columns

Before querying these tables, we need to understand their structure.

PostgreSQL provides the `information_schema.columns` view, which contains metadata about table columns, including their names and data types.

We can use another `string_agg()` query to retrieve the column names and types from public.secrets:

```
'|| (SELECT string_agg(column_name || ' (' || data_type || ')', ', ') FROM information_schema.columns WHERE table_schema = 'public' AND table_name = 'secrets') ||'
```

We can run a similar query against public.users:

```
'|| (SELECT string_agg(column_name || ' (' || data_type || ')', ', ') FROM information_schema.columns WHERE table_schema = 'public' AND table_name = 'users') ||'
```

By inspecting the results, we can determine which columns are relevant to our investigation. In particular, the secrets table contains the information needed to retrieve stored notes and associate them with their owners.

#### Retrieving Secret Notes

Now that we know the structure of the secrets table, we can query its contents.

For example, the following payload combines each note's owner ID with its content:

```
'|| (SELECT string_agg(owner_id || '.' || content, ', ') FROM public.secrets) ||'
```

This allows us to retrieve multiple records in a single result. However, it also returns notes belonging to every user, making the output potentially noisy.

We can narrow down our search by identifying the administrator's user ID and filtering the secrets by their owner.

#### Identifying the Administrator

Since we've already discovered the public.users table, we can query its user IDs and usernames:

```
'|| (SELECT string_agg(id || '.' || username, ', ') FROM public.users) ||'
```

By examining the returned results, we can identify the administrator's account and obtain the corresponding user ID.


#### Retrieving the Flag

Now we can query the secrets table again, filtering the results by the administrator's ID:

```
'|| (SELECT content FROM public.secrets WHERE owner_id = 'e2a66f7d-2ce6-4861-b4aa-be8e069601cb') ||'
```

This retrieves the content of the administrator's secret note, revealing the flag and completing the challenge.

---

### Security Impact
The SQL injection vulnerability allowed an attacker to:
- Enumerate database metadata: Discover table names, schemas, and column definitions through PostgreSQL's information_schema.
- Access other users' data: Retrieve secret notes belonging to users other than the authenticated account.
- Access administrator-owned secrets: Identify the administrator's user ID and retrieve the associated secret content.
- Potentially compromise additional database data: Depending on the application's database privileges and the exact injection context, the vulnerability could expose or affect other records.

In a real-world application, this could result in the disclosure of confidential information and undermine the trust users place in the service.

### Root Cause
The vulnerability was caused by the way user input was handled when creating a new secret. Based on the behavior I observed, the application appeared to include user input directly in an SQL query without properly separating it from the SQL syntax.

This allowed me to modify the structure of the query and execute additional SQL expressions. Using this technique, an attacker could retrieve information from the database instead of simply creating a new note.

Another issue was that the application returned detailed database errors. These errors can be used to understand the query structure, and although they were not the main cause of the vulnerability, they made it easier to investigate the application.

### Remediations

There are several ways to prevent this type of vulnerability:
- Use a parameterized query: The main fix would be to use parameterized queries instead of building SQL statements by concatenating strings. This ensures that user input is treated as data and cannot change the structure of the query.
- Avoid exposing detailed errors: Instead of displaying complete database errors to users, the application should show a generic error message and store the technical details in server-side logs. This would make it harder to discover information about the database through error messages.
- Test the application for SQL Injection: The application should be tested to make sure that user input cannot modify SQL queries. Security tests and code reviews could help identify similar vulnerabilities before the application is deployed.
- Limit database permissions: The database account used by the application should have only the permissions it actually needs. This would help limit the damage if another SQL injection vulnerability was discovered.

### Key Takeaways

Although the challenge also provided the source code, I deliberately chose not to inspect it. Instead, I wanted to approach the challenge as a black-box assessment, relying exclusively on the application's observable behavior and the information exposed by its responses.

I believe this approach provides a valuable learning experience. Rather than immediately locating the vulnerable code, we have to formulate hypotheses, test them, interpret database errors, enumerate the database structure, and gradually build an understanding of the application's behavior.

For me, this was a great opportunity to practice SQL injection and develop a more methodical approach to web application security testing.
