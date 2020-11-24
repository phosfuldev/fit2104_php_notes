# lecture 4: PHP basics

## about php
- server side language - executes on a web server
- cross platform
- embedded in html
- interpreted

```php
lines_end_with_semicolon();
uses_brackets();
// one line comment
/* multi
    line
    comment */
```

## check installation
```php
phpinfo();  
```

## common modules
curl, gd, iconv, json, libxml, mysqli or pdo-mysql, openssl, mcrypt, intl

## variables
- weakly typed
- prefix of `$`
```php
$var = 1;
```
- naming conventions:
    - start with letters or _
    - no special characters

## comparison operators
like normal, except:
```php
1 <> 2;  // true; this can mean !=
'a' === 'a';  // true; data type AND value is equal
'1' === 1;  // false
```
## logical operators
```php
$x = true && true || false ! false;
```

## strings
double quotes: variables inside will be interpreted to their value

single quotes: everything is just text
```php
echo('string');  // to print something
```
or use shorthand:
```php
<?='string'?>
```
### string methods(functions)
```php
$string = 'abcabc';
$a = strlen($string);
$b = substr($string, 0, 1);  // both inclusive
$c = substr($string, -1);
$d = strpos($string, 'b');  // first occurrence
$e = strrpos($string, 'b');  // last occurrence
$f = chop($string);  // remove whitespace from the end
$g = trim($string);  // remove whitespace from the beginning and end
$h = strtoupper($string);
$i = strtolower($string);
$j = ucfirst($string);  // capitalise string
$k = ucwords($string);  // title case - capitalise each word
$l = explode('b', $string);  // return array: 'a', 'ca', 'c'
$m = implode(' ', [$string, $string]);  // return joined string 'abcabc abcabc'
// the last 2 is useful to join formatted things like dates
```

## php numerics
    // 2 types: integers and doubles
### operators
normal

don't forget:
```php
$x = 5 % 2;  // modulud operation: x = 1
$x += 1;
$x ++;
```
### math functions
```php
$x = rand(0, 1);  // random number between 0 and 1
$x = ceil($x);  // round x up to an integer
$x = floor($x); // round x down
$x = sqrt($x);
$x = pow($x, 2);  // x to the power of 2
$x = dechex(15);  // x will be hex number f
$x = decoct($x); // x will be octal (15)
```

## php constants
```php
define("CONSTANT", 0);
```
- use `define` function
- does not have $ in front
- no scoping rules - all global

## data type manipulation
```php
gettype($x);  // see the type of x
settype($x, "string");  // set the type
$x = (integer)$x;  // casting
```

## other variable functions
```php
isset($var);  // return true if var exists
empty($var);  // return true if var doesn't exist or is a empty value, otherwise return false
unset($var);  // is like del in python
```
empty values: "", 0, 0.0, "0", NULL, array() (empty array)

## php arrays
```php
$things[] = "something";  // will (create and) append the value
$things[4] = "another thing";  // will put the value in index 4; index 1-3 can be empty
// adding multiple elements:
$things = array("something", "another thing");
// can specify starting index:
$things = array(2=>"something", "more things");
```

## array functions
```php
$a = sizeof($things);  // length of array
$b = current($things);
next($things);  // move pointer to next element
$c = current($things);
prev($things);  // move pointer to previous element
$d = key($things);  // current element's index
reset($things);  // reset internal pointer to first index
sort($things);  // sort ascending
rsort($things);  // sort descending
```

## iterate through the array:
- with index
```php
foreach($OzStates as $Index => $Contents) { 
    echo $Index." - ".$Contents."<br>";
}
```
will print in a format like this:

    1 - Victoria
    2 - New South Wales
    etc.

- no index
```php
foreach($OzStates as $Contents) { 
    echo $Contents."<br />";
}
```
will print in a format like this:

    Victoria
    New South Wales

there was also list/each, but was deprecated in 7.2

## conditional control
### if/else
```php
if ($condition) {
    // do something
} else if ($condition) {
    // do something else
    // can also be written as elseif
} else {
    // do another thing
}
```
### switch case
```php
switch ($value) {
    // use exact value
    case 'value1':
        // code...
        break;
    // or condition
    case $value > 10:
        // code...
        break;
    default:
    // code;
}
```
### ternary operator
condition ? thing to do if true : thing to do if false
```php
echo $x < $y ? "x is less than y" : "x is greater than y"
```

## loop
### for loop
```php 
for($counter = 1; $counter <= 3; $counter++) { 
    echo "This for loop will execute 3 times <br />";
}
```
### while loop
condition is checked before
```php
$loan = 600; 
while($loan > 0) {
    echo "You still owe ". $loan. "<br />"; $loan -=200;
}
```
### do...while loop
condition is checked after
```php
$loan = 600; 
do {
    echo "You still owe ". $loan. "<br />"; $loan -=200;
    } while ($loan >0);
```

## function call (jumping)
defining a function:
```php
function functionName(argument_list...) {
    // something
    return return_value;
}
```
2 ways to reference an argument: 
```php
$x  // 'normal'; 
&$x  // by reference, the variable itself is changed
```

## php include
```php
include("file.php"); 
``` 
will only be called when the code block is executed, if file not there will throw an error but rest of code can run (E_WARNING level error)
```php
require("file.php"); 
``` 
always called, will halt if file not exist (E_COMPILE_ERROR level error)

<br><br>

# lecture 5: database connection
php have multiple modules to support different database
-  mysql: can use phpmyadmin (php based) as GUI
-  xampp already has mysql built in

## using mysqli - simply way
### 1. establish connection
```php
$conn = mysqli_connect("DB_HOSTNAME","DB_USERNAME","DB_PASSWD", "DB_NAME"); 
``` 
- function prefix: mysqli_
- $conn would be a connection object or false
- can take more optional arguments like port number
### 2. store query string
```php
$query = "SELECT * FROM Customer";
``` 
### 3. execute query
```php
$result = mysqli_query($conn, $query);
```    
returns a mysqli_result object
### 4. display result
```php
while ($row = mysqli_fetch_row($result)) {
    echo "<tr>";
    echo "<td>".$row[0]."</td>";
    // ...
    echo "</tr>";
}
```
- can also use inline php here - i used php block just for easy of writing in markdown
    - remember the `:` at the start and the `endwhile;` at the end
- depends on how many/which columns needs to be displayed in the database table
-  can also use associative array like so: 
    - `mysqli_fetch_assoc($result);`
    - then use `$row['id']`, `$row['name']` etc
### 5. clean up
```php
mysqli_free_result($result);  
```
only use when memory is a concern
```php
mysqli_close($conn);
```
connection is actually automatically closed when script finishes executing

## using mysqli - object oriented way
### 1. connection
```php
$conn = new mysqli("DB_HOSTNAME","DB_USERNAME","DB_PASSWD", "DB_NAME");
```
### 2. storing query string - same
### 3. execute query
```php
$result = $conn->query($query);
```
### 4. display result
```php
while ($row = $result->fetch_object()){
    echo "<tr>";
    echo "<td>".$row->id."</td>";
    // go on as needed
    echo "</tr>";
}
```
### 5. clean up
```php
$result->free(); 
$conn->close();
```

## using PDO
PDO: database abstraction layer - does not depend on particular database
### 1. establish connection
```php
$dbh = new PDO('mysql:host=DB_HOSTNAME;dbname=DB_NAME','DB_USERNAME','DB_PASSWD'); 
```
database is specified here & only needs to be changed here if needed
### 2. query string
```php
$stmt = $dbh->prepare("SELECT * FROM Customer WHERE ?>18");
```
prepared statement with placeholder (prevent sql injection)
### 3. execute query
```php
$stmt->execute($age);
```
fill in placeholder variables and execute the query
### 4.1. display with array
```php
while ($row = $stmt->fetch()){
    echo "<tr>";
    echo "<td>".$row[0]."</td>";
    // can use index or name
    echo "</tr>";
}
```
there is a setting to change the return type of fetch()

- the default is `PDO::FETCH_BOTH` which returns both normal array and assoc array?
- there's another common one: `PDO::FETCH_OBJ` which will return object, but for clear meaning I think it's better to use another function for that (see below)
### 4.2. display with object
```php
while ($row = $stmt->fetchObject()){
    echo "<tr>";
    echo "<td>".$row->id."</td>";
    // go on as needed
    echo "</tr>";
}
```
### 5. close connection
```php
$stmt->closeCursor();
```

## using a cnnection.php file
usually store host name, db name etc as variables 

only need to change database details in one place

(I also put the new PDO thing in there but idk if that's good practice)

<br><br>

# lecture 6: database manipulation
### refresher on sql syntax:
- insert:
```php
$q = "INSERT INTO `tablename` (attribute1, attribute2.......) VALUES (value1, value2.......)";
```
- delete:
```php
$q = "DELETE FROM `tablename` WHERE $condition = true";
```

## http requests & html forms 
we will be using GET and POST requests

GET can be used with normal `<a>` tags, eg:
```html
<a href="something.php?user=abcd&page=4">Click This</a>
```
the query string (QueryString) shows up in the url like so
- user can bookmark the link or enter the QueryString directly

both GET and POST can be used with html forms

### form with POST
```html
<form method="post" action="codeToExecute.php">
    <label>first name</label>
    <input type="text" name="first_name"/>

    <input type="submit" value="submit"/>
</form>
```
a submit button is *required*.

the action attribute requests a new page (codeToExecute.php) from the web server. data entered into the form is sent along with the request.

if there is no action tag, means the form will be processed within the same page (just access the superglobal as you would in the separate page)
- does the page automatically refresh after form submit or something for that to work?

now the form input can be accessed from the $_POST super-global, like so:
```php
$fname = $_POST["first_name"];
```
subtle difference: if using the value directly in a string, no need to use quotes:
```php
echo("your name is $_POST[first_name].");
```
### form with GET
the same thing, only difference is to use `$_GET` instead of `$_POST`

## processing form inputs
### 1. first, check if the user entered anything - 
in the same file a the form, do something like this:
```php
if (empty($_POST["fname"]) || empty($_POST["sname"])):
```
if this is true (form was empty), display the form inside the if block.

or you can check if the $_POST or $_GET array has any values if this is in the action script
### 2. make a query and run it
```php
$query = "INSERT INTO `name` (`fname`, `sname`) VALUES (?, ?)"; 
$stmt = $dbh->prepare($query);
$stmt->execute(["$_POST[fname]", "$_POST[sname]"]);
```
### 3. add error checking - modify the execute line to something like this:
```php
if(!$stmt->execute()) { 
    $err= $stmt->errorInfo();
    echo "Error adding record to database – contact System Administrator. <br />Error is: <b>".$err[2]."</b>";
} else {
    // do normal stuff
}
```

## client side form validations

<br><br>

# lecture 7: advanced forms and security

## dropdown lists
higher data integrity

options are created via fk -> pk -> name
- the human readable name is displayed, and the ID is sent in the request

example: 
```html
<form method="post">
    <select name="category" value="">
        <option value="1">category A</option>
        <option value="2">category B</option>
    </select>
</form>
```
obviously, the 'value' attribute and the actual display names can/should be programmatically set, and the `<option>`s will be genereted in a loop

### pre-selection (display the value before editing)
example: 
```php
<form method="post">
    <select name="category" value="">
    <?php foreach ($categories as $category){
        if ($is_preselected){
            echo("<option value=".$category["id"]." selected>".$category["name"]."</option>");
        } else {
            echo("<option value=".$category["id"].">".$category["name"]."</option>");  // just missing the 'selected' attribute
        }
    } ?>
    </select>
</form>
```

## checkboxes
```html
Something<input type="checkbox" name="something"/>
```
can use a `<label>` tag, but don't have to.

### checking multiple boxes in a page
```php
<form method="post">
    <label>food:</label>
    Ice Cream<input type="checkbox" name="food[]" value="ice cream"/>
    Pancake<input type="checkbox" name="food[]" value="pancake"/>
    Blueberry<input type="checkbox" name="food[]" value="blueberry"/>
    
    <input type="submit" value="submit"/>
</form>

<?php
var_dump($_POST["food"]);
?>
```
the output: 

```
array(2) { [0]=> string(9) "ice cream" [1]=> string(7) "pancake" }
```
ofc the values will be programmatically retrieved and would probably be ids not text

something like: 
```php
while($row = $stmt->fetchObject()) {
    echo($item->name."<input type=\"checkbox\" name=\"food[]\" value=".$item->id/>);
}
```
used `fetchObject` this time to get familiar with object notation again.

## cookies
- stored in the browser (locally)
- overcome statelessness
- enable storing user identifier and then looking up user on server side

create cookie:
```php
setcookie('name', 'value', UnixTime);  // the last param is expire time (a time point)
```

retrieve cookie: another superglobal!
```php
$x = $_COOKIE['name'];
```
cookies cannot be retrieved until the next loading of the page
## sessions
- notes a single visit to the site (all pages of the site included)
- stored on the server side (?? contradictory)

start recording session:
```php
session_start();
```
- session is created or resumed based on current session id that's being passed via a request (eg. post or get)
- the attribute name needs to be `PHPSESSID`, which is automatically checked by php when processing request

get session id:
```php
$x = session_id();
```
you can use cookie to store session id, or encode it into the url if cookies are disabled

after this, can put things in the `$_SESSION` superglobal

example: 
```php
$_SESSION['uid'] = 'kowrjfuvbdscxaok';
```
it can be accessed across pages

session expires after a certain amount of time that the user havent visited the site
- can be set in .ini file

session can also be manually cleared:
```php
session_cache_expire();
```

session data is stored in location specified in save_file parameter in the .ini file

## handling password - encryption
Hash in MySQL: (when eg. creating a user/signing up)
```php
$q = "INSERT INT `users` (..., `password`) VALUES (..., SHA2('pgkpword',0))";
``` 
Hash in PHP: (when logging in)
```php
hash('sha256', $_POST["password"]);
if ($stmt->rowCount() == 1){  // check if there is any match
    // do stuff
} else {
    // login fail
}
```
when checking login etc, hash the entered data before the check because db stored the hashed version

## emails via php
```php
$subject = "name <example@company.com>, name2 <bademail@company.com>"  // the format here has to be right
mail($to, $subject, $message);
```
- subject can be empty (but shouldn't)
- message or body can be plain text or html
- can have more optional parameters, eg cc or bcc, or timed email
- php passes the email to a mail routing thing (usually `sendmail`)

<br><br>

# lecture 8: more php functions

## file manipulation (on the server)
- use the $_SERVER superglobal
    - contain headers, file paths and script locations
- there are a set of specific keys to access the attributes
- the content of $_SERVER is unstable, web servers may not provide all the above information

example: 
```php
$_SERVER['SCRIPT_FILENAME']; 
```
- `SCRIPT_FILENAME` is a constant? (at least it's one of the pre-set keys)
- returns the full path of the current executing script file

### file meta/path functions
```php
dirname($path, *$levels);
```
returns the parent directory of given path and (optional) levels to traverse (>0)
```php
opendir($path_to_file);
```
open that file, return a directory handle (?? shouldn't it be a file handle?)
```php
readdir($path);
```
get file name of the next file in given directory handle
```php
is_dir($path);
```
check if a directory exists (not a file)
```php
realpath($path_to_file);
```
return absolute path
```php
filetype($path_to_file);
```
common return values: dir, file, unknown etc
```php
$url_compatible_string = urlencode($filename);
```
returns a string that are converted to url-safe format and can go into QueryString

### reading file into a string
#### going through file handle
```php
$file_handle = fopen($path_to_file, 'r');
```
modes are pretty much the same as python
```php
feof($file_handle);
```
returns true/false - if the end of file is reached
```php
fgets($file_handle, $read_length);
```
return string - read from file until length-1; if no length is specified, read till eof
```php
fclose($file_handle);
```
close the file to release resources

#### not needing a file handle
```php
file_get_contents($path_to_file, $use_include_path, $start_not_inclusive, $read_length); 
``` 
return string or false - can also pass in a url; last 3 params are optional

### displaying source code
#### from a string
```php
htmlentities($string, $flags, $encoding);
```
return endoced string; last 2 params are optional
```php
html_entity_decode($string, $flags, $encoding);
```
return decoded string; last 2 params are optional
#### from a file
```php
highlight_file($path_to_file, $return_code);
```  
if $return_code is true, will return code; if false, will return just true of false on success/fail
#### prepare a translation table and encode later
```php
$trans = get_html_translation_table(HTML_ENTITIES);
// enter inside a loop
    // ...
    $line = strtr($line, $trans);
    // ...
```

### uploading file
#### 1. use html form with input type of `file` and method `POST`
```php
<form method="post">
    <input type="file" name="file"/>
</form>
```
the file will be in `$_FILES` superglobal 
- `$_FILES` contains original name, size etc
#### 2. check file type
```php
if($_FILES['file']['type'] == 'image/jpeg'):
```
#### 3. move file to a permanent location
```php
move_uploaded_file($file_name, $destination); 
```
return true/false


## client side form validation
### html
can use attributes `type`, `required`, `max/minlength`, `pattern` (regex pattern) to restrict
- if the type is `number`, can limit min/max
```html
<input type="text" name="thing" required minlength="10" maxlength="100" pattern=""/>

```
### javascript & Bootstrap
in JS:
1. first select the DOM object
2. a few things you can use: `str.length()`, `isNaN()`, `str.test(regex)` etc

libraries:
- jQuery Validation plugins
- FormValidation in Bootstrap
- Parsley

can bind html with js
- hook functions to `onInput`, `onInvalid`
with CSS
- `:invalid`, `:valid` selectors

## generating pdf

<br><br>

# lecture 9: Ajax, MVC and Cakephp






<br><br>

# lecture 10: Cakephp hands on (no lecture slides)
use bake all \<table-name\> to create scaffold code

set database connection through app_local.php

set emcoding, timezone/locale in app.php

## folders purposes:

### Model: 
- Table>xxx.php: data validators
- Entity>xxx.php: create virtual fields (=add a getter)
    - would be able to use it in corresponding View file like a normal field

### Controller:

### View:
- templates>layout>default.php: layout template for all pages
    - also add more js and css files here (the location of that line matters)

<br><br>

# lecture 11: oop in php
## advantage of oop:
- modularity
- reusability
- information hiding
- debugging
- reliability (easy to test)
## disadvantages:
- learning curve
- programs get larger
- possibly slightly slower (due to sligtly more code) (generally not noticeable)

## encapsulation
ensure each class is separated and self-contained and doesn't affect other classes

### access modifiers - called accessors (classes(?)/variables) or mutators (methods)
- public
- protected (can only be accessed within the class or a child class instance)
- private

private and protected variables are prefixed with _
```php
$_name;
```

## coding with oop in php
### create an object 
```php
class person {
    protected $_name;

    public function set_name($new_name) {
        $this->_name = $new_name;  // access attributes with arrow
    }

    public function get_name() {
        return $this->_name;
    }
}
```
### constructor
can include calls to class methods
```php
public function __construct($name) { 
    $this->_name = $name;
}
```
there are also destructors
### this
reference to the current object

## inheritance & polymorphism
overriding and overloading(*not possible in php)
```php
class employee extends person {
    // stuff here...

    // constructor can call existing setter to initialise
    // also can just call parent class constructor:
    // parent::__construct($name);

    // if create a method with the same name, the parent method will be overridden
}
```

## using OO to access database
create an abstract interface - can then CURD without writing sql
- similar to what Cakephp does!
- DAO - data access object


