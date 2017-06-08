## PHP

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
