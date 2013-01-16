
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

