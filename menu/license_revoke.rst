ライセンスのキャンセルリクエスト
==================================

顧客が契約の取り消し、または払い戻しの場合には、以下のリクエストを送るキーを取り消すことができます。
リクエストが処理された後、そのライセンスに関連する情報は削除、または無効化される可能性があります。

WebサービスのURL
-------------------

POSTリクエストを次のURLに送信します。

**/shop/api/ezpert/RevokeLicense.action**



サービス認証
------------------------

サービスにアクセスするには、ユーザーに対して認証する必要があります。

リクエストは次のヘッダーフィールドを使用して送信する必要があります::

   Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==

権限フィールドは、以下のように構成されます:

- ユーザ名とパスワードは、1つのコロンで結合されます。
- 結果の文字列は、Base64を使用してエンコードされます。
- 認証方法とスペース、すなわち「Basic」が符号化された文字列の前に置かれる。

.. note:: パスワードを開いたゴマを持つユーザ ``Aladdin`` の場合、 ``Aladdin`` + ``:`` + ``open sesame`` となり、Base64を使用してエンコードされ	   ます。結果は ``QWxhZGRpbjpvcGVuIHNlc2FtZQ==`` になります。

トークン認証を使用したリクエストヘッダーの例:

.. code-block:: none
    :linenos:
    :emphasize-lines: 5

    POST /shop/api/ezpert/RevokeLicense.action HTTP/1.1
    ...
    Origin: http://localhost:8003
    User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2785.143 Safari/537.36
    Authorization: Token ZGlzY28xMjM0OltCQDFiNDVkODE=
    Content-Type: application/x-www-form-urlencoded
    Referer: http://localhost:8003/acx/index.jsp
    ...

---------------




必要なリクエストパラメータ
----------------------------

client_id
    The unique identifier for the client, it should be a valid email associated with the license.

license
    The license key to revoke.


HTTPリクエストの例
^^^^^^^^^^^^^^^^^^^^^

Revoke a license sending the client id (*mario.rossi@sangah.com*) 
together with the associated license (*ACTR-9QGO-BNCC-JWM0*):

.. code-block:: bash

    $ curl \
    --data "client_id=mario.rossi%40sangah.com&license=ACTR-9QGO-BNCC-JWM0" \
    --user username:password \
    http://ezpert.com/shop/api/ezpert/RevokeLicense.action


.. note:: 上記のサンプルはlinuxで ``curl`` コマンドを利用していますので、使用する環境合わせて確認する必要があります。

.. note:: **.NET** ユーザーの場合、.NETアプリケーションを介してリクエストを送信する際の参照と
   下記ののウェブサイトで利用可能です:
   
   * https://msdn.microsoft.com/en-us/library/debx8sh9(v=vs.110).aspx
   * https://msdn.microsoft.com/en-us/library/system.net.httpwebrequest(v=vs.110).aspx
   * https://msdn.microsoft.com/en-us/library/system.net.httpwebrequest.headers(v=vs.110).aspx
   * https://msdn.microsoft.com/en-us/library/system.web.httprequest.inputstream.aspx
   * https://msdn.microsoft.com/en-us/library/system.web.script.serialization.javascriptserializer.aspx
   
リスポンスタイプ
----------------------

JSON出力
^^^^^^^^^^^

リスポンスが ``json`` の場合、結果は以下の応答と似ているものが出ると思います:

.. code-block:: json

    {
        "result": {
            "data": [{
                "client_id": "mario.rossi@sangah.com",
                "license_key": "HHZF-JWDP-QPG0-COVS-DXKL-8WAA",
                "mac_address": null,
                "product_code": "EZP5",
                "status": "REVOKED"
            }],
            "status": "REVOKED"
        }
    }


XML出力
^^^^^^^^^^

リスポンスが ``xml`` の場合、結果は以下のサンプルと似ていると思います:

.. code-block:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <Response>
        <Status>REVOKED</Status>
        <Data class="License-array">
            <License>
                <ClientId>mario.rossi@sangah.com</ClientId>
                <ProductCode>EZP5</ProductCode>
                <LicenseKey>HHZF-JWDP-QPG0-COVS-DXKL-8WAA</LicenseKey>
                <Status>REVOKED</Status>
            </License>
        </Data>
    </Response>



エラーリスポンス
------------------

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


The licence has not been found; the user or the license key might be wrong::

    {
        "error": {
            "type": "java.lang.IllegalStateException",
            "message": "License not found."
        }
    }
