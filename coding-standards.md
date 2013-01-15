
# Indenting and Line Length
Use an indent of 4 spaces, with no tabs. This helps to avoid problems with diffs, patches, version control history and annotations.

Here are Vim rules for indenting:

    set expandtab
    set shiftwidth=4
    set softtabstop=4
    set tabstop=4

It is recommended to keep lines at approximately 75-85 characters long for better code readability.

# Control Structures
These include if, for, while, switch, etc. Here is an example if statement, since it is the most complicated of them:

    <?php
    if ((condition1) || (condition2)) {
        action1;
    } elseif ((condition3) && (condition4)) {
        action2;
    } else {
        defaultaction;
    }
    ?>

Control statements should have one space between the control keyword and opening parenthesis, to distinguish them from function calls.

You are strongly encouraged to always use curly braces even in situations where they are technically optional. Having them increases readability and decreases the likelihood of logic errors being introduced when new lines are added.

For switch statements:

    <?php
    switch (condition) {
    case 1:
        action1;
        break;
    
    case 2:
        action2;
        break;
    
    default:
        defaultaction;
        break;
    }
    ?>

**Split long if statements onto several lines**

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

**Ternary operators**

The same rule as for if clauses also applies for the ternary operator: It may be split onto several lines, keeping the question mark and the colon at the front.

    <?php
    
    $a = $condition1 && $condition2
        ? $foo : $bar;
    
    $b = $condition3 && $condition4
        ? $foo_man_this_is_too_long_what_should_i_do
        : $bar;
    ?>

# Function Calls
Functions should be called with no spaces between the function name, the opening parenthesis, and the first parameter; spaces between commas and each parameter, and no space between the last parameter, the closing parenthesis, and the semicolon. Here's an example:

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

**Split function call on several lines**

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

Using fluent application programming interfaces often leads to many concatenated function calls. Those calls may be split onto several lines. When doing this, all subsequent lines are indented by 4 spaces and begin with the "->" arrow.

    <?php
    
    $someObject->someFunction("some", "parameter")
        ->someOtherFunc(23, 42)
        ->andAThirdFunction();
    ?>

**Alignment of assignments**

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

**Split long assigments onto several lines**

Assigments may be split onto several lines when the character/line limit would be exceeded. The equal sign has to be positioned onto the following line, and indented by 4 characters.

    <?php
    
    $GLOBALS['TSFE']->additionalHeaderData[$this->strApplicationName]
        = $this->xajax->getJavascript(t3lib_extMgm::siteRelPath('nr_xajax'));
    ?>

# Class Definitions
Class declarations have their opening brace on a new line:

    <?php
    class Foo_Bar
    {
    
        //... code goes here
    
    }
    ?>

# Function Definitions
Function declarations follow the "K&R style":

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

Split function definitions onto several lines
Functions with many parameters may need to be split onto several lines to keep the 80 characters/line limit. The first parameters may be put onto the same line as the function name if there is enough space. Subsequent parameters on following lines are to be indented 4 spaces. The closing parenthesis and the opening brace are to be put onto the next line, on the same indentation level as the "function" keyword.

    <?php
    
    function someFunctionWithAVeryLongName($firstParameter = 'something', $secondParameter = 'booooo',
        $third = null, $fourthParameter = false, $fifthParameter = 123.12,
        $sixthParam = true
    ) {
        //....
    ?>

# Arrays
Assignments in arrays may be aligned. When splitting array definitions onto several lines, the last value may also have a trailing comma. This is valid PHP syntax and helps to keep code diffs minimal:

    <?php
    
    $some_array = array(
        'foo'  => 'bar',
        'spam' => 'ham',
    );
    ?>

# Comments
Complete inline documentation comment blocks (docblocks) must be provided. Please read the Sample File and Header Comment Blocks sections of the Coding Standards to learn the specifics of writing docblocks for PEAR packages. Further information can be found on the phpDocumentor website.

Non-documentation comments are strongly encouraged. A general rule of thumb is that if you look at a section of code and think "Wow, I don't want to try and describe that", you need to comment it before you forget how it works.

C style comments (/* */) and standard C++ comments (//) are both fine. Use of Perl/shell style comments (#) is discouraged.

# Including Code
Anywhere you are unconditionally including a class file, use require_once. Anywhere you are conditionally including a class file (for example, factory methods), use include_once. Either of these will ensure that class files are included only once. They share the same file list, so you don't need to worry about mixing them - a file included with require_once will not be included again by include_once.

include_once and require_once are statements, not functions. Parentheses should not surround the subject filename.

# PHP Code Tags
Always use <?php ?> to delimit PHP code, not the <? ?> shorthand. This is required for PEAR compliance and is also the most portable way to include PHP code on differing operating systems and setups.

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

### PHP Versions
One of the following must go in the page-level docblock:

* PHP version 4
 * PHP version 5
 * PHP versions 4 and 5
### @license
There are several possible licenses. One of the following must be picked and placed in the page-level and class-level docblocks:

* @license   http://www.apache.org/licenses/LICENSE-2.0  Apache License 2.0
 * @license   http://www.freebsd.org/copyright/freebsd-license.html  BSD License (2 Clause)
 * @license   http://www.debian.org/misc/bsd.license  BSD License (3 Clause)
 * @license   http://www.freebsd.org/copyright/license.html  BSD License (4 Clause)
 * @license   http://www.opensource.org/licenses/mit-license.html  MIT License
 * @license   http://www.gnu.org/copyleft/lesser.html  LGPL License 2.1
 * @license   http://www.php.net/license/3_01.txt  PHP License 3.01
For more information, see the PEAR Group's Licensing Announcement.

### @link
The following must be used in both the page-level and class-level docblocks. Of course, change "PackageName" to the name of your package. This ensures the generated documentation links back your package.

* @link      http://pear.php.net/package/PackageName
### @author
There's no hard rule to determine when a new code contributor should be added to the list of authors for a given source file. In general, their changes should fall into the "substantial" category (meaning somewhere around 10% to 20% of code changes). Exceptions could be made for rewriting functions or contributing new logic.

Simple code reorganization or bug fixes would not justify the addition of a new individual to the list of authors.

### @since
This tag is required when a file or class is added after the package's initial release. Do not use it in an initial release.

### @deprecated
This tag is required when a file or class is no longer used but has been left in place for backwards compatibility.

## Optional Tags
### @copyright
Feel free to apply whatever copyrights you desire. When formatting this tag, the year should be in four digit format and if a span of years is involved, use a hyphen between the earliest and latest year. The copyright holder can be you, a list of people, a company, the PHP Group, etc. Examples:

* @copyright 2003 John Doe and Jennifer Buck
 * @copyright 2001-2004 John Doe
 * @copyright 1997-2004 The PHP Group
 * @copyright 2001-2004 XYZ Corporation
License Summary
If you are using the PHP License, use the summary text provided above. If another license is being used, please remove the PHP License summary. Feel free to substitute it with text appropriate to your license, though to keep things easy to locate, please preface the text with LICENSE: .

### @see
Add a @see tag when you want to refer users to other sections of the package's documentation. If you have multiple items, separate them with commas rather than adding multiple @see tags.

**Order and Spacing**

To ease long term readability of PEAR source code, the text and tags must conform to the order and spacing provided in the example above. This standard is adopted from the JavaDoc standard.

**@package_version@ Usage**
There are two ways to implement the @package_version@ replacements. The procedure depends on whether you write your own package.xml files or if you use the PackageFileManager.

For those authoring package.xml files directly, add a <replace> element for each file. The XML for such would look something like this:

    <file name="Class.php">
      <replace from="@package_version@" to="version" type="package-info" />
    </file>

Maintainers using the PackageFileManager need to call addReplacement() for each file:

    <?php
    $pkg->addReplacement('filename.php', 'package-info',
                         '@package_version@', 'version');
    ?>

# Naming Conventions
## Global Variables and Functions
If your package needs to define global variables, their names should start with a single underscore followed by the package name and another underscore. For example, the PEAR package uses a global variable called $_PEAR_destructor_object_list.

Global functions should be named using the "studly caps" style (also referred to as "bumpy case" or "camel caps"). In addition, they should have the package name as a prefix, to avoid name collisions between packages. The initial letter of the name (after the prefix) is lowercase, and each letter that starts a new "word" is capitalized. An example:

XML_RPC_serializeData()
##Classes
Classes should be given descriptive names. Avoid using abbreviations where possible. Class names should always begin with an uppercase letter. The PEAR class hierarchy is also reflected in the class name, each level of the hierarchy separated with a single underscore. Examples of good class names are:

Log	Net_Finger	HTML_Upload_Error
##Class Variables and Methods
Class variables (a.k.a properties) and methods should be named using the "studly caps" style (also referred to as "bumpy case" or "camel caps"). Some examples (these would be "public" members):

$counter	connect()	getData()	buildSomeWidget()
Private class members are preceded by a single underscore. For example:

$_status	_sort()	_initTree()
The following applies to PHP5.
Protected class members are not preceded by a single underscore. For example:

protected $somevar	protected function initTree()
## Constants
Constants should always be all-uppercase, with underscores to separate words. Prefix constant names with the uppercased name of the class/package they are used in. Some examples:

DB_DATASOURCENAME	SERVICES_AMAZON_S3_LICENSEKEY
The true, false and null constants are excepted from the all-uppercase rule, and must always be lowercase.

# File Formats
All scripts contributed to PEAR must:

Be stored as ASCII text

Use ISO-8859-1 or UTF-8 character encoding. The encoding may be declared using declare(encoding = 'utf-8'); at the top of the file.

Be Unix formatted

"Unix formatted" means two things:

1) Lines must end only with a line feed (LF). Line feeds are represented as ordinal 10, octal 012 and hex 0A. Do not use carriage returns (CR) like Macintosh computers do or the carriage return/line feed combination (CRLF) like Windows computers do.

2) There should be one line feed after the closing PHP tag (?>). This means that when the cursor is at the very end of the file, it should be one line below the closing PHP tag.

# E_STRICT-compatible code
Starting on 01 January 2007, all new code that is suggested for inclusion into PEAR must be E_STRICT-compatible. This means that it must not produce any warnings or errors when PHP's error reporting level is set to E_ALL | E_STRICT.

# Error Handling Guidelines
This part of the Coding Standards describes how errors are handled in PEAR packages that are developed for PHP 5 and 6. It uses Exceptions, introduced in PHP 5.0 with Zend Engine 2, as the error handling mechanism.

## Definition of an error
An error is defined as an unexpected, invalid program state from which it is impossible to recover. For the sake of definition, recovery scope is defined as the method scope. Incomplete recovery is considered a recovery.

One pretty straightforward example for an error

    <?php
    /*
     * Connect to Specified Database
     *
     * @throws Example_Datasource_Exception when it can't connect
     * to specified DSN.
     */
    function connectDB($dsn)
    {
        $this->db =& DB::connect($dsn);
        if (DB::isError($this->db)) {
            throw new Example_Datasource_Exception(
                    "Unable to connect to $dsn:" . $this->db->getMessage()
            );
        }
    }
    ?>

In this example the objective of the method is to connect to the given DSN. Since it can't do anything but ask PEAR DB to do it, whenever DB returns an error, the only option is to bail out and launch the exception.

## Error handling with recovery

    <?php
    /*
     * Connect to one of the possible databases
     *
     * @throws Example_Datasource_Exception when it can't connect to
     * any of the configured databases.
     *
     * @throws Example_Config_Exception when it can't find databases
     * in the configuration.
     */
    
    function connect(Config $conf)
    {
        $dsns =& $conf->searchPath(array('config', 'db'));
        if ($dsns === FALSE) throw new Example_Config_Exception(
            'Unable to find config/db section in configuration.'
        );
    
        $dsns =& $dsns->toArray();
    
        foreach($dsns as $dsn) {
            try {
                $this->connectDB($dsn);
                return;
            } catch (Example_Datasource_Exception $e) {
                // Some warning/logging code recording the failure
                // to connect to one of the databases
            }
        }
        throw new Example_Datasource_Exception(
            'Unable to connect to any of the configured databases'
        );
    }
    ?>

This second example shows an exception being caught and recovered from. Although the lower level connectDB() method is unable to do anything but throw an error when one database connection fails, the upper level connect() method knows the object can go by with any one of the configured databases. Since the error was recovered from, the exception is silenced at this level and not rethrown.

## Incomplete recovery

    <?php
    /*
     * loadConfig parses the provided configuration. If the configuration
     * is invalid, it will set the configuration to the default config.
     *
     */
    function loadConfig(Config $conf)
    {
        try {
            $this->config = $conf->parse();
        } catch (Config_Parse_Exception $e) {
            // Warn/Log code goes here
            // Perform incomplete recovery
            $this->config = $this->defaultConfig;
        }
    }
    ?>

The recovery produces side effects, so it is considered incomplete. However, the program may proceed, so the exception is considered handled, and must not be rethrown. As in the previous example, when silencing the exception, logging or warning should occur.

## Error Signaling in PHP 5 PEAR packages
Error conditions in PEAR packages written for PHP 5 must be signaled using exceptions. Usage of return codes or return PEAR_Error objects is deprecated in favor of exceptions. Naturally, packages providing compatibility with PHP 4 do not fall under these coding guidelines, and may thus use the error handling mechanisms defined in the PHP 4 PEAR coding guidelines.

An exception should be thrown whenever an error condition is met, according to the definition provided in the previous section. The thrown exception should contain enough information to debug the error and quickly identify the error cause. Note that, during production runs, no exception should reach the end-user, so there is no need for concern about technical complexity in the exception error messages.

The basic PEAR_Exception contains a textual error, describing the program state that led to the throw and, optionally, a wrapped lower level exception, containing more info on the lower level causes of the error.

The kind of information to be included in the exception is dependent on the error condition. From the point of view of exception throwing, there are three classes of error conditions:

## Errors detected during precondition checks
Lower level library errors signaled via error return codes or error return objects
Uncorrectable lower library exceptions
Errors detected during precondition checks should contain a description of the failed check. If possible, the description should contain the violating value. Naturally, no wrapped exception can be included, as there isn't a lower level cause of the error. Example:

    <?php
    function divide($x, $y)
    {
        if ($y == 0) {
            throw new Example_Aritmetic_Exception('Division by zero');
        }
    }
    ?>

Errors signaled via return codes by lower level libraries, if unrecoverable, should be turned into exceptions. The error description should try to convey all information contained in the original error. One example, is the connect method previously presented:

    <?php
    /*
     * Connect to Specified Database
     *
     * @throws Example_Datasource_Exception when it can't connect to specified DSN.
     */
    function connectDB($dsn)
    {
        $this->db =& DB::connect($dsn);
        if (DB::isError($this->db)) {
            throw new Example_Datasource_Exception(
                    "Unable to connect to $dsn:" . $this->db->getMessage()
            );
        }
    }
    ?>

Lower library exceptions, if they can't be corrected, should either be rethrown or bubbled up. When rethrowing, the original exception must be wrapped inside the one being thrown. When letting the exception bubble up, the exception just isn't handled and will continue up the call stack in search of a handler.

## Rethrowing an exception

    <?php
    function preTaxPrice($retailPrice, $taxRate)
    {
        try {
            return $this->divide($retailPrice, 1 + $taxRate);
        } catch (Example_Aritmetic_Exception $e) {
            throw new Example_Tax_Exception('Invalid tax rate.', $e);
        }
    }
    ?>

## Letting exceptions bubble up

    <?php
    function preTaxPrice($retailPrice, $taxRate)
    {
        return $this->divide($retailPrice, 1 + $taxRate);
    }
    ?>

The case between rethrowing or bubbling up is one of software architecture: Exceptions should be bubbled up, except in these two cases:

The original exception is from another package. Letting it bubble up would cause implementation details to be exposed, violating layer abstraction, conducing to poor design.
The current method can add useful debugging information to the received error before rethrowing.
Exceptions and normal program flow
Exceptions should never be used as normal program flow. If removing all exception handling logic (try-catch statements) from the program, the remaining code should represent the "One True Path" -- the flow that would be executed in the absence of errors.

This requirement is equivalent to requiring that exceptions be thrown only on error conditions, and never in normal program states.

One example of a method that wrongly uses the bubble up capability of exceptions to return a result from a deep recursion:

    <?php
    /**
     * Recursively search a tree for string.
     * @throws ResultException
     */
    public function search(TreeNode $node, $data)
    {
        if ($node->data === $data) {
            throw new ResultException( $node );
        } else {
            search( $node->leftChild, $data );
            search( $node->rightChild, $data );
        }
    }
    ?>

In the example the ResultException is simply using the "eject!" qualities of exception handling to jump out of deeply nested recursion. When actually used to signify an error this is a very powerful feature, but in the example above this is simply lazy development.

## Exception class hierarchies
All of PEAR packages exceptions must be descendant from PEAR_Exception. PEAR_Exception provides exception wrapping abilities, absent from the top level PHP Exception class, and needed to comply with the previous section requirements.

Additionally, each PEAR package must provide a top level exception, named <Package_Name>_Exception. It is considered best practice that the package never throws exceptions that aren't descendant from its top level exception.

## Documenting Exceptions
Because PHP, unlike Java, does not require you to explicitly state which exceptions a method throws in the method signature, it is critical that exceptions be thoroughly documented in your method headers.

Exceptions should be documented using the @throws phpdoc keyword

    <?php
    /**
     * This method searches for aliens.
     *
     * @return array Array of Aliens objects.
     * @throws AntennaBrokenException If the impedence readings indicate
     * that the antenna is broken.
     *
     * @throws AntennaInUseException If another process is using the
     * antenna already.
     */
    public function findAliens($color = 'green');
    ?>

In many cases middle layers of an application will rewrap any lower-level exceptions into more meaningful application exceptions. This also needs to be made clear:

    <?php
    /**
     * Load session objects into shared memory.
     *
     * @throws LoadingException Any lower-level IOException will be wrapped
     * and re-thrown as a LoadingException.
     */
    public function loadSessionObjects();
    ?>

In other cases your method may simply be a conduit through which lower level exceptions can pass freely. As challenging as it may be, your method should also document which exceptions it is not catching.

    <?php
    /**
     * Performs a batch of database queries (atomically, not in transaction).
     * @throws SQLException Low-level SQL errors will bubble up through this method.
     */
    public function batchExecute();
    ?>

## Exceptions as part of the API
Exceptions play a critical role in the API of your library. Developers using your library depend on accurate descriptions of where and why exceptions might be thrown from your package. Documentation is critical. Also maintaining the types of messages that are thrown is also an important requirement for maintaining backwards-compatibility.

Because Exceptions are critical to the API of your package, you must ensure that you don't break backwards compatibility (BC) by making changes to exceptions.

Things that break BC include:

Any change to which methods throw exceptions.
A change whereby a method throws an exception higher in the inheritance tree. For example, if you changed your method to throw a PEAR_Exception rather than a PEAR_IOException, you would be breaking backwards compatibility.
Things that do not break BC:

Throwing a subclass of the original exception. For example, changing a method to throw PEAR_IOException when before it had been throwing PEAR_Exception would not break BC (provided that PEAR_IOException extends PEAR_Exception).

# Best practices
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

# Sample File (including Docblock Comment standards)
The source code of PEAR packages are read by thousands of people. Also, it is likely other people will become developers on your package at some point in the future. Therefore, it is important to make life easier for everyone by formatting the code and docblocks in standardized ways. People can then quickly find the information they are looking for because it is in the expected location. Your cooperation is appreciated.

Each docblock in the example contains many details about writing Docblock Comments. Following those instructions is important for two reasons. First, when docblocks are easy to read, users and developers can quickly ascertain what your code does. Second, the PEAR website now contains the phpDocumentor generated documentation for each release of each package, so keeping things straight here means the API docs on the website will be useful.

Please take note of the vertical and horizontal spacing. They are part of the standard.

The "fold markers" (// {{{ and // }}}) are optional. If you aren't using fold markers, remove foldmethod=marker from the vim header.

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
    
    /**
     * This is a "Docblock Comment," also known as a "docblock."  The class'
     * docblock, below, contains a complete description of how to write these.
     */
    require_once 'PEAR.php';
    
    // {{{ constants
    
    /**
     * Methods return this if they succeed
     */
    define('NET_SAMPLE_OK', 1);
    
    // }}}
    // {{{ GLOBALS
    
    /**
     * The number of objects created
     * @global int $GLOBALS['_NET_SAMPLE_Count']
     */
    $GLOBALS['_NET_SAMPLE_Count'] = 0;
    
    // }}}
    // {{{ Net_Sample
    
    /**
     * An example of how to write code to PEAR's standards
     *
     * Docblock comments start with "/**" at the top.  Notice how the "/"
     * lines up with the normal indenting and the asterisks on subsequent rows
     * are in line with the first asterisk.  The last line of comment text
     * should be immediately followed on the next line by the closing asterisk
     * and slash and then the item you are commenting on should be on the next
     * line below that.  Don't add extra lines.  Please put a blank line
     * between paragraphs as well as between the end of the description and
     * the start of the @tags.  Wrap comments before 80 columns in order to
     * ease readability for a wide variety of users.
     *
     * Docblocks can only be used for programming constructs which allow them
     * (classes, properties, methods, defines, includes, globals).  See the
     * phpDocumentor documentation for more information.
     * http://phpdoc.org/docs/HTMLSmartyConverter/default/phpDocumentor/tutorial_phpDocumentor.howto.pkg.html
     *
     * The Javadoc Style Guide is an excellent resource for figuring out
     * how to say what needs to be said in docblock comments.  Much of what is
     * written here is a summary of what is found there, though there are some
     * cases where what's said here overrides what is said there.
     * http://java.sun.com/j2se/javadoc/writingdoccomments/index.html#styleguide
     *
     * The first line of any docblock is the summary.  Make them one short
     * sentence, without a period at the end.  Summaries for classes, properties
     * and constants should omit the subject and simply state the object,
     * because they are describing things rather than actions or behaviors.
     *
     * Below are the tags commonly used for classes. @category through @version
     * are required.  The remainder should only be used when necessary.
     * Please use them in the order they appear here.  phpDocumentor has
     * several other tags available, feel free to use them.
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
    class Net_Sample
    {
        // {{{ properties
    
        /**
         * The status of foo's universe
         *
         * Potential values are 'good', 'fair', 'poor' and 'unknown'.
         *
         * @var string
         */
        var $foo = 'unknown';
    
        /**
         * The status of life
         *
         * Note that names of private properties or methods must be
         * preceeded by an underscore.
         *
         * @var bool
         * @access private
         */
        var $_good = true;
    
        // }}}
        // {{{ setFoo()
    
        /**
         * Registers the status of foo's universe
         *
         * Summaries for methods should use 3rd person declarative rather
         * than 2nd person imperative, beginning with a verb phrase.
         *
         * Summaries should add description beyond the method's name. The
         * best method names are "self-documenting", meaning they tell you
         * basically what the method does.  If the summary merely repeats
         * the method name in sentence form, it is not providing more
         * information.
         *
         * Summary Examples:
         *   + Sets the label              (preferred)
         *   + Set the label               (avoid)
         *   + This method sets the label  (avoid)
         *
         * Below are the tags commonly used for methods.  A @param tag is
         * required for each parameter the method has.  The @return
         * and @access tags are mandatory.  The @throws tag is required if
         * the method uses exceptions.  @static is required if the method can
         * be called statically.  The remainder should only be used when
         * necessary.  Please use them in the order they appear here.
         * phpDocumentor has several other tags available, feel free to use
         * them.
         *
         * The @param tag contains the data type, then the parameter's
         * name, followed by a description.  By convention, the first noun in
         * the description is the data type of the parameter.  Articles like
         * "a", "an", and  "the" can precede the noun.  The descriptions
         * should start with a phrase.  If further description is necessary,
         * follow with sentences.  Having two spaces between the name and the
         * description aids readability.
         *
         * When writing a phrase, do not capitalize and do not end with a
         * period:
         *   + the string to be tested
         *
         * When writing a phrase followed by a sentence, do not capitalize the
         * phrase, but end it with a period to distinguish it from the start
         * of the next sentence:
         *   + the string to be tested. Must use UTF-8 encoding.
         *
         * Return tags should contain the data type then a description of
         * the data returned.  The data type can be any of PHP's data types
         * (int, float, bool, string, array, object, resource, mixed)
         * and should contain the type primarily returned.  For example, if
         * a method returns an object when things work correctly but false
         * when an error happens, say 'object' rather than 'mixed.'  Use
         * 'void' if nothing is returned.
         *
         * Here's an example of how to format examples:
         * <code>
         * require_once 'Net/Sample.php';
         *
         * $s = new Net_Sample();
         * if (PEAR::isError($s)) {
         *     echo $s->getMessage() . "\n";
         * }
         * </code>
         *
         * Here is an example for non-php example or sample:
         * <samp>
         * pear install net_sample
         * </samp>
         *
         * @param string $arg1 the string to quote
         * @param int    $arg2 an integer of how many problems happened.
         *                     Indent to the description's starting point
         *                     for long ones.
         *
         * @return int the integer of the set mode used. FALSE if foo
         *             foo could not be set.
         * @throws exceptionclass [description]
         *
         * @access public
         * @static
         * @see Net_Sample::$foo, Net_Other::someMethod()
         * @since Method available since Release 1.2.0
         * @deprecated Method deprecated in Release 2.0.0
         */
        function setFoo($arg1, $arg2 = 0)
        {
            /*
             * This is a "Block Comment."  The format is the same as
             * Docblock Comments except there is only one asterisk at the
             * top.  phpDocumentor doesn't parse these.
             */
            if ($arg1 == 'good' || $arg1 == 'fair') {
                $this->foo = $arg1;
                return 1;
            } elseif ($arg1 == 'poor' && $arg2 > 1) {
                $this->foo = 'poor';
                return 2;
            } else {
                return false;
            }
        }
    
        // }}}
    }
    
    // }}}
    
    /*
     * Local variables:
     * tab-width: 4
     * c-basic-offset: 4
     * c-hanging-comment-ender-p: nil
     * End:
     */
    
    ?>

# Rules
We will describe the list of rules that form the standards

## Namespace prefix
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

## Requirement
No Exceptions to this rule
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

## Requirement
No Exceptions to this rule
Directory structure
Follows the directory structure in the PEAR2 Subversion repository:

PEAR2/Package_Name/
    src/      <-- all role="php"
    data/     <-- all role="data"
    tests/    <-- all role="tests"
    docs/     <-- all role="doc"
    www/      <-- all role="www"
    examples/ <-- role="doc" example files 
                  (php executable files that exemplify package usage)
Note that all package.xml files must specify a baseinstalldir of "/" for the src/ directory:

    <contents>
      <dir name="/">
      <dir name="src" baseinstalldir="/">
      ...
    </contents>

## Requirement
Exceptions may be made to this rule with approval from the PEAR Group
Class-to-file convention
All public classes must be in their own file with underscores (_) or namespace separators (\) replaced by directory separator, so that PEAR2_PackageName_Base class or PEAR2\PackageName\Base class is always located in PEAR2/PackageName/Base.php (this is required to make autoload work)

## Requirement
Exceptions may be made to this rule only with explicit approval from the PEAR Group via a public vote
Base Exception class
PEAR2\Exception is used as base class for all exception classes. Each package must define a base class that is packagename_Exception. For example, the PEAR2\PackageName class defines an exception as follows in PEAR2/PackageName/Exception.php:

    <?php
    namespace PEAR2\PackageName;
    class Exception extends PEAR2\Exception {}
    ?>

'PEAR2\Exception will be its own package'

## Requirement
No Exceptions to this rule
Data files
package.xml replacement tasks should not be used to retrieve path locations for php, data, or www files. Replacements are still allowed in doc and test files.

The installation structure of a package has implicit php_dir/src installation location, and data files are always located in php_dir/data/channel/PackageName/. To retrieve a data file within PEAR2/PackageName/Subfile.php, you could use code like this example

    <?php
    ...
    // retrieve data from info.txt
    $info = file_get_contents(dirname(__FILE__) .
        '../../../data/pear2.php.net/PEAR2_PackageName/info.txt');
    ?>

## Requirement
No Exceptions to this rule
Loading other classes
Inside optional component loading methods (like factory or driver loading, etc.) class_exists($classname, true) should be used where a "class not found" fatal error would be confusing. For example, when loading a driver, a graceful exit via exception with helpful error message is preferrable to the fatal error:

    <?php
    if (!class_exists("PEAR2_PackageName_Driver_$class", true)) {
        throw new PEAR2\PackageName\Exception('Unknown driver ' .
        $class . ', be sure the driver exists and is loaded
        prior to use');
    }
    ?>

## Requirement
This rule is optional and is a suggested coding practice



# Coding standards

Note: The Drupal Coding Standards apply to code within Drupal and its contributed modules. This document is loosely based on the PEAR Coding standards. Comments and names should use US English spelling (e.g., "color" not "colour").

Drupal coding standards are version-independent and "always-current". All new code should follow the current standards, regardless of (core) version. Existing code in older versions may be updated, but doesn't necessarily have to be. Especially for larger code-bases (like Drupal core), updating the code of a previous version for the current standards may be too huge of a task. However, code in current versions should follow the current standards.

Note: Do not squeeze coding standards updates/clean-ups into otherwise unrelated patches. Only touch code lines that are actually relevant. To update existing code for the current standards, always create separate and dedicated issues and patches.

See the Helper modules section at the bottom of this page for information on modules that can review code for coding standards problems (and in some cases, even fix the problems).


## Indenting and Whitespace
Use an indent of 2 spaces, with no tabs.

Lines should have no trailing whitespace at the end.

Files should be formatted with \n as the line ending (Unix line endings), not \r\n (Windows line endings).

All text files should end in a single newline (\n). This avoids the verbose "\ No newline at end of file" patch warning and makes patches easier to read since it's clearer what is being changed when lines are added to the end of a file.

## Operators
All binary operators (operators that come between two values), such as +, -, =, !=, ==, >, etc. should have a space before and after the operator, for readability. For example, an assignment should be formatted as $foo = $bar; rather than $foo=$bar;. Unary operators (operators that operate on only one value), such as ++, should not have a space between the operator and the variable or number they are operating on.

## Casting
Put a space between the (type) and the $variable in a cast: (int) $mynumber.

## Control Structures
Control structures include if, for, while, switch, etc. Here is a sample if statement, since it is the most complicated of them:

    if (condition1 || condition2) {
      action1;
    }
    elseif (condition3 && condition4) {
      action2;
    }
    else {
      defaultaction;
    }

(Note: Don't use "else if" -- always use elseif.)
Control statements should have one space between the control keyword and opening parenthesis, to distinguish them from function calls.

Always use curly braces even in situations where they are technically optional. Having them increases readability and decreases the likelihood of logic errors being introduced when new lines are added.

For switch statements:

    switch (condition) {
      case 1:
        action1;
        break;
    
      case 2:
        action2;
        break;
    
      default:
        defaultaction;
    }

For do-while statements:

    do {
      actions;
    } while ($condition);
  
## Alternate control statement syntax for templates

In templates, the alternate control statement syntax using : instead of brackets is allowed. Note that there should not be a space between the closing paren after the control keyword, and the colon, and HTML/PHP inside the control structure should be indented. For example:
    <?php if (!empty($item)): ?>
      <p><?php print $item; ?></p>
    <?php endif; ?>
    
    <?php foreach ($items as $item): ?>
      <p><?php print $item; ?></p>
    <?php endforeach; ?>

## Line length and wrapping
The following rules apply to code. See Doxygen and comment formatting conventions for rules pertaining to comments.

In general, all lines of code should not be longer than 80 chars.
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

## Function Calls
Functions should be called with no spaces between the function name, the opening parenthesis, and the first parameter; spaces between commas and each parameter, and no space between the last parameter, the closing parenthesis, and the semicolon. Here's an example:

    $var = foo($bar, $baz, $quux);

As displayed above, there should be one space on either side of an equals sign used to assign the return value of a function to a variable. In the case of a block of related assignments, more space may be inserted to promote readability:

    $short         = foo($bar);
    $long_variable = foo($baz);

## Function Declarations

    function funstuff_system($field) {
      $system["description"] = t("This module inserts funny text into posts randomly.");
      return $system[$field];
    }

Arguments with default values go at the end of the argument list. Always attempt to return a meaningful value from a function if one is appropriate.


## Class Constructor Calls
When calling class constructors with no arguments, always include parentheses:

    $foo = new MyClassName();

This is to maintain consistency with constructors that have arguments:

    $foo = new MyClassName($arg1, $arg2);

Note that if the class name is a variable, the variable will be evaluated first to get the class name, and then the constructor will be called. Use the same syntax:

    $bar = 'MyClassName';
    $foo = new $bar();
    $foo = new $bar($arg1, $arg2);

## Arrays
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


## Quotes
Drupal does not have a hard standard for the use of single quotes vs. double quotes. Where possible, keep consistency within each module, and respect the personal style of other developers.

With that caveat in mind: single quote strings are known to be faster because the parser doesn't have to look for in-line variables. Their use is recommended except in two cases:

In-line variable usage, e.g. "<h2>$header</h2>".
Translated strings where one can avoid escaping single quotes by enclosing the string in double quotes. One such string would be "He's a good person." It would be 'He\'s a good person.' with single quotes. Such escaping may not be handled properly by .pot file generators for text translation, and it's also somewhat awkward to read.

## String Concatenations
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

## Comments
Comment standards are discussed on the separate Doxygen and comment formatting conventions page.


## Including Code
Anywhere you are unconditionally including a class file, use require_once(). Anywhere you are conditionally including a class file (for example, factory methods), use include_once(). Either of these will ensure that class files are included only once. They share the same file list, so you don't need to worry about mixing them - a file included with require_once() will not be included again by include_once().

Note: include_once() and require_once() are statements, not functions. You don't need parentheses around the file name to be included.

When including code from the same directory or a sub-directory, start the file path with ".":

    include_once ./includes/mymodule_formatting.inc

In Drupal 7.x and later versions, use DRUPAL_ROOT:
require_once DRUPAL_ROOT . '/' . variable_get('cache_inc', 'includes/cache.inc');


## PHP Code Tags
Always use <?php ?> to delimit PHP code, not the shorthand, <? ?>. This is required for Drupal compliance and is also the most portable way to include PHP code on differing operating systems and set-ups.

Note that as of Drupal 4.7, the ?> at the end of code files is purposely omitted. This includes for module and include files. The reasons for this can be summarized as:

Removing it eliminates the possibility for unwanted whitespace at the end of files which can cause "header already sent" errors, XHTML/XML validation issues, and other problems.
The closing delimiter at the end of a file is optional.
PHP.net itself removes the closing delimiter from the end of its files (example: prepend.inc), so this can be seen as a "best practice."

## Semicolons
The PHP language requires semicolons at the end of most lines, but allows them to be omitted at the end of code blocks. Drupal coding standards require them, even at the end of code blocks. In particular, for one-line PHP blocks:

    <?php print $tax; ?> -- YES
    <?php print $tax ?> -- NO

## Naming Conventions
### Functions and variables

Functions and variables should be named using lowercase, and words should be separated with an underscore. Functions should in addition have the grouping/module name as a prefix, to avoid name collisions between modules.

### Persistent Variables

Persistent variables (variables/settings defined using Drupal's variable_get()/variable_set() functions) should be named using all lowercase letters, and words should be separated with an underscore. They should use the grouping/module name as a prefix, to avoid name collisions between modules.

### Constants

Constants should always be all-uppercase, with underscores to separate words. (This includes pre-defined PHP constants like TRUE, FALSE, and NULL.)
Module-defined constant names should also be prefixed by an uppercase spelling of the module that defines them.
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

### Global Variables

If you need to define global variables, their name should start with a single underscore followed by the module/theme name and another underscore.

### Classes

All standards related to classes and interfaces, including naming, are covered on http://drupal.org/node/608152 instead of here.

### File names

All documentation files should have the file name extension ".txt" to make viewing them on Windows systems easier. Also, the file names for such files should be all-caps (e.g. README.txt instead of readme.txt) while the extension itself is all-lowercase (i.e. txt instead of TXT).

Examples: README.txt, INSTALL.txt, TODO.txt, CHANGELOG.txt etc.






# Doxygen and comment formatting conventions

This page covers the standards for documentation in Drupal code. There are two types of comments: in-line and headers. In-line comments are comments that are within functions. In Drupal, documentation headers on functions, classes, constants, etc. are specially-formatted comments used to build the documentation on api.drupal.org. The system for extracting documentation from the header comments uses the Doxygen generation system, and since the documentation is extracted directly from the sources, it is much easier to keep the documentation consistent with the source code.

There is an excellent Doxygen manual at the Doxygen site. This page describes the Drupal implementation of Doxygen, and our standards for both in-line and header comments.

## General documentation standards
These standards apply to both in-line and header comments:

All documentation and comments should form proper sentences and use proper grammar and punctuation.
Sentences should be separated by single spaces.
Comments and variable names should be in English, and use US English spelling (e.g., "color" not "colour").
All caps are used in comments only when referencing constants, for example TRUE
Comments should be word-wrapped if the line length would exceed 80 characters (i.e., go past the 80th column, including any leading spaces and comment characters in the 80 character count). They should be as long as possible within the 80-character limit. There are a few exceptions to this, which are noted in sections below. (@code, @link, and annotations are the main ones).
In-line comment standards
Non-header or in-line comments are strongly encouraged. A general rule of thumb is that if you look at a section of code and think "Wow, I don't want to try and describe that", you need to comment it before you forget how it works. Comments should be on a separate line immediately before the code line or block they reference. For example:

// Unselect all other contact categories.
db_query('UPDATE {contact} SET selected = 0');
If each line of a list needs a separate comment, the comments may be given on the same line and may be formatted to a uniform indent for readability.

C style comments (/* */) and standard C++ comments (//) are both fine, though the former is discouraged within functions (even for multiple lines, repeat the // single-line comment). Use of Perl/shell style comments (#) is discouraged.

## General header documentation syntax
To document a block of code, such as a file, function, class, method, constant, etc., the syntax we use is:
/**
 * Documentation here.
 */
Doxygen will parse any comments located in such a block. Our style is to use as few Doxygen-specific commands as possible, so as to keep the source legible. Any mentions of functions, classes, file names, topics, etc. within the documentation will automatically link to the referenced code, so typically no markup need be introduced to produce links. The documentation block should appear immediately before the function, class, method, etc. that is being documented, with no blank lines in between.

## Doxygen directives - general notes

/**
 * Summary here; one sentence on one line (should not, but can exceed 80 chars).
 *
 * A more detailed description goes here.
 *
 * A blank line forms a paragraph. There should be no trailing white-space
 * anywhere.
 *
 * @param string $first
 *   "@param" is a Doxygen directive to describe a function parameter. Like some
 *   other directives, it takes a term/summary on the same line and a
 *   description (this text) indented by 2 spaces on the next line. All
 *   descriptive text should wrap at 80 chars, without going over.
 *   Newlines are NOT supported within directives; if a newline would be before
 *   this text, it would be appended to the general description above.
 * @param int $second
 *   There should be no newline between multiple directives (of the same type).
 * @param bool $third
 *   (optional) TRUE if Third should be done. Defaults to FALSE.
 *   Only optional parameters are explicitly stated as such. The description
 *   should clarify the default value if omitted.
 *
 * @return array
 *   "@return" is a different Doxygen directive to describe the return value of
 *   a function, if there is any.
 */
function mymodule_foo($first, $second, $third = FALSE) {
}
Lists

 * @param array $variables
 *   An associative array containing:
 *   - tags: An array of labels for the controls in the pager:
 *     - first: A string to use for the first pager element.
 *     - last: A string to use for the last pager element.
 *   - element: (optional) Integer to distinguish between multiple pagers on one
 *     page. Defaults to 0 (zero).
 *   - style: Integer for the style, one of the following constants:
 *     - PAGER_FULL: (default) Full pager.
 *     - PAGER_MINI: Mini pager.
 *   Any further description - still belonging to the same param, but not part
 *   of the list.
 *
 * This no longer belongs to the param.
Lists can appear anywhere in Doxygen. The documentation parser requires you to follow a strict syntax to make them appear correctly in the parsed HTML output:

A hyphen is used to indicate the list bullet. The hyphen is aligned with (uses the same indentation level as) the paragraph before it, with no newline before or after the list.
No newlines between list items in the same list.
Each list item starts with the key, followed by a colon, followed by a space, followed by the key description. The key description starts with a capital letter and ends with a period.
If the list has no keys, start each list item with a capital letter and end with a period.
The keys should not be put in quotes unless they contain colons (which is unlikely).
If a list element is optional or default, indicate (optional) or (default) after the colon and before the key description.
If a list item exceeds 80 chars, it needs to wrap, and the following lines need to be aligned with the key (indented by 2 more spaces).
For text after the list that needs to be in the same block, use the same alignment/indentation as the initial text.
Again: within a Doxygen directive, or within a list, blank lines are NOT supported.
Lists can appear within lists, and the same rules apply recursively.
See Also sections

/**
 * (rest of function/file/etc. header)
 *
 * @see foo_bar()
 * @see ajax.inc
 * @see \My\Namespace\MyModuleClass
 * @see \My\Namespace\MyClass::myMethod()
 * @see groupname
 * @see http://drupal.org/node/1354
 */
The @see directive may be used to link to (existing) functions, files, classes, methods, constants, groups/topics, URLs, etc. @see directives should always be placed on their own line, and generally at the bottom of the documentation header. Use the same format in @see that you would for automatic links (see below, or see examples above). In addition, you can link to a group/topic using the internal name of the group (what follows the @defgroup in its definition).

Links

/**
 * (rest of function/file/etc. header)
 *
 * See also @link group_name Topic name goes here, @endlink and the
 * @link http://example.com web page explaining it further. @endlink
 */
The @link...@endlink directive may be used to output a HTML link in line with the text. Some notes:

Everything between @link and @endlink needs to be on one line. The line is allowed to exceed 80 characters, but if it does, it should be by itself on the line (break surrounding text before/after the link line).
The first word (i.e., string of characters without spaces) after @link is either a full URL, or the name of a function, group/topic defined elsewhere with @defgroup, or file.
The remaining words are used as the link text. Don't use @link for full URLs or file names if the link text is the URL or file name -- it's not necessary, since URLs and file names turn into links automatically anyway.
Put following punctuation into the link text, rather than after the @endlink.
Automatic links

 * This function invokes hook_foo() on all implementing modules.
 * It calls MyClass::methodName(), which includes foo.module as
 * a side effect.
Any function, file, constant, class, etc. in the documentation will automatically be linked to the page where that item is documented (assuming that item has a doxygen header). For functions and methods, you must include () after the function/method name to get the link. For files, just put the file name in (not the path to the file). Groups will not be automatically linked; use @see or @link.

Code samples

 * Example usage:
 * @code
 *   mymodule_print('Hello World!');
 * @endcode
 * Text to immediately follow the code block.
Code examples can be embedded in the Doxygen documentation using @code and @endcode directives. Any code in between will be formatted as code when output by the API module (on api.drupal.org for instance). Notes:

Indent code by two spaces relative to the @code tag.
It is permissible to exceed 80 characters in lines of code, but don't make really long lines, since they don't display well on web sites.
Todos

/**
 * ...
 * Previous line of documentation (leave a blank line before @todo).
 *
 * @todo Remove this in D8.
 * @todo Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam
 *   nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed
 *   diam voluptua.
 */
To document known issues and development tasks in code, @todo statements may be used. Notes:

Each @todo should form an atomic task.
Leave a blank line before the first @todo. Don't leave blank lines between multiple @todos.
Put @todo statements at the bottom of the doc block, unless they pertain to something specific (like a specific @param, in which case they should be placed in that @param section).
Wrap at 80 characters, indenting following lines by 2 spaces.
  // Some other comment here.
  // @todo Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam
  //   nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat,
  //   sed diam voluptua.
  // We explicitly delete all comments.
  comment_delete($cid);
@todo statements in inline comments follow the same rules as above, especially regarding indentation of subsequent comments, but you don't need to leave a blank line between other comments and the @todo line.

Documenting files
Each file should start with a comment describing what the file does. For example:

/**
 * @file
 * The theme system, which controls the output of Drupal.
 *
 * The theme system allows for nearly all output of the Drupal system to be
 * customized by user themes.
 */

Note that a blank line should follow a @file documentation block.
The line immediately following the @file directive is a summary that will be shown in the list of all files in the generated documentation. (Like other summaries, it should be one sentence in one line of up to 80 characters.) If the line begins with a verb, that verb should be in present tense, e.g., "Handles file uploads." Further description may follow after a blank PHPDoc line.

In general, @file directives should not contain large descriptions, because those are better placed into @defgroup directives, so developers can look up high-level documentation by reading the "group" topic. See http://api.drupal.org/api/drupal/groups for a list of API topics/groups in Drupal Core.

For .install files, the following template is used:

    /**
     * @file
     * Install, update, and uninstall functions for the XXX module.
     */

For files containing the definition of exactly one class/interface, use this template:

    /**
     * @file
     * Contains NameOfTheClass.
     */

## Documenting functions and methods
All functions and methods, whether meant to be private or public, should be documented. A function documentation block should immediately precede the declaration of the function itself. For example:

/**
 * Verifies the syntax of the given e-mail address.
 *
 * Empty e-mail addresses are allowed. See RFC 2822 for details.
 *
 * @param string $mail
 *   A string containing an email address.
 *
 * @return bool
 *   TRUE if the address is in a valid format, and FALSE if it isn't.
 */
function valid_email_address($mail) {
Note that some types of functions have special forms of documentation that do not follow this exact format -- see sections below for details (e.g., hook declarations, form generating functions, etc.).

Notes on function documentation syntax

First line: description
The first line of the block should contain a brief description of what the function does, limited to 80 characters, and beginning with a verb in the form "Does such and such" (third person, as in "This function does such and such", rather than second person imperative "Do such and such").

Additional description
A longer description with usage notes should follow after a blank line, if more explanation is needed.

Parameters
After the long description, each parameter should be listed with a @param directive, with a description indented by two extra spaces on the following line or lines. If there are no parameters, omit the @param section entirely. Do not include any blank lines in the @param section.

Variable arguments
If a function uses variable arguments, use this syntax:
* @param ...
*    Description of the variable arguments goes here.
Return value
After all the parameters, a @return directive should be used to document the return value if there is one. There should be a blank line between the @param section and @return directive.

If there is no return value, omit the @return directive entirely.

Short form
Functions that are easily described in one line may use the function summary only, for example:

/**
 * Converts an associative array to an anonymous object.
 */
function mymodule_array2object($array) {
If the abbreviated syntax is used, the parameters and return value must be described within the one-line summary.

Data types on param/return
Note: This section of the standard was adopted as of Drupal 8. In previous versions, data type specification was recommended in the case of non-obvious types or specific classes/interfaces only.

The data type of a parameter or return value MUST be specified in @param or @return directives, if it is not obvious or of a specific class or interface type.

The data type MAY be specified (as in: recommended) for other @param or @return directives, including those with primitive data types, arrays, and generic objects (stdClass).

 * @param int $number
 *   Description of this parameter, which takes an integer.
 * @param float $number
 *   Description of this parameter, which takes a floating-point number.
 * @param string $description
 *   Description of this parameter, which takes a string.
 * @param bool $flagged
 *   Description of this parameter, which takes a Boolean TRUE/FALSE value.
 * @param array $args
 *   Description of this parameter, which takes a PHP array value.
 * @param object $account
 *   Description of this parameter, which takes a PHP stdClass value.
 * @param \Drupal\Core\Database\StatementInterface $query
 *   Description of this parameter, which takes an object of a class that
 *   implements the StatementInterface interface.
 * @param string|bool $string_or_boolean
 *   Description of this parameter, which takes a string or a Boolean TRUE/FALSE
 *   value.
 * @param string|null $string_or_null
 *   (optional) Description of this optional parameter, which takes a string or
 *   can be NULL.
 *
 * @return string
 *   Description of the return value, which is a string.
Notes:

For primitive and non-specific types, use the lower-case type name; int, string, bool, array, object, etc.
For special constants, use the lower-case value name: null, false, true. Use bool instead of true|false though. Only specify true or false if the opposite is not supported (e.g., @return string|false).
For optional parameters, the default value should only be mentioned on the @param line if its type differs from non-default values. For instance, use @param string|null or @param int|false but never @param string|'' or @param int|0.
For classes and interfaces, use the most general class/interface possible, and prefix with the namespace. For example, the return value of db_select() is properly documented as SelectQueryInterface rather than a particular class implementation such as SelectQuery.
Return values are documented by @return and use the same conventions as @param, but with no variable name.
If a function may return NULL in some but not all cases, then document this possibility. For instance, if the return value may either be a string value or NULL, then use @return string|null followed by a description of the circumstances in which each may be returned.
The @return section should be omitted if the function lacks an explicit return value.
In general, if there are more than two choices (e.g., string|bool|null) for a parameter or return data type, the function should probably be refactored for simplicity.
Callback functions
For callbacks, add a blank line after the summary, followed by a line that identifies the callback's purpose. Use the following format:

/**
 * [standard first line]
 *
 * Callback for uasort() within system_themes_page().
This line should indicate which PHP or Drupal function directly takes this callback as an argument, and also which Drupal API function calls that function. If multiple API functions call it, format as a list.

Note that there are also specific standards for some functions that might be considered callbacks, such as hook implementations, menu callbacks, and form callbacks.

Documenting hook definitions
/**
 * Respond to node deletion.
 *
 * This hook is invoked from node_delete_multiple() after the node has been
 * removed from the node table in the database, after the type-specific
 * hook_delete() has been invoked, and before field_attach_delete() is called.
 *
 * @param object $node
 *   The node that is being deleted.
 *
 * @ingroup node_api_hooks
 * @ingroup hooks
 */
function hook_node_delete($node) {
  db_delete('mytable')
    ->condition('nid', $node->nid)
    ->execute();
}
The documentation for a hook definition follows the same guidelines as for regular functions (see above), except that the first line is a verb in the imperative form, e.g. "Respond to xyz". Also, a sample function body is provided, usually from a core module's implementation of the hook. It is also helpful to mention somewhere in the documentation where the hook is invoked.

Documenting hook implementations and theme preprocess functions
If the implementation of a hook is rather standard and does not require more explanation than the hook reference provides, a shorthand documentation form may be used in place of the full function documentation block described above:

/**
 * Implements hook_help().
 */
function blog_help($section) {
  // ...
}
This generates a link to the hook reference, reminds the developer that this is a hook implementation, and avoids having to document parameters and return values that are the same for every implementation of the hook.

Optionally, you can add more information in a separate paragraph to describe the particular quirks of your hook implementation. But do not include documentation of the parameters and return values -- this should be in the hook definition documentation, not in the hook implementation documentation.

In the case of hooks that have variables in the names, such as hook_form_FORM_ID_alter(), a slightly expanded syntax should be used:

/**
 * Implements hook_form_FORM_ID_alter() for node_type_form().
 */
function mymodule_form_node_type_form_alter(&$form, &$form_state) {
  // ...
}
This generates a link to the hook reference, as well as to the particular form that is being altered. Again, optionally you can add more information in a separate paragraph to describe the particular quirks of your hook implementation.

Theme preprocess functions work the same as hook_form_FORM_ID_alter(). Example:
/**
 * Implements hook_preprocess_HOOK() for theme_panels_pane().
 */
function mymodule_preprocess_panels_pane(&$variables) {}

Note that the "for ..." part could reference either a theme function like theme_panels_pane(), or a theme template file like search-result.tpl.php.
For hook_update_N() implementations, there is a different standard, because the documentation's first line is displayed to tell people what will happen (or did happen) when update.php is run. So they are documented as follows:
/**
 * Add several fields to the {node_revision} table.
 *
 * Longer description can go here, if necessary.
 */
function  module_name_update_7002() {
}

Note that the first line should start with an imperative verb, so it will make sense to people running update.php.
Documenting form-generating functions
/**
 * Form constructor for the user login form.
 *
 * @param string $message
 *   The message to display.
 *
 * @see user_login_form_validate()
 * @see user_login_form_submit()
 * @ingroup forms
 */
function user_login_form($form, &$form_state, $message = '')

/**
 * Form validation handler for user_login_form().
 *
 * @see user_login_form_submit()
 */
function user_login_form_validate($form, &$form_state) {
  ...
}

/**
 * Form submission handler for user_login_form().
 *
 * @see user_login_form_validate()
 */
function user_login_form_submit($form, &$form_state) {
  ...
}
Notes:

@param and @return for the standard parameters and return value should be omitted from all form-generating functions. Do include @param documentation for additional parameters beyond $form and $form_state, if there are any.
A form constructor is a callback meant to be used as an argument for drupal_get_form().
You may indicate that a function is a form constructor by adding the @ingroup forms directive. This groups all forms into the Form Builder topic on api.drupal.org.
Do not add @ingroup forms to validation, submission, or other handlers of the form.
Correlating validation and submit handlers should contain the form constructor function name in their phpDoc summary, as visible above.
You may add @see directives to link to correlating validation and submission handlers, and vice-versa.
If the form generates a page (i.e., in a hook_menu() implementation, the page callback is 'drupal_get_form' and the page argument is this form), include an @see reference to the hook_menu() implementation that uses it.

Documenting Render API callbacks
The Render API uses various callbacks functions, which you can assign to a render element when setting up a render array (or a form). Document them as follows:

/**
 * Render API callback: Validates the maximum upload size field.
 *
 * Ensures that a size has been entered and that it can be parsed by
 * parse_size().
 *
 * This function is assigned as an #element_validate callback in
 * file_field_instance_settings_form().
 */
function _file_generic_settings_max_filesize($element, &$form_state, $form) {

Notes:
- The first line starts with "Render API callback:", and then has a description of what the function does (with verb in 3rd person as is standard in functions).
- The next (optional) paragraphs contain further description.
- Somewhere in the function, there should be a paragraph saying where the callback is assigned and what particular Render API callback it is being used for.
- @param and @return are omitted if the parameters and return value are the standard callback parameters/return. If there are additional parameters, document them.
Note: This standard was added for Drupal version 8.

Documenting themeable functions
/**
 * Returns HTML for a foo.
 *
 * @param array $variables
 *   An associative array containing:
 *   - foo: The foo object that is being formatted.
 *   - show_bar: TRUE to show the bar component, FALSE to omit it.
 *
 * @ingroup themeable
 */
function theme_foo($variables) {
  ...
}
In order to provide a quick reference for themers, we tag all themeable functions so that Doxygen can group them together. A themeable function is defined as any function meant to be called via theme(), and you can indicate that a function is themeable by adding an @ingroup themeable directive. It is also beneficial to use @see directives to link the theme function with its preprocessing functions.

In the rare case that your theme function returns some other type of formatted output other than HTML, you should add a @return directive with a description of what the function generates and returns.

See also the hook implementations section for how to document preprocess functions.

Documenting theme templates
Note: There is a separate page on documenting Twig templates.

If a template file (themeablename.tpl.php) and a preprocess function is used instead of a theming function, both the template file and the preprocessing function need to be documented. The template file should be documented with a @file directive, should have @ingroup themeable in the file documentation header, and should contain a list of the variables that the template_preprocess_HOOK has prepared for it. (Note: The @ingroup themeable line should *not* be included in the documentation header for themes that override the file -- only in the base version.) If any of these variables contain data that is unsafe to output for XSS reasons, that should be documented; otherwise it can be assumed that variables available have already been appropriately filtered. Anything not listed should not be assumed to be safe to output. It should also contain a @see directive to link back to the preprocessing function.

/**
 * @file
 * Displays a list of forums.
 *
 * Available variables:
 * - $forums: An array of forums to display. Each $forum in $forums contains:
 *   - $forum->is_container: Is TRUE if the forum can contain other forums. Is
 *     FALSE if the forum can contain only topics.
 *   - $forum->depth: How deep the forum is in the current hierarchy.
 *   - $forum->name: The name of the forum.
 *   - $forum->link: The URL to link to this forum.
 *   - $forum->description: The description of this forum.
 *   - $forum->new_topics: True if the forum contains unread posts.
 *   - $forum->new_url: A URL to the forum's unread posts.
 *   - $forum->new_text: Text for the above URL which tells how many new posts.
 *   - $forum->old_topics: A count of posts that have already been read.
 *   - $forum->num_posts: The total number of posts in the forum.
 *   - $forum->last_reply: Text representing the last time a forum was posted
 *     or commented in.
 *
 * @see template_preprocess_forum_list()
 *
 * @ingroup themeable
 */
See also the hook implementations section for how to document preprocess functions and the page on documenting Twig templates.

Documenting hook_menu() callbacks
(This standard was adopted for Drupal version 8.x.) The page, access, title, and other callbacks that are registered in hook_menu() are documented as follows:

/**
 * Page callback: Displays a list of content.
 *
 * Longer description can go here.
 *
 * @param string $foo
 *   Description of a parameter for this page.
 *
 * @return array
 *   A render array for a page containing a list of content.
 *
 * @see node_menu()
 */
Notes:
In the first line, declare what type of callback it is (Page callback, Access callback, etc.), and give a short description of what it does (like you would for any function). Total line length: 80 characters, ending in "."
After a blank line, include a longer description of the function (if needed), followed by parameters and return value.
At the end, include an @see line that points to the hook_menu() implementation where this callback is registered. If multiple hook implementations use the callback, include one @see line for each.
Forms that generate pages (i.e., the page callback function is drupal_get_form() and the page argument is the form name): see the form generating function section.
Documenting constants and variables
/**
 * Denotes that a block is not enabled in any region and should not be shown.
 */
define('BLOCK_REGION_NONE', -1);

/**
 * The base URL of the drupal installation.
 *
 * @see conf_init()
 */
global $base_url;
Constants, global variables, and member variables of classes should have documentation blocks. As usual, the documentation block should start with a one-line description, and any additional needed documentation should be separated from the summary line by a blank line (though usually only one line is needed).

Documenting classes and interfaces
Each class and interface should have a doxygen documentation block, and each member variable, constant, and function/method within the class or interface should also have its own documentation block. Examples:

/**
 * Defines a common interface for a prepared statement.
 */
interface DatabaseStatementInterface extends Traversable {

  /**
   * Executes a prepared statement.
   *
   * @param array $args
   *   Array of values to substitute into the query.
   * @param array $options
   *   Array of options for this query.
   *
   * @return bool
   *   TRUE on success, FALSE on failure.
   */
  public function execute($args = array(), $options = array());

}
/**
 * Represents a prepared statement.
 *
 * Default implementation of DatabaseStatementInterface.
 */
class DatabaseStatementBase extends PDOStatement implements DatabaseStatementInterface {

  /**
   * The database connection object for this statement's DatabaseConnection.
   *
   * @var \Drupal\Core\Database\Connection
   */
  public $dbh;

  /**
   * Constructs a DatabaseStatementBase object.
   *
   * @param \Drupal\Core\Database\Connection $dbh
   *   Database connection object.
   */
  protected function __construct($dbh) {
    // Function body here.
  }

  /**
   * Implements DatabaseStatementInterface::execute().
   *
   * Optional explanation of the specifics of this implementation goes here.
   */
  public function execute($args = array(), $options = array()) {
    // Function body here.
  }

  /**
   * Returns the foo information.
   *
   * @return object
   *   The foo information as an object.
   *
   * @throws \My\Namespace\MyFooUndefinedException
   */
  public function getFoo() {
    // Function body here.
  }

  /**
   * Overrides PDOStatement::fetchAssoc().
   *
   * Optional explanation of the specifics of this override goes here.
  */
  public function fetchAssoc() {
    // Call PDOStatement::fetch to fetch the row.
    return $this->fetch(PDO::FETCH_ASSOC);
  }

}
Notes:

Make sure to have a docblock on each member variable and method. Short documentation forms should be used for the overridden/extended methods (don't repeat what is documented on the base method, just link to it by saying "Overrides BaseClassName::method()." or "Implements BaseInterfaceName::method().", and then document any specifics to this implementation, leaving out param/return docs).
Use a 3rd person verb to begin the description of a class, interface, or method (e.g. Represents not Represent).
For a member variable, use @var to tell what data type the variable is.
Use @throws if your method can throw an exception, followed by the name of the exception class. If the exception class is not specific enough to explain why the exception will be thrown, you should probably define a new exception class.
Make sure when documenting function and method return values, as well as member variable types, to use the most general class/interface possible. E.g., document the return value of db_select() to be a SelectQueryInterface, not a particular class that implements SelectQuery.
Annotations
/**
 * Defines a default fetcher implementation.
 *
 * Optional additional explanation goes here.
 *
 * @Plugin(
 *   id = "aggregator",
 *   title = @Translation("Default fetcher"),
 *   description = @Translation("Downloads data from a URL using Drupal's HTTP request handler.")
 * )
 */
 class DefaultFetcher implements FetcherInterface {

  /**
   * Implements FetcherInterface::fetch().
   */
  function fetch(&$feed) {
    // Function body here.
  }

}
Notes:

In Drupal 8, annotations should be at the absolute end of the docblock, below a blank line.
Each key of a plugin annotation should be on its own line.
Annotations are functional code, so individual lines should not be wrapped.
For more information on how annotations are used for plugins, see the plugin documentation.
Using namespaces and namespaced classes in documentation
The following guidelines apply to the use of namespaces in documentation blocks:

When using a class or interface name immediately after any @-tag in documentation, prefix it with the fully-qualified namespace name (starting with a backslash). If the class/interface is in the global namespace, prefix it with a backslash.
Elsewhere in documentation, omit the namespace if the class/interface is within the use/namespace declaration context of the file.
If you refer to a class/interface that is not in the current file's use/namespace declaration context, or that is in the global namespace, prefix it with the fully-qualified namespace name (starting with a backslash).
Example -- given that classes Foo, Bar, and Thingy are in the A\B\C namespace, and class Baz is in the X\Y namespace:
namespace A\B\C
use X\Y\Baz;

/**
 * Represents the data associated with foo.
 *
 * A Foo object can be used to do something interesting with Bar objects, but if
 * you need to do something else, you might need a Baz object. On the other hand,
 * the \J\K\L\Whatever class might be needed in some cases.
 *
 * @see \A\B\C\Bar
 * @see \A\B\C\Baz
 * @see \J\K\L\Whatever
 */
 class Foo {
   /**
    * Some kind of information stored on this class.
    *
    * @var \A\B\C\Thingy
    */
    protected $thingy;

   /**
    * Takes some kind of action.
    *
    * @param \A\B\C\Bar $bar
    *   The bar that this action depends on.
    *
    * @return \X\Y\Baz
    *   A baz that contains the returned information from this action.
    */
    public function some_action(Bar $bar) {
    }
}
Documenting Topics/Groups
You can make "Topics" pages by defining a "group", also known as a "module" in Doxygen terminology. We use topic/group pages to group functions that form an API that is meant to be used by multiple modules. Do not define a topic/group for functions that are internal to a particular file or module. See http://api.drupal.org/api/drupal/groups for a list of existing API topics/groups in Drupal Core.

Here is the basic syntax for defining a group:
/**
 * @defgroup group_identifier Name of the group for Topics page
 * @{
 * One line, one sentence description of the topic/group.
 *
 * One or more additional paragraphs of information, which will appear
 * on the topic page.
 * @}
 */
The @defgroup line gives the name of the group that will be used in the URL, and then the name that will be used as the page title and in the Topics listing. Any text within the @defgroup's own doxygen header appears as text on the topic page.

If you put the group definition immediately before one or more functions that belong in the group, you can automatically add functions to the group by using this syntax:
/**
 * @defgroup group_identifier Name of the group for Topics page
 * @{
 * One line, one sentence description of the topic/group.
 *
 * One or more additional paragraphs of information, which will appear
 * on the topic page.
 */

// Functions here are automatically added to the group.

/**
 * @} End of "defgroup group_identifier".
 */
If you have functions that need to be added to a group whose definition is not right above the functions, there are two ways to do it:

If there is just one function to be added, add
 * @ingroup group_identifier

to the documentation block for the function.
If there are multiple functions to be added, put
/**
 * @addtogroup group_identifier
 * @{
 */

before the functions, and
/**
 * @} End of "addtogroup group_identifier".
 */

after the group of functions.
A few notes:

Modules work at a global level, creating a new page for each group. They should be used only to group functions that provide some kind of API, which possibly spans multiple files. Or the other way round: they should not be used to group functions in a file when these functions are only used in that very file. Thats what Member Groups are for (which unfortunately aren't supported by api.module yet).
@defgroups can be defined only once - trying to define a second @defgroup name with a name already used will result in an error. Use @defgroup name in the "most important" section/file of that group and add to it from other places with @addtogroup / @ingroup (see above).
In pure Doxygen, the name in @defgroup name Explanation of that group must be single-word identifier, like a PHP variable or function name. Or, as regular expression: [a-zA-Z_][a-zA-Z0-9_]*. However, in the Drupal API module, we also allow period (.) and hyphen (-) in group names.
You can put @defgroup @{  when defining your group, and then the @} after a group of functions that should be included in the group. You can also do @addtogroup @{ @}, and everything between the {} will be included in the group. However, if you have a class or interface within the {}, that also means that every member function and member variable will be itself also included in the function/class/variable lists generated on the group page. This is probably not what you want, so it is best to just use the one-line @ingroup syntax when indicating that a particular class is part of a group.
Sections, sub-sections, and in-page links
As of October 2012 and API module version 7.x-1.1, you can add sections, sub-sections, and in-page links to your API documentation, to split up long pages. This is expected to be most useful for group/topic pages , but it can be used in any documentation.

Example:
 * @section sec_one Section 1
 * This is a link to @ref sec_two right here in the text.
 *
 * @subsection sub_a Sub-section A
 * Here's a sub-section.
 *
 * @section sec_two Section 2
 * This is a link to @ref sub_a right here in the text.
 

Syntax notes:
Each @section and @sub-section declaration must be on its own line.
@section and @subsection are followed by one space, then an identifier (letters, numbers, underscores, and hyphens only), then one space, and then the section/sub-section title.
@ref is used to make an in-page link to a section or subsection on the same page. The syntax is: @ref, followed by a single space, followed by the identifier of the (sub-)section (do not split the @ref from its identifier onto separate lines). The (sub-)section title is used as the link text. So the first paragraph of this example will read "This is a link to Section 2 right here in the text." when rendered.
Sections are rendered as H3 headers (same as the Parameters, Return Value, etc. headers on function pages, and the Functions header on a group/topic page), and sub-sections are rendered as H4 headers.
Limitations and hints
Drupal's Doxygen processing module, API module, currently only supports a small subset of all Doxygen commands and makes some assumptions about the formatting of the source. Code to be processed by api.module is advised to stick to these conventions.

The API module currently supports only one of Doxygen's three grouping mechanisms: Modules (@defgroup, @ingroup, @addtogroup, @{, @}). See groups section above for more information on how we use modules/groups.

Documenting contributed modules and themes

Don't use @mainpage. There can be only one @mainpage in the contributions repository, which is reserved for an index page of all contributes modules and themes.
Use Doxygen topics/groups (@defgroup, @ingroup, @addtogroup, see "Grouping" above) sparingly. There are currently thousands of modules in contrib, many of them consisting of more than one module. If each of these modules used just one @defgroup, there would be more than thousands more entries in the topic list on api.drupal.org. If each used more than one ...
If you do use Doxygen groups, make sure you give them a unique namespace, which would be your module's name. E.g. @defgroup views ... for the views.module, @defgroup views_ui ... for the views_ui.module. Don't use group names which are defined in Drupal core (hooks, themeable, file, batch, database, forms, form_api, format, image, validation, search, etc.).
If your module has only one file in it, it's not really necessary to define a group for it, because all of the functions, constants, etc. in the file will already be grouped together on the file page.





Namespaces

Last updated December 5, 2012. Created by Crell on November 26, 2011.
Edited by ksenzee, xjm, jhodgdon, zeta ζ. Log in to edit this page.

PHP 5.3 introduces namespaces to the language. This page documents how namespaces should be referenced within Drupal and it assumes that you are familiar with the concept of namespaces.

Not all files in Drupal declare a namespace. As of Drupal 8 an increasing number of files do, but not all. Prior to Drupal 8 virtually no code used namespaces, in order to remain compatible with PHP 5.2. There are therefore two slightly different standards.

"use"-ing classes

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
Class aliasing

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

Modules

Modules creating classes should place their code inside a custom namespace.
The convention for those namespaces is

namespace Drupal\example_module
To support autoloading, these classes should be placed inside the correct folder:
sites/all/modules/example_module/lib/Drupal/example_module
<module folder> /lib/ <namespace>







Object-oriented code

Last updated December 26, 2012. Created by Crell on October 19, 2009.
Edited by xjm, jhodgdon, Transition, tim.plunkett. Log in to edit this page.

Drupal follows common PHP conventions for object-oriented code, and established industry best practices. As always, though, there are Drupal-specific considerations.

Class-related coding standards covered elsewhere
Class/interface documentation: http://drupal.org/node/1354#classes
Namespaces: http://drupal.org/node/1353118
Plugin annotation: http://drupal.org/node/1354#annotations
Naming conventions
Classes and interfaces should use UpperCamel naming.
Methods and class properties should use lowerCamel naming.
If an acronym is used in a class or method name, make it all caps (SampleXMLClass, not SampleXmlClass).
Classes should not use underscores in class names unless absolutely necessary to derive names inherited class names dynamically. That is quite rare, especially as Drupal does not mandate a class-file naming match.
Names should not include "Drupal".
Class names should not have "Class" in the name.
Interfaces should always have the suffix "Interface".
Test classes should always have the suffix "Test".
Protected or private properties and methods should not use an underscore prefix.
Classes and interfaces should have names that stand alone to tell what they do without having to refer to the namespace, read well, and are as short as possible without losing functionality information or leading to ambiguity. Notes:
If necessary for clarity or to prevent ambiguity, include the last component of the namespace in the name.
Exception for Drupal 8.x: due to the way database classes are loaded, do not include the database engine name (MySQL, etc.) in engine-specific database class names.
Exception for test classes: Test classes only need to be unambiguous within the context of the module they are testing.
Stand-alone name examples:

Namespace	Good name	Bad names
Drupal\Core\Database\Query\	QueryCondition	Condition (ambiguous)
DatabaseQueryCondition (Database doesn't add to understanding)
Drupal\Core\FileTransfer\	LocalFileTransfer	Local (ambiguous)
Drupal\Core\Cache\	CacheDatabaseDriver	Database (ambiguous/misleading)
DatabaseDriver (ambiguous/misleading)
Drupal\entity\	Entity
EntityInterface	DrupalEntity (unnecessary words)
EntityClass (unnecessary words)
Drupal\comment\Tests\	ThreadingTest	CommentThreadingTest (only needs to be unambiguous in comment context)
Threading (not ending in Test)
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
Use of interfaces
The use of a separate interface definition from an implementing class is strongly encouraged because it allows more flexibility in extending code later. A separate interface definition also neatly centralizes documentation making it easier to read. All interfaces should be fully documented according to established documentation standards.

If there is even a remote possibility of a class being swapped out for another implementation at some point in the future, split the method definitions off into a formal Interface. A class that is intended to be extended must always provide an Interface that other classes can implement rather than forcing them to extend the base class.

Use an .inc file and use files[] in the .info file to extend a class or implement an interface.
If you include a file that extends a class or implements an interface, PHP generates a fatal error if the parent class or interface is not loaded. So, if a class is provided by a contributed module, or core in some cases, it is not safe to put your classes in a .module file. It's better to use an .inc file and use files[] in your .info file. For example, even if you have a dependency on a module, it's possible that both your module and the dependency are disabled when your .module file is included. Since the registry won't auto-load a class from a disabled module, this would cause an error. Also, when hook_boot() is run, module dependencies aren't loaded. So, if you add a class, then later implement hook_boot(), your module could be loaded without the dependency, and that will also generate a fatal error. Using an .inc file and using files[] in your .info file is needed to avoid those errors.

Visibility
All methods and properties of classes must specify their visibility: public, protected, or private. The PHP 4-style "var" declaration must not be used.

The use of public properties is strongly discouraged, as it allows for unwanted side effects. It also exposes implementation specific details, which in turn makes swapping out a class for another implementation (one of the key reasons to use objects) much harder. Properties should be considered internal to a class.

The use of private methods or properties is strongly discouraged. Private properties and methods may not be accessed or overridden by child classes, which limits the ability of other developers to extend a class to suit their needs.

Type hinting
PHP supports optional type specification for function and method parameters for classes and arrays. Although called "type hinting" it does make a type required, as passing an object that does not conform to that type will result in a fatal error.

DO specify a type when conformity to a specific interface is an assumption made by the function or method. Specifying the required interface makes debugging easier as passing in a bad value will give a more useful error message.
DO NOT use a class as the type in type hinting. If specifying a type, always specify an Interface. That allows other developers to provide their own implementations if necessary without modifying existing code.
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
Instantiation
Creating classes directly is discouraged. Instead, use a factory function that creates the appropriate object and returns it. This provides two benefits:

It provides a layer of indirection, as the function may be written to return a different object (with the same interface) in different circumstances as appropriate.
PHP does not allow class constructors to be chained, but does allow the return value from a function or method to be chained.
Chaining
PHP allows objects returned from functions and methods to be "chained", that is, a method on the returned object may be called immediately, like so:

<?php
// Unchained version
$result = db_query("SELECT title FROM {node} WHERE nid = :nid", array(':nid' => 42));
$title = $result->fetchField();

// Chained version
$title = db_query("SELECT title FROM {node} WHERE nid = :nid", array(':nid' => 42))->fetchField();
?>
As a general rule, a method should return $this, and thus be chainable, in any case where there is no other logical return value. Common examples are those methods that set some state or property on the object. It is better in those cases to return $this rather than TRUE/FALSE or NULL.





PHP Exceptions

Last updated November 8, 2011. Created by Crell on October 19, 2009.
Edited by jhodgdon, Marc-Antoine, sun. Log in to edit this page.

Basic conventions

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
Try-catch blocks

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
Inheritance

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





SQL coding conventions

Last updated November 8, 2011. Created by mr.baileys on August 10, 2003.
Edited by jhodgdon, eporama, davidfells, ywarnier. Log in to edit this page.

Don't use Reserved Words
Don't use (ANSI) SQL / MySQL / PostgreSQL / MS SQL Server / ... Reserved Words for column and/or table names. Even if this may work with your (MySQL) installation, it may not with others or with other databases. Some references:

(ANSI) SQL Reserved Words
MySQL Reserved Words: 5.1, 5.0, 3.23.x, 4.0, 4.1
PostgreSQL Reserved Words
Oracle Reserved Words (in particular UID is a problem in our context)
MS SQL Server Reserved Words
DB2 Reserved Words
Some commonly misused keywords: TIMESTAMP, TYPE, TYPES, MODULE, DATA, DATE, TIME, ...
See also [bug] SQL Reserved Words.

Capitalization and user-supplied data
Make SQL reserved words UPPERCASE. This is not a suggestion. Drupal db abstraction commands will fail if this convention is not followed.
Make column and constraint names lowercase.
Enclose each table name with {} (this allows Drupal to prefix table names).
Variable arguments (which are often user-supplied) should be moved out of the query body and passed in as separate parameters to db_query(), db_query_range(), and db_query_temporary(), etc. The query body should instead contain placeholders specifying the type of each argument (%d|%s|%%|%f|%b). This ensures that the data will be properly escaped and prevents SQL injection attacks.
Preventing SQL injection is easy; db_query provides a way to use parametrized queries. Drupal's database functions replace the sprintf-like placeholders with the properly escaped arguments in order of appearance:
%d - integers
%f - floats
%s - strings, enclose in single quotes
%b - binary data, do not enclose in single quotes
%% - replaced with %
Read more about Database Access
Literal (constant) arguments can either be included in the query body or handled in the same way as variable arguments.
Any string literal or %s placeholder must be enclosed by single quotes: ' . Never use double quotes.
Example:
<?php
db_query("INSERT INTO {filters} (format, module, delta, weight) VALUES (%d, 'php', 0, 0)", $format);
?>
NOTE: as of Drupal 6.x, table definitions and constraints (e.g. primary keys, unique keys, indexes) should be always handled by the Schema API, which solves cross-database compatibility concerns automatically.

Naming
Use singular nouns for table names since they describe the entity the table represents. Drupal 6.x mixed singular/plural usage and this convention changed for Drupal 7.x.
It is a good practice to prefix table names with the module name to prevent possible namespace conflicts.
Name every constraint (primary, foreign, unique keys) yourself. Otherwise, you'll see funny-looking system-generated names in error messages. This happened with the moderation_roles table which initially defined a key without explicit name as KEY (mid). This got mysqldump'ed as KEY mid (mid) which resulted in a syntax error as mid() is a mysql function (see [bug] mysql --ansi cannot import install database).
Index names should begin with the name of the table they depend on, eg. INDEX users_sid_idx.
NOTE: as of Drupal 6.x, table definitions and constraints should be always handled by the Schema API.

Configure your Database server for standard compliance
Most Database Servers use extension to standard SQL. However, many of them can be configured to run in a (more) standard compliant mode. Every developer is encouraged to use the mode most standard compliant to avoid sloppy coding and compatibility problems.

MySQL
Enable ANSI and Strict Mode
Please help growing this list for other database servers!

References
Joe Celko - Ten Things I Hate About You
Joe Celko - SQL for Smarties: Advanced SQL Programming
RDBMS Naming conventions
Indentation
Drupal does not have a standard method for indentation or formating of longer SQL queries on multiple lines. Some competing strategies include:
<?php
if (!(db_query(
  "
    INSERT INTO {mlsp_media_file_type}
    SET extension   = '%s',
    attributes      = '%s'
  ",
  $file_type_entry['extension'],
  $selected_attributes
))) {
  $errors = TRUE;
}
?>

or
<?php
$sql = "SELECT t.*, j1.col1, j2.col2"
. " FROM {table} AS t"
. " LEFT JOIN {join1} AS j1 ON j1.id = t.jid"
. " LEFT JOIN {join2} AS j2 ON j2.id = t.jjid"
. " WHERE t.col LIKE '%s'"
. " ORDER BY %s"
;
$result = db_query($sql, 'findme', 't.weight');
?>

or, using the concept of "rivers" and HEREDOC syntax
<?php
$sql = <<<SQL
 SELECT t.*
      , j1.col1
      , j2.col2 
   FROM {table} AS t 
   LEFT 
   JOIN {join1} AS j1 
     ON j1.id = t.jid 
   LEFT
   JOIN {join2} AS j2
     ON j2.id = t.jid
  WHERE t.col LIKE '%s%' 
  ORDER 
     BY %s
SQL;
$result = db_query($sql, 'findme', 't.weight');
?>




Temporary placeholders and delimiters

Last updated November 8, 2011. Created by zeta ζ on January 14, 2008.
Edited by jhodgdon, sillygwailo, kiamlaluno. Log in to edit this page.

Temporary place-holders and delimiters
When writing a content filter module, or any code that processes or modifies content, it is tempting to use an obscure character as a place-holder, especially if only your code will see it: But this cannot be guaranteed. Non-printing, invalid or undocumented characters might not be handled correctly in the unlikely event that they are seen by a browser or feed-reader. And the more unlikely they are to be seen – the less likely they are to be tested. This will mean that some code will be written to find and eradicate these insidious characters, possibly including the ones your code is using to do its work.

To avoid this happening, and extending the lifetime of your code, please use an appropriate alpha-numeric string – prefixed by the name of the module (as a name-space) and a hyphen - or underscore _ – and surrounded by […].

If you need delimiting place-holders, the closing delimiter can incorporate a / after the initial [ and may suffix the modulename.

Finding your placeholders
A PCRE such as

'@\[modulename-tag\](.+?)\[/modulename-tag\]@'
or

'@\[modulename-tag\](.+?)\[/tag-modulename\]@' if you suffixed the modulename as mentioned above
can be used to match the string you have previously delimited.




Twig coding standards

Last updated November 30, 2012. Created by jenlampton on October 25, 2012.
Edited by joelpittet, steveoliver, geerlingguy, podarok. Log in to edit this page.

Most Twig coding standards can be found on the Twig website along with great Twig documentation. Items on this page (below) are useful when understanding how to write Twig code for Drupal.

The DocBlock
A docblock at the top of a Twig template should be identical to the docblock at the top of a PHPTemplate file (see http://drupal.org/coding-standards/docs#templates), with the entire docblock wrapped in the twig comment markers, {# and #}.

{#
/**
 * @file
 * Default theme implementation for a region.
 *
 * Available variables:
 * - content: The content for this region, typically blocks.
 * - attributes: Remaining HTML attributes for the element.
 * - attributes.class: HTML classes that can be used to style contextually 
 *    through CSS.
 *
 * @see template_preprocess()
 * @see template_preprocess_region()
 *
 * @ingroup themeable
 */
#}
Variables in the DocBlock

Variables in a twig template docblock should be referenced by name. They will not be surrounded by the Twig print indicators {{ and }} and will not be preceded by the PHP variable indicator $.

Variable definitions in the DocBlock

Variable definitions should not include any information about the type of variable (array, object, string) since people working in template files should not have to care about type. Everything that can be printed in a Twig template will end up printing as a string.

HTML attributes
HTML attributes in Drupal 8 are drillable. This means that you can print all of them at once by printing {{ attributes }} or you can print each attribute individually, like so:

<div id="{{ attributes.id }}" class="{{ attributes.class }}"{{ attributes }}>
  {{ content }}
</div>
If you choose to print out individual attributes within a HTML tag, you should still include the complete {{ attributes }} at the end, so that attributes added by modules are still also printed.

In Drupal core, we will print only the class attribute specially, all the others will be printed as part of {{ attributes }}. The reason for this is that it needs to be very easy for front end developers to be able to add a class, anywhere. By printing the class="" attribute directly in the template file, people familiar with HTML and CSS will see and recognize the correct way to add a class without needing to read documentation, or understand that Drupal has a preprocess layer.

<div class="{{ attributes.class }}"{{ attributes }}>
  {{ content }}
</div>
More advanced theme developers may choose to add their classes in preprocess and remove these separately printed classes from templates. This will prevent the empty class="" attribute from being printed when an element has no classes.

<div{{ attributes }}>
  {{ content }}
</div>
The Twig whitespace controller

Twig has a Twig whitespace control modifier that allows us to remove the whitespace between Twig {% conditions %} and {{ print }} statements. With careful use, it can help produce appropriately indented markup. NOTE: It does not have any affect on the output of {{ print }} statements; it affects only the space between Twig tags and other markup.

Example usage:

<!-- THIS IS WRONG DO NOT DO IT LIKE THIS -->
{% if block.show %}
<div class="admin-panel">
  {% if block.title -%}
    <h3>{{ block.title }}</h3>
  {% endif %}
  {% if block.content -%}
    <div class="body">
      {{- block.content -}}
    </div>
  {% elseif block.description -%}
    <div class="description">
      {{- block.description -}}
    </div>
  {% endif %}
</div>
{% endif %}
You'll note that we do not need to use the modifier very often. In most cases, it is clearer to remove the leading or trailing space, rather than add a space for legibility and then also use a whitespace controller to remove the space. For now, we will use whitespace controllers very sparingly in core.

The correct way to output the template in the example above, would be as follows:
Example usage:
(from /core/themes/stark/templates/system/admin-block.html.twig):

{% if block.show %}
<div class="admin-panel">
  {% if block.title %}<h3>{{ block.title }}</h3>{% endif %}
  {% if block.content %}<div class="body">{{ block.content }}</div>
  {% elseif block.description %}<div class="description">{{ block.description }}</div>{% endif %}
</div>
{% endif %}
Both of the examples above produce the same appropriately indented markup:

<div class="admin-panel">
  <h3>People</h3>
  <div class="body">Content</div>
</div>
We may revisit this decision about use of whitespace controllers later, after converting everything to twig and examining our resulting markup. For now, we will be using them as sparingly as possible, as not to confuse people who are new to Twig.

DOs and DON'Ts for using spaces and whitespace controllers in Twig templates:

Never add a whitespace controllers around classes class="{{ attributes.class }}"
Do not use a whitespace controller before all attributes {{ attributes }}
Do not use a whitespace controller before or after commands {%- no -%}
Do not use a whitespace controller before or after comments {#- no -#}
Filters

Some of the most common Drupal functions like t and url have been made available in your twig templates as filters. Filters are triggered by using the pipe character  | .

This is how you would use the t() function as a filter in a twig template.

  <div class="preview-image-wrapper">
      {{ 'original'|t }} 
  </div>
Please do not put spaces on either side of the pipe.

Inline comments
Inline comments will be surrounded by the twig comment indicator {# and #}.

Comments that span one line will have the comment indicators on the same line as the comment.

  <div class="content{{ content_attributes.class }}"{{ content_attributes }}>
    {# We hide the comments and links now so that we can render them later. #}
    {{ hide(content.comments) }}
    {{ hide(content.links) }}
    {{ content }}
  </div>
Comments that span more than one line will have the comment indicators on separate lines. Comments should be wrapped so the line is less than 80 characters.

  {#
    This is a very long comment.  It spans more than one line. This is a very long 
    comment.  It spans more than one line.  This is a very long comment.  It spans
    more than one line. This is a very long comment.  It spans more than one line. 
  #}
  <div class="{{ attributes.class }}"{{ attributes }}>
    {{ content }}
  </div>
‹ Temporary placeholders and delimitersupUse Drupal Unicode functions for strings ›
Login or register to post comments
Looking for support? Visit the Drupal.org forums, or join #drupal-support in IRC.

Comments
Twig whitespace control (dashes) in condition tags
Posted by steveoliver on November 18, 2012 at 12:40am
Here's a case FOR using whitespace control dashes within Twig condition tags:

Without whitespace control dashes:

{% if block.show %}
<div class="admin-panel">
  {% if block.title %}
    <h3>{{ block.title }}</h3>
  {% endif %}
  {% if block.content %}
    <div class="body">{{ block.content }}</div>
  {% elseif block.description %}
    <div class="description">{{ block.description }}</div>
  {% endif %}
</div>
{% endif %}

...produces unsightly indentation:
<div class="admin-panel">
      <h3>People</h3>
        <div class="body">
With whitespace control dashes:

{% if block.show %}
<div class="admin-panel">
  {% if block.title -%}
    <h3>{{ block.title }}</h3>
  {% endif %}
  {% if block.content -%}
    <div class="body">{{ block.content }}</div>
  {% elseif block.description -%}
    <div class="description">{{ block.description }}</div>
  {% endif %}
</div>
{% endif %}

...produces appropriately indented markup:
<div class="admin-panel">
  <h3>People</h3>
    <div class="body">
--



Use Drupal Unicode functions for strings

Last updated November 8, 2011. Created by jhodgdon on May 26, 2009.
Edited by kiamlaluno, sun. Log in to edit this page.

If you are writing a module or theme, consider that it may be used on web sites around the world, some of which use languages whose characters are multi-byte Unicode, rather than single-byte ASCII or European formats. Some of the built-in PHP string processing functions do not work correctly on multi-byte text.

For that reason, Drupal provides replacements for PHP's built-in text functions. You should use these replacements when you are programming for Drupal, except where noted. The Coder module can be used to check your module for these replacement functions.

Here are the replacement functions:

drupal_strlen(): Replaces PHP strlen(). Note that if you are using strlen() just to check whether the length of a string is non-zero, it is not necessary to use the Drupal replacement.
drupal_strtolower(): Replaces PHP strtolower().
drupal_strtoupper(): Replaces PHP strtoupper(). See also drupal_ucfirst().
drupal_substr(): Replaces PHP substr(). See also truncate_utf8() and drupal_truncate_bytes().
For a complete list of all functions wrapping a regular PHP function in Drupal: PHP wrapper functions

If you are programming with text, you should also read the guide to handling text in a secure fashion.




Write E_ALL compliant code

Last updated November 8, 2011. Created by beginner on October 17, 2005.
Edited by jhodgdon, Andrew Schulman, mfb, willmoy. Log in to edit this page.

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
Use of isset() or !empty()
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
Beware!

The function isset() returns TRUE when the variable is set to 0, but FALSE if the variable is set to NULL. In some cases, is_null() is a better choice, especially when testing the value of a variable returned by an SQL query.




















The following describes the mandatory requirements that must be adhered
to for autoloader interoperability.

Mandatory
---------

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

Examples
--------

* `\Doctrine\Common\IsolatedClassLoader` => `/path/to/project/lib/vendor/Doctrine/Common/IsolatedClassLoader.php`
* `\Symfony\Core\Request` => `/path/to/project/lib/vendor/Symfony/Core/Request.php`
* `\Zend\Acl` => `/path/to/project/lib/vendor/Zend/Acl.php`
* `\Zend\Mail\Message` => `/path/to/project/lib/vendor/Zend/Mail/Message.php`

Underscores in Namespaces and Class Names
-----------------------------------------

* `\namespace\package\Class_Name` => `/path/to/project/lib/vendor/namespace/package/Class/Name.php`
* `\namespace\package_name\Class_Name` => `/path/to/project/lib/vendor/namespace/package_name/Class/Name.php`

The standards we set here should be the lowest common denominator for
painless autoloader interoperability. You can test that you are
following these standards by utilizing this sample SplClassLoader
implementation which is able to load PHP 5.3 classes.

Example Implementation
----------------------

Below is an example function to simply demonstrate how the above
proposed standards are autoloaded.
```php
<?php

function autoload($className)
{
    $className = ltrim($className, '\\');
    $fileName  = '';
    $namespace = '';
    if ($lastNsPos = strrpos($className, '\\')) {
        $namespace = substr($className, 0, $lastNsPos);
        $className = substr($className, $lastNsPos + 1);
        $fileName  = str_replace('\\', DIRECTORY_SEPARATOR, $namespace) . DIRECTORY_SEPARATOR;
    }
    $fileName .= str_replace('_', DIRECTORY_SEPARATOR, $className) . '.php';

    require $fileName;
}
```

SplClassLoader Implementation
-----------------------------

The following gist is a sample SplClassLoader implementation that can
load your classes if you follow the autoloader interoperability
standards proposed above. It is the current recommended way to load PHP
5.3 classes that follow these standards.

* [http://gist.github.com/221634](http://gist.github.com/221634)





Basic Coding Standard
=====================

This section of the standard comprises what should be considered the standard
coding elements that are required to ensure a high level of technical
interoperability between shared PHP code.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in [RFC 2119][].

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md


1. Overview
-----------

- Files MUST use only `<?php` and `<?=` tags.

- Files MUST use only UTF-8 without BOM for PHP code.

- Files SHOULD *either* declare symbols (classes, functions, constants, etc.)
  *or* cause side-effects (e.g. generate output, change .ini settings, etc.)
  but SHOULD NOT do both.

- Namespaces and classes MUST follow [PSR-0][].

- Class names MUST be declared in `StudlyCaps`.

- Class constants MUST be declared in all upper case with underscore separators.

- Method names MUST be declared in `camelCase`.


2. Files
--------

### 2.1. PHP Tags

PHP code MUST use the long `<?php ?>` tags or the short-echo `<?= ?>` tags; it
MUST NOT use the other tag variations.

### 2.2. Character Encoding

PHP code MUST use only UTF-8 without BOM.

### 2.3. Side Effects

A file SHOULD declare new symbols (classes, functions, constants,
etc.) and cause no other side effects, or it SHOULD execute logic with side
effects, but SHOULD NOT do both.

The phrase "side effects" means execution of logic not directly related to
declaring classes, functions, constants, etc., *merely from including the
file*.

"Side effects" include but are not limited to: generating output, explicit
use of `require` or `include`, connecting to external services, modifying ini
settings, emitting errors or exceptions, modifying global or static variables,
reading from or writing to a file, and so on.

The following is an example of a file with both declarations and side effects;
i.e, an example of what to avoid:

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

The following example is of a file that contains declarations without side
effects; i.e., an example of what to emulate:

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


3. Namespace and Class Names
----------------------------

Namespaces and classes MUST follow [PSR-0][].

This means each class is in a file by itself, and is in a namespace of at
least one level: a top-level vendor name.

Class names MUST be declared in `StudlyCaps`.

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

4. Class Constants, Properties, and Methods
-------------------------------------------

The term "class" refers to all classes, interfaces, and traits.

### 4.1. Constants

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

### 4.2. Properties

This guide intentionally avoids any recommendation regarding the use of
`$StudlyCaps`, `$camelCase`, or `$under_score` property names.

Whatever naming convention is used SHOULD be applied consistently within a
reasonable scope. That scope may be vendor-level, package-level, class-level,
or method-level.

### 4.3. Methods

Method names MUST be declared in `camelCase()`.







Coding Style Guide
==================

This guide extends and expands on [PSR-1][], the basic coding standard.

The intent of this guide is to reduce cognitive friction when scanning code
from different authors. It does so by enumerating a shared set of rules and
expectations about how to format PHP code.

The style rules herein are derived from commonalities among the various member
projects. When various authors collaborate across multiple projects, it helps
to have one set of guidelines to be used among all those projects. Thus, the
benefit of this guide is not in the rules themselves, but in the sharing of
those rules.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in [RFC 2119][].

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md
[PSR-1]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-1-basic-coding-standard.md


1. Overview
-----------

- Code MUST follow [PSR-1][].

- Code MUST use 4 spaces for indenting, not tabs.

- There MUST NOT be a hard limit on line length; the soft limit MUST be 120
  characters; lines SHOULD be 80 characters or less.

- There MUST be one blank line after the `namespace` declaration, and there
  MUST be one blank line after the block of `use` declarations.

- Opening braces for classes MUST go on the next line, and closing braces MUST
  go on the next line after the body.

- Opening braces for methods MUST go on the next line, and closing braces MUST
  go on the next line after the body.

- Visibility MUST be declared on all properties and methods; `abstract` and
  `final` MUST be declared before the visibility; `static` MUST be declared
  after the visibility.
  
- Control structure keywords MUST have one space after them; method and
  function calls MUST NOT.

- Opening braces for control structures MUST go on the same line, and closing
  braces MUST go on the next line after the body.

- Opening parentheses for control structures MUST NOT have a space after them,
  and closing parentheses for control structures MUST NOT have a space before.

### 1.1. Example

This example encompasses some of the rules below as a quick overview:

```php
<?php
namespace Vendor\Package;

use FooInterface;
use BarClass as Bar;
use OtherVendor\OtherPackage\BazClass;

class Foo extends Bar implements FooInterface
{
    public function sampleFunction($a, $b = null)
    {
        if ($a === $b) {
            bar();
        } elseif ($a > $b) {
            $foo->bar($arg1);
        } else {
            BazClass::bar($arg2, $arg3);
        }
    }

    final public static function bar()
    {
        // method body
    }
}
```

2. General
----------

### 2.1 Basic Coding Standard

Code MUST follow all rules outlined in [PSR-1][].

### 2.2 Files

All PHP files MUST use the Unix LF (linefeed) line ending.

All PHP files MUST end with a single blank line.

The closing `?>` tag MUST be omitted from files containing only PHP.

### 2.3. Lines

There MUST NOT be a hard limit on line length.

The soft limit on line length MUST be 120 characters; automated style checkers
MUST warn but MUST NOT error at the soft limit.

Lines SHOULD NOT be longer than 80 characters; lines longer than that SHOULD
be split into multiple subsequent lines of no more than 80 characters each.

There MUST NOT be trailing whitespace at the end of non-blank lines.

Blank lines MAY be added to improve readability and to indicate related
blocks of code.

There MUST NOT be more than one statement per line.

### 2.4. Indenting

Code MUST use an indent of 4 spaces, and MUST NOT use tabs for indenting.

> N.b.: Using only spaces, and not mixing spaces with tabs, helps to avoid
> problems with diffs, patches, history, and annotations. The use of spaces
> also makes it easy to insert fine-grained sub-indentation for inter-line 
> alignment.

### 2.5. Keywords and True/False/Null

PHP [keywords][] MUST be in lower case.

The PHP constants `true`, `false`, and `null` MUST be in lower case.

[keywords]: http://php.net/manual/en/reserved.keywords.php



3. Namespace and Use Declarations
---------------------------------

When present, there MUST be one blank line after the `namespace` declaration.

When present, all `use` declarations MUST go after the `namespace`
declaration.

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


4. Classes, Properties, and Methods
-----------------------------------

The term "class" refers to all classes, interfaces, and traits.

### 4.1. Extends and Implements

The `extends` and `implements` keywords MUST be declared on the same line as
the class name.

The opening brace for the class MUST go on its own line; the closing brace
for the class MUST go on the next line after the body.

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

Lists of `implements` MAY be split across multiple lines, where each
subsequent line is indented once. When doing so, the first item in the list
MUST be on the next line, and there MUST be only one interface per line.

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

### 4.2. Properties

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

### 4.3. Methods

Visibility MUST be declared on all methods.

Method names SHOULD NOT be prefixed with a single underscore to indicate
protected or private visibility.

Method names MUST NOT be declared with a space after the method name. The
opening brace MUST go on its own line, and the closing brace MUST go on the
next line following the body. There MUST NOT be a space after the opening
parenthesis, and there MUST NOT be a space before the closing parenthesis.

A method declaration looks like the following. Note the placement of
parentheses, commas, spaces, and braces:

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

### 4.4. Method Arguments

In the argument list, there MUST NOT be a space before each comma, and there
MUST be one space after each comma.

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

### 4.5. `abstract`, `final`, and `static`

When present, the `abstract` and `final` declarations MUST precede the
visibility declaration.

When present, the `static` declaration MUST come after the visibility
declaration.

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

### 4.6. Method and Function Calls

When making a method or function call, there MUST NOT be a space between the
method or function name and the opening parenthesis, there MUST NOT be a space
after the opening parenthesis, and there MUST NOT be a space before the
closing parenthesis. In the argument list, there MUST NOT be a space before
each comma, and there MUST be one space after each comma.

```php
<?php
bar();
$foo->bar($arg1);
Foo::bar($arg2, $arg3);
```

Argument lists MAY be split across multiple lines, where each subsequent line
is indented once. When doing so, the first item in the list MUST be on the
next line, and there MUST be only one argument per line.

```php
<?php
$foo->bar(
    $longArgument,
    $longerArgument,
    $muchLongerArgument
);
```

5. Control Structures
---------------------

The general style rules for control structures are as follows:

- There MUST be one space after the control structure keyword
- There MUST NOT be a space after the opening parenthesis
- There MUST NOT be a space before the closing parenthesis
- There MUST be one space between the closing parenthesis and the opening
  brace
- The structure body MUST be indented once
- The closing brace MUST be on the next line after the body

The body of each structure MUST be enclosed by braces. This standardizes how
the structures look, and reduces the likelihood of introducing errors as new
lines get added to the body.


### 5.1. `if`, `elseif`, `else`

An `if` structure looks like the following. Note the placement of parentheses,
spaces, and braces; and that `else` and `elseif` are on the same line as the
closing brace from the earlier body.

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

The keyword `elseif` SHOULD be used instead of `else if` so that all control
keywords look like single words.


### 5.2. `switch`, `case`

A `switch` structure looks like the following. Note the placement of
parentheses, spaces, and braces. The `case` statement MUST be indented once
from `switch`, and the `break` keyword (or other terminating keyword) MUST be
indented at the same level as the `case` body. There MUST be a comment such as
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


### 5.3. `while`, `do while`

A `while` statement looks like the following. Note the placement of
parentheses, spaces, and braces.

```php
<?php
while ($expr) {
    // structure body
}
```

Similarly, a `do while` statement looks like the following. Note the placement
of parentheses, spaces, and braces.

```php
<?php
do {
    // structure body;
} while ($expr);
```

### 5.4. `for`

A `for` statement looks like the following. Note the placement of parentheses,
spaces, and braces.

```php
<?php
for ($i = 0; $i < 10; $i++) {
    // for body
}
```

### 5.5. `foreach`
    
A `foreach` statement looks like the following. Note the placement of
parentheses, spaces, and braces.

```php
<?php
foreach ($iterable as $key => $value) {
    // foreach body
}
```

### 5.6. `try`, `catch`

A `try catch` block looks like the following. Note the placement of
parentheses, spaces, and braces.

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

6. Closures
-----------

Closures MUST be declared with a space after the `function` keyword, and a
space before and after the `use` keyword.

The opening brace MUST go on the same line, and the closing brace MUST go on
the next line following the body.

There MUST NOT be a space after the opening parenthesis of the argument list
or variable list, and there MUST NOT be a space before the closing parenthesis
of the argument list or variable list.

In the argument list and variable list, there MUST NOT be a space before each
comma, and there MUST be one space after each comma.

Closure arguments with default values MUST go at the end of the argument
list.

A closure declaration looks like the following. Note the placement of
parentheses, commas, spaces, and braces:

```php
<?php
$closureWithArgs = function ($arg1, $arg2) {
    // body
};

$closureWithArgsAndVars = function ($arg1, $arg2) use ($var1, $var2) {
    // body
};
```

Argument lists and variable lists MAY be split across multiple lines, where
each subsequent line is indented once. When doing so, the first item in the
list MUST be on the next line, and there MUST be only one argument or variable
per line.

When the ending list (whether or arguments or variables) is split across
multiple lines, the closing parenthesis and opening brace MUST be placed
together on their own line with one space between them.

The following are examples of closures with and without argument lists and
variable lists split across multiple lines.

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

Note that the formatting rules also apply when the closure is used directly
in a function or method call as an argument.

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


7. Conclusion
--------------

There are many elements of style and practice intentionally omitted by this
guide. These include but are not limited to:

- Declaration of global variables and global constants

- Declaration of functions

- Operators and assignment

- Inter-line alignment

- Comments and documentation blocks

- Class name prefixes and suffixes

- Best practices

Future recommendations MAY revise and extend this guide to address those or
other elements of style and practice.


Appendix A. Survey
------------------

In writing this style guide, the group took a survey of member projects to
determine common practices.  The survey is retained herein for posterity.

### A.1. Survey Data

    url,http://www.horde.org/apps/horde/docs/CODING_STANDARDS,http://pear.php.net/manual/en/standards.php,http://solarphp.com/manual/appendix-standards.style,http://framework.zend.com/manual/en/coding-standard.html,http://symfony.com/doc/2.0/contributing/code/standards.html,http://www.ppi.io/docs/coding-standards.html,https://github.com/ezsystems/ezp-next/wiki/codingstandards,http://book.cakephp.org/2.0/en/contributing/cakephp-coding-conventions.html,https://github.com/UnionOfRAD/lithium/wiki/Spec%3A-Coding,http://drupal.org/coding-standards,http://code.google.com/p/sabredav/,http://area51.phpbb.com/docs/31x/coding-guidelines.html,https://docs.google.com/a/zikula.org/document/edit?authkey=CPCU0Us&hgd=1&id=1fcqb93Sn-hR9c0mkN6m_tyWnmEvoswKBtSc0tKkZmJA,http://www.chisimba.com,n/a,https://github.com/Respect/project-info/blob/master/coding-standards-sample.php,n/a,Object Calisthenics for PHP,http://doc.nette.org/en/coding-standard,http://flow3.typo3.org,https://github.com/propelorm/Propel2/wiki/Coding-Standards,http://developer.joomla.org/coding-standards.html
    voting,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,no,no,no,?,yes,no,yes
    indent_type,4,4,4,4,4,tab,4,tab,tab,2,4,tab,4,4,4,4,4,4,tab,tab,4,tab
    line_length_limit_soft,75,75,75,75,no,85,120,120,80,80,80,no,100,80,80,?,?,120,80,120,no,150
    line_length_limit_hard,85,85,85,85,no,no,no,no,100,?,no,no,no,100,100,?,120,120,no,no,no,no
    class_names,studly,studly,studly,studly,studly,studly,studly,studly,studly,studly,studly,lower_under,studly,lower,studly,studly,studly,studly,?,studly,studly,studly
    class_brace_line,next,next,next,next,next,same,next,same,same,same,same,next,next,next,next,next,next,next,next,same,next,next
    constant_names,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper,upper
    true_false_null,lower,lower,lower,lower,lower,lower,lower,lower,lower,upper,lower,lower,lower,upper,lower,lower,lower,lower,lower,upper,lower,lower
    method_names,camel,camel,camel,camel,camel,camel,camel,camel,camel,camel,camel,lower_under,camel,camel,camel,camel,camel,camel,camel,camel,camel,camel
    method_brace_line,next,next,next,next,next,same,next,same,same,same,same,next,next,same,next,next,next,next,next,same,next,next
    control_brace_line,same,same,same,same,same,same,next,same,same,same,same,next,same,same,next,same,same,same,same,same,same,next
    control_space_after,yes,yes,yes,yes,yes,no,yes,yes,yes,yes,no,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes,yes
    always_use_control_braces,yes,yes,yes,yes,yes,yes,no,yes,yes,yes,no,yes,yes,yes,yes,no,yes,yes,yes,yes,yes,yes
    else_elseif_line,same,same,same,same,same,same,next,same,same,next,same,next,same,next,next,same,same,same,same,same,same,next
    case_break_indent_from_switch,0/1,0/1,0/1,1/2,1/2,1/2,1/2,1/1,1/1,1/2,1/2,1/1,1/2,1/2,1/2,1/2,1/2,1/2,0/1,1/1,1/2,1/2
    function_space_after,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no,no
    closing_php_tag_required,no,no,no,no,no,no,no,no,yes,no,no,no,no,yes,no,no,no,no,no,yes,no,no
    line_endings,LF,LF,LF,LF,LF,LF,LF,LF,?,LF,?,LF,LF,LF,LF,?,,LF,?,LF,LF,LF
    static_or_visibility_first,static,?,static,either,either,either,visibility,visibility,visibility,either,static,either,?,visibility,?,?,either,either,visibility,visibility,static,?
    control_space_parens,no,no,no,no,no,no,yes,no,no,no,no,no,no,yes,?,no,no,no,no,no,no,no
    blank_line_after_php,no,no,no,no,yes,no,no,no,no,yes,yes,no,no,yes,?,yes,yes,no,yes,no,yes,no
    class_method_control_brace,next/next/same,next/next/same,next/next/same,next/next/same,next/next/same,same/same/same,next/next/next,same/same/same,same/same/same,same/same/same,same/same/same,next/next/next,next/next/same,next/same/same,next/next/next,next/next/same,next/next/same,next/next/same,next/next/same,same/same/same,next/next/same,next/next/next

### A.2. Survey Legend

`indent_type`:
The type of indenting. `tab` = "Use a tab", `2` or `4` = "number of spaces"

`line_length_limit_soft`:
The "soft" line length limit, in characters. `?` = not discernible or no response, `no` means no limit.

`line_length_limit_hard`:
The "hard" line length limit, in characters. `?` = not discernible or no response, `no` means no limit.

`class_names`:
How classes are named. `lower` = lowercase only, `lower_under` = lowercase with underscore separators, `studly` = StudlyCase.

`class_brace_line`:
Does the opening brace for a class go on the `same` line as the class keyword, or on the `next` line after it?

`constant_names`:
How are class constants named? `upper` = Uppercase with underscore separators.

`true_false_null`:
Are the `true`, `false`, and `null` keywords spelled as all `lower` case, or all `upper` case?

`method_names`:
How are methods named? `camel` = `camelCase`, `lower_under` = lowercase with underscore separators.

`method_brace_line`:
Does the opening brace for a method go on the `same` line as the method name, or on the `next` line?

`control_brace_line`:
Does the opening brace for a control structure go on the `same` line, or on the `next` line?

`control_space_after`:
Is there a space after the control structure keyword?

`always_use_control_braces`:
Do control structures always use braces?

`else_elseif_line`:
When using `else` or `elseif`, does it go on the `same` line as the previous closing brace, or does it go on the `next` line?

`case_break_indent_from_switch`:
How many times are `case` and `break` indented from an opening `switch` statement?

`function_space_after`:
Do function calls have a space after the function name and before the opening parenthesis?

`closing_php_tag_required`:
In files containing only PHP, is the closing `?>` tag required?

`line_endings`:
What type of line ending is used?

`static_or_visibility_first`:
When declaring a method, does `static` come first, or does the visibility come first?

`control_space_parens`:
In a control structure expression, is there a space after the opening parenthesis and a space before the closing parenthesis? `yes` = `if ( $expr )`, `no` = `if ($expr)`.

`blank_line_after_php`:
Is there a blank line after the opening PHP tag?

`class_method_control_brace`:
A summary of what line the opening braces go on for classes, methods, and control structures.

### A.3. Survey Results

    indent_type:
        tab: 7
        2: 1
        4: 14
    line_length_limit_soft:
        ?: 2
        no: 3
        75: 4
        80: 6
        85: 1
        100: 1
        120: 4
        150: 1
    line_length_limit_hard:
        ?: 2
        no: 11
        85: 4
        100: 3
        120: 2
    class_names:
        ?: 1
        lower: 1
        lower_under: 1
        studly: 19
    class_brace_line:
        next: 16
        same: 6
    constant_names:
        upper: 22
    true_false_null:
        lower: 19
        upper: 3
    method_names:
        camel: 21
        lower_under: 1
    method_brace_line:
        next: 15
        same: 7
    control_brace_line:
        next: 4
        same: 18
    control_space_after:
        no: 2
        yes: 20
    always_use_control_braces:
        no: 3
        yes: 19
    else_elseif_line:
        next: 6
        same: 16
    case_break_indent_from_switch:
        0/1: 4
        1/1: 4
        1/2: 14
    function_space_after:
        no: 22
    closing_php_tag_required:
        no: 19
        yes: 3
    line_endings:
        ?: 5
        LF: 17
    static_or_visibility_first:
        ?: 5
        either: 7
        static: 4
        visibility: 6
    control_space_parens:
        ?: 1
        no: 19
        yes: 2
    blank_line_after_php:
        ?: 1
        no: 13
        yes: 8
    class_method_control_brace:
        next/next/next: 4
        next/next/same: 11
        next/same/same: 1
        same/same/same: 6
