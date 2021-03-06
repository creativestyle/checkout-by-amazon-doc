Amazon Sign-in
=================
**Amazon Sign-in** allows users to login to your shop using their Amazon user name and password. All available data needed for creating an account or placing an order in your OpenMage shop (including name, email address, and zip code) are fetched automatically from customer's Amazon account.

Requirements
------------
**Amazon Sign-in** service requires you to have a valid **Amazon Payments** account and OpenMage store with a valid SSL certificate installed and properly configured in your shop. By "installed and properly configured SSL certificate" it is meant that your webserver is configured to serve pages via HTTPS protocol, `Base URL` config option is set to the HTTPS-based URL and in :menuselection:`Web --> Secure` section of OpenMage settings.

:guilabel:`Amazon Sign-in` button
------------------------------------
The :guilabel:`Amazon Sign-in` button appears in several places in the shop:

* on the customer login page,
* on the customer registration page.

.. image:: /images/login_with_amazon_1.png

You can also place the `Amazon Sign-in` button in any place you like by including following statement in the template file:

.. code-block:: html

   <div data-lpa-role="login-button"></div>


Pressing the `Amazon Sign-in` button launches the Amazon authentication window, where the customer is asked for his Amazon account e-mail address and password.

.. image:: /images/login_with_amazon_2.png

After a successful login the customer is redirected to the user area in your shop.

.. note:: When the customer uses the `Amazon Sign-in` for the first time in your shop and account with the same e-mail address already exists in your shop, he will be asked to enter his shop password to match both shop and Amazon accounts. Moreover, if you require additional data from your customers which cannot be provided by Amazon (like date of birth or gender), the customer will be asked for those data in a dedicated form, that appears after the first use of `Amazon Sign-in` button.
