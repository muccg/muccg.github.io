The Python Package Index (PyPI)
-------------------------------

CCG utility packages can be uploaded to PyPI_. This makes the packages
easier to install with ``pip``. It also makes them available to other
developers on the wider Internet

.. _PyPI: https://pypi.python.org/pypi


Accounts
========

The CCG account for PyPI is called **muccg**.

You should create your own account for uploading. It's easy enough to
do.

To upload a new version, your account needs the maintainer/owner
role. Multiple users can be designated as maintainers of a package. To
edit roles, login to PyPI and visit the relevant package page. There
should be a ``Package: roles`` link if the logged in user has correct
access.


Required Software
=================

All is available in Ubuntu::

    apt-get install twine python-setuptools python3-setuptools python-wheel python3-wheel


Preparing the Release
=====================

Before uploading, you need to make a good source dist tarball and
wheel::

    python setup.py sdist
    python setup.py bdist_wheel

Inspect the results in ``dist/``.

Note there is also a directory created called
``my_package.egg-info/``. The file ``PKG-INFO`` is relevant to our
interests.


Python 3
========

To build the wheel for Python 3, just use ``python3``::

    python3 setup.py bdist_wheel

To build a "universal" wheel for both Python 2 and Python 3, run
this::

    python3 setup.py bdist_wheel --universal

If universal wheels are possible, it's best to make them the
default. Put the following in ``setup.cfg``::

    [bdist_wheel]
    universal=1


Uploading
=========

For uploading to PyPI, apparently Twine is best.


New Packages
~~~~~~~~~~~~

When creating a brand-new package, go to the "Package Submission" page
on PyPI and upload the ``PKG-INFO`` file mentioned earlier. Then it
will be possible to upload the first release.


Upload a Release
~~~~~~~~~~~~~~~~

Add your credentials to ``~/.pypirc`` like so::

    [distutils]
    index-servers=pypi

    [pypi]
    repository = https://pypi.python.org/pypi
    username=muccg
    password=secret

Then run Twine from the project directory::

    twine upload dist/MY-PACKAGE-0.0.0.tar.gz dist/MY_PACKAGE-0.0.0-py2.py3-none-any.whl

If unsure whether the upload worked, compare the checksums claimed by
PyPI with your own::

    md5sum dist/*


Other Considerations
====================

* Don't forget to git tag releases, and push the tag.
* It's always less confusing when the git tag of a release exactly
  matches what's uploaded to PyPI.
* Check the ``classifiers`` and ``keywords`` in ``setup.py`` to see
  if they are approximately correct.
* Check the software license is correct, or at least all mentions of
  licence say the same thing.
* This website has badge graphics for README files:
  https://badge.fury.io/for/py


References
==========

At present, the least outdated document about Python packaging is:

  https://python-packaging-user-guide.readthedocs.org/en/latest/
