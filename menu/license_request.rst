Request New License
======================

Web Service URL
-------------------

Send a POST request to the following URL:

**/shop/api/ezpert/CreateLicense.action**



Service Authentication
------------------------

To access the service the user need to be authenticated.

The request must be sent using the following header field::

	Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==

The Authorization field is constructed as follows:

- The username and password are combined with a single colon.
- The resulting string is encoded using Base64.
- The authorization method and a space i.e. "Basic " is then put before the encoded string.

.. note:: For a user ``Aladdin`` having password ``open sesame`` the combined string would be:
   ``Aladdin`` + ``:`` + ``open sesame`` 
   and then encoded using Base64 the result will be ``QWxhZGRpbjpvcGVuIHNlc2FtZQ==``.


An example of request header with Token Authentication:

.. code-block:: none
    :linenos:
    :emphasize-lines: 5

    POST /shop/api/ezpert/CreateLicense.action HTTP/1.1
    ...
    Origin: http://localhost:8003
    User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2785.143 Safari/537.36
    Authorization: Token ZGlzY28xMjM0OltCQDFiNDVkODE=
    Content-Type: application/x-www-form-urlencoded
    Referer: http://localhost:8003/acx/index.jsp
    ...

---------------




Required Request Parameters
------------------------------

client_id
    It should be a valid email address that will be used as unique identifier for the client and for sending the license to the user.

client_name
    The name of the client.

licenses (default: 1)
    The number of licenses to generate.

output (default: ``json``)
    The type of response can be ``json`` or ``xml``.


HTTP Request Examples
^^^^^^^^^^^^^^^^^^^^^^^^^

Request one license for a client sending his name (*Mario Rossi*) and his ID (*mario.rossi@gmail.com*):

.. code-block:: bash

    $ curl \
    --data "client_name=Mario%20Rossi&client_id=mario.rossi%40gmail.com" \
    --user username:password \
    http://ezpert.com/shop/api/ezpert/CreateLicense.action

Same as above but this time we request two license:

.. code-block:: bash

    $ curl \
    --data "client_name=Mario%20Rossi&client_id=mario.rossi%40gmail.com&licenses=2" \
    --user username:password \
    http://ezpert.com/shop/api/ezpert/CreateLicense.action


.. note:: The samples above make use of ``curl`` command on linux, and they should be translated according to the language you want to use.

.. note:: For **.NET** users, reference and examples about sending requests through .NET applications 
   are availables at the following websites: 
   
   * https://msdn.microsoft.com/en-us/library/debx8sh9(v=vs.110).aspx
   * https://msdn.microsoft.com/en-us/library/system.net.httpwebrequest(v=vs.110).aspx
   * https://msdn.microsoft.com/en-us/library/system.net.httpwebrequest.headers(v=vs.110).aspx
   * https://msdn.microsoft.com/en-us/library/system.web.httprequest.inputstream.aspx
   * https://msdn.microsoft.com/en-us/library/system.web.script.serialization.javascriptserializer.aspx

Response Type
---------------

JSON Output
^^^^^^^^^^^^^^

If the response is in ``json`` the result might be similar to the response below for one license:

.. code-block:: json

    {
        "response": {
            "data": [{
                "mac_address": null,
                "status": "NEW",
                "product_code": "EZP5",
                "license_key": "BB8N-9XFB-JAM6-AL7C-RORI-RAAA",
                "client_id": "emanuele.disco@sangah.com",
                "reg_date": "2017-02-27 16:14:48"
            }],
            "status": "CREATED"
        }
    }

For two or more licenses:

.. code-block:: json

    {
        "response": {
            "data": [{
                "mac_address": null,
                "status": "NEW",
                "product_code": "EZP5",
                "license_key": "LCGQ-VRSM-CLAG-ETGO-FBXL-6WAA",
                "client_id": "emanuele.disco@sangah.com",
                "reg_date": "2017-02-27 16:17:06"
            }, {
                "mac_address": null,
                "status": "NEW",
                "product_code": "EZP5",
                "license_key": "DCD6-SYBH-EIPX-YIVU-6CEH-MAAA",
                "client_id": "emanuele.disco@sangah.com",
                "reg_date": "2017-02-27 16:17:06"
            }],
            "status": "CREATED"
        }
    }


XML Output
^^^^^^^^^^^^^

If the response is in ``xml`` the result will be similar to the sample below:

.. code-block:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <Response>
        <Status>CREATED</Status>
        <Data class="License-array">
            <License>
                <ClientId>mario.rossi@sangah.com</ClientId>
                <ProductCode>EZP5</ProductCode>
                <LicenseKey>HLNY-PSGN-1GZD-NFFF-MIFV-KAAA</LicenseKey>
                <Status>NEW</Status>
            </License>
            <License>
                <ClientId>mario.rossi@sangah.com</ClientId>
                <ProductCode>EZP5</ProductCode>
                <LicenseKey>B7RM-KWNC-3AYC-LJFA-4TPO-KQAA</LicenseKey>
                <Status>NEW</Status>
            </License>
        </Data>
    </Response>


Common Errors
---------------------

In case the authentication credentials have not been sent::

    {
        "error": {
            "message": "Unauthorized operation."
        }
    }

In case the credentials are not valid the authentication will fail with the following response::

    {
        "error": {
            "type": "org.springframework.security.BadCredentialsException",
            "message": "Login failed - username or password incorrect; nested exception is java.lang.RuntimeException: Login failed - username or password incorrect"
        }
    }


Client Id has not been sent with the request::

    {
        "error": {
            "type": "java.lang.NullPointerException",
            "message": "A client_id must be provided."
        }
    }