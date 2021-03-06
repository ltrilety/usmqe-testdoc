Elementary checks of services
******************************

:author: - mbukatov@redhat.com
         - mkudlej@redhat.com

Description
===========

Elementary check of tendrl-api, tendrl-node-agent and tendrl-monitoring-integration 
systemd service unit file. This test case doesn't change any state in any way.

Checks are based on:

* fedora `systemd packaging policy`_
* systemd upstream `daemon(7)`_ manpage

Setup
=====

Follow TODO to install Tendrl. No particular
cluster configuration is required for this test case, so we use the default
one.

All test steps are performed for these services:

* ``tendrl-api``,
* ``tendrl-monitoring-integration``,
* ``tendrl-node-agent``,
* ``tendrl-gluster-integration``,
* ``tendrl-notifier``.

Test Steps
==========

.. test_action::
   :step:
       Check that all systemd service files are available:

       .. code-block:: bash

           # rpm -qa | grep tendrl | xargs rpm -ql | grep "systemd.*service"
   :result:
       Tendrl systemd service files are shown in the output, like this::

           /usr/lib/systemd/system/tendrl-api.service
           /usr/lib/systemd/system/tendrl-node-agent.service
           /usr/lib/systemd/system/tendrl-monitoring-integration.service
           /usr/lib/systemd/system/tendrl-gluster-integration.service
           /usr/lib/systemd/system/tendrl-notifier.service

       Note: we don't expect any other systemd unit files here now. When other
       systemd units are added, we need to update this test case to do the
       initial validation of these files as well.

.. test_action::
   :step:
       Run the following commands on the tendrl service files:

       .. code-block:: bash

           # systemd-analyze verify /usr/lib/systemd/system/tendrl-api.service
           # systemd-analyze verify /usr/lib/systemd/system/tendrl-node-agent.service
           # systemd-analyze verify /usr/lib/systemd/system/tendrl-monitoring-integration.service
           # systemd-analyze verify /usr/lib/systemd/system/tendrl-gluster-integration.service
           # systemd-analyze verify /usr/lib/systemd/system/tendrl-notifier.service
   :result:
       Commands produce no output and return zero.

.. test_action::
   :step:
       Show the content of tendrl systemd unit files:

       .. code-block:: bash

           # systemctl cat tendrl-api.service
           # systemctl cat tendrl-node-agent.service
           # systemctl cat tendrl-monitoring-integration.service
           # systemctl cat tendrl-gluster-integration.service
           # systemctl cat tendrl-notifier.service
   :result:
       The content of the service unit files are shown and they contain:

       * A good human-readable description string with ``Description=``.
       * Reference to documentation/manpage is available in ``Documentation=``
         and this manpage is availabe on the system.
       * There is an ``[Install]`` section including installation information
         for the unit file which contains ``WantedBy=multi-user.target``.

       Based on suggestions from `daemon(7)`_ manpage and `systemd packaging
       policy`_.

.. test_action::
   :step:
       List dependencies of the services:

       .. code-block:: bash

           # systemctl list-dependencies tendrl-api
           # systemctl list-dependencies tendrl-node-agent
           # systemctl list-dependencies tendrl-monitoring-integration
           # systemctl list-dependencies tendrl-gluster-integration
           # systemctl list-dependencies tendrl-notifier
   :result:
       Dependency trees are shown.

.. test_action::
   :step:
       Check status of the service:

       .. code-block:: bash

           # systemctl status tendrl-api
           # systemctl status tendrl-node-agent
           # systemctl status tendrl-monitoring-integration
           # systemctl status tendrl-gluster-integration
           # systemctl status tendrl-notifier
   :result:
       Statuses are shown, systemctl return zero return codes.

Teardown
========

Teardown is not needed.
