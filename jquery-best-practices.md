# jQuery best pratices

Prefix variables that point to jQuery objects with a dollar sign
var $foo = $('.myClass');
Avoid compatibility issues
jQuery is evolving quickly but attempts to maintain backward compatibility with previous releases. Although multiple jQuery versions are not officially supported within a single Drupal major release, many sites use the jQuery update module to take advantage of performance and bug fixes offered by newer versions of jQuery. Custom modules using jQuery should keep up to date with syntax best-practices in order to avoid conflicts with updated versions.

.attr()

This applies only for D6 and D7 because D8 will ship with jQuery 1.7+ which instead uses .prop() for property.
Wrong use of .attr is the cause of the many compatibility problems.

Use booleans to set a property, not an empty string

//correct
$yourElement.attr('disabled', false);
$yourElement.attr('disabled', true);
//incorrect
$yourElement.attr('disabled', '');
Do not assume that property values are boolean
.attr('checked') could return either true or 'checked' depending on jQuery version and markup.

//correct
if ( $('input:checkbox').attr('checked') ) {}
//incorrect
if ( $('input:checkbox').attr('checked') === true ) {}

