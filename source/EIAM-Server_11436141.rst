===========================
EIAM Server
===========================

.. container::
   :name: page

   .. container:: aui-page-panel
      :name: main

      .. container::
         :name: main-header

         .. container::
            :name: breadcrumb-section

            #. `Eden Platform <index.html>`__
            #. `EdenChain Doc <EdenChain-Doc_120848728.html>`__
            #. `dApp Development <dApp-Development_124780598.html>`__

         .. rubric:: EIAM Server
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by James Ahn, last modified by Hailee Kim on Mar 27,
            2019

         .. container:: wiki-content group
            :name: main-content

            .. rubric:: Overview
               :name: EIAMServer-Overview

            EIAM is an identity server in Edenchain platform. IAM plays
            important role because it provides identity information to
            requester.

            IAM does not have a UI, and all functions are provided using
            JSON RPC.

            | 

            EIAM is not exposed to the outside world for security
            purposes, only through the API Gateway service.

            | 

            | 

            .. rubric:: Functions
               :name: EIAMServer-Functions

            .. container:: table-wrap

               +----------------+-----------------------------------------------------+
               | Function       | Description                                         |
               +================+=====================================================+
               | Signup         | User Register                                       |
               +----------------+-----------------------------------------------------+
               | Signin         | Login                                               |
               +----------------+-----------------------------------------------------+
               | Signout        | Logout                                              |
               +----------------+-----------------------------------------------------+
               | update_profile | Update user profile information                     |
               +----------------+-----------------------------------------------------+
               | get_userinfo   | User info                                           |
               |                |                                                     |
               |                | Provide every information hold by the user.         |
               +----------------+-----------------------------------------------------+
               | sign_transacti | Sign submitted transaction after a validation       |
               | on             | check                                               |
               +----------------+-----------------------------------------------------+
               | del_user       | Delete user's information from EIAM.                |
               +----------------+-----------------------------------------------------+
               | add_eth_addres | Register ETH Address hold by the user.              |
               | s              |                                                     |
               |                | User is allowed to register multiple ETH Addresses. |
               +----------------+-----------------------------------------------------+
               | del_eth_addres | Delete ETH Address.                                 |
               | s              |                                                     |
               +----------------+-----------------------------------------------------+

            .. rubric:: Data Model
               :name: EIAMServer-DataModel

            The below tables explains data provided by identity server

            .. container:: table-wrap

               ================ ======== =========== ========================================================================
               Name             Type     Cardinality Description
               ================ ======== =========== ========================================================================
               Display Name              1:1         Name
               Userid                    1:1        
               Password                  1:1        
               email                     1:1        
               ETH address               1:M         Handled using String instead of separate tables for speed and simplicity
               TEDN private key          1:1         Crypted
               TEDN public key           1:1         Used as a TEDN Address.
               member_since     Datetime             When user does sign-up
               last_login       Datetime             It stores the time whenever the user logs in.
               last_logout      Datetime             It stores the time whenever the user logs out.
               ================ ======== =========== ========================================================================

            .. rubric:: ETH Address Field
               :name: EIAMServer-ETHAddressField

            An ETH address must be able to store multiple data because
            one user can have multiple ETH addresses.

            Because of the nature of the IAM server, you do not have to
            create a separate table.

            The ETH address consists of the following.

            -  \|eth_addr0|eth_addr1|eth_addr2|eth_addr3|eth_addr4

            Because it is configured as above, some data can not be read
            as it is, and it must be split and processed using '|'.

            .. rubric:: EIAM User
               :name: EIAMServer-EIAMUser

            IAM has two kinds of users.

            -  Normal User

               -  Normal users performing the common Auth functions such
                  as Signup, Signin, Signout.
               -  There is a functional limitation and you can only
                  check your own information
               -  Tokens use tokens generated through authentication.

            -  Server User

               -  It refers to the servers that exist in the Eden
                  Platform and is a server to perform specific functions
                  such as Coin, Balance, and TX Servers.
               -  The Server User can use by calling every API without
                  any functional limitations.
               -  It uses TLS that uses a certificate using its own CA
                  without using a token through authentication.

            .. rubric:: Database
               :name: EIAMServer-Database

            The IAM uses a model called EUser.

            For reasons of performance and simplicity, only one EUser
            model is used, which seems to be sufficient at this point.

            | 

               class **EUser**\ (db.Model):

                  \ *"""A naive Contact model."""*

                   

                  # Basic info.

                   email = db.StringProperty()

                   passwd = db.StringProperty()

                   display_name = db.StringProperty()

                   birth_day = db.DateProperty()

                  # Address info.

                   address = db.StringProperty()

                   eth_address = db.StringProperty(default='')

                  # Phone info.

                   phone_number = db.StringProperty()

                  # Company info.

                   tedn_private_key = db.StringProperty()

                   tedn_public_key = db.StringProperty()

                   member_since = db.DateTimeProperty(auto_now_add=True)

                   last_login = db.DateTimeProperty()

                   last_logout = db.DateTimeProperty()

            .. rubric:: Signin, Signout Handling
               :name: EIAMServer-Signin,SignoutHandling

            EUser has two datetime fields added: last_login and
            last_logout.

            Each time Sign-in, Sign-out JSON-RPC call occurs, the
            Timestamp value is recorded in the above two fields.

            For example, if the Current Timestamp value is 100 and
            last_signout is 90, the call is considered to have occurred
            after signing out and returns an error message.

            It handles the signout message of the JSON RPC call in this
            way.

            .. rubric:: Signing Transaction
               :name: EIAMServer-SigningTransaction

            The EIAM Server has the ability to sign for a Submitted
            transaction.

            In principle, EIAM is an Identity Server, so Transaction
            Sign is not correct, but Transaction Signing is handled by
            EIAM for Private Key Protection.

            After processing the signing, it passes the value to the
            Requester.

            For more information, see the JSON RPC Description section.

            .. container:: page

               .. container:: page

                  .. container:: layoutArea

                     .. container:: page

                        .. container:: layoutArea

                           .. container:: column

                              .. container:: page

                                 .. container:: layoutArea

                                    .. container:: column

                                       ::

            .. rubric:: TEDN Wallet
               :name: EIAMServer-TEDNWallet

            The TEDN wallet makes the Token easy to use inside the
            Edenchain.

            At this time, TEDN is a virtual token which is defined
            slightly different from Native Token of Mainnet and can be
            used easily and quickly in E-Garden. It is pegging 1: 1 with
            EDN.

            It is a form of PKI using ECDSA. Random generation uses two
            values: private key and public key. The public key is used
            as TEDN wallet address.

            | 

            .. rubric:: JSON-RPC Specification
               :name: EIAMServer-JSON-RPCSpecification

            The token_id that is created after authentication is called
            by using a JavaScript called call_jsonrpc. It does not need
            to be included because it attaches token_id to the Ajax call
            header.

            Therefore, we did not include the token_id to the parameter
            in the following document.

            The API below does not need to be called directly by the
            developer or user but is used by other services.

            | 

                 function call_jsonrpc(uri,method,param,callback)

                 { 

                 console.log('call_jsonrpc',arr_param);

                 $.ajax(backendHostUrl + uri , {

                       headers: {

                         'Authorization': 'Bearer ' + userIdToken

                       },

                   method: 'POST',

                   data: JSON.stringify( {"jsonrpc":"2.0",
               "method":method, "params":param, "id":1} ),

                   contentType : 'application/json'

                   }).then(function(response){

                   callback(response);

                   });

                 }

            .. rubric:: user.update_profile
               :name: EIAMServer-user.update_profile

            -  POST /api

            .. rubric:: Parameter
               :name: EIAMServer-Parameter

            .. container:: table-wrap

               ============ ====== ============================
               Name         Type   Description
               ============ ====== ============================
               display_name String Name displayed in the screen
               ============ ====== ============================

            .. rubric:: Response
               :name: EIAMServer-Response

            -  err_code
            -  msg
            -  data

               -  display_name

            | 

            .. rubric:: user.get_info
               :name: EIAMServer-user.get_info

            -  POST /api

            user.get_info is for the general user to retrieve his / her
            information. The user can only retrieve his / her own
            information, and no other user information is known. So you
            do not need a separate parameter.

            .. rubric:: Parameter
               :name: EIAMServer-Parameter.1

            -  None!!!

            .. rubric:: Response
               :name: EIAMServer-Response.1

            -  err_code
            -  msg
            -  data

               -  email
               -  address
               -  eth_address

                  -  eth_address1
                  -  eth_address2

            .. rubric:: user.signup
               :name: EIAMServer-user.signup

            -  POST /api

            In EIAM, when the EIAM Server receives signup, it creates a
            user. It creates a tedn_wallet for the user created, and
            stores it on the server.

            .. rubric:: Parameter
               :name: EIAMServer-Parameter.2

            .. container:: table-wrap

               ==== ==== ===========
               Name Type Description
               ==== ==== ===========
               ==== ==== ===========

            In the test, there is no parameter because no user
            information is input, but user information can be added
            later.

            .. rubric:: Response
               :name: EIAMServer-Response.2

            -  err_code
            -  msg
            -  data

            .. rubric:: user.signin
               :name: EIAMServer-user.signin

            -  POST /api

            Adjusts the time of last_siginin, which causes an error that
            other users access when signed out.

            Therefore, it is possible to register multiple browsers with
            one account at the same time. However, once you sign out
            from one of the browsers, you will be signed out from all
            accounts. Therefore, you have to log in again.

            If you do not have a wallet, signup is done internally.

            .. rubric:: Parameter
               :name: EIAMServer-Parameter.3

            .. container:: table-wrap

               ==== ==== ===========
               Name Type Description
               ==== ==== ===========
               ==== ==== ===========

            .. rubric:: Response
               :name: EIAMServer-Response.3

            -  err_code
            -  msg
            -  data

            .. rubric:: user.signout
               :name: EIAMServer-user.signout

            -  POST /api

            | 

            .. rubric:: Parameter
               :name: EIAMServer-Parameter.4

            .. container:: table-wrap

               ==== ==== ===========
               Name Type Description
               ==== ==== ===========
               ==== ==== ===========

            .. rubric:: Response
               :name: EIAMServer-Response.4

            -  err_code
            -  msg
            -  data

            | 

            .. rubric:: server.user_info
               :name: EIAMServer-server.user_info

            -  POST /api

            It can request information of a specific user, not a
            connected user. This function can access only specific
            services or servers.

            When you pass a token, it retrieves the user by its value
            and passes the information.

            Servers will use TLS to handle them without separate token
            authentication

            .. rubric:: Parameter
               :name: EIAMServer-Parameter.5

            .. container:: table-wrap

               ===== ====== ========================
               Name  Type   Description
               ===== ====== ========================
               token String Authentificate the Token
               ===== ====== ========================

            .. rubric:: Response
               :name: EIAMServer-Response.5

            -  err_code
            -  msg
            -  data

               -  email
               -  address
               -  eth_address

                  -  eth_address1
                  -  eth_address2

            .. rubric:: user.delete
               :name: EIAMServer-user.delete

            -  POST /api

            It is possible for the user to delete only his / her own
            information stored in the EIAM DB.

            .. rubric:: Parameter
               :name: EIAMServer-Parameter.6

            .. container:: table-wrap

               ==== ==== ===========
               Name Type Description
               ==== ==== ===========
               ==== ==== ===========

            .. rubric:: Response
               :name: EIAMServer-Response.6

            -  err_code
            -  msg
            -  data

            | 

            .. rubric:: server.delete_user
               :name: EIAMServer-server.delete_user

            -  POST /api

            Since it is used by the server by deleting the user
            information stored in the EIAM DB, it is possible to delete
            a specific user.

            .. rubric:: Parameter
               :name: EIAMServer-Parameter.7

            .. container:: table-wrap

               ===== ====== ========================
               Name  Type   Description
               ===== ====== ========================
               token string Authentificate the Token
               ===== ====== ========================

            .. rubric:: Response
               :name: EIAMServer-Response.7

            -  err_code
            -  msg
            -  data

            | 

            .. rubric:: eth.\ *add_address*
               :name: EIAMServer-eth.add_address

            -  POST /api

            | 

            Add the ETH Address to the current user.

            Compare the Signature value with the ETH Address value to
            determine if the person to add the ETH Address is really the
            owner of the account.

            This is for security reasons and to add after confirming
            that the person who is adding the specific ETH address is
            really the owner of the account.

            For this reason, the client should proceed in the following
            order.

            -  Generate Signing Key using Private Key
            -  Sign the Signing Key by putting ETH Address as a message
            -  Send generated Signature, ETH Address, and Public Key to
               the server as parameters

            | 

            If the server identifies and confirms that the signature is
            correct, it stores the ETH address on the server.

            .. rubric:: Parameter
               :name: EIAMServer-Parameter.8

            .. container:: table-wrap

               ========== ====== ==========================================
               Name       Type   Description
               ========== ====== ==========================================
               address    string eth address to add (with 0x)
               public_key string The public key of ETH account (without 0x)
               signature  string Signature of Signing Key (with 0x)
               ========== ====== ==========================================

            .. rubric:: Response
               :name: EIAMServer-Response.8

            -  err_code
            -  msg
            -  data

            | 

            .. rubric:: eth.del\ *\_address*
               :name: EIAMServer-eth.del_address

            -  POST /api

            Delete the ETH Address for the current user.

            Just like adding ETH Address, it creates Signing Key and
            sends the signed Signature and other parameters to the
            server.

            .. rubric:: Parameter
               :name: EIAMServer-Parameter.9

            .. container:: table-wrap

               ========== ====== =============================
               Name       Type   Description
               ========== ====== =============================
               address    string eth address to delete
               public_key string The public key of ETH account
               signature  string Signature of Signing Key
               ========== ====== =============================

            .. rubric:: Response
               :name: EIAMServer-Response.9

            -  err_code
            -  msg
            -  data

            .. rubric:: server.sign_transaction
               :name: EIAMServer-server.sign_transaction

            -  POST /api

            Call this when you want to sign in specific data. Find the
            email from the Token value, read the TEDN private key from
            the DB, sign the data and return the Signature.

            .. rubric:: Parameter
               :name: EIAMServer-Parameter.10

            .. container:: table-wrap

               ===== ====== ============================================================================================
               Name  Type   Description
               ===== ====== ============================================================================================
               token String Delivers Authentification token value to parameter
               msg   String The contents of the message to be signed is (String type and it is based on base64 encoding.
               ===== ====== ============================================================================================

            | 

            .. rubric:: Response
               :name: EIAMServer-Response.10

            -  err_code
            -  msg
            -  data

               -  signed transaction signature

            | 

            | 

            --------------

            --------------

            .. rubric:: Overview
               :name: EIAMServer-Overview.1

            EIAM is an identity server in Edenchain platform. IAM plays
            important role because it provides identity information to
            requester

            IAM은 UI를 가지고 있지 않으며 모든 기능은 JSON RPC를
            이용해서 제공한다. 

            | 

            EIAM은 보안상 실제 서비스를 위해서 외부에 드러나지 않으며,
            API Gateway서비스를 통해서만 제공된다.

            | 

            | 

            .. rubric:: Functions
               :name: EIAMServer-Functions.1

            .. container:: table-wrap

               +----------------+-----------------------------------------------------+
               | Function       | Description                                         |
               +================+=====================================================+
               | Signup         | 사용자등록                                          |
               +----------------+-----------------------------------------------------+
               | Signin         | 로그인                                              |
               +----------------+-----------------------------------------------------+
               | Signout        | 로그아웃                                            |
               +----------------+-----------------------------------------------------+
               | update_profile | 사용자 프로파일 정보 업데이트                       |
               +----------------+-----------------------------------------------------+
               | get_userinfo   | User info                                           |
               |                |                                                     |
               |                | 사용자가 가지고 있는 모든 정보를 제공한다.          |
               +----------------+-----------------------------------------------------+
               | sign_transacti | Sign submitted transaction after validation check   |
               | on             |                                                     |
               +----------------+-----------------------------------------------------+
               | del_user       | 사용자정보를 EIAM에서 삭제한다.                     |
               +----------------+-----------------------------------------------------+
               | add_eth_addres | 사용자가 가지고 있는 ETH Address를 등록             |
               | s              |                                                     |
               |                | 사용자는 다수의 ETH Address를 등록할 수 있다.       |
               +----------------+-----------------------------------------------------+
               | del_eth_addres | ETH Address 삭제한다.                               |
               | s              |                                                     |
               +----------------+-----------------------------------------------------+

            .. rubric:: Data Model
               :name: EIAMServer-DataModel.1

            The below tables explains data provided by identity server

            .. container:: table-wrap

               ================ ======== =========== ===========================================================================
               Name             Type     Cardinality Description
               ================ ======== =========== ===========================================================================
               Display Name              1:1         이름 
               Userid                    1:1        
               Password                  1:1        
               email                     1:1        
               ETH address               1:M         속도와 간편성을 위해 별도의 테이블을 사용하지 않고 String을 사용해서 처리함
               TEDN private key          1:1         암호화 되어 있다.
               TEDN public key           1:1         TEDN Address로 활용한다. 
               member_since     Datetime             When user does sign-up
               last_login       Datetime             사용자가 Login을 할때 마다 그 시간을 저장한다.
               last_logout      Datetime             사용자가 Logout을 할때 마다 그 시간을 저장한다.
               ================ ======== =========== ===========================================================================

            .. rubric:: ETH Address Field
               :name: EIAMServer-ETHAddressField.1

            ETH Address는 한명의 사용자가 여러개의 ETH Address를 가질 수
            있기 때문에 복수의 데이터를 저장할 수 있어야 한다.

            IAM서버는 특성상 액세스가 많기 때문에 별도의 테이블을
            구성하지 않고 EUser Table 하나에 모든 것을 집어 넣어
            처리하도록 한다.

            ETH Adress는 다음과 같이 구성한다.

            -  \|eth_addr0|eth_addr1|eth_addr2|eth_addr3|eth_addr4

            위와 같이 구성하기 때문에 약간은 데이터를 그대로 읽지 못하고
            '|'를 이용해 split해서 처리해야 한다.

            .. rubric:: EIAM User
               :name: EIAMServer-EIAMUser.1

            IAM은 2가지 종류의 User를 가지고 있다. 

            -  Normal User

               -  Signup, Signin, Signout등 일반적인 Auth기능을 수행하는
                  정상적인 사용자
               -  기능상의 제약이 있으며, 자신의 정보만 확인할 수 있음 
               -  Token은 인증을 통해  생성되는 토큰을 이용한다.

            -  Server User

               -  Eden Platform내에 존재하는 서버들을 지칭하는 것으로
                  Coin, Balance, TX Server들처럼 특정 기능을 수행하기
                  위한 서버이다.
               -  Server User는 기능상의 제약없이 모든 API를 호출해
                  사용할 수 있다.
               -  인증을 통한 토큰을 사용하지 않고 자체 CA를 사용한
                  인증서를 사용하는 TLS를 이용한다.

            .. rubric:: Database
               :name: EIAMServer-Database.1

            IAM은 EUser라는 Model을 만들어 사용하고 있다.

            Performance와 Simplicity의 이유로 EUser Model 하나만을
            사용하며, 현시점에서는 이것으로 충분해보인다.

            | 

               class **EUser**\ (db.Model):

                  \ *"""A naive Contact model."""*

                   

                  # Basic info.

                   email = db.StringProperty()

                   passwd = db.StringProperty()

                   display_name = db.StringProperty()

                   birth_day = db.DateProperty()

                  # Address info.

                   address = db.StringProperty()

                   eth_address = db.StringProperty(default='')

                  # Phone info.

                   phone_number = db.StringProperty()

                  # Company info.

                   tedn_private_key = db.StringProperty()

                   tedn_public_key = db.StringProperty()

                   member_since = db.DateTimeProperty(auto_now_add=True)

                   last_login = db.DateTimeProperty()

                   last_logout = db.DateTimeProperty()

            .. rubric:: Signin, Signout Handling
               :name: EIAMServer-Signin,SignoutHandling.1

            EUser에 last_login, last_logout이라는 2개의 Datetime Field가
            추가되어 있다.

            Sign-in,Sign-out JSON-RPC Call이 발생할때마다 위의 2개
            Field에 Timestamp 값이 기록된다.

            예를 들어 Current Timestamp 값이 100 이고, last_signout이 90
            이면 해당 Call은 signout된 후에 발생한 것으로 생각하고,
            에러메시지를 반환한다.

            이러한 방식으로 JSON RPC Call의 signout 메시지를 처리한다.

            | 

            .. rubric:: Signing Transaction
               :name: EIAMServer-SigningTransaction.1

            EIAM Server에서는 Submitted transaction에 대해 Sign을 하는
            기능을 가지고 있다.

            원칙적으로 이야기하면 EIAM은 Identity Server이기 때문에
            Transaction Sign이 맞지 않지만 Private Key Protection을 위해
            Transaction Signing을 EIAM에서 처리한다.

            Signing을 처리한 후에 해당 값을 Requester에 전달한다.

            자세한 내용은 JSON RPC설명부분을 보면 된다.

            .. container:: page

               .. container:: page

                  .. container:: layoutArea

                     .. container:: page

                        .. container:: layoutArea

                           .. container:: column

                              .. container:: page

                                 .. container:: layoutArea

                                    .. container:: column

                                       ::

            .. rubric:: TEDN Wallet
               :name: EIAMServer-TEDNWallet.1

            TEDN Wallet은 에덴체인 내부에서 Token을 쉽게 사용할 수
            있도록 한 것이다.

            이때, TEDN은 Main Net의 Native Token과는 조금 다른 것으로,
            E-Garden에서 쉽고 빠르게 여러 App을 사용할 수 있도록 정의한
            Virtual Token이다. EDN과는 1:1 로 pegging하고 있다.

            ECDSA를 사용한 PKI 형태이며  생성시에는  Random
            Generation하고 Private Key, Public Key 2개의 값을 이용하며,
            **Public Key를 TEDN Wallet Address로 활용한다.**

            | 

            .. rubric:: JSON-RPC Specification
               :name: EIAMServer-JSON-RPCSpecification.1

            인증 후에 생성되는 token_id는 call_jsonrpc라는
            자바스크립트를 사용해서 호출하면 Ajax call header에
            token_id를 붙이기 때문에 별도로 포함시킬 필요가 없음

            따라서 아래 문서의 parameter에는 token_id를 포함시키지
            않았다.

            아래 API는 개발사나 사용자가 직접 호출할 일은 없으며, 다른
            서비스에 의해 이용된다.

            | 

                 function call_jsonrpc(uri,method,param,callback)

                 { 

                 console.log('call_jsonrpc',arr_param);

                 $.ajax(backendHostUrl + uri , {

                       headers: {

                         'Authorization': 'Bearer ' + userIdToken

                       },

                   method: 'POST',

                   data: JSON.stringify( {"jsonrpc":"2.0",
               "method":method, "params":param, "id":1} ),

                   contentType : 'application/json'

                   }).then(function(response){

                   callback(response);

                   });

                 }

            .. rubric:: user.update_profile
               :name: EIAMServer-user.update_profile.1

            -  POST /api

            .. rubric:: Parameter
               :name: EIAMServer-Parameter.11

            .. container:: table-wrap

               ============ ====== =============================
               Name         Type   Description
               ============ ====== =============================
               display_name String 화면에 표시할때 사용하는 이름
               ============ ====== =============================

            .. rubric:: Response
               :name: EIAMServer-Response.11

            -  err_code
            -  msg
            -  data

               -  display_name

            | 

            .. rubric:: user.get_info
               :name: EIAMServer-user.get_info.1

            -  POST /api

            user.get_info는 일반 사용자가 자신의 정보를 가져오기 위한
            것으로 자신의 정보만 가져올 수 있으며, 다른 사용자정보는
            알수가 없다. 그래서 별도의 파라메터가 필요없다.

            .. rubric:: Parameter
               :name: EIAMServer-Parameter.12

            -  None!!!

            .. rubric:: Response
               :name: EIAMServer-Response.12

            -  err_code
            -  msg
            -  data

               -  email
               -  address
               -  eth_address

                  -  eth_address1
                  -  eth_address2

            .. rubric:: user.signup
               :name: EIAMServer-user.signup.1

            -  POST /api

            EIAM에서는 EIAM Server는 signup을 받으면 해당 사용자를 생성
            후  생성된 사용자에게 tedn_wallet를 생성하고 이것을 서버에
            저장한다.

            .. rubric:: Parameter
               :name: EIAMServer-Parameter.13

            .. container:: table-wrap

               ==== ==== ===========
               Name Type Description
               ==== ==== ===========
               ==== ==== ===========

            테스트에서는 별도의 사용자정보를 입력받지 않았기 때문에
            파라메터가 없는데, 추후 간단하게 사용자정보를 추가할 수
            있다.

            .. rubric:: Response
               :name: EIAMServer-Response.13

            -  err_code
            -  msg
            -  data

            .. rubric:: user.signin
               :name: EIAMServer-user.signin.1

            -  POST /api

            last_siginin시간을 조정하며, 이를 통해 signout되었을 때 다른
            사용자가 access하는 것을 에러가 나게 한다.

            따라서, 만약 1개의 계정으로 동시에 여러 Browser에 등록하는
            것은 가능하나, 그 중 하나에서 signout하였을 경우 모든
            계정에서 signout되므로 다시 로그인하여야 한다.

            또한, signin을 하였을때 만약 wallet이 없다면 내부적으로
            signup을 수행하도록 한다.

            .. rubric:: Parameter
               :name: EIAMServer-Parameter.14

            .. container:: table-wrap

               ==== ==== ===========
               Name Type Description
               ==== ==== ===========
               ==== ==== ===========

            .. rubric:: Response
               :name: EIAMServer-Response.14

            -  err_code
            -  msg
            -  data

            .. rubric:: user.signout
               :name: EIAMServer-user.signout.1

            -  POST /api

            | 

            .. rubric:: Parameter
               :name: EIAMServer-Parameter.15

            .. container:: table-wrap

               ==== ==== ===========
               Name Type Description
               ==== ==== ===========
               ==== ==== ===========

            .. rubric:: Response
               :name: EIAMServer-Response.15

            -  err_code
            -  msg
            -  data

            | 

            .. rubric:: server.user_info
               :name: EIAMServer-server.user_info.1

            -  POST /api

            접속해 있는 사용자가 아니라 특정사용자의 정보를 요청할 수
            있는 것으로 이 기능은 특정 서비스나 서버들만 접속이
            가능하다.

            token을 전달하면 그 값으로 사용자를 검색해 정보를 전달한다.

            서버들은 별도의 토큰인증없이 TLS를 사용해서 처리할 것이다.

            .. rubric:: Parameter
               :name: EIAMServer-Parameter.16

            .. container:: table-wrap

               ===== ====== ===========
               Name  Type   Description
               ===== ====== ===========
               token String 인증 Token
               ===== ====== ===========

            .. rubric:: Response
               :name: EIAMServer-Response.16

            -  err_code
            -  msg
            -  data

               -  email
               -  address
               -  eth_address

                  -  eth_address1
                  -  eth_address2

            .. rubric:: user.delete
               :name: EIAMServer-user.delete.1

            -  POST /api

            EIAM의 DB에 저장되어 있는 사용자정보를 삭제하는 것으로
            자신의 사용자정보만 삭제할 수 있다.

            .. rubric:: Parameter
               :name: EIAMServer-Parameter.17

            .. container:: table-wrap

               ==== ==== ===========
               Name Type Description
               ==== ==== ===========
               ==== ==== ===========

            .. rubric:: Response
               :name: EIAMServer-Response.17

            -  err_code
            -  msg
            -  data

            | 

            .. rubric:: server.delete_user
               :name: EIAMServer-server.delete_user.1

            -  POST /api

            EIAM의 DB에 저장되어 있는 사용자정보를 삭제하는 것으로
            서버에서 사용하는 것이기 때문에 특정 사용자를 지정해
            삭제하는 것이 가능하다.

            .. rubric:: Parameter
               :name: EIAMServer-Parameter.18

            .. container:: table-wrap

               ===== ====== ===========
               Name  Type   Description
               ===== ====== ===========
               token string 인증 Token
               ===== ====== ===========

            .. rubric:: Response
               :name: EIAMServer-Response.18

            -  err_code
            -  msg
            -  data

            | 

            .. rubric:: eth.\ *add_address*
               :name: EIAMServer-eth.add_address.1

            -  POST /api

            현재 사용하고 있는 사용자에 ETH Address를 추가한다.

            ETH Address를 추가하려는 사람이 진짜 해당 계좌의 주인인지를
            확인하기 위해 Signature값과 ETH Address 값을 비교한다.

            이렇게 하는 가장 주된 이유는 보안문제로, 특정 ETH address를
            추가하려는 사람이 진짜 해당 계좌의 주인인지를 확인한 후에
            추가하기 위함이다.

            이러한 이유로 client에서는 다음과 순서로 진행해야 한다.

            -  Private Key를 이용해서 Signing Key 생성 
            -  Signing Key에 ETH Address를 메시지로 넣어서 사인
            -  생성된 Signature, ETH Address, Public Key를 파라메터로
               서버에 전송

            서버에서 Signature를 확인후 올바르다고 확인되면 해당 ETH
            Address를 서버에 저장한다.

            .. rubric:: Parameter
               :name: EIAMServer-Parameter.19

            .. container:: table-wrap

               ========== ====== ========================================
               Name       Type   Description
               ========== ====== ========================================
               address    string 추가하려고 하는 eth address (0x가 있다.)
               public_key string ETH account의 public key (0x가 없다.)
               signature  string Signing Key의 Signature (0x가 있다.)
               ========== ====== ========================================

            .. rubric:: Response
               :name: EIAMServer-Response.19

            -  err_code
            -  msg
            -  data

            | 

            .. rubric:: eth.del\ *\_address*
               :name: EIAMServer-eth.del_address.1

            -  POST /api

            현재 사용하고 있는 사용자에 ETH Address를 삭제한다.

            ETH Address를 추가하는 것과 마찬가지로 Signing Key생성후
            사인한 Signature와 다른 파라메터를 서버에 전송한다.

            .. rubric:: Parameter
               :name: EIAMServer-Parameter.20

            .. container:: table-wrap

               ========== ====== ===========================
               Name       Type   Description
               ========== ====== ===========================
               address    string 삭제하려고 하는 eth address
               public_key string ETH account의 public key
               signature  string Signing Key의 Signature
               ========== ====== ===========================

            .. rubric:: Response
               :name: EIAMServer-Response.20

            -  err_code
            -  msg
            -  data

            .. rubric:: server.sign_transaction
               :name: EIAMServer-server.sign_transaction.1

            -  POST /api

            특정 데이터에 Sign을 하고 싶을 때 호출한다. Token 값에서
            email을 찾고, 그것으로 TEDN private key를 DB에서 읽어오고,
            이것으로 데이터에 사인을 하고 Signature를 반환한다.

            .. rubric:: Parameter
               :name: EIAMServer-Parameter.21

            .. container:: table-wrap

               ===== ====== ===================================================================
               Name  Type   Description
               ===== ====== ===================================================================
               token String 인증 token 값을 파라메터로 전달한다.
               msg   String Sign할 메시지 내용(String Type이고 base64 Encoding을 기준으로 한다.
               ===== ====== ===================================================================

            | 

            .. rubric:: Response
               :name: EIAMServer-Response.21

            -  err_code
            -  msg
            -  data

               -  signed transaction signature

            | 

            | 

   .. container::
      :name: footer

      .. container:: section footer-body

         Document generated by Confluence on Mar 27, 2019 18:40

         .. container::
            :name: footer-logo

            `Atlassian <http://www.atlassian.com/>`__

