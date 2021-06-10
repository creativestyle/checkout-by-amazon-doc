.. _workflow:

Order & payment workflow
========================

The **Amazon Pay** extension follows the standard OpenMage order and payment workflow, and thus processing **Amazon Pay** payments doesn't differ significantly from other payment methods available in OpenMage, making it easy to handle. The most important difference, comparing to the standard OpenMage orders, is delayed access to the billing address, which is backfilled later in the synchronization process after the successful authorization.

All Amazon Pay objects (ChargePermission, Charge and Refund) are reflected in corresponding payment transactions in OpenMage, which are connected with appropriate document entities provided by the OpenMage, (invoices for captures, credit memos for refunds).


`Amazon Pay` button
-------------------

The `Amazon Pay` button appears in several places in the shop:

* on the shopping cart page,
* in the 1st step of the default One Page Checkout,
* in the sidebar cart widget.

.. image:: /images/1.7.2/order_step_1.png

You can also place the `Amazon Pay` button in any place you like by including following statement in the template file:

.. code-block:: html

   <div data-lpa-role="pay-button"></div>

Pressing the `Amazon Pay` button launches the Amazon Payments authentication window, where the customer is asked for his Amazon account e-mail address and password.

.. image:: /images/1.7.2/order_step_2.png

After a successful login the customer is redirected to the Amazon Pay checkout page in your shop.

Placing an order
----------------
The **Amazon Pay** checkout form consists of 4 steps arranged within a single page (unlike OpenMage default checkout, which uses accordion for showing and hiding particular steps of the checkout). These steps are: shipping address (provided by Amazon Pay), payment method (provided by Amazon Pay), shipping method and order review (handled by default OpenMage checkout templates). All fields in the form (shipping address, payment method and shipping method) are pre-filled, which means that in very basic scenario customer can finish the checkout with just one click. Unfortunately, pre-filling doesn't apply to the terms and conditions checkbox (if used at all) and can raise the number of required clicks, which, however, doesn't affect the easiness and user-friendliness of the **Amazon Pay** payment method.

.. image:: /images/1.7.2/order_step_3.png

.. note:: The value selected in each checkout step is saved in a separate AJAX call. When the checkout form shows up for the first time, depending on the internet connection speed  and the web-server's response time, it may take up to few seconds until `Place order` button gets active and can be clicked by the customer.

After selecting the desired shipping address, payment method, shipping method and pressing `Place order` button (preceded by accepting terms and conditions if needed), the customer is redirected to the success page. **Amazon Pay** uses the default OpenMage success page, which means there's no need to add any tracking scripts or additional page layout elements that you use in default OpenMage checkout and want also use in Amazon checkout, all features implemented additionally on the OpenMage success page shall also appear on Amazon checkout success page.

.. image:: /images/1.7.2/order_step_4.png

The created order will be transferred to Amazon and will appear in your OpenMage admin in `Pending` (by default) or `Processing` (if you are using :ref:`synchronous authorization <configuration-authorization-processing-mode>`) state.

.. note:: You may notice in the OpenMage admin that the billing address may be incorrect at this point (as mentioned in the introduction to this chapter). That's true if the billing differs from the shipping data. The billing address will be updated as soon as authorization is confirmed by Amazon Pay. Keep also in mind that the billing address is available only for the sellers that provided a valid VAT number in Amazon Seller Central.

.. _workflow-multicurrency:

Multicurrency support
---------------------

**Amazon Pay** extension supports multicurrency payments on the website level. Website is the top level organizational unit in OpenMage, it may share customer accounts, shipping and payment methods. To offer multicurrency support that is provided by Amazon Pay service in your OpenMage shop, you have to create a separate website for each currency you plan to offer in your shop, with the selected currency set as a base currency in OpenMage settings.

.. image:: /images/multicurrency_websites.png

By default the base currency is set globally in OpenMage, to make it available on the website level, please go to :menuselection:`System --> Configuration --> Catalog --> Catalog --> Price --> Catalog Price Scope` and set its value to `Website`.

.. image:: /images/catalog_price_scope.png

Next go to :menuselection:`System --> Configuration --> General --> Currency Setup --> Currency Options --> Base Currency` and set its value to the selected currency for each of the websites you created previously to offer multicurrency support for your customers. The website configuration scope can be changed by selecting appropriate entry in the `Current Configuration Scope` dropdown list in the top right corner of the configuration page.

.. image:: /images/website_currency_setup.png

.. note:: Please note, that multicurrency support in the extension doesn't use Display Currency setting, which may be set on the store view level. Display currency is used just for the storefront presentation purposes and doesn’t play any role in further order post-processing, because all transactions are processed using base currency. Such a behavior is implemented in OpenMage core classes and Amazon Pay extension do not break this rule.

.. _workflow-authorization:

Payment authorization
---------------------

An authorization can be requested after the order data is successfully transferred to Amazon. Depending on the value you've selected for :ref:`configuration-payment-action` option it can be processed in several ways. For `Authorize` and `Authorize & capture` actions it will be requested automatically as soon as order is placed in your shop and successfully transferred to Amazon. The requested authorization will be therefore either confirmed or declined by Amazon either via IPN message or via data polling, see :ref:`workflow-synchronizing-order-data` to get more details. The order, for which a payment authorization has been confirmed changes its state to **Processing**, an order email confirmation is sent to the customer (if not disabled in the extension settings, see :ref:`configuration-order-confirmation`) and you can start the fulfilment process.

.. warning:: Never dispatch ordered items before the authorization is confirmed. Only the confirmed authorization guarantees that you will be able to capture the order amount (if you capture within 7 days).


Declined authorizations
~~~~~~~~~~~~~~~~~~~~~~~~

If the authorization is declined by Amazon Pay due to problem with the payment method selected, your customer will be informed about this case via e-mail and requested to visit the Amazon Pay web site. The customer can on this page update the payment method by following the instructions on the web page. The e-mail sent to the customer can be adjusted according to the :ref:`customization-email-templates` section. After the successful payment method update, Amazon Pay will notify OpenMage about the new authorization status and payment will get back on the track (via polling or IPN).

In case the authorization has been declined due to any other reason then problems with the selected payment method, the notification email will be sent to shop administrator and appropriate action must be undertaken according to the Amazon Payments Integration Guide.


Capturing the payment amount
----------------------------

After a successful authorization, you can capture funds against the authorization. :ref:`configuration-payment-action` option in the extension settings allows to switch between manual and automatic capture mode. For `Manual authorization` actions the capture is triggerd by creating manually an invoice for the order in the OpenMage admin. For `Authorize & capture` action, the capture is requested automatically as soon as authorization is confirmed by Amazon Pay.


Manual capture
~~~~~~~~~~~~~~

To capture the order amount, you must create an invoice first. To create an invoice, login to the OpenMage admin, open the order for which you want to capture the amount and click the `Invoice` button located in the top buttons rows. Please make sure that the order you want to process has been successfully authorized, which basically means that it is in **Processing** state.

.. image:: /images/workflow_screenshot_4.png

After clicking the `Invoice` button, a new invoice form will appear with most of the crucial data (like products quantity) already filled in. You can adjust some invoice fields if needed. At this point you can create a shipment as well, by checking `Create Shipment` checkbox and adding a tracking number if needed. Before submitting the form, please **make absolutely sure** that `Amount` selectbox is set to `Capture online` and press `Submit Invoice` button. A new invoice and a new shipment (if checked `Create Shipment` checkbox) will be created for the order and the capture request is sent to Amazon Pay.

.. image:: /images/workflow_screenshot_5.png

.. warning:: To collect the funds that were authorized, you must capture the amount within 30 days of a successful authorization (two days in Sandbox mode). We strongly recommend that you capture funds within seven days of authorization to reduce the likelihood of declines (within 7 days the a successful captures is guaranteed). In case your fulfilment process exceeds 30 days, consider using the `Manual authorization` as payment action in the configuration and authorize the payment later in any suitable time (typically in the week before the shipping) before the shipping.

.. note:: Partial captures are not supported by the extension at this moment.

The capture status, similar to authorizations, will be updated either via IPN message or via data polling, see :ref:`workflow-synchronizing-order-data` for more details.


Automatic capture
~~~~~~~~~~~~~~~~~

In this mode the capture is requested automatically after the successful authorization. Also the invoice that covers all ordered items is created automatically. Post-request processing (capture status synchronization) is carried the same way as in capture invoked manually from OpenMage backend.


Refunding order items
---------------------

The order, which payment has been captured for, can be refunded either fully or partially. Refunds are made against the invoices and thus having a paid invoice assigned to the order is a necessary condition that has to be met to refund any order item. Refunds in OpenMage are recorded as credit memos, so for requesting a refund with Amazon Payments you should create a credit memo first. To create a credit memo login to the OpenMage admin, open the order you want refund, click `Invoices` tab on the right, select an invoice you want to refund and click on it.

.. image:: /images/workflow_screenshot_6.png

A preview of the selected invoice will appear. Make sure that you are on the single invoice preview page and click the `Credit Memo` button.

.. image:: /images/workflow_screenshot_7.png

A new credit memo form will appear with most of the crucial data (like products quantity to be refunded) already filled in. If you want to refund the invoice partially (i.e. only a part of the invoiced items) adjust the product quantities to be refunded (set 0 for items that shall not be refunded) and click `Update Qty's` button to update refund totals. You can also set the refunded items back to stock by checking `Return to Stock` checkbox. Next choose if you want to refund shipping costs or apply any refunds adjustment and fill in the appropriate fields. Next before submitting the credit memo form, double check that you have `Refund` button available and click it. A credit memo will be created and a refund will be requested with Amazon Payments. Its status will be updated either via IPN or data polling, depending on the update method selected in the extension settings.

.. image:: /images/workflow_screenshot_8.png

.. warning:: For the successful refund (recorded in OpenMage and requested (!) with Amazon Pay) always use `Refund` button available on the new credit memo form invoked from the single invoice preview page. If you click `Credit Memo` button directly on the order page you will be redirected to the new credit memo form with `Refund offline` button only, which admittedly will record credit memo in OpenMage, but surely won't call refund request at Amazon Payments gateway. If in any case you will get a credit memo with `Refund offline` button only then surely something had to go wrong and you should stop the refund process immediately and start it from the beginning following the above guideline.


Cancelling an order
-------------------

For a variety of reasons it sometimes becomes necessary to cancel an order. To cancel an order and notify Amazon Pay about the payment cancellation:

* Please make sure the amount of the order you want to cancel hasn't been captured yet,
* Go to :menuselection:`Sales --> Orders` and select the order that you would like to cancel by clicking the `Edit button` on its respective row,
* Click `Cancel` in order page to remove this order.

.. image:: /images/workflow_screenshot_9.png
