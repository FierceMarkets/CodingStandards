# Standard Coding Standards

This guide is heavily inspired by the guidelines from [PHP-FIG](http://www.php-fig.org/), [the Drupal Coding Standards](http://drupal.org/coding-standards), and [the PEAR coding standards](http://pear.php.net/manual/en/standards.php).

The intent of this guide is to reduce cognitive friction when scanning code from different authors. It does so by enumerating a shared set of rules and expectations about how to format PHP code.

The style rules herein are derived from commonalities among the various PHP-FIG member projects. When various authors collaborate across multiple projects, it helps to have one set of guidelines to be used among all those projects.

[PSR-0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md
[PSR-1]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-1-basic-coding-standard.md
[PSR-2]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md

---

# Files

All PHP files MUST use the Unix LF (linefeed) line ending.

All PHP files MUST end with a single blank line. This avoids the verbose "\ No newline at end of file" patch warning and makes patches easier to read since it's clearer what is being changed when lines are added to the end of a file.


## PHP Tags

PHP code MUST use the long `<?php ?>` tags or the short-echo `<?= ?>` tags; it MUST NOT use the other tag variations.

This is the most portable way to include PHP code on differing operating systems and set-ups.  In PHP 5.4 and newer, the short-echo format is guarenteed to be enabled.  It makes the most sense in template files.

The `?>` at the end of files containing only PHP MUST be omitted.

 > Removing it eliminates the possibility for unwanted    whitespace at the end of files which can cause "header already sent" errors, XHTML/XML validation issues, and other problems.
 > The closing delimiter at the end of a file is optional.
PHP.net itself removes the closing delimiter from the end of its files, so this can be seen as a "best practice."


## Character Encoding

PHP code MUST use only UTF-8 without BOM.


## Lines

There MUST NOT be a hard limit on line length.

The soft limit on line length MUST be 120 characters; automated style checkers MUST warn but MUST NOT error at the soft limit.

Lines SHOULD NOT be longer than 80 characters; lines longer than that SHOULD be split into multiple subsequent lines of no more than 80 characters each.

There MUST NOT be trailing whitespace at the end of non-blank lines.

Blank lines MAY be added to improve readability and to indicate related blocks of code.

There MUST NOT be more than one statement per line.


## Indenting

Code MUST use an indent of 4 spaces, and MUST NOT use tabs for indenting.

> Using only spaces, and not mixing spaces with tabs, helps to avoid
> problems with diffs, patches, history, and annotations. The use of spaces
> also makes it easy to insert fine-grained sub-indentation for inter-line 
> alignment.


*Pear* - Use an indent of 4 spaces, with no tabs.

*Drupal* - Use an indent of 2 spaces, with no tabs.


## Side Effects

A file SHOULD declare new symbols (classes, functions, constants, etc.) and cause no other side effects, or it SHOULD execute logic with side effects, but SHOULD NOT do both.

The phrase "side effects" means execution of logic not directly related to declaring classes, functions, constants, etc., *merely from including the file*.

"Side effects" include but are not limited to: generating output, explicit use of `require` or `include`, connecting to external services, modifying ini settings, emitting errors or exceptions, modifying global or static variables, reading from or writing to a file, and so on.

The following is an example of a file with both declarations and side effects; i.e, an example of what to avoid:

```php
<?php
// side effect: change ini settings
ini_set('error_reporting', E_ALL);

// side effect: loads a file
include "file.php";

// side effect: generates output
echo "<html>\n";

// declaration
function foo()
{
    // function body
}
```

The following example is of a file that contains declarations without side effects; i.e., an example of what to emulate:

```php
<?php
// declaration
function foo()
{
    // function body
}

// conditional declaration is *not* a side effect
if (! function_exists('bar')) {
    function bar()
    {
        // function body
    }
}
```

---

# Namespace and Class Names

Namespaces and classes SHOULD follow [PSR-0][].

This means each class is in a file by itself, and is in a namespace of at least one level: a top-level vendor name.

Class names MUST be declared in `StudlyCaps`.

Code written for PHP 5.3 and after SHOULD use formal namespaces.

For example:

```php
<?php
// PHP 5.3 and later:
namespace Fierce\Model;

class Foo
{
}
```

Code written for 5.2.x and before SHOULD use the pseudo-namespacing convention of `Vendor_` prefixes on class names.

```php
<?php
// PHP 5.2.x and earlier:
class Fierce_Model_Foo
{
}
```

---


# Class Definitions

Opening braces for classes MUST go on the next line, and closing braces MUST go on the next line after the body.

```php
<?php
class Foo_Bar
{

    //... code goes here

}
?>
```

---

# Class Constructor Calls

When calling class constructors with no arguments, always include parentheses:

    $foo = new MyClassName();

This is to maintain consistency with constructors that have arguments:

    $foo = new MyClassName($arg1, $arg2);

Note that if the class name is a variable, the variable will be evaluated first to get the class name, and then the constructor will be called. Use the same syntax:

    $bar = 'MyClassName';
    $foo = new $bar();
    $foo = new $bar($arg1, $arg2);


---


# Class Constants, Properties, and Methods

The term "class" refers to all classes, interfaces, and traits.

Classes should be given descriptive names. Avoid using abbreviations where possible.

*Drupal* - See this [Drupal page on Object Oriented Code](http://drupal.org/node/608152)



## `extends` and `implements`

The `extends` and `implements` keywords MUST be declared on the same line as the class name.

The opening brace for the class MUST go on its own line; the closing brace for the class MUST go on the next line after the body.

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements \ArrayAccess, \Countable
{
    // constants, properties, methods
}
```

Lists of `implements` MAY be split across multiple lines, where each subsequent line is indented once. When doing so, the first item in the list MUST be on the next line, and there MUST be only one interface per line.

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class ClassName extends ParentClass implements
    \ArrayAccess,
    \Countable,
    \Serializable
{
    // constants, properties, methods
}
```


## `abstract`, `final`, and `static`

When present, the `abstract` and `final` declarations MUST precede the visibility declaration.

When present, the `static` declaration MUST come after the visibility declaration.


```php
<?php
namespace Vendor\Package;

abstract class ClassName
{
    protected static $foo;

    abstract protected function zim();

    final public static function bar()
    {
        // method body
    }
}
```

## Method and Function Calls

When making a method or function call, there MUST NOT be a space between the method or function name and the opening parenthesis, there MUST NOT be a space after the opening parenthesis, and there MUST NOT be a space before the
closing parenthesis. In the argument list, there MUST NOT be a space before each comma, and there MUST be one space after each comma.

```php
<?php
bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
```

Argument lists MAY be split across multiple lines, where each subsequent line is indented once. When doing so, the first item in the list MUST be on the next line, and there MUST be only one argument per line.

```php
<?php
$foo->bar(
    $longArgument,
    $longerArgument,
    $muchLongerArgument
);
```



## Class Constants

Class constants MUST be declared in all upper case with underscore separators.
For example:

```php
<?php
namespace Vendor\Model;

class Foo
{
    const VERSION = '1.0';
    const DATE_APPROVED = '2012-06-01';
}
```

## Properties

Visibility MUST be declared on all properties.

The `var` keyword MUST NOT be used to declare a property.

There MUST NOT be more than one property declared per statement.

Property names SHOULD NOT be prefixed with a single underscore to indicate protected or private visibility.

A property declaration looks like the following.

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public $foo = null;
}
```


## Methods

Visibility MUST be declared on all methods.

Method names MUST be declared in `camelCase()`.

Method names SHOULD NOT be prefixed with a single underscore to indicate protected or private visibility.

Method names MUST NOT be declared with a space after the method name. The opening brace MUST go on its own line, and the closing brace MUST go on the next line following the body. There MUST NOT be a space after the opening parenthesis, and there MUST NOT be a space before the closing parenthesis.

A method declaration looks like the following. Note the placement of parentheses, commas, spaces, and braces:

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function fooBarBaz($arg1, &$arg2, $arg3 = [])
    {
        // method body
    }
}
```    

## Method Arguments

In the argument list, there MUST NOT be a space before each comma, and there MUST be one space after each comma.

Method arguments with default values MUST go at the end of the argument list.

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function foo($arg1, &$arg2, $arg3 = [])
    {
        // method body
    }
}
```

Argument lists MAY be split across multiple lines, where each subsequent line is indented once. When doing so, the first item in the list MUST be on the next line, and there MUST be only one argument per line.

When the argument list is split across multiple lines, the closing parenthesis and opening brace MUST be placed together on their own line with one space between them.

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public function aVeryLongMethodName(
        ClassTypeHint $arg1,
        &$arg2,
        array $arg3 = []
    ) {
        // method body
    }
}
```



---

# Keywords and True/False/Null

PHP [keywords][] MUST be in lower case.

The PHP constants `true`, `false`, and `null` MUST be in lower case.

*Pear* - All lowercase.

*Drupal* - All uppercase.

[keywords]: http://php.net/manual/en/reserved.keywords.php

--

# Namespace and Use Declarations

When present, there MUST be one blank line after the `namespace` declaration.

When present, all `use` declarations MUST go after the `namespace` declaration.

There MUST be one `use` keyword per declaration.

There MUST be one blank line after the `use` block.

For example:

```php
<?php
namespace Vendor\Package;

use FooClass;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

// ... additional PHP code ...

```


---


# Function Calls


When making a method or function call, there MUST NOT be a space between the method or function name and the opening parenthesis, there MUST NOT be a space after the opening parenthesis, and there MUST NOT be a space before the
closing parenthesis. In the argument list, there MUST NOT be a space before each comma, and there MUST be one space after each comma.

```php
<?php
bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
```

Argument lists MAY be split across multiple lines, where each subsequent line is indented once. When doing so, the first item in the list MUST be on the next line, and there MUST be only one argument per line.

```php
<?php
$foo->bar(
    $longArgument,
    $longerArgument,
    $muchLongerArgument
);
```


Functions MUST be called with no spaces between the function name, the opening parenthesis, and the first parameter; spaces between commas and each parameter, and no space between the last parameter, the closing parenthesis, and the semicolon. Here's an example:

    <?php
    $var = foo($bar, $baz, $quux);
    ?>

As displayed above, there should be one space on either side of an equals sign used to assign the return value of a function to a variable. In the case of a block of related assignments, more space may be inserted to promote readability:
    
    <?php
    $short         = foo($bar);
    $long_variable = foo($baz);
    ?>

---

# Assignments

To support readability, the equal signs MAY be aligned in block-related assignments:

    <?php
    $short  = foo($bar);
    $longer = foo($baz);
    ?>

## Split long assigments onto several lines

Assigments may be split onto several lines when the character/line limit would be exceeded. The equal sign has to be positioned onto the following line, and indented by 4 characters.

    <?php
    
    $GLOBALS['TSFE']->additionalHeaderData[$this->strApplicationName]
        = $this->xajax->getJavascript(t3lib_extMgm::siteRelPath('nr_xajax'));
    ?>

---

# Function Definitions

*Drupal* - 
    function funstuff_system($field) {
      $system["description"] = t("This module inserts funny text into posts randomly.");
      return $system[$field];
    }

*Pear* -

    <?php
    function fooFunction($arg1, $arg2 = '')
    {
        if (condition) {
            statement;
        }
        return $val;
    }
    ?>

*PSR* - Has no opinion on the function definition formatting.

Arguments with default values go at the end of the argument list. Always attempt to return a meaningful value from a function if one is appropriate.

---

# Control Structures

The general style rules for control structures are as follows:

- There MUST be one space after the control structure keyword
- There MUST NOT be a space after the opening parenthesis
- There MUST NOT be a space before the closing parenthesis
- There MUST be one space between the closing parenthesis and the opening brace
- The structure body MUST be indented once
- The closing brace MUST be on the next line after the body

The body of each structure MUST be enclosed by braces. This standardizes how the structures look, and reduces the likelihood of introducing errors as new lines get added to the body.


## `if`, `elseif`, `else`

An `if` structure looks like the following. Note the placement of parentheses, spaces, and braces; and that `else` and `elseif` are on the same line as the closing brace from the earlier body.

```php
<?php
if ($expr1) {
    // if body
} elseif ($expr2) {
    // elseif body
} else {
    // else body;
}
```

The keyword `elseif` SHOULD be used instead of `else if` so that all control keywords look like single words.


## `switch`, `case`

A `switch` structure looks like the following. Note the placement of parentheses, spaces, and braces. The `case` statement MUST be indented once from `switch`, and the `break` keyword (or other terminating keyword) MUST be indented at the same level as the `case` body. There MUST be a comment such as
`// no break` when fall-through is intentional in a non-empty `case` body.

```php
<?php
switch ($expr) {
    case 0:
        echo 'First case, with a break';
        break;
    case 1:
        echo 'Second case, which falls through';
        // no break
    case 2:
    case 3:
    case 4:
        echo 'Third case, return instead of break';
        return;
    default:
        echo 'Default case';
        break;
}
```


## `while`, `do while`

A `while` statement looks like the following. Note the placement of parentheses, spaces, and braces.

```php
<?php
while ($expr) {
    // structure body
}
```

Similarly, a `do while` statement looks like the following. Note the placement of parentheses, spaces, and braces.

```php
<?php
do {
    // structure body;
} while ($expr);
```

## `for`

A `for` statement looks like the following. Note the placement of parentheses, spaces, and braces.

```php
<?php
for ($i = 0; $i < 10; $i++) {
    // for body
}
```

## `foreach`
    
A `foreach` statement looks like the following. Note the placement of parentheses, spaces, and braces.

```php
<?php
foreach ($iterable as $key => $value) {
    // foreach body
}
```

## `try`, `catch`

A `try catch` block looks like the following. Note the placement of parentheses, spaces, and braces.

```php
<?php
try {
    // try body
} catch (FirstExceptionType $e) {
    // catch body
} catch (OtherExceptionType $e) {
    // catch body
}
```


  
## Drupal - Alternate control statement syntax for templates

In templates (ex. .tpl.php files), the alternate control statement syntax using : instead of brackets is allowed. Note that there should not be a space between the closing paren after the control keyword, and the colon, and HTML/PHP inside the control structure should be indented. For example:

    <?php if (!empty($item)): ?>
      <p><?php print $item; ?></p>
    <?php endif; ?>
    
    <?php foreach ($items as $item): ?>
      <p><?php print $item; ?></p>
    <?php endforeach; ?>


---



# Closures

Closures MUST be declared with a space after the `function` keyword, and a space before and after the `use` keyword.

The opening brace MUST go on the same line, and the closing brace MUST go on the next line following the body.

There MUST NOT be a space after the opening parenthesis of the argument list or variable list, and there MUST NOT be a space before the closing parenthesis of the argument list or variable list.

In the argument list and variable list, there MUST NOT be a space before each comma, and there MUST be one space after each comma.

Closure arguments with default values MUST go at the end of the argument list.

A closure declaration looks like the following. Note the placement of parentheses, commas, spaces, and braces:

```php
<?php
$closureWithArgs = function ($arg1, $arg2) {
    // body
};

$closureWithArgsAndVars = function ($arg1, $arg2) use ($var1, $var2) {
    // body
};
```

Argument lists and variable lists MAY be split across multiple lines, where each subsequent line is indented once. When doing so, the first item in the list MUST be on the next line, and there MUST be only one argument or variable per line.

When the ending list (whether or arguments or variables) is split across multiple lines, the closing parenthesis and opening brace MUST be placed together on their own line with one space between them.

The following are examples of closures with and without argument lists and variable lists split across multiple lines.

```php
<?php
$longArgs_noVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) {
   // body
};

$noArgs_longVars = function () use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgs_longVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};

$longArgs_shortVars = function (
    $longArgument,
    $longerArgument,
    $muchLongerArgument
) use ($var1) {
   // body
};

$shortArgs_longVars = function ($arg) use (
    $longVar1,
    $longerVar2,
    $muchLongerVar3
) {
   // body
};
```

Note that the formatting rules also apply when the closure is used directly in a function or method call as an argument.

```php
<?php
$foo->bar(
    $arg1,
    function ($arg2) use ($var1) {
        // body
    },
    $arg3
);
```


---

# Operators

## Unary Operators

Unary operators (operators that operate on only one value), such as ++, should not have a space between the operator and the variable or number they are operating on.  Ex. `$count++;`

## Binary Operators

All binary operators (operators that come between two values), such as +, -, =, !=, ==, >, etc. should have a space before and after the operator, for readability. For example, an assignment should be formatted as `$foo = $bar;` rather than `$foo=$bar;`. 

## Ternary operator

The same rule as for if clauses also applies for the ternary operator: It may be split onto several lines, keeping the question mark and the colon at the front.

    <?php
    
    $a = $condition1 && $condition2
        ? $foo : $bar;
    
    $b = $condition3 && $condition4
        ? $foo_man_this_is_too_long_what_should_i_do
        : $bar;
    ?>

---


# Arrays


Arrays should be formatted with a space separating each element (after the comma), and spaces around the => key association operator, if applicable:

    $some_array = array('hello', 'world', 'foo' => 'bar');

Note that if the line declaring an array spans longer than 80 characters (often the case with form and menu declarations), each element should be broken into its own line, and indented one level:

    $form['title'] = array(
      '#type' => 'textfield',
      '#title' => t('Title'),
      '#size' => 60,
      '#maxlength' => 128,
      '#description' => t('The title of your node.'),
    );

Note the comma at the end of the last array element; This is not a typo! It helps prevent parsing errors if another element is placed at the end of the list later.

Starting with PHP 5.4, shorthand `[]` syntax MAY be used.

---

# Comments

Documentation standards might be coming soon.

Non-documentation comments are strongly encouraged. A general rule of thumb is that if you look at a section of code and think "Wow, I don't want to try and describe that", you need to comment it before you forget how it works.

C style comments (/* */) and standard C++ comments (//) are both fine. Use of Perl/shell style comments (#) is discouraged.

---

# Including Code

Anywhere you are unconditionally including a class file, use `require_once`. Anywhere you are conditionally including a class file (for example, factory methods), use `include_once`. Either of these will ensure that class files are included only once. They share the same file list, so you don't need to worry about mixing them - a file included with `require_once` will not be included again by `include_once`.

Note: `include_once` and `require_once` are statements, not functions. Parentheses should not surround the subject filename.

When including code from the same directory or a sub-directory, start the file path with ".":

    include_once ./includes/mymodule_formatting.inc

In Drupal 7.x and later versions, use DRUPAL_ROOT:

    require_once DRUPAL_ROOT . '/' . variable_get('cache_inc', 'includes/cache.inc');

For new projects when you're not using autoloading, you should create an `APP_ROOT` constant for this purpose.

---


# Semicolons

Semicolons that are optional in PHP are required in this standard.

    <?php print $tax; ?> -- YES
    <?php print $tax ?> -- NO


---


# Quotes

There's no hard standard on double quotes versus single quotes.  But stay consistent.

The rule of thumb is single quote strings are known to be faster because the parser doesn't have to look for in-line variables. Their use is recommended except in two cases:

  1. In-line variable usage, e.g. "<h2>$header</h2>".
  2. Translated strings where one can avoid escaping single quotes by enclosing the string in double quotes. One such string would be "He's a good person." It would be 'He\'s a good person.' with single quotes. Such escaping may not be handled properly by .pot file generators for text translation, and it's also somewhat awkward to read.

---

# String Concatenations
Always use a space between the dot and the concatenated parts to improve readability.

    <?php 
      $string = 'Foo' . $bar;
      $string = $bar . 'foo';
      $string = bar() . 'foo';
      $string = 'foo' . 'bar';
    ?>

When you concatenate simple variables, you can use double quotes and add the variable inside; otherwise, use single quotes.

    <?php
      $string = "Foo $bar";
    ?>

When using the concatenating assignment operator ('.='), use a space on each side as with the assignment operator:

    <?php
    $string .= 'Foo';
    $string .= $bar;
    $string .= baz();
    ?>

---

# Persistent Variables in Drupal

Persistent variables (variables/settings defined using Drupal's variable_get()/variable_set() functions) should be named using all lowercase letters, and words should be separated with an underscore. They should use the grouping/module name as a prefix, to avoid name collisions between modules.

---

# Global Functions

*Pear* - Global functions should be named using the "studly caps" style (also referred to as "bumpy case" or "camel caps"). In addition, they should have the package name as a prefix, to avoid name collisions between packages. The initial letter of the name (after the prefix) is lowercase, and each letter that starts a new "word" is capitalized. An example:

    XML_RPC_serializeData()

*Drupal* - Functions and variables should be named using lowercase, and words should be separated with an underscore. Functions should in addition have the grouping/module name as a prefix, to avoid name collisions between modules.

---


# Global Constants

Constants should always be all-uppercase, with underscores to separate words. Prefix constant names with the uppercased name of the class/package/module they are used in. Some examples:

    DB_DATASOURCENAME
    SERVICES_AMAZON_S3_LICENSEKEY

In PHP 5.3 and later, constants should be defined using the const PHP language keyword (instead of define()), because it is better for performance:

    <?php
    /**
     * Indicates that the item should be removed at the next general cache wipe.
     */
     const CACHE_TEMPORARY = -1;
    ?>

Note that const does not work with PHP expressions. define() should be used when defining a constant conditionally or with a non-literal value:

    <?php
    if (!defined('MAINTENANCE_MODE')) {
      define('MAINTENANCE_MODE', 'error');
    }
    ?>
---

# Readability of code blocks

Related lines of code should be grouped into blocks, separated from each other to keep readability as high as possible. The definition of "related" depends on the code :)

For example:

    <?php
    
    if ($foo) {
        $bar = 1;
    }
    if ($spam) {
        $ham = 1;
    }
    if ($pinky) {
        $brain = 1;
    }
    ?>

is a lot easier to read when separated:

    <?php
    
    if ($foo) {
        $bar = 1;
    }
    
    if ($spam) {
        $ham = 1;
    }
    
    if ($pinky) {
        $brain = 1;
    }
    ?>

---

# Return early

To keep readability in functions and methods, it is wise to return early if simple conditions apply that can be checked at the beginning of a method:

    <?php
    
    function foo($bar, $baz)
    {
        if ($foo) {
            //assume
            //that
            //here
            //is
            //the
            //whole
            //logic
            //of
            //this
            //method
            return $calculated_value;
        } else {
            return null;
        }
    }
    ?>

It's better to return early, keeping indentation and brain power needed to follow the code low.

    <?php
    
    function foo($bar, $baz)
    {
        if (!$foo) {
            return null;
        }
    
        //assume
        //that
        //here
        //is
        //the
        //whole
        //logic
        //of
        //this
        //method
        return $calculated_value;
    }
    ?>

---


# Directory Structure for New Projects

/RepoName/
    build/
        phpcs.xml
        phpmd.xml
    src/
    tests/
    build.xml
    phpdox.xml.dist
    phpunit.xml.dist

---

# Class-to-file convention

All public classes SHOULD be in their own file with underscores (_) or namespace separators (\) replaced by directory separator, so that `PEAR2_PackageName_Base` class or `PEAR2\PackageName\Base` class is always located in `PEAR2/PackageName/Base.php`.

---


# PHP Exceptions

## Base Exception Class

Each package SHOULD define a base class that is packagename_Exception. For example, the `PEAR2\PackageName` class defines an exception as follows in `PEAR2/PackageName/Exception.php`:

    <?php
    namespace PEAR2\PackageName;
    class Exception extends \Exception {}
    ?>


## Exception conventions

As Exceptions are classes, they should follow all coding standards for object-oriented code like any other class.
All Exceptions MUST end with the suffix "Exception".

Exception classes SHOULD be named for the subsystem to which they relate, and the type of error. That is, [Subsystem][ErrorType]Exception.

The use of subclassed Exceptions is preferred over reusing a single generic exception class with different error messages as different classes may then be caught separately.

Example:

    <?php
    class WidgetNotFoundException extends Exception {}
    
    function use_widget($widget_name) {
      $widget = find_widget($widget_name);
    
      if (!$widget) {
        throw new WidgetNotFoundException(t('Widget %widget not found.', array('%widget' => $widget_name)));
      }
    }
    ?>


## Exception Inheritance

PHP requires that all exceptions inherit off of the Exception class, either directly or indirectly.

When creating a new exception class, it should be named according to the subsystem they relate to and the error message they involve. If a given subsystem includes multiple exceptions, they should all extend from a common base exception. That allows for multiple catch blocks as necessary.

    <?php
    class FelineException extends Exception {}
    
    class FelineHairBallException extends FelineException {}
    
    class FelineKittenTooCuteException extends FelineException {}
    
    try {
      $nermal = new Kitten();
      $nermal->playWith($string);
    }
    catch (FelineHairBallException $e) {
      // Do error handling here.
    }
    catch (FelineKittenTooCuteException $e) {
      // Do different error handling here.
    }
    catch (FelineException $e) {
      // Do generic error handling here.
    }
    // Optionally also catch Exception so that all exceptions stop here instead of propagating up.
    ?>


---

# Loading other classes

Inside optional component loading methods (like factory or driver loading, etc.) class_exists($classname, true) should be used where a "class not found" fatal error would be confusing. For example, when loading a driver, a graceful exit via exception with helpful error message is preferrable to the fatal error:

    <?php
    if (!class_exists("PEAR2_PackageName_Driver_$class", true)) {
        throw new PEAR2\PackageName\Exception('Unknown driver ' .
        $class . ', be sure the driver exists and is loaded
        prior to use');
    }
    ?>

---

# Type Hinting

Introduced in PHP 5.3. It only applies to arrays, classes, and interfaces, not to the primitive types (int, float, double, string).  PHP 5.4 adds support for the `callable` type.

PHP supports optional type specification for function and method parameters for classes and arrays. Although called "type hinting" it does make a type required, as passing an object that does not conform to that type will result in a fatal error.

**DO** specify a type when conformity to a specific interface is an assumption made by the function or method. Specifying the required interface makes debugging easier as passing in a bad value will give a more useful error message.
**DO NOT** use a class as the type in type hinting. If specifying a type, always specify an Interface. That allows other developers to provide their own implementations if necessary without modifying existing code.
Example:

    <?php
    // Wrong:
    function make_cat_speak(GarfieldTheCat $cat) {
      print $cat->meow();
    }
    
    // Correct:
    function make_cat_speak(FelineInterface $cat) {
      print $cat->meow();
    }
    ?>

---

# Class Instantiation

Creating classes directly is discouraged. Instead, use a factory function that creates the appropriate object and returns it. This provides two benefits:

  1. It provides a layer of indirection, as the function may be written to return a different object (with the same interface) in different circumstances as appropriate.
  2. PHP does not allow class constructors to be chained, but does allow the return value from a function or method to be chained.

---

# Fluent Interfaces (Chaining)


PHP allows objects returned from functions and methods to be "chained", that is, a method on the returned object may be called immediately, like so:

    <?php
    // Unchained version
    $result = db_query("SELECT title FROM {node} WHERE nid = :nid", array(':nid' => 42));
    $title = $result->fetchField();
    
    // Chained version
    $title = db_query("SELECT title FROM {node} WHERE nid = :nid", array(':nid' => 42))->fetchField();
    ?>

As a general rule, a method should return $this, and thus be chainable, in any case where there is no other logical return value. Common examples are those methods that set some state or property on the object. It is better in those cases to return $this rather than TRUE/FALSE or NULL.

Using fluent application programming interfaces often leads to many concatenated function calls. Those calls may be split onto several lines. When doing this, all subsequent lines are indented by 4 spaces and begin with the "->" arrow.

    <?php
    
    $someObject->someFunction("some", "parameter")
        ->someOtherFunc(23, 42)
        ->andAThirdFunction();
    ?>


---



# Error Levels

## Pear - E_STRICT

Pear requires that code be E_STRICT-compatible. This means that it must not produce any warnings or errors when PHP's error reporting level is set to E_ALL | E_STRICT.

## Drupal - E_ALL

Drupal 6.x releases ignore E_NOTICE, E_STRICT, and E_DEPRECATED notices for the benefit of production sites. To view all PHP errors on development or testing sites, you may change includes/common.inc from:

    <?php
      if ($errno & (E_ALL ^ E_DEPRECATED ^ E_NOTICE)) {
    ?>

to:

    <?php
      if ($errno & (E_ALL | E_STRICT)) {
    ?>

Drupal 7.x releases report any error levels which are part of E_ALL, and allow PHP to be configured to report additional error levels, such as E_STRICT. To view all PHP errors on development or testing sites, you may set

    php_value error_reporting -1

in the .htaccess file.


---


# Use of isset() or !empty()

If you want to test the value of a variable, array element or object property, you may need to use

    <?php
     if (isset($var)) 
    ?>

or

    <?php
     if (!empty($var)) 
    ?>

rather than

    <?php
     if ($var) 
    ?>

if there is a possibility that $var has not been defined.
The difference between isset() and !empty() is that unlike !empty(), isset() will return TRUE even if the variable is set to an empty string or zero. In order to decide which one to use, consider whether '' or 0 are valid and expected values for your variable.

The following code may trigger an E_NOTICE error:

    <?php
    function _form_builder($form, $parents = array(), $multiple = FALSE) {
      // (...)
      if ($form['#input']) {
        // some code (...)
      }
    }
    ?>

Here, the variable $form is passed on to the function. If $form['#input'] evaluates as true, some code is executed. However, if $form['#input'] does not exist, the function outputs the following error message: notice: Undefined index:  #input in includes/form.inc on line 194.

Even though the array $form is already declared and passed to the function, each array index must be explicitly declared. The previous code should read:

    <?php
    function _form_builder($form, $parents = array(), $multiple = FALSE) {
      // (...)
      if (!empty($form['#input'])) {
        // some code (...)
      }
    }
    ?>

**Beware!**

The function isset() returns TRUE when the variable is set to 0, but FALSE if the variable is set to NULL. In some cases, is_null() is a better choice, especially when testing the value of a variable returned by an SQL query.

---



# Casting
Put a space between the (type) and the $variable in a cast: `(int) $mynumber`.

---


# Use Master Functions instead of Function Aliases

For example, use implode() instead of join(). See http://php.net/manual/en/aliases.php

---

# Class Member Visibility

The use of public properties is strongly discouraged, as it allows for unwanted side effects. It also exposes implementation specific details, which in turn makes swapping out a class for another implementation (one of the key reasons to use objects) much harder. Properties should be considered internal to a class.

The use of private methods or properties is strongly discouraged. Private properties and methods may not be accessed or overridden by child classes, which limits the ability of other developers to extend a class to suit their needs.

