
# Drupal SimpleTest coding standards


You should follow these conventions when writing a test.

Naming conventions
Drupal 8

namespace Drupal\$module\Tests;

use Drupal\simpletest\[Web|Unit|DrupalUnit]TestBase;

Foo[Unit]Test extends [Web|Unit|DrupalUnit]TestBase
Drupal 6/7

Foo[Unit]Test extends [Web|Unit]TestCase
File names

For modules, a single [module name].test file is the general rule. It should be placed directly in the module folder.
For core facilities (all API functions that are in includes/), tests files are named [facility name].test and are placed in the modules/system/tests folder.
If other supporting files are required for the tests to work (for example, hidden modules, XML files, images) they should be located in a /tests/ subdirectory within the module directory.
Mock module names

If a test requires defining hooks, it can create a mock module.
This module should be named [test file name]_test.module (its associated info file would then be [test file name]_test.info.
Set the property hidden = TRUE in the .info file to prevent it from being enabled in the interface.
Test function names

All tests functions are prefixed with test.

Splitting up your tests
When you're unit testing a single function--as opposed to doing functional testing of a module's page--each test function should focus on a specific aspect of the function's operation. The "kitchen sink" style of tests is common in core right now but it's bad practice. Test should serve two purposes:

ensure that the code operates in a certain way
provide a runnable example of the code's usage
Monolithic tests generally fail badly because once one portion of the tests fails it cascades through the later tests. This makes it hard to isolate exactly where the fault lies. By breaking the code up into separate tests you may duplicate some code--though if you're duplicating code between a majority of the tests it should probably go into the setUp() function--but you'll have an isolated test that passes or fails on its own.

You also get several positive side effects by writing smaller tests:

Each test becomes a simple, self-contained example of how the function can be used, answering the question "Given these inputs what behavior can I expect?"
Since each test only looks at a single aspect you'll find that you're much more willing to test edge cases. Unlike kitchen sink test you won't have to worry about "breaking up the flow" with an extra test.
Test template
After the opening PHP tag and a blank line comes the @file declaration:

<?php
/**
 * @file
 * Tests for Aggregator module.
 */
?>
or

<?php
/**
 * @file
 * Tests for common.inc.
 */
?>
The @file declaration should contain a simple summary of what tests are included in the file. Either for the module, or for the file.

<?php
/**
 * Functional tests for the Kitten Basket.
 */
class KittenBasketTestCase extends DrupalWebTestBase {
?>
The comment above the class is mandatory, it is a rephrasing of the 'name' of the test found in getInfo() function ('Kitten basket functionality' -> 'Functional tests for the Kitten basket').

<?php
  public static function getInfo() {
    return array(
      'name' => 'Kitten basket functionality',
      'description' => 'Move kittens in and out of their basket and assert that none of them were hurt in the process.',
      'group' => 'Kitten basket',
    );
  }
?>
The very first function has to be getInfo().
name, description and group are not enclosed in t() calls.
name should be short and should not contain any period. It should describes an overview of what subsystem is tested by the class; for example, "User access rules" or "Menu link creation/deletion"
description should start with a verb describing the test. It should end with a period.
group should be the human-readable name of the module (Node, Statistics), or the human-readable name of the Drupal facility tested (Form API or XML-RPC).
There is no PHPDoc on this function since it is an inherited method.
<?php
  public function setUp() {
    parent::setUp('kitten_module', 'basket_module');
     // Setup tasks go here.
  }
?>
setUp() is optional. If your test is for specific module(s), make sure to pass each module's name as a parameter to setUp() so it will be automatically enabled for you. If the module don't have to perform any particular setup, it should not be defined.
setUp() and tearDown() are called before and after every test function is called.
There is no PHPDoc on this function since it is an inherited method.
<?php
  public function tearDown() {
    // Teardown tasks go here.
     parent::tearDown();
  }
?>
tearDown() should not be used, except in very particular cases. Simpletest will simply drop the test database, so you don't even have to undo the setup tasks performed in setUp().
setUp() and tearDown() are called before and after every test function is called.
There is no PHPDoc on this function since it is an inherited method.
<?php
  /**
   * One-sentence description of what test entails.
   */
  public function testExampleThing() {
    // Assertions, etc. go here.
  }

  /**
   * One-sentence description of what test entails.
   */
  public function testNextExampleThing() {
    // Assertions, etc. go here.
  }
  // Can have many more testFoo functions.
}
?>

