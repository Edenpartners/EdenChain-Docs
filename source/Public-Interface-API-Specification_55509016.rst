==================================================
Public Interface API Specification
==================================================

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

         .. rubric:: Public Interface API Specification
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by Jacki Heo, last modified on Mar 28, 2019

         .. container:: wiki-content group
            :name: main-content

            .. rubric:: Overview
               :name: PublicInterfaceAPISpecification-Overview

            It is to explain the interface for interworking with
            E-Wallet.

            | 

            .. rubric:: Interfaces
               :name: PublicInterfaceAPISpecification-Interfaces

            All interfaces are written in JSON-RPC 2.0. The decimal of
            all Amounts is 18.

            The result value of JSON-RPC consists of JSON with a unified
            response form.

            | 

            -  API Response

               -  Err code

                  -  Informs the result of the execution with the error
                     code. 

               -  Err Message

                  -  Use it to deliver the message, if needed.

               -  Data

                  -  If there is data that should be provided as a
                     result of processing, insert data here.

            -  Example

               -  {'err_code':0 , 'msg': 'ok', 'data':{} }

               -  {'err_code':0 , 'msg': 'ok', 'data': { 'data1':1,
                  'data2':2 } }
               -  {'err_code':-1 , 'msg': 'no matched data', 'data': {}
                  }

            -  Error code

            If the error code is '0', it indicates success, and if it is
            negative, it is defined as Server Error and if it is
            positive, it is defined as Client Error.

            The predefined error codes are as follows.

            .. container:: table-wrap

               .. container:: table-wrap

                  ======================================= =======================================
                  .. container:: tablesorter-header-inner .. container:: tablesorter-header-inner
                                                         
                     Error Code                              Description
                  ======================================= =======================================
                  0                                       Success
                  -1                                      Server Internal Error
                  1                                       Client Parameter Missing
                  ======================================= =======================================

            | 

            #. .. rubric:: Confirm EDN deposit
                  :name: PublicInterfaceAPISpecification-ConfirmEDNdeposit

            To convert the EDN to TEDN, perform a transfer to the
            EdenChain server and verify that the transfer is succeeded.

            If the result value is True, it is successful. Here, TEDN
            balance can be confirmed.

            | 

            POST /api

            **Request**

            **JSON-RPC method : "user.deposit"**

            **JSON-RPC Parameter:**

            "iamtoken" (string) - IAM authentication token - value
            received when authentication through Firebase is successful

            "txhash" (string) - Transaction Hash - Transaction Hash
            value when EDN is delivered via Ethereum

            | 

            **Result**

            **        Status Code:**

            200  - OK

            202  - Transaction is pending

            404 - Requested TX not found

            **Return: inresult**

            Return to defined API Response value

            Success: err_code = 0

            | 

            .. rubric:: 2. Withdraw TEDN
               :name: PublicInterfaceAPISpecification-2.WithdrawTEDN

            It is an API used to convert TEDN to EDN.

            Upon receipt of the API, if the TEDN balance is sufficient,
            it is decreased by the amount requested. The TEDN is pegged
            1: 1 with the EDN, so the amount is transferred to the user.

            Since the corresponding tx hash value is transmitted as a
            return, it is possible to check whether tx is succeeded or
            not by using the corresponding value.

            In this case, the money is rolled back within the server,
            but there is no item to be transmitted to the user.
            Therefore, it must be checked separately.

            | 

            POST /api

            **Request**

            **JSON-RPC method : "user.withdraw"**

            **JSON-RPC Parameter:**

            "iamtoken" (string) - IAM authentication token

            "ethaddress" (string) - to EDN address (may be registered to
            IAM profile)

            "amount" (string) - TDEN value(18 decimals) to withdraw/
            transaction fee (gas) is provided in CS wallet.

            **Result**

            **Status Code:**

            200 - OK

            403 - Your Request is forbidden

            **Return: in result**

            Return to defined API Response value

            Success: err_code = 0, data = {'txhash':
            transaction_hash_value}

            | 

            .. rubric:: 3. Get CoinServer Ethereum Address
               :name: PublicInterfaceAPISpecification-3.GetCoinServerEthereumAddress

            For deposits, EDN must be delivered to the Edenchain server.
            It executes the API and receives the address to be
            transferred to the EdenChain.

            | 

            POST /api

            **Request**

            **JSON-RPC method : "server.coinhdaddress"**

            **JSON-RPC Parameter: **

            "iamtoken" (string) - IAM authentication token

            **Result**

            **Status Code:**

            200 - OK

            403 - Your Request is forbidden

            **Return: in result**

            Return to the defined API Response value

            Success: err_code=0, data={'hdaddress':
            coinserver_hd_address_to_transfer}

            | 

            .. rubric:: 4. Get My Balance
               :name: PublicInterfaceAPISpecification-4.GetMyBalance

            Requests the current balance of TEDN through Edenchain
            server.

            | 

            POST /api

            **Request**

            **JSON-RPC method : "user.getbalance"**

            **JSON-RPC Parameter:**

            "iamtoken" (string) - IAM Authentification Token

            **Result**

            **        Status Code:**

            200  - OK

            **Return: in result**

            Return to defined API Response value

            Success: err_code=0, data={'amount':
            current_balance_of_tedn_account}

            | 

            .. rubric:: 5. List User Transaction
               :name: PublicInterfaceAPISpecification-5.ListUserTransaction

            Get transaction information of user corresponding to IAM
            token.

            Both from_addr and to_addr should be verified in the search.

            The transaction is the information completed after block
            committing in SuperNode. (This means TRANSACTIONCOMMIT.)

            | 

            POST /api

            **Request**

            **JSON-RPC method : "user.lstransaction"**

            **JSON-RPC Parameter:**

            "iamtoken" (string) - IAM authentification token

            "page" (integer) - Page to be read. It is a recent value if
            it is 0.

            "countperpage" (integer) - Record value to display on page 1

            | 

            **Result**

            **        Status Code:**

            200  - OK

            202  - Transaction is pending

            404 - Requested TX not found

            **Return:  in result**

            Return to defined API Response Value

            Success: err_code=0, data={

            "totalcount" (integer) - Total count

            "currentpage" (integer) - Current page requested by the
            request

            "transactions" (array) - It is returned as an array of the
            following object.

             [ {"from_addr", (string) - The address where the amount is
            withdrawn.

                "to_addr", (string) - The address where the amount is
            deposited. 

                "amount", (string) - amount, decimal 18 digits

               "regdate", (int) -  Time, in second

              }]

            }

            | 

            .. rubric:: 6. user.update_profile
               :name: PublicInterfaceAPISpecification-6.user.update_profile

            -  POST /api

            .. rubric:: Parameter
               :name: PublicInterfaceAPISpecification-Parameter

            .. container:: table-wrap

               .. container:: table-wrap

                  ======================================= ======================================= =======================================
                  .. container:: tablesorter-header-inner .. container:: tablesorter-header-inner .. container:: tablesorter-header-inner
                                                                                                 
                     Name                                    Type                                    Description
                  ======================================= ======================================= =======================================
                  iamtoken                                String                                  IAM Authentification Token
                  display_name                            String                                  Name displayed in the screen
                  ======================================= ======================================= =======================================

            .. rubric:: Response
               :name: PublicInterfaceAPISpecification-Response

            -  err_code
            -  msg
            -  data

               -  display_name

            | 

            .. rubric:: 7. user.get_info
               :name: PublicInterfaceAPISpecification-7.user.get_info

            -  POST /api

            user.get_info is for the general user to retrieve his / her
            information, no other user information is known. A separate
            parameter is not required.

            .. rubric:: Parameter
               :name: PublicInterfaceAPISpecification-Parameter.1

            .. container:: table-wrap

               ========= ======= ========================
               Name      Type    Description
               ========= ======= ========================
               ​iamtoken String​ IAM Authentication Token
               ========= ======= ========================

            .. rubric:: Response
               :name: PublicInterfaceAPISpecification-Response.1

            -  err_code
            -  msg
            -  data

               -  email
               -  address
               -  eth_address

                  -  eth_address1
                  -  eth_address2

            .. rubric:: 8. user.signup
               :name: PublicInterfaceAPISpecification-8.user.signup

            -  POST /api

            Because the actual signup is handled by firebase, EIAM uses
            it for incidental processing besides email and password
            information.

            When the EIAM Server receives the signup, it creates the
            user in the NDB of the EIAM. And it creates a tedn_wallet
            for the created user, and stores it on the server.

            .. rubric:: Parameter
               :name: PublicInterfaceAPISpecification-Parameter.2

            .. container:: table-wrap

               .. container:: table-wrap

                  ======== ======= ==========================
                  Name     Type    Description
                  ======== ======= ==========================
                  iamtoken String  IAM Authentification Token
                  ======== ======= ==========================

            In the test, there is no parameter because no user
            information is input, but user information can be added
            later.

            .. rubric:: Response
               :name: PublicInterfaceAPISpecification-Response.2

            -  err_code
            -  msg
            -  data

            .. rubric:: 9. user.signin
               :name: PublicInterfaceAPISpecification-9.user.signin

            -  POST /api

            There is no parameter to call after successful sign-in in
            Firebase.

            Adjusts the time of last_siginin, which causes an error when
            other users access after signed out.

            Therefore, it is possible to register multiple browsers with
            one account at the same time. However, once you sign out
            from one of the browsers, you will be signed out from all
            accounts. Therefore, login is required.

            If you do not have a wallet, signup is done internally.

            .. rubric:: Parameter
               :name: PublicInterfaceAPISpecification-Parameter.3

            .. container:: table-wrap

               .. container:: table-wrap

                  ======== ====== ==========================
                  Name     Type   Description
                  ======== ====== ==========================
                  iamtoken String IAM Authentification Token
                  ======== ====== ==========================

            .. rubric:: Response
               :name: PublicInterfaceAPISpecification-Response.3

            -  err_code
            -  msg
            -  data

            .. rubric:: 10. user.signout
               :name: PublicInterfaceAPISpecification-10.user.signout

            -  POST /api

            There is no parameter to call after successful sign-out in
            Firebase. Nothing is being done at this time.

            .. rubric:: Parameter
               :name: PublicInterfaceAPISpecification-Parameter.4

            .. container:: table-wrap

               .. container:: table-wrap

                  ======== ====== ==========================
                  Name     Type   Description
                  ======== ====== ==========================
                  iamtoken String IAM Authentification Token
                  ======== ====== ==========================

            .. rubric:: Response
               :name: PublicInterfaceAPISpecification-Response.4

            -  err_code
            -  msg
            -  data

            .. rubric:: 11. eth.\ *add_address*
               :name: PublicInterfaceAPISpecification-11.eth.add_address

            -  POST /api

            Add the ETH Address to the current user.

            Compare the Signature value with the ETH Address value to
            determine if the person to add the ETH Address is really the
            owner of the account.

            This is for security reasons and to add after it is
            confirmed that the person who is adding the specific ETH
            address is really the owner of the account.

            For this reason, the following order is required to proceed
            in the client.

            - Generate Signing Key using Private Key

            - Sign by adding the ETH Address on Signing Key as a message

            - Send generated Signature, ETH Address, and Public Key to
            the server as parameters

            If the server identifies and confirms the signature is
            correct, it stores the ETH address on the server.

            .. rubric:: Parameter
               :name: PublicInterfaceAPISpecification-Parameter.5

            .. container:: table-wrap

               .. container:: table-wrap

                  ======================================= ======================================= =======================================
                  .. container:: tablesorter-header-inner .. container:: tablesorter-header-inner .. container:: tablesorter-header-inner
                                                                                                 
                     Name                                    Type                                    Description
                  ======================================= ======================================= =======================================
                  iamtoken                                String                                  IAM Authentification Token
                  address                                 string                                  eth address to add (with 0x)
                  public_key                              string                                  A public key of ETH account(without 0x)
                  signature                               string                                  Signature of Signing Key(with 0x)
                  ======================================= ======================================= =======================================

            .. rubric:: Response
               :name: PublicInterfaceAPISpecification-Response.5

            -  err_code
            -  msg
            -  data

            | 

            .. rubric:: 12. eth.del\ *\_address*
               :name: PublicInterfaceAPISpecification-12.eth.del_address

            -  POST /api

            Delete the ETH Address for the current user. The following
            parameter is equivalent to the above command,
            eth.add_address.

            .. rubric:: Parameter
               :name: PublicInterfaceAPISpecification-Parameter.6

            .. container:: table-wrap

               .. container:: table-wrap

                  ======================================= ======================================= =======================================
                  .. container:: tablesorter-header-inner .. container:: tablesorter-header-inner .. container:: tablesorter-header-inner
                                                                                                 
                     Name                                    Type                                    Description
                  ======================================= ======================================= =======================================
                  iamtoken                                string                                  IAM Authentification Token
                  address                                 string                                  eth address to delete
                  public_key                              string                                  A public key of ETH account
                  signature                               string                                  Signature of Signing Key
                  ======================================= ======================================= =======================================

            .. rubric:: Response
               :name: PublicInterfaceAPISpecification-Response.6

            -  err_code
            -  msg
            -  data

            .. rubric:: Test
               :name: PublicInterfaceAPISpecification-Test

            #. URL to access the test version of the Beta Release

            `https://api-ep-br.edenchain.io/api/browse <https://api-ep-br.edenchain.io/api/browse/#/>`__

            .. rubric:: Copyright
               :name: PublicInterfaceAPISpecification-Copyright

            (c) 2018 Eden Partners, Inc. Company Confidential

            | 

            | 

   .. container::
      :name: footer

      .. container:: section footer-body

         Document generated by Confluence on Mar 28, 2019 12:30

         .. container::
            :name: footer-logo

            `Atlassian <http://www.atlassian.com/>`__

