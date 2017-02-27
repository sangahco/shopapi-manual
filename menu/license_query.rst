Query User Licenses
===============================

Web Service URL
-------------------

Send a POST request to the following URL:

**/shop/api/ezpert/QueryLicenses.action**



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


HTTP Request Examples
^^^^^^^^^^^^^^^^^^^^^^^^^

Get the licenses purchased by a client sending his ID (*mario.rossi%40sangah.com*):

.. code-block:: bash

    $ curl \
    --data "client_id=mario.rossi%40sangah.com" \
    --user username:password \
    http://ezpert.com/shop/api/ezpert/QueryLicenses.action


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
        "result": {
            "data": [{
                "client_id": "mario.rossi@sangah.com",
                "license_key": "ZWSO-E4TC-HS0H-QTQD-BTFK-WWAA",
                "mac_address": null,
                "product_code": "EZP5",
                "status": "NEW"
            }, {
                "client_id": "mario.rossi@sangah.com",
                "license_key": "AUQ8-DHR4-VKSD-JPEY-WSFV-8AAA",
                "mac_address": "08-00-27-AA-6H-7N",
                "product_code": "EZP5",
                "status": "ATTACHED"
            }, {
                "client_id": "mario.rossi@sangah.com",
                "license_key": "PDKY-J3SO-N5M7-1IEM-TEFY-CQAA",
                "mac_address": null,
                "product_code": "EZP5",
                "status": "NEW"
            }],
            "status": "OK"
        }
    }


XML Output
^^^^^^^^^^^^^

If the response is in ``xml`` the result will be similar to the sample below:

.. code-block:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <Response>
        <Status>OK</Status>
        <Data>
            <License>
                <ClientId>mario.rossi@sangah.com</ClientId>
                <ProductCode>EZP5</ProductCode>
                <LicenseKey>ZWSO-E4TC-HS0H-QTQD-BTFK-WWAA</LicenseKey>
                <Status>NEW</Status>
            </License>
            <License>
                <ClientId>mario.rossi@sangah.com</ClientId>
                <ProductCode>EZP5</ProductCode>
                <LicenseKey>AUQ8-DHR4-VKSD-JPEY-WSFV-8AAA</LicenseKey>
                <Status>NEW</Status>
            </License>
            <License>
                <ClientId>mario.rossi@sangah.com</ClientId>
                <ProductCode>EZP5</ProductCode>
                <LicenseKey>PDKY-J3SO-N5M7-1IEM-TEFY-CQAA</LicenseKey>
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

In case the user or the key doesn't exist the followig response might be generated::

    {
        "result": {
            "data": [],
            "status": "OK"
        }
    }

or in case of an `xml` response::

    <?xml version="1.0" encoding="UTF-8"?>
    <Response>
        <Status>OK</Status>
        <Data />
    </Response>