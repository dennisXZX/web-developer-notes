## PHP

#### Debugging

Use the `pre` HTML tag to print out values nicely in PHP for debugging purposes.

```
echo '<pre>' . print_r($variable, true) . '</pre>';
exit();
```

#### Call by reference

By default, PHP passes parameter by value, however, you can change it to pass by reference by using `&`.

```
function triple(&$value) {
  $realthing = $realthing * 3;
}

$val = 10;
triple($value);

// this will print out 30 because the function is passed by reference
echo "Triple = $val";
```

#### Alternative syntax for control structures

PHP provides alternative syntax for while, for, foreach and switch.

```
<?php foreach($person as $feature => $val): ?>
  <li><?php echo $feature . ': ' .$val ?></li>
<?php endforeach; ?>
```

#### Sanitize user inputs

We can sanitize user inputs by using `htmlspecialchars()` method.

#### Super globals

`$_GET['key']` to retrieve variables passed to the current script via the URL parameters.

`$_POST['key']` to retrieve variables passed to the current script via the HTTP POST method.

`$_SERVER` to retrieve info related to the server.

#### PHP Data Objects (PDO)

we can connect to database using PDO.

```
// PDO MySQL example
$host = '127.0.0.1';
$db   = 'test';
$user = 'root';
$pass = '';
$charset = 'utf8mb4';

$dsn = "mysql:host=$host;dbname=$db;charset=$charset";
$opt = [
    PDO::ATTR_ERRMODE            => PDO::ERRMODE_EXCEPTION,
    PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
    PDO::ATTR_EMULATE_PREPARES   => false,
];

$pdo = new PDO($dsn, $user, $pass, $opt);
```

Now we are connected to the database and can run our queries.

```
$sql = "SELECT * FROM users";
$stmt = $pdo->prepare($sql);
$stmt->execute();
// fetch the results and cast them into Task objects
$tasks = $stmt->fetchAll(PDO::FETCH_CLASS, 'Task');
```

#### Launch an internal server

For PHP 5.4 or newer, you can use the following command to start a built-in web server.

```
php -S localhost:8888
```

#### Multiple checkbox array

In order to pass checkbox values to thisform.php, we need to pass the checkbox name as an array, so in PHP the checked values will be stored in `$_POST['checkboxvar']`.
```
<form method='post' id='userform' action='thisform.php'>
  <div>
  <input type='checkbox' name='checkboxvar[]' value='Option One'>1<br>
  <input type='checkbox' name='checkboxvar[]' value='Option Two'>2<br>
  <input type='checkbox' name='checkboxvar[]' value='Option Three'>3
  </div>
  <input type='submit' class='buttons'>
</form>
```

#### PHP class

```
class Person {
  // declare a const variable
  const AVG_LIFE_SPAN = 79;

  // declare public variables
  public $firstName;
  public $lastName;

  // declare a public static variable
  public static $fortune = 1000;

  // declare a constructor, with two optional parameters
  function __construct($firstName = "", $lastName = "") {
    $this->firstName = $firstName;
    $this->lastName = $lastName;
  }

  // declare a public method
  public function getFirstName() {
    return $this->firstName;
  }

  // declare a public method
  public function setFirstName($newName) {
    $this->firstName = $newName;
  }

  // declare a static function
  public static function getAuthorFortune() {
    return self::$fortune;
  }
}

// create an Author class which inherits Person class
class Author extends Person {
  public $penName = "dennisboys";

  public function getPenName() {
    return $this->penName;
  }
}

// create a Person instance
$dennis = new Person("Dennis", "Xiao");

// read the value
echo $dennis->firstName;
// write the value
$dennis->firstName = "Den";

// access constant variable through an instance
echo $dennis::AVG_LIFE_SPAN;
// access constant variable through a class 
echo Person::AVG_LIFE_SPAN;

// call setFirstName method
$dennis->setFirstName("Zoe");
// call getFirstName method
echo $dennis->getFirstName();

// call static getAuthorLastName() function
echo Person::getAuthorFortune();
```

#### PHP index array and associative array

PHP array can contain values of different types.

Index arrray
```
$authors = array("Dennis", 1, 1.2);
$books = ["Dennis", "Zoe", "Ken"];

print_r($authors)
```

Associative array
```
$authors = array(
  "name" => "Dennis",
  "age" => 32,
  "sex" => "male"
);
```

We can delete an element from the array by using `unset()`.

```
// delete the 'name' property from the $author array
unset($authors['name']);
```

#### Use global variable inside a function

```
$authorName = "Dennis";

function setAuthorName() {
  // use the global keyword to indicate to use the global $authorName variable inside the function
  global $authorName;
  $authorName = "New Dennis";
}

setAuthorName();

// output "New Dennis"
echo $authorName;
```

#### What's the difference between single and double quote?

Everything in sinlge quote will be treated as a string, while variables and escape sequences will be evaluated in double quote.

#### What's the best situation to use heredoc?

Heredoc should be used in documents containing multi-line strings that require formatting and variable interpolation. Following is an SQL query using heredoc.

```
$sql = <<<SQL
select *
  from $tablename
 where id in [$order_ids_list]
   and product_name = "widgets"
SQL;
```
#### How do you use the spaceship `<=>` operator?

This is actually a shortcut to compare two values.

- Return 0 if values on either side are equal
- Return 1 if value on the left is greater
- Return -1 if the value on the right is greater

```
// Comparing Integers
echo 1 <=> 1; // ouputs 0
echo 3 <=> 4; // outputs -1
echo 4 <=> 3; // outputs 1

// Comparing Strings
echo "x" <=> "x"; // outputs 0
echo "x" <=> "y"; // outputs -1
echo "y" <=> "x"; // outputs 1
```
One common scenario is to use `<=>` in a callback function in sort() to simplify the code.
```
sort($friends, function($a, $b) {
  return $a['lastName'] <=> $b['lastName'];
}
```

#### What's the difference between include() and require()?

These two functions can be used to include files on the server.

- include() attempts to continue processing even if file is missing
- require() stops processing if the external file is not available
- include_once() and require_once() prevent the file from beling included more than once


