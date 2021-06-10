.. _installation:

Installation
============

Pre-installation steps
----------------------

* Create a backup of your shop before proceeding to install.
* If your shop is using compilation (you can check it in :menuselection:`System --> Tools --> Compilation`), disable it please before proceeding to install.


.. _installation-process:

Installation process
--------------------

.. _installation-magento-connect-manager:

Installation via Connect Manager
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* Go to :menuselection:`System --> Magento Connect --> Magento Connect Manager` and enter your admin credentials to get logged in.

.. image:: /images/connect_manager_step_1.png

.. image:: /images/connect_manager_step_2.png

* In the :guilabel:`Install New Extensions` section paste the following extension key:

.. centered:: `http://connect.creativestyle.de/Creativestyle_AmazonCheckout`

and click :guilabel:`Install` button. Connect Manager will validate the key and check the extension dependencies and will display a pre-installation summary. Click :guilabel:`Proceed` button to start the installation.

.. image:: /images/connect_manager_step_3.png

* After the successful installation, please click on :guilabel:`Refresh` button to refresh the list of the installed extensions and assure that **Amazon Checkout** is present on that list.

* Proceed to the :ref:`post-installation-steps`.

Manual installation
~~~~~~~~~~~~~~~~~~~

* Go to `Creativestyle OpenMage connect channel <https://connect.creativestyle.de/Creativestyle_AmazonCheckout>`_ and download the recent installation package of the extension. Unpack the downloaded file to your OpenMage root directory.

.. tip::
   If you are using any VCS (git, svn, mercurial) for versioning your shop basecode, you can also commit the content of the downloaded file to the VCS repository and deploy it to your shop.

* Proceed to the :ref:`post-installation-steps`.

.. _post-installation-steps:

Post-installation steps
-----------------------

* If you're using custom design theme, refer to the :ref:`Templates customization <customization-frontend-templates>` section to find out how to adjust **Amazon Pay (Checkout v2)** templates to your needs.
* Go to :menuselection:`System --> Cache Management` and flush OpenMage cache storage.
* If you have disabled compiler in pre-installation stage, you can go now to :menuselection:`System --> Tools --> Compilation`, recompile and enable compiler again.
* Logout from the OpenMage admin and login again.

Voila! The **Amazon Pay (Checkout v2)** extension shall be installed now. You can proceed to the :ref:`configuration` followed by :ref:`customization-frontend-templates` customization (if applicable).


Upgrade
-------

Pre-upgrade steps
~~~~~~~~~~~~~~~~~

1. Create a backup of your shop before proceeding to upgrade.
2. If your shop utilises compilation (you can check it in :menuselection:`System --> Tools --> Compilation`), disable it please before proceeding to upgrade.

Upgrade process
~~~~~~~~~~~~~~~

* Go to :menuselection:`System --> Magento Connect --> Magento Connect Manager` and enter your admin credentials to get logged in.

.. image:: /images/connect_manager_step_1.png

.. image:: /images/connect_manager_step_2.png

* Click :guilabel:`Check for Upgrades` button in the :guilabel:`Manage Existing Extensions` section. If the newest version of Amazon Checkout is available, the Creativestyle_AmazonCheckout extension on the list will be highlighted with the yellow color. In the corresponding action dropdown list please select :guilabel:`Upgrade to X.X.X (stable)` option and click :guilabel:`Commit changes` button.

* After the successful upgrade, please click on :guilabel:`Refresh` button to refresh the list of the installed extensions and assure that **Amazon Chekout v2** (identified as `Creativestyle_AmazonCheckout`) was upgraded to the desired version.

* Proceed to the :ref:`post-upgrade-steps` section.

.. _post-upgrade-steps:

Post-upgrade steps
~~~~~~~~~~~~~~~~~~

* Go to :menuselection:`System --> Cache Management` and flush Magento cache storage.
* If you have disabled compiler in pre-installation stage, you can go now to :menuselection:`System --> Tools --> Compilation`, recompile and enable compiler again.
