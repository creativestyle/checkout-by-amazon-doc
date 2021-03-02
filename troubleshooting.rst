.. important::
   This extension is under the active development even though Adobe dropped the support for Magento 1.x e-commerce platform (Jul 1st, 2020). Creativestyle will provide the necessary updates and support for this extension as long as this extension will be used by the active Amazon Pay merchants.

.. _troubleshooting:

Troubleshooting
===============

.. _troubleshooting-older-magento:

Installing the extension on Magento lower than 1.9.4.0
------------------------------------------------------

**Amazon Checkout** extension doesn't work out of the box with Magento lower than 1.9.4.0 because of the unsupported version of **phpseclib** that comes with those Magento versions. However, it is possible to upgrade **phpseclib** and start using **Amazon Checkout** extension. To do so, please follow the steps:

1. Identify your Magento version (it's visible in the footer in Magento admin panel).
2. Check which security patches you have installed in your Magento shop by running the following command in the root directory of your Magento installation:

.. code-block:: bash

   cat app/etc/applied.patches.list

3. Go to `Magento 1.x Patches <https://magentary.com/kb/php-7-2-patches-for-magento-1-x-without-ssh>`_ page, find your Magento version and compare the patches which are installed in your Magento shop with the ones required for your version.
4. Install all missing required patches, next download and install PHP 7 compatibility patch.
5. Now your Magento is ready to use **Amazon Checkout** extension. Go to :ref:`installation` section and install the extension.
