.. _doc_rest:

===============================================================================
reStructuredText
===============================================================================

For comprehensiveness, the content in this document is largely drawn directly
from the `reStructuredText technical specification`__. A great overview to
reStructuredText can be found in this `reST + Sphinx syntax article`__ as well.
A cheatsheet for reStructuredText is also available `here`__.
Additionally, this
`article <https://docness.readthedocs.io/en/latest/documentation-usage.html>`_
also covers relatively insightful points regarding the design of documentation.

__ https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html
__ https://thomas-cokelaer.info/tutorials/sphinx/rest_syntax.html
__ https://docutils.sourceforge.io/docs/user/rst/quickref.html

This document is intended to work as a primer to the RST system,
to both cover the fundamentals and as well as recommended styling guidelines
for this project.



Introduction
============

reStructuredText (RST) is a markup system, which borrows Python's
indentation rules.

.. note::

    Alternatives include Markdown which is a loosely grouped set of a
    collection of different standards. Read this
    `blogpost <https://www.ericholscher.com/blog/2016/mar/15/dont-use-
    markdown-for-technical-docs/>`_
    by Eric Holscher for an argument against Markdown.



Structure
=========

All elements in reStructuredText are generally demarcated by at one least one
blank line. Consecutive lines are grouped together.

.. code-block:: rest

    This is the first paragraph
    continued in the second line.

    This is the second paragraph.

Indentation is semantically meaningful to nest elements within another.

.. code-block:: rest

    This is a paragraph.

    .. tip::

        This paragraph is inside the ``tip`` admonition.
        The following code block is nested inside ``tip``.

        .. code-block:: python

            "This is a Python string"

        This paragraph is still nested inside ``tip``.

    This paragraph is no longer part of ``tip``.

        This paragraph is also not part of ``tip``.

Indentation can be of any amount - even within the same document! - as long as
elements sharing the same nesting (block scope) have the same indentation
height.

.. tip::

    Keep lines within the document to within a length of 79.
    This is inherited from the Python styleguide found in
    `PEP8 <https://www.python.org/dev/peps/pep-0008/#maximum-line-length>`_.

    Indentations should be consistently kept to 4 spaces, as with in Python.

Section headers
---------------

A section header is a single line of text with an adornment, either
an underline alone, or both an overline and underline of equal length. The
underline must be at least as long as the title text.

.. code-block:: rest

    Title
    =====

    -------
    Chapter
    -------

    ^^^^^^^^^^^
      Section
    ^^^^^^^^^^^

    Subsection
    :::::::::::::

Any of the following characters
``! " # $ % & ' ( ) * + , - . / : ; < = > ? @ [ \ ] ^ _ ` { | } ~`` can be used
as the adornment character. There is no grouping between the corresponding
overline and underline styled headers.
Overline headers can additionally be inset, as with in the example above for
``Section``.

.. tip::

    The following convention for section headers is adhered to:

    * Heading 1 (Title): ``=`` with overline of length 79
    * Heading 2 (Chapters): ``=`` underline only
    * Heading 3 (Section): ``-`` underline only
    * Heading 4 (Subsection): ``~`` underline only
    * Heading 5 (Subsubsection): ``"`` underline only

    If more than 5 headers are required, create a new document instead.

Transitions
-----------

Similar to HTML horizontal rules, transitions introduce extra space between
text divisions to signal change in subject or emphasis. These are achieved
using a horizontal line of at least 4 non-alphanumeric printable ASCII
characters between two blank lines.

.. code-block::

    This is before the transition.

    ----------------

    This is after the transition.



Formatting
==========

Text can have inline markup, such as
*italics* using ``*italics*``,
**bold** using ``**bold**``,
``monospaced`` using ````monospaced````.

There is generally no need to escape the delimiters in regular text,
except in cases where it will be interpreted as markup. To escape the
delimiters, use a backslash ``\`` before the character. In the following
example, none of the characters are marked up.

.. code-block::

    The result of 5 * 6, otherwise written as 5*6,
    is.. \*drumroll intensifies\* 30!

.. note::

    The inline markup recognition rules for reStructuredText are
    comprehensively listed in the
    `reStructuredText markup technical specification <https://docutils.
    sourceforge.io/docs/ref/rst/restructuredtext.html#inline-markup
    -recognition-rules>`_.


Paragraphs
----------

The most basic pattern recognized is a paragraph, which is a group of
consecutive left-aligned text.

.. code-block:: rest

    Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod
    tempor incididunt ut labore et dolore magna aliqua.


Paragraphs can be indented to nest it as a block quote.

.. code-block:: rest

    This is a regular paragraph.

        This is a nested paragraph.

            Paragraphs can be nested as well.

Bullet and Numbered Lists
-------------------------

A bullet list is a text block that begins with one of ``* + - • ‣ ⁃`` followed
by a whitespace. List items must be left-aligned and indented relative to
the bullet.

For an enumerated list, the starting character should belong to one of the
following supported enumeration sequences:

- Arabic numerals: ``1 2 3 ...`` with no upper limit
- Uppercase alphabets: ``A B C ... Z``
- Lowercase alphabets: ``a b c ... z``
- Uppercase Roman numerals: ``I II III IV ... MMMMCMXCIX`` (4999)
- Lowercase Roman numerals: ``i ii iii iv ... mmmmcmxcix`` (4999)
- Auto-enumeration: ``#`` (defaults to Arabic if no other sequence specified)

The following formatting types are also recognized:

- Period suffix, e.g. ``1. a. I.``
- Parenthesis suffix, e.g. ``1) a) I)``
- Wrapped parentheses, e.g. ``(1) (a) (I)``

List enumeration must be consecutive and of the same sequence type to be
considered as part of the same list, otherwise a new list will be started.
Lists can begin enumeration from any value, though note that
``I i`` is treated as a Roman numeral sequence while ``V X L C D M`` are
treated as an alphabet sequence instead.

.. code-block:: rest

    This is a paragraph.

    A. Einstein states
    that this is also a paragraph.

    - This is the first bullet list item.
      Paragraphs must be indented. Blank line required before first point and
      after last point, but optional between list items.

      This is the second paragraph in the first bullet list item.
    - This is the second bullet list item.

      1. This is the first enumerated sublist item.
         Bullet line must line up with the left edge of the text blocks above.
      #. This is the second enumerated sublist item, with auto-increment.

      5. This is a new sublist.
    - This is the third bullet list item.

    This paragraph is not part of the list.

Definition Lists
----------------

A definition list contains a list of terms with their corresponding definitions
and optional classifiers. Classifiers follow the term on the same line, and
are delimited by `` : `` (space, colon, space).

There must be no blank line between the term and definition blocks (to
distinguish it from block quotes).
As with the other lists, a blank line is optional between list items, but
required before the first and after the last definition list item.

.. code-block:: rest

    taper_length
        Length of the taper

    angle : int : float
        Subtending angle of the focused grating coupler.

        Restricted to the interval [0, 360)

    n : int, optional
        The number of gratings of the grating coupler,
        defaults to 50.

Field Lists
-----------

Field lists are used as directive options or as a two-column table-like
structure to represent label-data pairs. Parsers for reStructuredText may
recognized specialized field names in certain contexts.

.. code-block:: rest

    :Date: 2020-08-02
    :Version: 1
    :Parameter\: i: int

When a field list is the first non-comment element after the document title,
it may have its fields transformed into document bibliographic data.
The registered fields include: ``Abstract``, ``Address``, ``Author``,
``Authors``, ``Contact``, ``Copyright``, ``Date``, ``Dedication``,
``Organization``, ``Revision``, ``Status``, ``Version``.

Literal Blocks
--------------

A literal block is composed of a paragraph containing two colons, to present
subsequent indented or quoted text that is left as-is and typically rendered
in monospaced font.
For quoted text, the text block is not indented, but instead each line begins
with the same non-alphanumeric printable ASCII character (same list of
characters as that of title adornments).

For convenience, the ``::`` is recognized at the end of any paragraph, and is
truncated by one colon if immediately preceded by text, both colons if preceded
by whitespace.
In the following example, all of them are equivalent:

.. code-block:: rest

    Paragraph:

    ::

        Literal block (expanded form)

    Paragraph: ::

        Literal block (partially minimized form)

    Paragraph::

        Literal block (fully minimized form)

    Paragraph::

    > Literal block (quoted literal)

Doctest blocks
--------------

Doctest blocks are interactive Python sessions cut-and-pasted into docstrings,
meant to illustrate usage by example and provide a testing environment using
the `doctest <https://docs.python.org/3/library/doctest.html>`_ Python library.

.. code-block:: rest

    This is a paragraph.

    >>> print("This is a Doctest block")
    This is a Doctest block

.. tip::

    As with in `numpydocs <https://numpydoc.readthedocs.io/en/latest/
    format.html#sections>`_,
    testing is performed using an external testing framework (``pytest`` in
    this project). Doctest comments are intended to illustrate usage only.


Tables
------

reStructuredText provides two methods of defining a table:

* Grid tables provide both row and column span, but are cumbersome to complete
* Simple tables are easy to create, but has no row span, and require
  the first column to be filled (use ``..`` for empty cells)

Markup is available within cells in both tables.
These examples are intuitive to understand:

.. code-block::

    +------------------------+------------+----------+----------+
    | Header row, column 1   | Header 2   | Header 3 | Header 4 |
    | (header rows optional) |            |          |          |
    +========================+============+==========+==========+
    | body row 1, column 1   | column 2   | column 3 | column 4 |
    +------------------------+------------+----------+----------+
    | body row 2             | Cells may span columns.          |
    +------------------------+------------+---------------------+
    | body row 3             | Cells may  | - Table cells       |
    +------------------------+ span rows. | - contain           |
    | body row 4             |            | - body elements.    |
    +------------------------+------------+---------------------+

    =====  =====  ======
       Inputs     Output
    ------------  ------
      A      B    A or B
    =====  =====  ======
    False  False  False
    True   False  True
    False  True   True
    True   True   True
    =====  =====  ======



Explicit Markup
===============

To extend the limited functionality of inline markup, markup can be explicitly
specified following the syntax rules:

1. First line begins with two periods and a space (the markup start)
2. Second and/or subsequent lines are indented relative to the first
3. Ends before an unindented line.

.. code-block:: rest

    .. name
        body elements

        more body elements

Explicit markups are used for footnotes, citations, hyperlinks, directives,
substitution definitions, comments.

Footnotes
---------

Each footnote is an explicit markup denoted by a label wrapped in square
brackets. The label can be listed in one of four ways:

1. An integer (explicitly numbered footnote)
2. A single ``#`` (auto-numbered footnote)
3. A ``#`` followed by a simple reference name (an autonumber label)
4. A single ``*`` (auto-symbol footnote)

The auto-numbering behavior is similar to that in enumerated lists.
Footnotes with an autonumber label can be referred to more than once, either as
a footnote reference or hyperlink reference.

.. code-block:: rest

    The footnote reference [#note]_ will be displayed as "[1]" in the
    footnotes. This footnote can be referred to again as a footnote hyperlink
    [#note]_ or as an internal hyperlink note_.

    .. [#note] Footnote content

Auto-symbol footnotes use the following sequence of 10 symbols
``* † ‡ § ¶ # ♠ ♥ ♦ ♣`` for footnote marks instead of numbers. If more than 10
symbols are required, the same sequence will be reused but doubled (``**``),
and then tripled, and so on.

.. warning::

    Avoid mixing manual and auto-numbered footnotes in the same document. See
    the `reStructuredText technical specification <https://docutils.
    sourceforge.io/docs/ref/rst/restructuredtext.html#mixed-manual-and
    -auto-numbered-footnotes>`_ for more details.

    In a similar vein, avoid nesting auto-numbered footnotes within footnotes.

.. tip::

    The sole use of autonumber-labeled footnotes is recommended for this
    project, by virtue of its ability to be referenced multiple times.


Citations
---------

Citations are identical to footnotes, except that they use only non-numeric
labels that are case-insensitive simple reference names. Citations may be
rendered separately from footnotes.

.. code-block:: rest

    Quoting Nielsen & Chuang [NC2000]_, ...

    .. [NC2001] Citation body.

Hyperlinks
----------

Hyperlink targets identify a location within or outside of a document, which
may be linked to by hyperlink references. Hyperlinks will always point to the
succeeding or containing element, be it a paragraph element, URL, or another
hyperlink. Hyperlinks can have fallthroughs.

There exist hyperlink targets that are implicitly defined as well, from
section headers, footnotes and citations. Explicitly defined hyperlink targets
override implicit targets.

.. code-block:: rest

    This is a hyperlink target_, and so is `this
    hyperlink`_, both of which point to the `bottom`_ of the example. Broken
    hyperlinks are joined with a space, while broken URIs are concatenated.

    .. _bottom:
        `this hyperlink`_

    This hyperlink `points to`_ an inline hyperlink target, while this
    hyperlink points to the section header Links_.

    .. _this:
    .. _URI:
        http://docutils.sourceforge
        .net/some_resource\_

    In contrast, this_ hyperlink _`points to` a URI_ broken into multiple
    lines. The trailing underscore in the URI is escaped to avoid
    being treated as another hyperlink.

    Links
    =====

    .. _`target`:
    .. _this hyperlink:

    This is the explicit document hyperlink target.

Inline hyperlink targets are generally preferred if the link is to an optional
resource that need not be listed in the list of document reference links.
Listing of such resources can be alternatively achieved using anonymous
hyperlinks.

.. code-block::

    I am an `inline hyperlink <https://docutils.sourceforge.io
    /docs/ref/rst/restructuredtext.html#anonymous-hyperlinks>`_ which is not
    separately referenced in the document.

    In contrast, this is an `anonymous hyperlink`__.

    __ https://www.google.com/

.. tip::

    There are many variations to the type of hyperlinks accepted by
    reStructuredText. To standardize links within the project,
    all phrase-references (containing more than one word) should be enclosed
    within backquotes, to avoid potential pitfalls with colons and spaces.

Directives
----------

Directives serve as an extension mechanism for reStructuredText. They are
indicated by an explicit markup start, following by the directive type,
two colons and whitespace.

Directive arguments may be supplied on the same line as the directive type,
while directive options, if available, are specified as `field lists`_ in the
following line. Directive content begin after a blank line if arguments and/or
options are employed by the directive.

.. code-block:: rest

    This is the image directive with a single argument.

    .. image:: mylogo.jpg

    This directive takes in only a content block.

    .. note::
        Be aware of your surroundings.

    This directive contains an argument, `scale` option, and content caption.

    .. figure:: larch.png
        :scale: 50

        The larch.

A list of the standard directives may be found `here <https://docutils.
sourceforge.io/docs/ref/rst/directives.html>`_.

Admonitions
~~~~~~~~~~~

Admonitions are specially marked topics that are rendered as an offset block
to place focus on the content. Several admonitions have been implemented as
part of the standard, a list of which can be found in this
`reference <https://docutils.sourceforge.io/docs/ref/rst/directives.html
#admonitions>`_.

.. tip::
    Strictly only five admonitions will be used in this project, including:

    - ``tip`` highlights best practices
    - ``note`` provides context or supplementary information
    - ``warning`` describes common pitfalls and/or errors
    - ``deprecated`` warns of deprecated features
    - ``todo`` suggests future design improvements

Substitution References
-----------------------

Substitution definitions have an embedded inline-compatible directive, which
can be referenced inline within text. Some use cases include:

1. Referencing unique objects to disambiguate text
2. Referencing an image
3. Referencing an externally defined presentation style
4. Macro substitution

.. code-block::

    |Michael| and |Jon| are our widget-wranglers.

    .. |Michael| user:: mjones
    .. |Jon|     user:: jhl

    West led the |H| 3, covered by dummy's |H| Q, East's |H| K,
    and trumped in hand with the |S| 2.

    .. |H| image:: /images/heart.png
       :height: 11
       :width: 11
    .. |S| image:: /images/spade.png
       :height: 11
       :width: 11

    Even |the text in Texas| is big.

    .. |the text in Texas| style:: big

    |RST|_ is a little annoying to type over and over, especially
    when writing about |RST| itself, and spelling out the
    bicapitalized word |RST| every time isn't really necessary for
    |RST| source readability.

    .. |RST| replace:: reStructuredText
    .. _RST: http://docutils.sourceforge.net/rst.html

Comments
--------

Arbitrary indented text following the explicit markup start will be processed
as a comment element. An empty comment element serves to terminate a preceding
construct, e.g. have a block quote follow a list or any indented construct.

.. code-block:: rest

    10. This is an enumerated list

    ..

        This is a block quote.

    ..
        This is a comment.


References
==========

.. target-notes::
