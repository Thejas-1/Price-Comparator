Introduction
============

|Build Status| |Latest Version| |Supported Python versions| |Development
Status| |Wheel Status| |Download format| |License|

RedBaron is a python library and tool powerful enough to be used into
IPython solely that intent to make the process of **writing code that
modify source code** as easy and as simple as possible. That include
writing custom refactoring, generic refactoring, tools, IDE or directly
modifying you source code into IPython with an higher and more powerful
abstraction than the advanced texts modification tools that you find in
advanced text editors and IDE.

RedBaron guaranteed you that **it will only modify your code where you
ask him to**. To achieve this, it is based on
`Baron <https://github.com/PyCQA/baron>`__ a lossless
`AST <https://en.wikipedia.org/wiki/Abstract_syntax_tree>`__ for Python
that guarantees the operation ast\_to\_code(code\_to\_ast(source\_code))
== source\_code. (Baron's AST is called a FST, a Full Syntax Tree).

RedBaron API and feel is heavily inspired by BeautifulSoup. It tries to
be simple and intuitive and that once you've get the basics principles,
you are good without reading the doc for 80% of your operations.

**For now, RedBaron can be considered in alpha, the core is quite stable
but it is not battle tested yet and is still a bit rough.** Feedback and
contribution are very welcome.

The public documented API on the other side is guaranteed to be
retro-compatible and won't break until 2.0 (if breaking is needed at
that point). There might be the only exception that if you directly call
specific nodes constructors with FST that this API change, but this is
not documented and simply horribly unpracticable, so I'm expecting no
one to do that.

**Disclamer**: RedBaron (and baron) is **working** with python3 but it
NOT fully parsing it yet.

Installation
============

::

    pip install redbaron[pygments]

Or if you don't want to have syntax highlight in your shell or don't
need it:

::

    pip install redbaron

Running tests
=============

::

    pip install pytest
    py.test tests

Community
=========

You can reach us on
`irc.freenode.net#baron <https://webchat.freenode.net/?channels=%23baron>`__
or
`irc.freenode.net##python-code-quality <https://webchat.freenode.net/?channels=%23%23python-code-quality>`__.

Code of Conduct
===============

As a member of `PyCQA <https://github.com/PyCQA>`__, RedBaron follows
its `Code of
Conduct <http://meta.pycqa.org/en/latest/code-of-conduct.html>`__.

Links
=====

**RedBaron is fully documented, be sure to check the turorial and
documentation**.

-  `Tutorial <https://redbaron.pycqa.org/en/latest/tuto.html>`__
-  `Documentation <https://redbaron.pycqa.org>`__
-  `Baron <https://github.com/PyCQA/baron>`__
-  IRC chat:
   `irc.freenode.net#baron <https://webchat.freenode.net/?channels=%23baron>`__

.. |Build Status| image:: https://travis-ci.org/PyCQA/redbaron.svg?branch=master
   :target: https://travis-ci.org/PyCQA/redbaron
.. |Latest Version| image:: https://pypip.in/version/redbaron/badge.svg
   :target: https://pypi.python.org/pypi/redbaron/
.. |Supported Python versions| image:: https://pypip.in/py_versions/redbaron/badge.svg
   :target: https://pypi.python.org/pypi/redbaron/
.. |Development Status| image:: https://pypip.in/status/redbaron/badge.svg
   :target: https://pypi.python.org/pypi/redbaron/
.. |Wheel Status| image:: https://pypip.in/wheel/redbaron/badge.svg
   :target: https://pypi.python.org/pypi/redbaron/
.. |Download format| image:: https://pypip.in/format/redbaron/badge.svg
   :target: https://pypi.python.org/pypi/redbaron/
.. |License| image:: https://pypip.in/license/redbaron/badge.svg
   :target: https://pypi.python.org/pypi/redbaron/


Changelog
=========

0.6.2 (2016-10-03)
----------------

- fix some old call to log() weren't lazy, that could cause a crash in some situations by an infinite recursive call and also reduce performances
- fix in _iter_in_rendering_order method to avoid bug in edge cases (issue #107)

0.6.1 (2016-03-28)
----------------

- fix setup.py, package weren't pushed on pypi since splitting of redbaron.py
  into multiple files.

0.6 (2016-03-28)
----------------

This release is guaranteed to have a retro-compatible public documented API
from now on until maybe 2.0.
There might be the only exception that if you directly call specific nodes
constructors with FST that this API change, but this is not documented and
simply horribly unpracticable, so I'm expecting no one to do that.

>From now on the focus will be on moving to a stable 1.0 meaning: bugs fixes and
API additions for missing needed features and no more big modifications, this
will be for other releases, the workload is already big enough.

- BIG improvement on the proxy list merging algorithm, it is not perfect yet (comments aren't handled yet) but it's really a big move forward
- possible retrocompatibility breaking change: from now on the node.find("name") to node.name shortcut ONLY works with possible nodes identifiers. For example node.i_dont_exist_as_an_identifier will raise AttributeError
- new helper method .to_python that wrap ast.literal_eval on compatibile nodes https://redbaron.readthedocs.io/en/latest/other.html#to-python
- breaking: IntNode no longer return an int on .value but a .string instead, use .to_python to have an evaluated version
- fix node.decrease_indentation (that was simply not working)
- fix code_block_node.value was broken on node with no parent
- add string representation for Path object
- now redbaron Path() class can be compared directly to baron paths
  without using to_baron_path() helper.
- fix by novocaine: 'function' was used as a function type detector instead of 'def'
- add getitem() method with same api on NodeList and ProxyList
- fix: inconsistencies when inserting lines around code blocks
- inserting a blank lines inserts effectively a \n in a LineProxyList
- new helper methods: .next_recursive and .previous_recursive https://redbaron.readthedocs.io/en/latest/other.html
- fix: doc is tested in CI now, it shouldn't break anymore
- more rendering test for python3, it shouldn't break anymore
- pygments is now an optional dependancy, "pip install redbaron" won't install it, "pip install redbaron[pygments"] will
- new node.next_intuitive and node.previous_intuitive methods for situations where .next/previous doesn't behave the way the user expect it https://redbaron.readthedocs.io/en/latest/other.html#next-intuitive-previous-intuitive

0.5.1 (2015-03-11)
------------------

- fix whitespace duplication when using .insert()
- DecoratorProxyList of the last method of a function wasn't handling correctly the indentation of its last endl token

0.5 (2015-01-31)
----------------

- fix index handling in get_absolute_bounding_box_of_attribute method in
  a LineProxyList
- pretty rendering of RedBaron repr in ipython notebook using _repr_html_, see:
  https://cloud.githubusercontent.com/assets/41827/5731132/65ff4c92-9b80-11e4-977c-0faebbf63415.png
- fix: RedBaron repr was crashing in bpython and in ipython notebook. The new
  behavior should be way more stable and never crash.
- new helpers .names, .modules, .full_path_modules for from_import node https://redbaron.readthedocs.io/en/latest/other.html#index-on-parent-raw
- add a node.index_on_parent_raw and make node.index_on_parent works has it
  should be intuitivly according to the proxy list api https://redbaron.readthedocs.io/en/latest/other.html#index-on-parent-raw
- new helper methods: .insert_before and .insert_after https://redbaron.readthedocs.io/en/latest/other.html#insert-before-insert-after
- fix: some white space bugs in the merging algorithm of line proxy
- fix: on_attribute and parent were correctly set on newly added elements to
  the root node

0.4 (2014-12-11)
----------------

- compatibility with baron upstream (removal of def_argument_node and
  uniformisation of def_arguments structure)
- fix: long wasn't supported in redbaron (due to a bug in baron)

0.3 (2014-11-12)
----------------

- proxy lists, major improvement in the management of list of things
- .append_value is no more since it is useless now due to proxy lists
- .index has been renamed to .index_on_parent to be more coherent

0.2 (2014-09-23)
----------------

- for EVERY NODES in RedBaron, the automagic behavior when passing a string to
  modify an attribute has been done, this is HUGE improvement
  https://redbaron.readthedocs.io/en/latest/modifying.html#full-documentations
- it's now possible to use regex, globs, list/tuple and lambda (callable) in .find and
  .find_all, see https://redbaron.readthedocs.io/en/latest/querying.html#advanced-querying
- new method on node: .replace() to replace in place a node
  https://redbaron.readthedocs.io/en/latest/other.html#replace
- .map .filter and .apply are now documented https://redbaron.readthedocs.io/en/latest/other.html#map-filter-apply
- .edit() new helper method to launch a text editor on the selected node and
  replace the node with the modified code https://redbaron.readthedocs.io/en/latest/other.html#edit
- .root node attribute (property) that return the root node of the tree in which the
  node is stored https://redbaron.readthedocs.io/en/latest/other.html#root
- .index node attribute (property) that returns the index at which the node is
  store if it's store in a nodelist, None otherwise https://redbaron.readthedocs.io/en/latest/other.html#index
- setitem (a[x] = b) on nodelist now works as expected (accepting string, fst
  node and redbaron node)
- new method to handle indentation: .increase_indentation and .decrease_indentation https://redbaron.readthedocs.io/en/latest/other.html#increase-indentation-and-decrease-indentation
- various small bugfix
- we have one new contributor \o/ https://github.com/ze42
- to_node has been move to a class method of Node: Node.from_fst
- pretty print of nodes when using redbaron in a script

0.1 (2014-06-13)
----------------

- First release


