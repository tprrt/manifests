.. image:: https://circleci.com/gh/tprrt/manifests.svg?style=svg&circle-token=8794b4eb585ada86a0521f8c215903faa223de40
    :alt: Circle badge
    :target: https://app.circleci.com/pipelines/github/tprrt/manifests

==================
git-repo manifests
==================

A collection of `git-repo`_ manifests to set up OE/Yocto build environments.

----

Install git-repo
================

In first, you should install the `git-repo`_ utility:

.. code-block:: bash

    mkdir ~/bin
    curl https://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
    chmod a+rx ~/bin/repo
    export PATH=${PATH}:~/bin


*Note: make sure ~/bin exists and it is part of your PATH.*

Pull the OE/Yocto build environment
===================================

Download the OE/Yocto build environment:

.. code-block:: bash

    mkdir dist
    cd dist
    repo init -u git@github.com:tprrt/manifests.git [-b <branch>][-m <manifest>]
    repo sync -j4


By default, when the branch and the manifest aren't specified, the default
manifest from the master branch is used.
Here, the default manifest will fetch the master branch of required layers to
build the nodistro flavor.

Then Bitbake and all required layers will be downloaded.

Following, an example to pull the layers of the Dunfell (LTS) release:

.. code-block:: bash

    repo init -u git@github.com:tprrt/manifests.git -b master -m nodistro/branch/dunfell.xml
    repo sync -j4


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

For example, to select the exiguous distro and build it into a separated folder:

.. code-block:: bash

    export BDIR=build-exiguous
    export DISTRO=exiguous
    export TEMPLATECONF=meta-exiguous/conf

    source ./layers/oe-init-build-env


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

By default, if the `MACHINE` isn't specified then the `Qemux86-64` target will
be used.

The `MACHINE` environment variable can be set to all available machine
configurations defined into `meta-\*/conf/machine/\*.conf`:

- `raspberrypi4-64` to use `meta-raspberrypi/conf/machine/raspberrypi4-64.conf`,
- `sama5d2-xplained-sd` to use `meta-atmel/conf/machine/sama5d2-xplained-sd.conf`,
- `imx8qxp-mek` to use `meta-freescale/conf/machine/imx8qxp-mek.conf`,
- `beaglebone` to use `meta-ti/conf/machine/beaglebone.conf`,
- etc.

For example, to use a Raspberrypi 4 target:

.. code-block:: bash

    bitbake-layers add-layer ../layers/meta-raspberrypi
    export MACHINE="raspberrypi4-64"


Build an image or a SDK
=======================

Finally, you will be able to build an image for the given target:

.. code-block:: bash

    bitbake core-image-minimal


Or, to build a SDK:

.. code-block:: bash

    bitbake -c populate_sdk core-image-minimal


----

Use the following command to validate the `circleci`_ pipeline:

.. code-block:: bash

    podman run --rm --security-opt seccomp=unconfined --security-opt label=disable -v $(pwd):/data circleci/circleci-cli:alpine config validate /data/.circleci/config.yml --token $TOKEN


.. _circleci: https://circleci.com
.. _git-repo: https://gerrit.googlesource.com/git-repo
