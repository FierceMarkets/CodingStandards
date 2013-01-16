# Casting
Put a space between the (type) and the $variable in a cast: (int) $mynumber.

---

# TRUE, FALSE, NULL, or true, false, null?

*Pear* - All lowercase.

*Drupal* - All uppercase.

*PSR-2* - The PHP constants `true`, `false`, and `null` MUST be in lower case.

---

# Keywords

PHP [keywords][] MUST be in lower case.

[keywords]: http://php.net/manual/en/reserved.keywords.php

---

# Indenting, Line Length, and Whitespace

## 2 space tabs or 4 space tabs?

*PSR-2* - Code MUST use an indent of 4 spaces, and MUST NOT use tabs for indenting.

> N.b.: Using only spaces, and not mixing spaces with tabs, helps to avoid
> problems with diffs, patches, history, and annotations. The use of spaces
> also makes it easy to insert fine-grained sub-indentation for inter-line 
> alignment.


*Pear* - Use an indent of 4 spaces, with no tabs.

*Drupal* - Use an indent of 2 spaces, with no tabs.

This helps to avoid problems with diffs, patches, version control history and annotations.

Files should be formatted with \n as the line ending (Unix line endings), not \r\n (Windows line endings).

All text files should end in a single newline (\n). This avoids the verbose "\ No newline at end of file" patch warning and makes patches easier to read since it's clearer what is being changed when lines are added to the end of a file.

## Vim Settings

Here are Vim rules for indenting:

    set expandtab
    set shiftwidth=4
    set softtabstop=4
    set tabstop=4

## Sublime Text 2 Settings

Add these to your **Sublime Text 2** User Preferences.

    {
        "default_line_ending": "unix",
        "ensure_newline_at_eof_on_save": true,
        "tab_size": 2,
        "translate_tabs_to_spaces": true,
        "trim_trailing_white_space_on_save": true
    }

## Whitespace

*PSR-2* - There MUST NOT be trailing whitespace at the end of non-blank lines.

*PSR-2* - Blank lines MAY be added to improve readability and to indicate related blocks of code.

## Line Length

*PSR-2* - There MUST NOT be a hard limit on line length; the soft limit MUST be 120 characters; lines SHOULD be 80 characters or less. Lines longer than that SHOULD be split into multiple subsequent lines of no more than 80 characters each.

*Drupal* - In general, all lines of code should not be longer than 80 characters for better code readability.

Lines containing longer function names, function/class definitions, variable declarations, etc are allowed to exceed 80 chars.
Control structure conditions may exceed 80 chars, if they are simple to read and understand:

    if ($something['with']['something']['else']['in']['here'] == mymodule_check_something($whatever['else'])) {
    ...
    }
    if (isset($something['what']['ever']) && $something['what']['ever'] > $infinite && user_access('galaxy')) {
    ...
    }
    // Non-obvious conditions of low complexity are also acceptable, but should
    // always be documented, explaining WHY a particular check is done.
    if (preg_match('@(/|\\)(\.\.|~)@', $target) && strpos($target_dir, $repository) !== 0) {
    return FALSE;
    }

Conditions should not be wrapped into multiple lines.
Control structure conditions should also NOT attempt to win the Most Compact Condition In Least Lines Of Code Award™:

      // DON'T DO THIS!
      if ((isset($key) && !empty($user->uid) && $key == $user->uid) || (isset($user->cache) ? $user->cache : '') == ip_address() || isset($value) && $value >= time())) {
        ...
      }

Instead, it is recommended practice to split out and prepare the conditions separately, which also permits documenting the underlying reasons for the conditions:

    // Key is only valid if it matches the current user's ID, as otherwise other
    // users could access any user's things.
    $is_valid_user = (isset($key) && !empty($user->uid) && $key == $user->uid);
    
    // IP must match the cache to prevent session spoofing.
    $is_valid_cache = (isset($user->cache) ? $user->cache == ip_address() : FALSE);
    
    // Alternatively, if the request query parameter is in the future, then it
    // is always valid, because the galaxy will implode and collapse anyway.
    $is_valid_query = $is_valid_cache || (isset($value) && $value >= time());
    
    if ($is_valid_user || $is_valid_query) {
    ...
    }

Note: This example is still a bit dense. Always consider and decide on your own whether people unfamiliar with your code will be able to make sense of the logic.

## More

*PSR-2* - There MUST NOT be more than one statement per line.

---


# Control Structures

Control structures include if, for, while, switch, etc.

The body of each structure MUST be enclosed by braces. This standardizes how the structures look, and reduces the likelihood of introducing errors as new lines get added to the body.

Control structure keywords MUST have one space between the control keyword and opening parenthesis, to distinguish them from function calls.

- Opening parentheses for control structures MUST NOT have a space after them, and closing parentheses for control structures MUST NOT have a space before.

- Opening braces for control structures MUST go on the same line, and closing braces MUST go on the next line after the body.

- There MUST be one space between the closing parenthesis and the opening brace
- The structure body MUST be indented once
- The closing brace MUST be on the next line after the body



### `if`, `elseif`, `else`

The keyword `elseif` MUST be used instead of `else if` so that all control keywords look like single words.

#### Drupal and Pear if/elseif/else structure

Note the elseif and else are on new lines.

    if (condition1 || condition2) {
      action1;
    }
    elseif (condition3 && condition4) {
      action2;
    }
    else {
      defaultaction;
    }

#### PSR if/elseif/else structure

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


### `switch`, `case`

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


### `while`, `do while`

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



### `for`

A `for` statement looks like the following. Note the placement of parentheses, spaces, and braces.

```php
<?php
for ($i = 0; $i < 10; $i++) {
    // for body
}
```

### `foreach`
    
A `foreach` statement looks like the following. Note the placement of parentheses, spaces, and braces.

```php
<?php
foreach ($iterable as $key => $value) {
    // foreach body
}
```

### `try`, `catch`

#### PSR try-catch

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

#### Drupal Try-catch blocks

try-catch blocks should follow a similar line-breaking pattern to if-else statements, with each catch statement beginning a new line.

Example:

    <?php
    try {
      $widget = 'thingie';
      $result = use_widget($widget);
    
      // Continue processing the $result.
      // If an exception is thrown by use_widget(), this code never gets called.
    }
    catch (WidgetNotFoundException $e) {
      // Error handling specific to the absence of a widget.
    }
    catch (Exception $e) {
      // Generic exception handling if something else gets thrown.
      watchdog('widget', $e->getMessage(), WATCHDOG_ERROR);
    }
    ?>



  
## Drupal - Alternate control statement syntax for templates

In templates (ex. .tpl.php files), the alternate control statement syntax using : instead of brackets is allowed. Note that there should not be a space between the closing paren after the control keyword, and the colon, and HTML/PHP inside the control structure should be indented. For example:

    <?php if (!empty($item)): ?>
      <p><?php print $item; ?></p>
    <?php endif; ?>
    
    <?php foreach ($items as $item): ?>
      <p><?php print $item; ?></p>
    <?php endforeach; ?>


## Pear and Drupal - Split long if statements onto several lines

Long if statements may be split onto several lines when the character/line limit would be exceeded. The conditions have to be positioned onto the following line, and indented 4 characters. The logical operators (&&, ||, etc.) should be at the beginning of the line to make it easier to comment (and exclude) the condition. The closing parenthesis and opening brace get their own line at the end of the conditions.

Keeping the operators at the beginning of the line has two advantages: It is trivial to comment out a particular line during development while keeping syntactically correct code (except of course the first line). Further is the logic kept at the front where it's not forgotten. Scanning such conditions is very easy since they are aligned below each other.

    <?php
    
    if (($condition1
        || $condition2)
        && $condition3
        && $condition4
    ) {
        //code here
    }
    ?>
    The first condition may be aligned to the others.
    
    <?php
    
    if (   $condition1
        || $condition2
        || $condition3
    ) {
        //code here
    }
    ?>

The best case is of course when the line does not need to be split. When the if clause is really long enough to be split, it might be better to simplify it. In such cases, you could express conditions as variables an compare them in the if() condition. This has the benefit of "naming" and splitting the condition sets into smaller, better understandable chunks:

    <?php
    
    $is_foo = ($condition1 || $condition2);
    $is_bar = ($condition3 && $condtion4);
    if ($is_foo && $is_bar) {
        // ....
    }
    ?>

There were suggestions to indent the parantheses "groups" by 1 space for each grouping. This is too hard to achieve in your coding flow, since your tab key always produces 4 spaces. Indenting the if clauses would take too much finetuning.

---

# Operators

## Unary Operators

Unary operators (operators that operate on only one value), such as ++, should not have a space between the operator and the variable or number they are operating on.

## Binary Operators

All binary operators (operators that come between two values), such as +, -, =, !=, ==, >, etc. should have a space before and after the operator, for readability. For example, an assignment should be formatted as $foo = $bar; rather than $foo=$bar;. 

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

To support readability, parameters in subsequent calls to the same function/method may be aligned by parameter name:

    <?php
    $this->callSomeFunction('param1',     'second',        true);
    $this->callSomeFunction('parameter2', 'third',         false);
    $this->callSomeFunction('3',          'verrrrrrylong', true);
    ?>



## Split function call on several lines

The CS require lines to have a maximum length of 80 chars. Calling functions or methods with many parameters while adhering to CS is impossible in that cases. It is allowed to split parameters in function calls onto several lines.

    <?php
    
    $this->someObject->subObject->callThisFunctionWithALongName(
        $parameterOne, $parameterTwo,
        $aVeryLongParameterThree
    );
    ?>

Several parameters per line are allowed. Parameters need to be indented 4 spaces compared to the level of the function call. The opening parenthesis is to be put at the end of the function call line, the closing parenthesis gets its own line at the end of the parameters. This shows a visual end to the parameter indentations and follows the opening/closing brace rules for functions and conditionals.

The same applies not only for parameter variables, but also for nested function calls and for arrays.

    <?php
    
    $this->someObject->subObject->callThisFunctionWithALongName(
        $this->someOtherFunc(
            $this->someEvenOtherFunc(
                'Help me!',
                array(
                    'foo'  => 'bar',
                    'spam' => 'eggs',
                ),
                23
            ),
            $this->someEvenOtherFunc()
        ),
        $this->wowowowowow(12)
    );
    ?>

Nesting those function parameters is allowed if it helps to make the code more readable, not only when it is necessary when the characters per line limit is reached.


## Alignment of assignments

To support readability, the equal signs may be aligned in block-related assignments:

    <?php
    
    $short  = foo($bar);
    $longer = foo($baz);
    ?>

The rule can be broken when the length of the variable name is at least 8 characters longer/shorter than the previous one:

    <?php
    
    $short = foo($bar);
    $thisVariableNameIsVeeeeeeeeeeryLong = foo($baz);
    ?>

## Split long assigments onto several lines

Assigments may be split onto several lines when the character/line limit would be exceeded. The equal sign has to be positioned onto the following line, and indented by 4 characters.

    <?php
    
    $GLOBALS['TSFE']->additionalHeaderData[$this->strApplicationName]
        = $this->xajax->getJavascript(t3lib_extMgm::siteRelPath('nr_xajax'));
    ?>

---

# Class Definitions

Opening braces for classes MUST go on the next line, and closing braces MUST go on the next line after the body.

    <?php
    class Foo_Bar
    {
    
        //... code goes here
    
    }
    ?>


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

# Function Definitions

## Brace on the same line or on a new line?

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

Arguments with default values go at the end of the argument list. Always attempt to return a meaningful value from a function if one is appropriate. Here is a slightly longer example:

    <?php
    function connect(&$dsn, $persistent = false)
    {
        if (is_array($dsn)) {
            $dsninfo = &$dsn;
        } else {
            $dsninfo = DB::parseDSN($dsn);
        }
    
        if (!$dsninfo || !$dsninfo['phptype']) {
            return $this->raiseError();
        }
    
        return true;
    }
    ?>

## Split function definitions onto several lines

Functions with many parameters may need to be split onto several lines to keep the 80 characters/line limit. The first parameters may be put onto the same line as the function name if there is enough space. Subsequent parameters on following lines are to be indented 4 spaces. The closing parenthesis and the opening brace are to be put onto the next line, on the same indentation level as the "function" keyword.

    <?php
    
    function someFunctionWithAVeryLongName($firstParameter = 'something', $secondParameter = 'booooo',
        $third = null, $fourthParameter = false, $fifthParameter = 123.12,
        $sixthParam = true
    ) {
        //....
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


---

# Comments

Sorry Mario, the Commenting standards are in another document.

Non-documentation comments are strongly encouraged. A general rule of thumb is that if you look at a section of code and think "Wow, I don't want to try and describe that", you need to comment it before you forget how it works.

C style comments (/* */) and standard C++ comments (//) are both fine. Use of Perl/shell style comments (#) is discouraged.

---

# Including Code

Anywhere you are unconditionally including a class file, use require_once(). Anywhere you are conditionally including a class file (for example, factory methods), use include_once(). Either of these will ensure that class files are included only once. They share the same file list, so you don't need to worry about mixing them - a file included with require_once() will not be included again by include_once().

Note: include_once() and require_once() are statements, not functions. Parentheses should not surround the subject filename.

When including code from the same directory or a sub-directory, start the file path with ".":

    include_once ./includes/mymodule_formatting.inc

In Drupal 7.x and later versions, use DRUPAL_ROOT:
require_once DRUPAL_ROOT . '/' . variable_get('cache_inc', 'includes/cache.inc');

For new projects when you're not using autoloading, you should define an APP_ROOT for this purpose.

---

# PHP Code Tags

Always use <?php ?> to delimit PHP code, not the shorthand, <? ?>. This is required for Drupal and Pear compliance and is also the most portable way to include PHP code on differing operating systems and set-ups.

Note that the ?> at the end of code files is purposely omitted. This includes module and include files. The reasons for this can be summarized as:

 > Removing it eliminates the possibility for unwanted    whitespace at the end of files which can cause "header already sent" errors, XHTML/XML validation issues, and other problems.
 > The closing delimiter at the end of a file is optional.
PHP.net itself removes the closing delimiter from the end of its files, so this can be seen as a "best practice."

---

# Semicolons
The PHP language requires semicolons at the end of most lines, but allows them to be omitted at the end of code blocks. Drupal coding standards require them, even at the end of code blocks. In particular, for one-line PHP blocks:

    <?php print $tax; ?> -- YES
    <?php print $tax ?> -- NO


---


# Quotes
There's no hard standard on double quotes versus single quotes.  Stay consistent.

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

# Header Comment Blocks
All source code files in the PEAR repository shall contain a "page-level" docblock at the top of each file and a "class-level" docblock immediately above each class. Below are examples of such docblocks.

    <?php
    
    /* vim: set expandtab tabstop=4 shiftwidth=4 softtabstop=4: */
    
    /**
     * Short description for file
     *
     * Long description for file (if any)...
     *
     * PHP version 5
     *
     * LICENSE: This source file is subject to version 3.01 of the PHP license
     * that is available through the world-wide-web at the following URI:
     * http://www.php.net/license/3_01.txt.  If you did not receive a copy of
     * the PHP License and are unable to obtain it through the web, please
     * send a note to license@php.net so we can mail you a copy immediately.
     *
     * @category   CategoryName
     * @package    PackageName
     * @author     Original Author <author@example.com>
     * @author     Another Author <another@example.com>
     * @copyright  1997-2005 The PHP Group
     * @license    http://www.php.net/license/3_01.txt  PHP License 3.01
     * @version    SVN: $Id$
     * @link       http://pear.php.net/package/PackageName
     * @see        NetOther, Net_Sample::Net_Sample()
     * @since      File available since Release 1.2.0
     * @deprecated File deprecated in Release 2.0.0
     */
    
    /*
    * Place includes, constant defines and $_GLOBAL settings here.
    * Make sure they have appropriate docblocks to avoid phpDocumentor
    * construing they are documented by the page-level docblock.
    */
    
    /**
     * Short description for class
     *
     * Long description for class (if any)...
     *
     * @category   CategoryName
     * @package    PackageName
     * @author     Original Author <author@example.com>
     * @author     Another Author <another@example.com>
     * @copyright  1997-2005 The PHP Group
     * @license    http://www.php.net/license/3_01.txt  PHP License 3.01
     * @version    Release: @package_version@
     * @link       http://pear.php.net/package/PackageName
     * @see        NetOther, Net_Sample::Net_Sample()
     * @since      Class available since Release 1.2.0
     * @deprecated Class deprecated in Release 2.0.0
     */
    class Foo_Bar
    {
    }
    
    ?>

## Required Tags That Have Variable Content
### Short Descriptions
Short descriptions must be provided for all docblocks. They should be a quick sentence, not the name of the item. Please read the Coding Standard's Sample File about how to write good descriptions.

### @deprecated
This tag is required when a file or class is no longer used but has been left in place for backwards compatibility.

## Optional Tags

### @see
Add a @see tag when you want to refer users to other sections of the package's documentation. If you have multiple items, separate them with commas rather than adding multiple @see tags.

## Order and Spacing

To ease long term readability of PEAR source code, the text and tags must conform to the order and spacing provided in the example above. This standard is adopted from the JavaDoc standard.

---

# Persistent Variables in Drupal

Persistent variables (variables/settings defined using Drupal's variable_get()/variable_set() functions) should be named using all lowercase letters, and words should be separated with an underscore. They should use the grouping/module name as a prefix, to avoid name collisions between modules.

---

# Global Variables

If your package needs to define global variables, their names should start with a single underscore followed by the package/module name and another underscore. For example, the PEAR package uses a global variable called 

    $_PEAR_destructor_object_list.

---

# Global Functions

*Pear* - Global functions should be named using the "studly caps" style (also referred to as "bumpy case" or "camel caps"). In addition, they should have the package name as a prefix, to avoid name collisions between packages. The initial letter of the name (after the prefix) is lowercase, and each letter that starts a new "word" is capitalized. An example:

    XML_RPC_serializeData()

*Drupal* - Functions and variables should be named using lowercase, and words should be separated with an underscore. Functions should in addition have the grouping/module name as a prefix, to avoid name collisions between modules.

---

# Classes

The term "class" refers to all classes, interfaces, and traits.

A single file MUST contain only one class.

*PSR* - Class names MUST be declared in `StudlyCaps`.

*PSR* - Class constants MUST be declared in all upper case with underscore separators.

*Drupal* - See http://drupal.org/node/608152

Classes should be given descriptive names. Avoid using abbreviations where possible. Class names should always begin with an uppercase letter. The PEAR class hierarchy is also reflected in the class name, each level of the hierarchy separated with a single underscore. Examples of good class names are:

    Log
    Net_Finger
    HTML_Upload_Error

---

# Namespaces

Namespaces came along in PHP 5.3.

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


## Class and Namespaces based on PSR 0

The following describes the mandatory requirements that must be adhered to for autoloader interoperability.

### Mandatory

* A fully-qualified namespace and class must have the following
  structure `\<Vendor Name>\(<Namespace>\)*<Class Name>`
* Each namespace must have a top-level namespace ("Vendor Name").
* Each namespace can have as many sub-namespaces as it wishes.
* Each namespace separator is converted to a `DIRECTORY_SEPARATOR` when
  loading from the file system.
* Each `_` character in the CLASS NAME is converted to a
  `DIRECTORY_SEPARATOR`. The `_` character has no special meaning in the
  namespace.
* The fully-qualified namespace and class is suffixed with `.php` when
  loading from the file system.
* Alphabetic characters in vendor names, namespaces, and class names may
  be of any combination of lower case and upper case.

### Examples

* `\Doctrine\Common\IsolatedClassLoader` => `/path/to/project/lib/vendor/Doctrine/Common/IsolatedClassLoader.php`
* `\Symfony\Core\Request` => `/path/to/project/lib/vendor/Symfony/Core/Request.php`
* `\Zend\Acl` => `/path/to/project/lib/vendor/Zend/Acl.php`
* `\Zend\Mail\Message` => `/path/to/project/lib/vendor/Zend/Mail/Message.php`


### Underscores in Namespaces and Class Names

* `\namespace\package\Class_Name` => `/path/to/project/lib/vendor/namespace/package/Class/Name.php`
* `\namespace\package_name\Class_Name` => `/path/to/project/lib/vendor/namespace/package_name/Class/Name.php`

The standards we set here should be the lowest common denominator for painless autoloader interoperability.

Namespaces and classes SHOULD follow PSR-0.

This means each class is in a file by itself, and is in a namespace of at least one level: a top-level vendor name.

Code written for PHP 5.3 and after MUST use formal namespaces.

For example:

```php
<?php
// PHP 5.3 and later:
namespace Vendor\Model;

class Foo
{
}
```

Code written for 5.2.x and before SHOULD use the pseudo-namespacing convention
of `Vendor_` prefixes on class names.

```php
<?php
// PHP 5.2.x and earlier:
class Vendor_Model_Foo
{
}
```
---

# Class Properties

*Pear* - Class properties (a.k.a variables) and methods should be named using the "studly caps" style (also referred to as "bumpy case" or "camel caps"). Some examples (these would be "public" members):

    $counter

Private class members are preceded by a single underscore. For example:

    $_status
    _sort()
    _initTree()

The following applies to PHP5.
Protected class members are not preceded by a single underscore. For example:

    protected $somevar
    protected function initTree()

Visibility MUST be declared on all properties.

The `var` keyword MUST NOT be used to declare a property.

There MUST NOT be more than one property declared per statement.

Property names SHOULD NOT be prefixed with a single underscore to indicate
protected or private visibility.

A property declaration looks like the following.

```php
<?php
namespace Vendor\Package;

class ClassName
{
    public $foo = null;
}
```

---

# Class Methods

*PSR* - Method names MUST be declared in `camelCase`.

*Pear* - Class methods should be named using the "studly caps" style (also referred to as "bumpy case" or "camel caps"). Some examples (these would be "public" members):

    connect()
    getData()
    buildSomeWidget()

*PSR* - Opening braces for methods MUST go on the next line, and closing braces MUST go on the next line after the body.

Visibility MUST be declared on all methods.

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
  
---

# Method Arguments

In the argument list, there MUST NOT be a space before each comma, and there MUST be one space after each comma.

Method arguments with default values MUST go at the end of the argument
list.

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

Argument lists MAY be split across multiple lines, where each subsequent line
is indented once. When doing so, the first item in the list MUST be on the
next line, and there MUST be only one argument per line.

When the argument list is split across multiple lines, the closing parenthesis
and opening brace MUST be placed together on their own line with one space
between them.

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

# `abstract`, `final`, and `static`

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

---

# Class Constants

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

---

# Visibility

- Visibility (public, private, protected) MUST be declared on all properties and methods; `abstract` and `final` MUST be declared before the visibility; `static` MUST be declared after the visibility.

---

# Extends and Implements

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



---

# Global Constants


Constants should always be all-uppercase, with underscores to separate words. Prefix constant names with the uppercased name of the class/package/module they are used in. Some examples:

    DB_DATASOURCENAME
    SERVICES_AMAZON_S3_LICENSEKEY

In Drupal 8 and later, constants should be defined using the const PHP language keyword (instead of define()), because it is better for performance:

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

# File Formats

All PHP files MUST use the Unix LF (linefeed) line ending.

All PHP files MUST end with a single blank line.

The closing `?>` tag MUST be omitted from files containing only PHP.

All PHP files MUST be stored as ASCII text

All PHP files MUST use UTF-8 character encoding without BOM.  Whatever "without BOM" means.

All PHP files SHOULD *Either* declare symbols (classes, functions, constants, etc.) *or* cause side-effects (e.g. generate output, change .ini settings, etc.) but SHOULD NOT do both.

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


---

# Best practices from Pear
There are other things not covered by PEAR Coding Standards which are mostly subject of personal preference and not directly related to readability of the code. Things like "single quotes vs double quotes" are features of PHP itself to make programming easier and there are no reasons not use one way in preference to another. Such best practices are left solely on developer to decide. The only recommendation could be made to keep consistency within package and respect personal style of other developers.

## Readability of code blocks
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

## Return early

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

# Namespace Prefix
All classes and functions must have a namespace of at the minimum PEAR2. An example:

    <?php
    namespace PEAR2;
    class MyClass {}
    ?>

Classes may use longer namespaces, for instance an HTTP_Request class may instead choose to use this declarative syntax:

    <?php
    namespace PEAR2\HTTP;
    class Request {}
    ?>

As such, underscores are no longer required of any classes if there is a namespace. Class PEAR2_HTTP_Request instead becomes PEAR2\HTTP\Request. Package names, however, will use underscores, making PEAR2_HTTP_Request the package name.

---

# Nice-to-have - Class autoloading
use of include/require/require_once/include_once not allowed
include/require/require_once/include_once is not allowed for loading class files. Users will be expected to load files either with __autoload() or a customized solution for more advanced users. Instead, classes should simply be used. import with a comment describing the class's location must be used to document all internal dependencies (as written below). Instead of:

    <?php
    require_once 'PEAR2/OtherPackage.php';
    $class = new PEAR2\OtherPackage;
    ?>

this class should simply be used:
    
    <?php
    $class = new PEAR2\OtherPackage;
    ?>

This allows packages to work without modification no matter how they are structured on-disk, including running out of a single large file, inside a phar archive, and provides much-needed flexibility.

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

# Nice-to-have - Class-to-file convention

All public classes must be in their own file with underscores (_) or namespace separators (\) replaced by directory separator, so that PEAR2_PackageName_Base class or PEAR2\PackageName\Base class is always located in PEAR2/PackageName/Base.php (this is required to make autoload work)

---

# Base Exception class

Each package must define a base class that is packagename_Exception. For example, the PEAR2\PackageName class defines an exception as follows in PEAR2/PackageName/Exception.php:

    <?php
    namespace PEAR2\PackageName;
    class Exception extends \Exception {}
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

# Namespaces

PHP 5.3 introduces namespaces to the language. This page documents how namespaces should be referenced within Drupal and it assumes that you are familiar with the concept of namespaces.

Not all files in Drupal declare a namespace. As of Drupal 8 an increasing number of files do, but not all. Prior to Drupal 8 virtually no code used namespaces, in order to remain compatible with PHP 5.2. There are therefore two slightly different standards.

---

# "use"-ing classes

Classes and interfaces with a backslash \ inside their fully-qualified name (for example: Drupal\simpletest\WebTestBase) must not use their fully-qualified name inside the code. If the namespace differs from the namespace of the current file, put a use statement on the top of the file. For example:

    <?php
    namespace Drupal\mymodule\Tests\Foo;
    
    use Drupal\simpletest\WebTestBase;
    
    /**
     * Tests that the foo bars.
     */
    class BarTest extends WebTestBase {
    ?>

Classes and interfaces without a backslash \ inside their fully-qualified name (for example, the built-in PHP Exception class) must be fully qualified when used in a namespaced file. For example: new \Exception();.
In a file that does not declare a namespace (and is therefore in the global namespace), classes in any namespace other than global must be specified with a "use" statement at the top of the file.
When importing a class with "use", do not include a leading \. (The PHP documentation makes the same recommendation.)
When specifying a class name in a string, use its full name including namespace, without leading \.
Escape the namespace separator in double-quoted strings: "Drupal\\Context\\ContextInterface"

Do not escape it in single-quoted strings: 'Drupal\Context\ContextInterface'

As stated elsewhere, single-quoted strings are generally preferred.

Specify a single class per use statement. Do not specify multiple classes in a single use statement.
API documentation (in .api.php files) should use full class names. Note that if a class is used more than once in multiple hook signatures, it must still be "use"ed, and then only the short names of the class should be used in the function.
Example:

    <?php
    /**
     * @file
     * Contains Drupal\Subsystem\Foo.
     */
    
    namespace Drupal\Subsystem;
    
    // This imports just the Cat class from the Drupal\Othersystem namespace.
    use Drupal\Othersystem\Cat;
    
    // Bar is a class in the Drupal\Subsystem namespace in another file.
    // It is already available without any importing.
    
    /**
     * Defines a Foo.
     */
    class Foo {
    
      /**
       * Constructs a new Foo object.
       */
      public function __construct(Bar $b, Cat $c) {
        // Global classes must be prefixed with a \ character.
        $d = new \DateTime();
      }
    
    }
    ?>

    <?php
    /**
     * @file
     * The Example module.
     *
     * This file is not part of any namespace, so all global namespaced classes
     *  are automatically available.
     */
    
    use Drupal\Subsystem\Foo;
    
    /**
     * Does stuff with Foo stuff.
     *
     * @param Drupal\Subsystem\Foo $f
     *   A Foo object used to bar the baz.
     */
    function do_stuff(Foo $f) {
      // The DateTime class does not need to be imported as it is already global
      $d = new DateTime();
    }
    ?>

---

# Class aliasing

PHP allows classes to be aliased when they are imported into a namespace. In general that should only be done to avoid a name collision. If a collision happens, alias both colliding classes by prefixing the next higher portion of the namespace.

Example:

    <?php
    use Foo\Bar\Baz as BarBaz;
    use Stuff\Thing\Baz as ThingBaz;
    
    /**
     * Tests stuff for the whichever.
     */
    function test() {
      $a = new BarBaz(); // This will be Foo\Bar\Baz
      $b = new ThingBaz(); // This will be Stuff\Thing\Baz
    }
    ?>

That helps keep clear which one is which, and where it comes from. Aliasing should only be done to avoid name collisions.

---

# Drupal Module Namespacing

Modules creating classes should place their code inside a custom namespace.
The convention for those namespaces is

    namespace Drupal\example_module

To support autoloading, these classes should be placed inside the correct folder:
sites/all/modules/example_module/lib/Drupal/example_module
<module folder> /lib/ <namespace>



---



# Class/Interface/Method Naming Conventions

A complete example of class/interface/method names:

    <?php
    interface FelineInterface {
    
      public function meow();
    
      public function eatLasagna($amount);
    
    }
    
    class GarfieldTheCat implements FelineInterface {
    
      protected $lasagnaEaten = 0;
    
      public function meow() {
        return t('Meow!');
      }
    
      public function eatLasagna($amount) {
        $this->lasagnaEaten += $amount;
      }
    }
    ?>

---

# Class Member Visibility
All methods and properties of classes must specify their visibility: public, protected, or private. The PHP 4-style "var" declaration must not be used.

The use of public properties is strongly discouraged, as it allows for unwanted side effects. It also exposes implementation specific details, which in turn makes swapping out a class for another implementation (one of the key reasons to use objects) much harder. Properties should be considered internal to a class.

The use of private methods or properties is strongly discouraged. Private properties and methods may not be accessed or overridden by child classes, which limits the ability of other developers to extend a class to suit their needs.

---

# Type hinting

Introduced in PHP 5.3. It only applies to arrays, classes, and interfaces, not to the primitive types (int, float, double, string).

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

## Chaining
PHP allows objects returned from functions and methods to be "chained", that is, a method on the returned object may be called immediately, like so:

    <?php
    // Unchained version
    $result = db_query("SELECT title FROM {node} WHERE nid = :nid", array(':nid' => 42));
    $title = $result->fetchField();
    
    // Chained version
    $title = db_query("SELECT title FROM {node} WHERE nid = :nid", array(':nid' => 42))->fetchField();
    ?>

As a general rule, a method should return $this, and thus be chainable, in any case where there is no other logical return value. Common examples are those methods that set some state or property on the object. It is better in those cases to return $this rather than TRUE/FALSE or NULL.

## Chaining Style

Using fluent application programming interfaces often leads to many concatenated function calls. Those calls may be split onto several lines. When doing this, all subsequent lines are indented by 4 spaces and begin with the "->" arrow.

    <?php
    
    $someObject->someFunction("some", "parameter")
        ->someOtherFunc(23, 42)
        ->andAThirdFunction();
    ?>

---

# PHP Exceptions

## Basic Exception conventions

As Exceptions are classes, they should follow all coding standards for object-oriented code like any other class.
All Exceptions must end with the suffix "Exception".
All Exceptions must include an appropriate translated string as a message, unless the Exception is thrown very early in the bootstrap process before the translation system is available. That is extremely rare.
Exception classes should be named for the subsystem to which they relate, and the type of error. That is, [Subsystem][ErrorType]Exception.
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

# Closures

Closures were introduced in PHP 5.3.

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

# E_STRICT-compatible code
Starting on 01 January 2007, all new code that is suggested for inclusion into PEAR must be E_STRICT-compatible. This means that it must not produce any warnings or errors when PHP's error reporting level is set to E_ALL | E_STRICT.

---


# Drupal - Write E_ALL compliant code

Adjusting the error reporting level
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

# Use Master Functions instead of Function Aliases

See http://php.net/manual/en/aliases.php
