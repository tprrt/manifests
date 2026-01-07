============
Contributing
============

Thank you for your interest in contributing to this project! This document
provides guidelines for contributing to the git-repo manifests collection for
OE/Yocto build environments.

License
=======

By contributing to this project, you agree that your contributions will be
licensed under the GNU General Public License v3.0 or later (GPL-3.0-or-later).

Copyright (C) 2026 Thomas Perrot

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

Code of Conduct
===============

Please be respectful and constructive in all interactions. We aim to maintain
a welcoming and inclusive environment for all contributors.

How to Contribute
=================

Fork and Clone
--------------

1. Fork the repository on GitHub
2. Clone your fork locally:

.. code-block:: bash

    git clone git@github.com:YOUR_USERNAME/manifests.git
    cd manifests

3. Add the upstream repository as a remote:

.. code-block:: bash

    git remote add upstream git@github.com:tprrt/manifests.git

Create a Branch
---------------

Create a feature branch for your changes:

.. code-block:: bash

    git checkout -b feature/your-feature-name

Use descriptive branch names such as:

- ``feature/add-layer-xyz``
- ``fix/kirkstone-revision``
- ``update/meta-raspberrypi``

Making Changes
--------------

Manifest Files
~~~~~~~~~~~~~~

When modifying or creating manifest files:

1. Follow the existing XML structure and formatting
2. Use 2 spaces for indentation (as defined in ``.editorconfig``)
3. Include the SPDX license identifier and copyright notice:

.. code-block:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <!-- SPDX-License-Identifier: GPL-3.0-or-later -->
    <!-- Copyright (C) 2026 Thomas Perrot -->

4. Organize remotes and projects logically by category
5. Use comments to separate different layer categories
6. Ensure revision attributes point to valid branches or tags

Documentation
~~~~~~~~~~~~~

When updating documentation:

1. Use reStructuredText format for ``.rst`` files
2. Follow the 79-character line limit for RST files
3. Test documentation rendering if possible
4. Keep examples clear and practical

EditorConfig
~~~~~~~~~~~~

This project uses EditorConfig to maintain consistent coding styles. Ensure
your editor respects the ``.editorconfig`` file settings:

- UTF-8 encoding
- LF line endings
- Trailing whitespace trimmed
- Final newline inserted
- For XML files: 2 spaces indentation
- For RST files: 4 spaces indentation, 79 char line limit

Testing Changes
---------------

Before submitting your changes:

1. Validate manifest syntax:

.. code-block:: bash

    # Test with git-repo init
    mkdir test-dist
    cd test-dist
    repo init -u /path/to/your/manifests -m path/to/manifest.xml
    repo sync -j4

2. Verify the manifest fetches all required layers successfully

3. If applicable, test building an image:

.. code-block:: bash

    export TEMPLATECONF=meta/conf
    source ./layers/oe-init-build-env
    bitbake core-image-minimal

CircleCI Validation
~~~~~~~~~~~~~~~~~~~

If you modify the ``.circleci/config.yml`` file, validate it locally:

.. code-block:: bash

    podman run --rm --security-opt seccomp=unconfined \
        --security-opt label=disable \
        -v $(pwd):/data \
        circleci/circleci-cli:alpine \
        config validate /data/.circleci/config.yml --token $TOKEN

Commit Guidelines
-----------------

Write Clear Commit Messages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Follow these guidelines for commit messages:

1. Use the imperative mood in the subject line ("Add feature" not "Added feature")
2. Limit the subject line to 50 characters
3. Capitalize the subject line
4. Do not end the subject line with a period
5. Separate subject from body with a blank line
6. Wrap the body at 72 characters
7. Use the body to explain what and why, not how

Example:

.. code-block:: text

    Add meta-mchp layer to scarthgap manifest

    The meta-mchp layer provides support for Microchip devices and
    is required for building images targeting these platforms.

    This addition enables users to build for Microchip-based boards
    using the scarthgap release.

Commit Scope
~~~~~~~~~~~~

Keep commits focused and atomic:

- One logical change per commit
- Don't mix unrelated changes
- Make sure each commit builds successfully

Submitting Changes
------------------

Push to Your Fork
~~~~~~~~~~~~~~~~~

.. code-block:: bash

    git push origin feature/your-feature-name

Create a Pull Request
~~~~~~~~~~~~~~~~~~~~~

1. Go to the GitHub repository
2. Click "New Pull Request"
3. Select your fork and branch
4. Fill in the pull request template with:

   - Clear description of the changes
   - Rationale for the changes
   - Testing performed
   - Related issues (if any)

5. Submit the pull request

Pull Request Review Process
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Maintainers will review your changes
- Address any feedback or requested changes
- Keep your branch up to date with the main branch
- Once approved, your changes will be merged

Updating Your Pull Request
~~~~~~~~~~~~~~~~~~~~~~~~~~

If changes are requested:

.. code-block:: bash

    # Make the necessary changes
    git add .
    git commit -m "Address review feedback"
    git push origin feature/your-feature-name

Keep Your Fork Updated
~~~~~~~~~~~~~~~~~~~~~~~

Regularly sync your fork with upstream:

.. code-block:: bash

    git fetch upstream
    git checkout master
    git merge upstream/master
    git push origin master

Reporting Issues
================

Found a Bug?
------------

1. Check if the issue already exists in the GitHub issue tracker
2. If not, create a new issue with:

   - Clear, descriptive title
   - Detailed description of the problem
   - Steps to reproduce
   - Expected behavior
   - Actual behavior
   - Environment details (OS, git-repo version, etc.)
   - Relevant manifest file and branch

Feature Requests
----------------

1. Open an issue describing the feature
2. Explain the use case and benefits
3. Discuss implementation approach if possible

Questions and Support
=====================

- For questions about using the manifests, refer to the README.rst
- For contribution questions, open an issue on GitHub
- For general Yocto/OpenEmbedded questions, refer to the Yocto Project documentation

What to Contribute
==================

Areas where contributions are particularly welcome:

- Adding new layer manifests
- Updating layer revisions
- Improving documentation
- Testing on different platforms
- Reporting and fixing bugs
- Adding support for new hardware platforms
- Improving CI/CD workflows

Style Guide
===========

XML Manifests
-------------

.. code-block:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <!-- SPDX-License-Identifier: GPL-3.0-or-later -->
    <!-- Copyright (C) 2026 Thomas Perrot -->
    <manifest>

      <default sync-j="5" revision="master"/>

      <!-- Use descriptive comments for section headers -->
      <remote fetch="git://git.yoctoproject.org" name="yocto"/>

      <!-- Group related projects together -->
      <project remote="yocto" revision="kirkstone" name="meta-arm" path="layers/meta-arm" />

    </manifest>

reStructuredText
----------------

- Use headers consistently (=, -, ~, ^)
- Keep line length under 79 characters
- Use code blocks with proper syntax highlighting
- Include blank lines between sections

Recognition
===========

Contributors will be acknowledged in:

- Git commit history
- GitHub contributors page
- Release notes (for significant contributions)

Getting Help
============

If you need help with your contribution:

1. Review the README.rst for project usage
2. Check existing pull requests for examples
3. Open an issue to discuss your proposed changes
4. Reach out through GitHub discussions

Thank You
=========

Your contributions help improve this project and benefit the entire community.
Thank you for taking the time to contribute!

Support the Project
===================

If you find this project useful, please consider supporting its development:

- PayPal: https://paypal.me/tprrt

Your support helps maintain and improve this project. Thank you!
