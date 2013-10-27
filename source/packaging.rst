===============
Packaging Guide
===============

Lay out your project
====================

The smallest python project is two files: a :ref:`setup.py
<setup_py_description>` file which describes the metadata about your project,
and a file containing Python code to implement the functionality of your
project.

In this example project we are going to add a little more to the
project to provide the typical minimal layout of a project. We will call our
project TowelStuff and keep it in a directory with the same name.
We'll create a full Python package called
:ref:`towelstuff/ <towelstuff_description>` as a directory with an
`__init__.py` file. This anticipates future growth as our project's source
code is likely to grow beyond a single module file.

We'll also create a :ref:`README.txt <readme_txt_description>`
file giving an overview of your project, and
a :ref:`LICENSE.txt <license_txt_description>` file containing the
license of your project.

That's enough to start. There are a number of other types of files a
project will often have, see the :ref:`directory_layout` for an example of
a more fully fleshed out project.

Your project will now look like::

    TowelStuff/
        LICENSE.txt
        README.txt
        setup.py
        towelstuff/
            __init__.py


Describe your project
=====================

The :ref:`setup.py <setup_py_description>` file is at the heart of a Python
project. It describes all of the metadata about your project. There a quite
a few fields you can add to a project to give it a rich set of
metadata describing the project. However, there are only three required
fields: `name`, `version`, and `packages`. The `name` field must be unique
if you wish to publish your package on the
:ref:`Python Package Index (PyPI) <PyPI>`. The `version` field keeps track of
different releases of the project. The `packages` field describes where you've
put the Python source code within your project. The `classifiers` field lists
various facts about your project, e.g. what versions of Python it is
compatible with.

Our initial `setup.py` will also include information about the license
and will re-use the `README.txt` file for the `long_description` field.
This will look like::

    from setuptools import setup

    with open('README.txt') as file:
        long_description = file.read()

    setup(
        name='TowelStuff',
        version='0.1.dev1',
        packages=['towelstuff',],
        license='Creative Commons Attribution-Noncommercial-Share Alike license',
        long_description=long_description,
        classifiers=[
            'Programming Language :: Python :: 3.3',
        ],
    )


Specifying Dependencies
=======================

Configuring Scripts
===================

Configuring Additional Files
============================

Create your first release
=========================

In the `version` field, we specified `0.1.dev1`. This indicates that we
are *developing* towards the `0.1` version. Right now there isn't any code
in the project. After you've written enough code to make your first release
worthwhile, you will change the version field to just `0.1`, dropping the
`.dev1` marker.

To create a release, your source code needs to be packaged into a single
archive file for distribution. This can be done with the `sdist` command:

 $ python setup.py sdist

This will create a `dist` sub-directory in your project, and will wrap-up
all of your project's source code files into a distribution file,
a compressed archive file in the form of::

    TowelStuff-0.1.tar.gz

The compressed archive format defaults to `.tar.gz` files on POSIX systems,
and `.zip` files on Windows systems.

By default, Distutils does **not** include all files in your project's
directory. Only the following files will be included by default:

 * all Python source files implied by the `py_modules` and `packages` options

 * all C source files mentioned in the `ext_modules` or `libraries` options

 * scripts identified by the `scripts` option

 * anything that looks like a test script: test/test*.py

 * Top level files named: `README.txt`, `README`, `setup.py`, or `setup.cfg`

If you want to include additional files, then there are a couple options
for including those files:

 * Use a package which extends Distutils with more functionality.
   Setuptools allows you to include all files checked into
   your version control system.

 * Write a top-level `MANIFEST.in` file. This is a template file which
   specifies which files (and file patterns) should be included.
   (TO-DO: link to a MANIFEST.in document)

When you run the **sdist** command, a file named `MANIFEST` will be
generated which contains a complete list of all files included. You can
view this file to double check that you've setup your project to include
all files correctly. Now your project is ready for a release. Before
releasing, it's a good idea to double check to make sure that you have:

* The correct version number.

  While it's handy to append a `.devN` marker to the version number during
  development, so that you can distinguish between code under development
  and a released version, you **never** want to publish a release with
  `.devN` in the version name.

* All desired project files are included.

  Go over the MANIFEST file, or open the archive file generated by
  running the **sdist** command.


Register your package
=====================

The distribution file generated by running **sdist** can be published
anywhere. There is a central index of all publically available Python projects
maintained on the python.org web site named the :ref:`pypi_info`. This is
where you will want to release your distribution if you intend to make your
project public.

You will first have to visit that site, where you can register for an account.
Project's are published on PyPI in the format of::

  http://pypi.python.org/pypi/<projectname>

Your project will have to choose a name which is not already taken on PyPI.
You can then claim your new project's name by registering the package
by running the command::

$ python setup.py register


Upload your release
===================

Now that you are happy that you can create a valid source distribution,
it's time to upload the finished product to PyPI. We'll also create a
`bdist_wininst` distribution file of our project, which will create a Windows
installable file. There are a few different file formats that Python
distributions can be created for. Each format must be specified when
you run the **upload** command, or they won't be uploaded (even if you've
previously built them previously). You should always upload a
source distribution file. The other formats are optional, and will depend upon
the needs of your anticipated user base::

 $ python setup.py sdist bdist_wininst upload

At this point you should announce your package to the community!

Finally, in your `setup.py` you can make plans for your next release,
by changing the `version` field to indicate which version you want to work
towards next (e.g. `0.2.dev1`).

Where to get more details
=========================
