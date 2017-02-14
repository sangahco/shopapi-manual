License Revoke Request
=========================

In case of revoke of contract or refund on the store is possible to revoke the key sending the following request,
After the request is processed the information related to that license might be deleted or invalidated.

Web Service URL
-------------------

Send a POST request to the following URL:

**/shop/api/license/revoke.action**



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

    POST /ezpert/api/license/create.action HTTP/1.1
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

client_code
    The unique identifier for the client requesting a new license, this identifier is saved together with license and other information for Ezpert Login process.

license
    The license key to revoke


HTTP Request Examples
^^^^^^^^^^^^^^^^^^^^^^^^^

Request one license for the client with code `CLIENT0001`

.. code-block:: bash

    $ curl \
    --data "client_code=CLIENT0001&license=ACTR-9QGO-BNCC-JWM0" \
    --user username:password \
    http://ezpert.com/shop/api/license/revoke.action


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

If the response is in ``json`` the result might be similar to the response below:

.. code-block:: json

    {
        "client_code": "CLIENT0001",
        "license": ["ACTR-9QGO-BNCC-JWM0"]
        "status": "REVOKED"
    }

XML Output
^^^^^^^^^^^^^

If the response is in ``xml`` the result will be similar to the sample below:

.. code-block:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <Response>
        <ClientCode>CLIENT0001</ClientCode
        <Licenses>
            <License>ACTR-9QGO-BNCC-JWM0</License>
        <Licenses>
        <Status>REVOKED</Status>
    </Response>


Error Responses
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