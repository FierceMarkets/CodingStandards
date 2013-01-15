
# CSS coding standards


Status
Draft. Drupal core (HEAD) should adhere to the latest conventions revision.
Links
CSS coding standards issues
Drupal core "markup" component issues
Originating groups.drupal.org discussion
Initial proposal by Nick Lewis
Write valid CSS
All CSS code should be valid CSS, preferably to CSS 2.1. CSS 3.0 is acceptable too, provided the usage can be justified and the principles of graceful degradation / progressive enhancement are followed.

Concise terminology used in these standards:

selector {
  property: value;
}
Selectors
Selectors should

be on a single line
have a space after the previous selector
end in an opening brace
be closed with a closing brace on a separate line without indentation
.book-navigation .page-next {
}
.book-navigation .page-previous {
}

.book-admin-form {
}
A blank line should be placed between each group, section, or block of multiple selectors of logically related styles.

Multiple selectors

Multiple selectors should each be on a single line, with no space after each comma:

#forum td.posts,
#forum td.topics,
#forum td.replies,
#forum td.pager {
Properties
All properties should be on the following line after the opening brace. Each property should:

be on its own line
be indented with two spaces, i.e., no tabs
have a single space after the property name and before the property value
end in a semi-colon
#forum .description {
  color: #efefef;
  font-size: 0.9em;
  margin: 0.5em;
}
Alphabetizing properties

Multiple properties should be listed in alphabetical order.

NOT OK:

body {
  font-weight: normal;
  background: #000;
  font-family: helvetica, sans-serif;
  color: #FFF;
}
OK:

body {
  background: #000;
  color: #fff;
  font-family: helvetica, sans-serif;
  font-weight: normal;
}
Colors (especially hex values) are preferred to be in lowercase.

Properties with multiple values

Where properties can have multiple values, each value should be separated with a space.

  font-family: helvetica, sans-serif;
CSS3 properties, vendor prefixes, progressive enhancement

.progressive-enhancement {
  background: #000 none;
  filter: progid:DXImageTransform.Microsoft.gradient(startColorstr=#E5000000,endColorstr=#E5000000);
  background-color: rgba(0, 0, 0, 0.9);
}
.progressive-prefixes {
  background-color: #003471;
  background-image: -webkit-gradient(linear, left top, left bottom, from(#003471), to(#448CCB));
  background-image: -webkit-linear-gradient(top, #003471, #448CCB);
  background-image: -moz-linear-gradient(top, #003471, #448CCB);
  filter: progid:DXImageTransform.Microsoft.gradient(startColorstr=#00003471,endColorstr=#00448CCB);
  background-image: -ms-linear-gradient(top, #003471, #448CCB);
  background-image: -o-linear-gradient(top, #003471, #448CCB);
  background-image: linear-gradient(to bottom, #003471, #448CCB);
}
.vendor-prefixes {
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
  -moz-box-shadow: 0 3px 20px #000;
  filter: progid:DXImageTransform.Microsoft.Shadow(color=#000000, direction='180', strength='10');
  -ms-filter: "progid:DXImageTransform.Microsoft.Shadow(color=#000000, direction='180', strength='10')";
  -webkit-box-shadow: 0 3px 20px #000;
  box-shadow: 0 3px 20px #000;
}
Order all properties alphabetically.
Group vendor-specific with their respective standard CSS3 properties.
Order vendor-specific properties alphabetically.
Declare vendor-specific properties before standard properties.
Declare potentially unsupported values after known to be supported values (progressive enhancement). (see CSS spec)
The special case of filter: should be grouped with ms-filter: and ordered before it (as in above example).

Vendor specific extensions may cause browser inconsistencies when declaring CSS3 before the standard has reached recommended status (a.k.a. finalized). Do not take for granted that a -vendor-xyz-property is and works the same as the final xyz-property.

It is recommended to avoid features of proprietary vendor extensions that are not supported by CSS3 standards.

Comments
In general, all comments should follow Drupal's common Doxygen formatting conventions to stay consistent with the rest of Drupal's code base. In areas, where those common conventions cannot be applied, we resort to the CSSDoc syntax (draft).

In line with PHP coding standards, block level documentation should be used as follows, to describe a section of code below the comment.

/**
 * Documentation here.
 */
Further details on the common comment syntax may be found in the Doxygen formatting conventions.

Shorter inline comments may be added after a property, preceded with a space:

  background-position: 0.2em 0.2em; /* LTR */
  padding-left: 2em; /* LTR */
Right-To-Left
Drupal supports conditional loading of CSS files with specific override rules for right-to-left languages. For a module, the override rule should be defined in a file named <module>-rtl.css (e.g., node-rtl.css). For a theme, the override rule should be defined in a file named style-rtl.css. The rule that is overridden should be commented in the default CSS rule.

From node-rtl.css:

#node-admin-buttons {
  clear: left;
  float: right;
  margin-left: 0;
  margin-right: 0.5em;
}
Rules in node.css which will be overridden if the rtl.css file is loaded:

#node-admin-buttons {
  clear: right; /* LTR */
  float: left; /* LTR */
  margin-left: 0.5em; /* LTR */
}
See also: http://drupal.org/node/132442#language-rtl

As a rule of thumb, add a /* LTR */ comment in your style:

when you use the keywords, 'left' or 'right' in a property, e.g. float: left;
where you use unequal margin, padding or borders on the sides of a box, e.g.,
margin-left: 1em;
margin-right: 2em;
where you specify the direction of the language, e.g. direction: ltr;
‹ Drupal Markup Style GuideupDRAFT - CSS coding standards for Drupal 8 ›
Login or register to post comments
Looking for support? Visit the Drupal.org forums, or join #drupal-support in IRC.

Comments
about alphabetizing properties
Posted by hkirsman on March 14, 2012 at 9:41am
I think grouping properties together by theyr similarity is more logical and intuitive. Consider these examples when you'd want to change change positioning.

/* Grouping properties by they similarity */
.foo {
  /* text  */
  color: red;
  font-size: 10px;
  font-weight: bold;
  /* Box model *
  position: absolute;
  bottom: 0;
}

/* Alphabetical ordering */
.foo {
  bottom: 0;
  color: red;
  font-size: 10px;
  font-weight: bold;
  position: absolute;
}
I think it's better to have position and bottom next to each rather putting one as the first property and other as the last.

It's like going to bookshop where books are categorized first by their topic and then by theyr name.

So lets start creating rules for this kind of grouping?

More read here:
http://www.impressivewebs.com/css-property-ordering/

