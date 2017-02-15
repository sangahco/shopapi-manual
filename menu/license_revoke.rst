ライセンスキャンセルリクエスト
================================

顧客が契約の取り消し、または払い戻しの場合には、以下のリクエストを送るキーを取り消すことができます。
リクエストが処理された後、そのライセンスに関連する情報は削除、または無効化される可能性があります。

WebサービスのURL
-------------------

POSTリクエストを次のURLに送信します。

** / shop / api / license / revoke.action **



サービス認証
------------------------

サービスにアクセスするには、ユーザー認証する必要があります。

要求は次のヘッダーフィールドを使用して送信する必要があります::

Authorization: Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==

権限フィールドは、以下のように構成されます:

- ユーザ名とパスワードは、1つのコロンで結合されます。
- 結果の文字列は、Base64を使用してエンコードされます。
- 認証方法とスペース、すなわち「Basic 」が符号化された文字列の前に置かれる。

.. note:: For a user ``Aladdin`` having password ``open sesame`` the combined string would be:
   ``Aladdin`` + ``:`` + ``open sesame`` 
    and then encoded using Base64 the result will be ``QWxhZGRpbjpvcGVuIHNlc2FtZQ==``.

トークン認証を使用したリクエストヘッダーの例：

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




必要なリクエストパラメータ
------------------------------

client_code
    新しいライセンスを要求するクライアントの識別子。この識別子は、Ezpertログインプロセスのライセンスおよびその他の情報とともに保存されます。

ライセンス
    取り消すライセンスキー


HTTPリクエストの例
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

`CLIENT0001`というコードでクライアントのライセンスを1つリクエストします

.. code-block:: bash

    $ curl \
    --data "client_code=CLIENT0001&license=ACTR-9QGO-BNCC-JWM0" \
    --user username:password \
    http://ezpert.com/shop/api/license/revoke.action


.. note ::上記のサンプルはlinuxで `` curl``コマンドを利用していますので、使用する環境合わせて確認する必要があります。

.. note :: **.NET**ユーザーの場合、.NETアプリケーションを介して要求を送信する際の参照と、
   下記ののウェブサイトで利用可能です：

   * https://msdn.microsoft.com/en-us/library/debx8sh9(v=vs.110).aspx
   * https://msdn.microsoft.com/en-us/library/system.net.httpwebrequest(v=vs.110).aspx
   * https://msdn.microsoft.com/en-us/library/system.net.httpwebrequest.headers(v=vs.110).aspx
   * https://msdn.microsoft.com/en-us/library/system.web.httprequest.inputstream.aspx
   * https://msdn.microsoft.com/en-us/library/system.web.script.serialization.javascriptserializer.aspx

リスポンスタイプ
--------------------

JSON出力
^^^^^^^^^^^^^^^^^

リスポンスが `` json``の場合、結果は以下の応答と似ているものが出ると思います：

.. code-block:: json

    {
        "client_code": "CLIENT0001",
        "license": ["ACTR-9QGO-BNCC-JWM0"]
        "status": "REVOKED"
    }


XML出力
^^^^^^^^^^^^^^^

リスポンスが `` xml``の場合、結果は以下のサンプルと似ていると思います：

.. code-block:: xml

    <?xml version="1.0" encoding="UTF-8"?>
    <Response>
        <ClientCode>CLIENT0001</ClientCode
        <Licenses>
            <License>ACTR-9QGO-BNCC-JWM0</License>
        <Licenses>
        <Status>REVOKED</Status>
    </Response>



エラーリスポンス
---------------------

認証資格情報が送信されていない場合::

    {
        "error": {
            "message": "Unauthorized operation."
        }
    }


認証情報が有効でない場合、認証は次のリスポンスになり、失敗します。::

    {
        "error": {
            "type": "org.springframework.security.BadCredentialsException",
            "message": "Login failed - username or password incorrect; nested exception is java.lang.RuntimeException: Login failed - username or password incorrect"
        }
    }
