## PHP

#### Debugging

Use the <pre> HTML tag to print out values nicely in PHP for debugging purposes.

```
echo '<pre>' . print_r($variable, true) . '</pre>';
exit();
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

#### How to print out $_GET and $_POST values nicely in a browser

Use the `<pre>` HTML markup to format the output.
```
echo "<pre>"
print_r($_GET);
echo "</pre>"

// read property from the $_GET
echo $_GET['author'];
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


