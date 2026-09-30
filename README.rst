**Element Synapse - Matrix homeserver implementation**

|support| |development| |documentation| |license| |pypi| |python|

Synapse is an open source `Matrix <https://matrix.org>`__ homeserver
implementation.
Matrix is the open standard for secure and
interoperable real-time communications. You can directly run and manage the
source code in this repository, available under an AGPL license (or
alternatively under a commercial license from Element).

🚀 Getting started
==================

It gets shipped as part of the **Element Server Suite (ESS)** which provides the
official means of deployment.

ESS is a Matrix distribution from Element with focus on quality and ease of use.
It ships a full Matrix stack tailored to the respective use case.

🛠️ Standalone installation and configuration
============================================

The Synapse documentation describes `options for installing Synapse standalone
<docs/setup/installation.md>`_. See
below for more useful documentation links.

- `Synapse configuration options <docs/usage/configuration/config_documentation.md>`_
- `Synapse configuration for federation <docs/federate.md>`_
- `Using a reverse proxy with Synapse <docs/reverse_proxy.md>`_
- `Upgrading Synapse <docs/upgrade.md>`_

🛠️ Development
==============

We welcome contributions to Synapse from the community!
The best place to get started is our
`guide for contributors <docs/development/contributing_guide.md>`_.

Developers might be particularly interested in:

* `Run the demo server directly <demo/start.sh>`_,
* `Demo docker cluster guide <demo/docker/README.md>`_,
* `Synapse's database schema <docs/development/database_schema.md>`_,
* `notes on Synapse's implementation details <docs/development/internal_documentation/README.md>`_, and
* `how we use git <docs/development/git.md>`_.
