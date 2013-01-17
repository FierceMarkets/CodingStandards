
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


