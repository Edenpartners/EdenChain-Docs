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

            Created by James Ahn, last modified by Jacki Heo on Mar 28,
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

            .. code:: p1

               class EUser(db.Model):
                   """A naive Contact model."""
                  

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

            .. code:: p1

                 function call_jsonrpc(uri,method,param,callback)
                 {  
                  console.log('call_jsonrpc',arr_param);
                  $.ajax(backendHostUrl + uri , {
                       headers: {
                         'Authorization': 'Bearer ' + userIdToken
                       },

                    method: 'POST',
                    data: JSON.stringify( {"jsonrpc":"2.0", "method":method, "params":param, "id":1} ),
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
.. toctree::
   :maxdepth: 1
   
   eiam_data_encryption_120390674.rst



