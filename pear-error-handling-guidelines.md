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

