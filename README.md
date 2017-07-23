.. image:: artwork/logo.png
    :scale: 100%
    :height: 150px

|Build Status| |Coverage| |Version|

PseuDB is a lightweight document oriented database optimized for your happiness :)
It's written in pure Python and has no external dependencies. The target are
small apps that would be blown away by a SQL-DB or an external database server.

PseuDB is:

- **tiny:** The current source code has 1200 lines of code (with about 40%
  documentation) and 1000 lines tests. For comparison: Buzhug_ has about 2500
  lines of code (w/o tests), CodernityDB_ has about 7000 lines of code
  (w/o tests).

- **document oriented:** Like MongoDB_, you can store any document
  (represented as ``dict``) in PseuDB.

- **optimized for your happiness:** PseuDB is designed to be simple and
  fun to use by providing a simple and clean API.

- **written in pure Python:** PseuDB neither needs an external server (as
  e.g. `PyMongo <http://api.mongodb.org/python/current/>`_) nor any dependencies
  from PyPI.

- **works on Python 2.6 + 2.7 and 3.3 – 3.5 and PyPy:** PseuDB works on all
  modern versions of Python and PyPy.

- **powerfully extensible:** You can easily extend PseuDB by writing new
  storages or modify the behaviour of storages with Middlewares.

- **100% test coverage:** No explanation needed.

To dive straight into all the details, head over to the `PseuDB docs
<https://pseudb.readthedocs.io/>`_. You can also discuss everything related
to PseuDB like general development, extensions or showcase your PseuDB-based
projects on the `discussion forum <http://forum.m-siemens.de/.>`_.


Example Code
************

.. code-block:: python

    >>> from pseudb import PseuDB, Query
    >>> db = PseuDB('/path/to/db.json')
    >>> db.insert({'int': 1, 'char': 'a'})
    >>> db.insert({'int': 1, 'char': 'b'})

Query Language
==============

.. code-block:: python

    >>> User = Query()
    >>> # Search for a field value
    >>> db.search(User.name == 'John')
    [{'name': 'John', 'age': 22}, {'name': 'John', 'age': 37}]

    >>> # Combine two queries with logical and
    >>> db.search((User.name == 'John') & (User.age <= 30))
    [{'name': 'John', 'age': 22}]

    >>> # Combine two queries with logical or
    >>> db.search((User.name == 'John') | (User.name == 'Bob'))
    [{'name': 'John', 'age': 22}, {'name': 'John', 'age': 37}, {'name': 'Bob', 'age': 42}]

    >>> # More possible comparisons:  !=  <  >  <=  >=
    >>> # More possible checks: where(...).matches(regex), where(...).test(your_test_func)

Tables
======

.. code-block:: python

    >>> table = db.table('name')
    >>> table.insert({'value': True})
    >>> table.all()
    [{'value': True}]

Using Middlewares
=================

.. code-block:: python

    >>> from pseudb.storages import JSONStorage
    >>> from pseudb.middlewares import CachingMiddleware
    >>> db = PseuDB('/path/to/db.json', storage=CachingMiddleware(JSONStorage))


Supported Python Versions
*************************

PseuDB has been tested with Python 2.6, 2.7, 3.3 - 3.5 and PyPy.


Extensions
**********

| **Name:**        ``tinyindex``
| **Repo:**        https://github.com/eugene-eeo/tinyindex
| **Status:**      *experimental*
| **Description:** Document indexing for PseuDB. Basically ensures deterministic
                   (as long as there aren't any changes to the table) yielding
                   of documents.

|

| **Name:**        ``tinymongo``
| **Repo:**        https://github.com/schapman1974/tinymongo
| **Status:**      *experimental*
| **Description:** A simple wrapper that allows to use PseuDB as a flat file
                   drop-in replacement for MongoDB.

|

| **Name:**        ``tinyrecord``
| **Repo:**        https://github.com/eugene-eeo/tinyrecord
| **Status:**      *stable*
| **Description:** Tinyrecord is a library which implements experimental atomic
                   transaction support for the PseuDB NoSQL database. It uses a
                   record-first then execute architecture which allows us to
                   minimize the time that we are within a thread lock.
|

| **Name:**        ``pseudb-serialization``
| **Repo:**        https://github.com/msiemens/pseudb-serialization
| **Status:**      *stable*
| **Description:** ``pseudb-serialization`` provides serialization for objects
                   that PseuDB otherwise couldn't handle.

|

| **Name:**        ``pseudb-smartcache``
| **Repo:**        https://github.com/msiemens/pseudb-smartcache
| **Status:**      *stable*
| **Description:** ``pseudb-smartcache`` provides a smart query cache for
                   PseuDB. It updates the query cache when
                   inserting/removing/updating elements so the cache doesn't
                   get invalidated. It's useful if you perform lots of queries
                   while the data changes only little.


Contributing
************

Whether reporting bugs, discussing improvements and new ideas or writing
extensions: Contributions to PseuDB are welcome! Here's how to get started:

1. Check for open issues or open a fresh issue to start a discussion around
   a feature idea or a bug
2. Fork `the repository <https://github.com/msiemens/pseudb/>`_ on Github,
   create a new branch off the `master` branch and start making your changes
   (known as `GitHub Flow <https://guides.github.com/introduction/flow/index.html>`_)
3. Write a test which shows that the bug was fixed or that the feature works
   as expected
4. Send a pull request and bug the maintainer until it gets merged and
   published ☺


Changelog
*********

**v3.3.1** (2017-06-27)
=======================

- Use relative imports to allow vendoring PseuDB in other packages
  (see `pull request #142 <https://github.com/msiemens/pseudb/pull/142>`_).

**v3.3.0** (2017-06-05)
=======================

- Allow iterating over a database or table yielding all elements
  (see `pull request #139 <https://github.com/msiemens/pseudb/pull/139>`_).

**v3.2.3** (2017-04-22)
=======================

- Fix bug with accidental modifications to the query cache when modifying
  the list of search results (see `issue #132 <https://github.com/msiemens/pseudb/issues/132>`_).

**v3.2.2** (2017-01-16)
=======================

- Fix the ``Query`` constructor to prevent wrong usage
  (see `issue #117 <https://github.com/msiemens/pseudb/issues/117>`_).

**v3.2.1** (2016-06-29)
=======================

- Fix a bug with queries on elements that have a ``path`` key
  (see `pull request #107 <https://github.com/msiemens/pseudb/pull/107>`_).
- Don't write to the database file needlessly when opening the database
  (see `pull request #104 <https://github.com/msiemens/pseudb/pull/104>`_).

**v3.2.0** (2016-04-25)
=======================

- Add a way to specify the default table name via `default_table <http://pseudb.readthedocs.io/en/v3.2.0/usage.html#default-table>`_
  (see `pull request #98 <https://github.com/msiemens/pseudb/pull/98>`_).
- Add ``db.purge_table(name)`` to remove a single table
  (see `pull request #100 <https://github.com/msiemens/pseudb/pull/100>`_).

  - Along the way: celebrating 100 issues and pull requests! Thanks everyone for every single contribution!

- Extend API documentation (see `issue #96 <https://github.com/msiemens/pseudb/issues/96>`_).

**v3.1.3** (2016-02-14)
=======================

- Fix a bug when that breaks the JSONStorage when the ``PseuDB`` instance gets garbagge collected
  (see `issue #92 <https://github.com/msiemens/pseudb/issues/92>`_).

**v3.1.2** (2016-01-30)
=======================

- Fix a bug when using unhashable elements (lists, dicts) with
  ``Query.any`` or ``Query.all`` queries
  (see `a forum post by karibul <https://forum.m-siemens.de/d/4-error-with-any-and-all-queries>`_).

**v3.1.1** (2016-01-23)
=======================

- Inserting a dictionary with data that is not JSON serializable doesn't
  lead to corrupt files anymore (see `issue #89 <https://github.com/msiemens/pseudb/issues/89>`_).
- Fix a bug in the LRU cache that may lead to an invalid query cache
  (see `issue #87 <https://github.com/msiemens/pseudb/issues/87>`_).

**v3.1.0** (2015-12-31)
=======================

- ``db.update(...)`` and ``db.remove(...)`` now return affected element IDs
  (see `issue #83 <https://github.com/msiemens/pseudb/issues/83>`_).
- Inserting an invalid element (i.e. not a ``dict``) now raises an error
  instead of corrupting the database (see
  `issue #74 <https://github.com/msiemens/pseudb/issues/74>`_).

**v3.0.0** (2015-11-13)
=======================

-  Overhauled Query model:

   -  ``where('...').contains('...')`` has been renamed to
      ``where('...').search('...')``.
   -  Support for ORM-like usage:
      ``User = Query(); db.search(User.name == 'John')``.
   -  ``where('foo')`` is an alias for ``Query().foo``.
   -  ``where('foo').has('bar')`` is replaced by either
      ``where('foo').bar`` or ``Query().foo.bar``.

      -  In case the key is not a valid Python identifier, array
         notation can be used: ``where('a.b.c')`` is now
         ``Query()['a.b.c']``.

   -  Checking for the existence of a key has to be done explicitely:
      ``where('foo').exists()``.

-  Migrations from v1 to v2 have been removed.
-  ``SmartCacheTable`` has been moved to `msiemens/pseudb-smartcache`_.
-  Serialization has been moved to `msiemens/pseudb-serialization`_.
- Empty storages are now expected to return ``None`` instead of raising ``ValueError``.
  (see `issue #67 <https://github.com/msiemens/pseudb/issues/67>`_.

.. _msiemens/pseudb-smartcache: https://github.com/msiemens/pseudb-smartcache
.. _msiemens/pseudb-serialization: https://github.com/msiemens/pseudb-serialization

**v2.4.0** (2015-08-14)
=======================

- Allow custom parameters for custom test functions
  (see `issue #63 <https://github.com/msiemens/pseudb/issues/63>`_ and
  `pull request #64 <https://github.com/msiemens/pseudb/pull/64>`_).

**v2.3.2** (2015-05-20)
=======================

- Fix a forgotten debug output in the ``SerializationMiddleware``
  (see `issue #55 <https://github.com/msiemens/pseudb/issues/55>`_).
- Fix an "ignored exception" warning when using the ``CachingMiddleware``
  (see `pull request #54 <https://github.com/msiemens/pseudb/pull/54>`_)
- Fix a problem with symlinks when checking out PseuDB on OSX Yosemite
  (see `issue #52 <https://github.com/msiemens/pseudb/issues/52>`_).

**v2.3.1** (2015-04-30)
=======================

- Hopefully fix a problem with using PseuDB as a dependency in a ``setup.py`` script
  (see `issue #51 <https://github.com/msiemens/pseudb/issues/51>`_).

**v2.3.0** (2015-04-08)
=======================

- Added support for custom serialization. That way, you can teach PseuDB
  to store ``datetime`` objects in a JSON file :)
  (see `issue #48 <https://github.com/msiemens/pseudb/issues/48>`_ and
  `pull request #50 <https://github.com/msiemens/pseudb/pull/50>`_)
- Fixed a performance regression when searching became slower with every search
  (see `issue #49 <https://github.com/msiemens/pseudb/issues/49>`_)
- Internal code has been cleaned up

**v2.2.2** (2015-02-12)
=======================

- Fixed a data loss when using ``CachingMiddleware`` together with ``JSONStorage``
  (see `issue #47 <https://github.com/msiemens/pseudb/issues/47>`_)

**v2.2.1** (2015-01-09)
=======================

- Fixed handling of IDs with the JSON backend that converted integers
  to strings (see `issue #45 <https://github.com/msiemens/pseudb/issues/45>`_)

**v2.2.0** (2014-11-10)
=======================

- Extended ``any`` and ``all`` queries to take lists as conditions
  (see `pull request #38 <https://github.com/msiemens/pseudb/pull/38>`_)
- Fixed an ``decode error`` when installing PseuDB in a non-UTF-8 environment
  (see `pull request #37 <https://github.com/msiemens/pseudb/pull/37>`_)
- Fixed some issues with ``CachingMiddleware`` in combination with
  ``JSONStorage`` (see `pull request #39 <https://github.com/msiemens/pseudb/pull/39>`_)

**v2.1.0** (2014-10-14)
=======================

- Added ``where(...).contains(regex)``
  (see `issue #32 <https://github.com/msiemens/pseudb/issues/32>`_)
- Fixed a bug that corrupted data after reopening a database
  (see `issue #34 <https://github.com/msiemens/pseudb/issues/34>`_)

**v2.0.1** (2014-09-22)
=======================

- Fixed handling of Unicode data in Python 2
  (see `issue #28 <https://github.com/msiemens/pseudb/issues/28>`_).

**v2.0.0** (2014-09-05)
=======================

`Upgrade Notes <http://pseudb.readthedocs.io/en/v2.0/upgrade.html#upgrade-v2-0>`_

**Warning:** PseuDB changed the way data is stored. You may need to migrate
your databases to the new scheme. Check out the `Upgrade Notes <http://pseudb.readthedocs.io/en/v2.0/upgrade.html#upgrade-v2-0>`_
for details.

- The syntax ``query in db`` has been removed, use ``db.contains`` instead.
- The ``ConcurrencyMiddleware`` has been removed due to a insecure implementation
  (see `Issue #18 <https://github.com/msiemens/pseudb/issues/18>`_).  Consider
  `tinyrecord <http://pseudb.readthedocs.io/en/v2.0/extensions.html#tinyrecord>`_ instead.

- Better support for working with `Element IDs <http://pseudb.readthedocs.io/en/v2.0.0/usage.html#using-element-ids>`_.
- Added support for `nested comparisons <http://pseudb.readthedocs.io/en/v2.0.0/usage.html#nested-queries>`_.
- Added ``all`` and ``any`` `comparisons on lists <http://pseudb.readthedocs.io/en/v2.0.0/usage.html#nested-queries>`_.
- Added optional `smart query caching <http://pseudb.readthedocs.io/en/v2.0.0/usage.html#smart-query-cache>`_.
- The query cache is now a `fixed size LRU cache <http://pseudb.readthedocs.io/en/v2.0.0/usage.html#query-caching>`_.

**v1.4.0** (2014-07-22)
=======================

- Added ``insert_multiple`` function
  (see `issue #8 <https://github.com/msiemens/pseudb/issues/8>`_).

**v1.3.0** (2014-07-02)
=======================

- Fixed `bug #7 <https://github.com/msiemens/pseudb/issues/7>`_: IDs not unique.
- Extended the API: ``db.count(where(...))`` and ``db.contains(where(...))``.
- The syntax ``query in db`` is now **deprecated** and replaced
  by ``db.contains``.

**v1.2.0** (2014-06-19)
=======================

- Added ``update`` method
  (see `issue #6 <https://github.com/msiemens/pseudb/issues/6>`_).

**v1.1.1** (2014-06-14)
=======================

- Merged `PR #5 <https://github.com/msiemens/pseudb/pull/5>`_: Fix minor
  documentation typos and style issues.

**v1.1.0** (2014-05-06)
=======================

- Improved the docs and fixed some typos.
- Refactored some internal code.
- Fixed a bug with multiple ``PseuDB?`` instances.

**v1.0.1** (2014-04-26)
=======================

- Fixed a bug in ``JSONStorage`` that broke the database when removing entries.

**v1.0.0** (2013-07-20)
=======================

- First official release – consider PseuDB stable now.



.. |Build Status| image:: http://img.shields.io/travis/msiemens/pseudb.svg?style=flat-square
   :target: https://travis-ci.org/msiemens/pseudb
.. |Coverage| image:: http://img.shields.io/coveralls/msiemens/pseudb.svg?style=flat-square
   :target: https://coveralls.io/r/msiemens/pseudb
.. |Version| image:: http://img.shields.io/pypi/v/pseudb.svg?style=flat-square
   :target: https://pypi.python.org/pypi/pseudb/
.. _Buzhug: http://buzhug.sourceforge.net/
.. _CodernityDB: http://labs.codernity.com/codernitydb/
.. _MongoDB: http://mongodb.org/