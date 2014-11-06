
.. toctree::
     :maxdepth: 1

.. _alt_inst_methods:

Alternative installation methods
================================

This page describes the alternative installation methods:

-  `Standard installation of latest release`_
-  `Installation from downloaded sources`_
-  `Installation of latest release from GitHub`_
-  `Installation of latest development version`_

--------------


Standard installation of latest release
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Usually, you just want to install the latest (stable) version of each of
the EasyBuild packages (framework, easyblocks, easyconfigs).

Python provides a couple of ways to do that.
Every version of the EasyBuild packages is released via PyPi.


Installing EasyBuild without admin rights
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you do not have EasyBuild installed yet, or if you just want to
install the most recent version of each of the EasyBuild packages,
you can use one of the following simple commands:

-  using ``easy_install`` (old tool, but still works)::

       easy_install --prefix /tmp easybuild

-  using ``pip`` (more recent and better installation tool for Python software)::

       pip install --prefix /tmp easybuild

The ``--prefix /tmp`` part in these commands allows you to install EasyBuild without admin rights into ``/tmp``.


Adjusting ``$PATH`` and ``$PYTHONPATH`` environment variables
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

After installing EasyBuild with either ``easy_install`` or ``pip``
like this, you will need to update the ``$PATH`` and ``$PYTHONPATH``
environment variable to make sure the system can find the main EasyBuild command ``eb``.
On (most) Linux distributions, the command for doing this is:

.. code:: bash

    export PATH=/tmp/bin:$PATH
    export PYTHONPATH=/tmp/lib/python2.7/site-packages:$PYTHONPATH

.. tip::

  To determine the path that should be added to the ``$PYTHONPATH``
  environment variable for a given installation prefix, you can use the
  following command::

    python -c "import distutils.sysconfig; print distutils.sysconfig.get_python_lib(prefix='/tmp');"


Install with admin rights
^^^^^^^^^^^^^^^^^^^^^^^^^

If you do have admin rights on the system where you want to install
EasyBuild, you can simply omit the ``--prefix /tmp``
to have EasyBuild installed system-wide. In that case, you do not need
to touch the ``$PATH`` or ``$PYTHONPATH`` environment variables since
the ``eb`` command will be installed in one of the default paths.

Alternatives to --prefix
^^^^^^^^^^^^^^^^^^^^^^^^

As an alternative to ``--prefix`` when you do not have admin rights,
you can specify that EasyBuild should be installed in your ``$HOME``
directory using the ``--user`` option.

The full list of commands to install EasyBuild in your ``$HOME``
directory using ``pip`` would be::

    pip install --user easybuild
    export PATH=$HOME/.local/bin:$PATH

.. warning::

  In our experience, using ``--user`` creates more problems than it solves.
  We have run into unexpected behavior with Python software installed in your
  home directory using ``--user``, for example it **always** being preferred over
  versions installed somewhere else. Hence, we strongly discourage using `--user`
  to install EasyBuild (or other Python software).

Installing the EasyBuild packages separately
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Each of the EasyBuild packages can also be installed separetely::

    pip install --prefix /tmp easybuild-framework
    pip install --prefix /tmp easybuild-easyblocks
    pip install --prefix /tmp easybuild-easyconfigs

This is basically the exact same sequence of steps as they will be
performed when running ``pip install --prefix /tmp easybuild``.

Installation from downloaded sources
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To install one of the EasyBuild packages from a downloaded source
tarball, use the following steps::

    tar xfvz easybuild-framework-1.0.tar.gz
    cd easybuild-framework-1.0
    pip install --prefix /tmp .

Do note that when an EasyBuild package is being installed without
having the EasyBuild packages that it depends upon available,
both ``easy_install`` and ``pip`` will try and pull in the latest
available version of those packages from PyPi.

Thus, to have full control over the EasyBuild installation, you need
to respect the following installation order:
easybuild-framework, easybuild-easyblocks, easybuild-easyconfigs. The
easyblocks package depends on the framework package;
the easyconfigs package depends on both the framework and easyblocks packages.

If you do not have ``pip`` or ``easy_install`` available, you can also
fall back to using the ``setup.py`` script directly::

    python setup.py --prefix /tmp install


Installation of latest release from GitHub
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To install the latest (stable) release of an EasyBuild package directly
from GitHub, use the following command::

    pip install --prefix /tmp http://github.com/hpcugent/easybuild-framework/archive/master.tar.gz

Again, the order in which the EasyBuild packages are installed is
important to have full control over the installation process, see
previous section.

Installation of latest development version
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To install the latest development version of an EasyBuild package from
GitHub, you can simply adjust the command
from the previous section to install from the ``develop`` branch (or
any of the available feature branches in any
EasyBuild repository for that matter)::

    pip install --prefix /tmp http://github.com/hpcugent/easybuild-framework/archive/develop.tar.gz

.. note::
  You should use this only if you are interested in developing for EasyBuild.
  Although it is well tested, the development version of EasyBuild may be unstable at a given point in time.

Installation of latest development version using provided script
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You can set up a development version of EasyBuild, in the following manner::

    curl -O https://raw.githubusercontent.com/hpcugent/easybuild-framework/master/easybuild/scripts/install-EasyBuild-develop.sh
    bash install-EasyBuild-develop.sh hpcugent /tmp/$$ ## N.B. repo-to-be-used & installation prefix
    module load /tmp/$$/module-EasyBuild-develop
    eb --version  ## This should ensure you have a reasonable instance of EasyBuild

.. note:: The above creates a modulefile which you can load/inspect at will.
  The interesting aspect about it is that it is pointing to an EasyBuild
  installation directly on local git repositories, which allows you to
  customise it easily. Remember to commit/push or otherwise save your changes,
  if you intend to use them later.

.. note:: Using ``$$`` (process id of current shell) is there to ensure that you do not step on other people's toes,
  in case you happen to run the same command, on the same system, at the same time.


