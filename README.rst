.. image:: https://circleci.com/gh/tprrt/manifests.svg?style=svg&circle-token=8794b4eb585ada86a0521f8c215903faa223de40
    :alt: Circle badge
    :target: https://app.circleci.com/pipelines/github/tprrt/manifests

==================
repo-git manifests
==================

A collection of `git-repo`_ manifests to set up OE/Yocto build environments.

----

Install git-repo
================

In first, you should install the `git-repo`_ utility:

.. code-block:: bash

    mkdir ~/bin
    curl https://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
    chmod a+x ~/bin/repo


Make sure ~/bin exists and it is part of your PATH.

Pull the OE/Yocto build environment
===================================

Download the OE/Yocto build environment:

.. code-block:: bash

    export PATH=${PATH}:~/bin
    mkdir dist
    cd dist
    repo init -u git@github.com:tprrt/manifests.git [-b <branch>][-m <manifest>]
    repo sync -j4


Then Bitbake and all required layers will be downloaded. Moreover, the default
manifest will fetch the master branch of required layers to build the nodistro
flavor.

For example, to fetch the dunfell branch of layers:

.. code-block:: bash

    repo init -u git@github.com:tprrt/manifests.git -b master -m nodistro/branch/dunfell.xml


Then now, you will be able to build an image for the default target:

.. code-block:: bash

    . ./layers/oe-init-build-env
    bitbake core-image-minimal


Select the distribution
=======================

Currently two distro flavors are available:

- nodistro
- exiguous *(coming soon)*

To select the right distro configuration, it is only required to set both
environment variables:

- `DISTRO`
- `TEMPLATECONF`

You can also specify `BDIR`, if you want build each distro flavor in a separated
folder.

For example, to build the exiguous distro:

.. code-block:: bash

    export BDIR=build-exiguous
    export DISTRO=exiguous
    export TEMPLATECONF=meta-yocto/meta-exiguous/conf

    . ./layers/oe-init-build-env
    bitbake core-image-minimal


By default, the variable:

- `DISTRO` is equal to `nodistro`
- `TEMPLATECONF` is equal to `meta/conf`
- `BDIR` is equal to `build`

If you when reduce the build time you should modify the paths to share the
following folders between your different build environments:

- `CCACHE_TOP_DIR`
- `SSTATE_DIR`
- `DL_DIR`

Which are by default located into `BDIR`.

Select the machine
==================

.. TODO

----

Use the following command to validate the `circleci`_ pipeline:

.. code-block:: bash

    podman run --rm --security-opt seccomp=unconfined --security-opt label=disable -v $(pwd):/data circleci/circleci-cli:alpine config validate /data/.circleci/config.yml --token $TOKEN


.. _circleci: https://circleci.com
.. _git-repo: https://gerrit.googlesource.com/git-repo
