# CTF 8 - SQL Injection

In this Capture The Flag (CTF) challenge, we encounter a website that provides a PHP file to the user each time they attempt to access it. The relevant source code is included in the document's appendix. The website's login functionality is susceptible to SQL injection, and we'll explore the details below.

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
            // Login failed
            echo "<p>Invalid username or password <a href=\"index.php\">Try again</a></p>";
        }
    }
?>
```

## Questions

### Q1. Which SQL Query is executed everytime a user tries to login?

The SQL query executed on login is as follows:

```sql
SELECT username FROM user WHERE username = '<username>' AND password = '<password>'
```

### Q2. Which inputs you can manipulate to encroach the query? Which Special Characters you use and why?

To exploit the system, special characters like ;, --, and ' are crucial:

* `;`: Denotes the end of an SQL statement.

* `--`: Represents the beginning of an SQL comment.

* `'`: Marks the beginning and end of an SQL string.

### Q3. Which SQL query is effectively executed with your malicious login attempt? Why does this query allow you to login?

To achieve a successful malicious login attempt, we can manipulate the inputs as follows:

* Username: `admin';--`
* Password : `Anything that is not empty`

When utilizing the input `admin';--` for the username, the SQL injection exploit strategically employs special characters. First, the injected `'`serves to prematurely close the string that queries the database for the specified user, in this case, 'admin.' Following this, the semicolon (`;`) is employed to terminate the ongoing SQL statement. Finally, the double hyphen (`--`) is used to initiate an SQL comment, effectively nullifying any subsequent code that may follow in the original query.

This input manipulates the query as follows:

```sql
SELECT username FROM user WHERE username = 'admin'; -- AND password = '".$password."'";
```

This is semantically equivalent to:

```sql
SELECT username FROM user WHERE username = 'admin';
```

which do not require the password to enter into the system.

And the final result is this:


<div align="center">
  <img src="images/CTF8/sql_final.png" alt="Form2" width="50%">
</div>




## Appendix A - Source code



```php
<?php
   ob_start();
   session_start();
   include "config.php";
?>

<?
   // error_reporting(E_ALL);
   // ini_set("display_errors", 1);
?>

<html lang = "en">
   
   <head>
      <title>EasyVault</title>
      <link href = "css/custom.css" rel = "stylesheet">
      <!-- JavaScript Bundle with Popper -->
      <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-ka7Sk0Gln4gmtz2MlQnikT1wXgYsOg+OMhuP+IlRH9sENBO0LRn5q+8nbTov4+1p" crossorigin="anonymous"></script>

      
      <style>
        
      </style>
      
   </head>
	
   <body>
      
      <h2>Unlock the vault</h2> 
      <div class = "container form-signin">
         
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
      </div> <!-- /container -->
      
      <div class = "container">
      
         <form class = "form-signin" role = "form" style="text-align:center"
            action = "<?php echo htmlspecialchars($_SERVER['PHP_SELF']); 
            ?>" method = "post">
            <h4 class = "form-signin-heading">Login</h4>
            <input type = "text" class = "form-control" 
               name = "username" placeholder = "username" 
               required autofocus></br>
            <input type = "password" class = "form-control"
               name = "password" placeholder = "password" required></br>
            <button class = "btn btn-lg btn-primary btn-block" type = "submit" 
               name = "login">Login</button>
         </form>
			<div class="footer">
         Click here to clean the <a href = "logout.php" tite = "Logout">session.
         </div>
         
      </div> 
      
   </body>
</html>

```
