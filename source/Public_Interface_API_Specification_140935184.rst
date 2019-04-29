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

         .. rubric:: Public Interface API Specification
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by Jacki Heo, last modified by Jay Lee on Apr 29,
            2019

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

            | 

            .. container:: table-wrap

               ============== ========================
               **Error Code** **Description**
               0              Success
               -1             Server Internal Error
               1              Client Parameter Missing
               ============== ========================

            | 

            .. rubric:: 1. Confirm EDN deposit
               :name: PublicInterfaceAPISpecification-1.ConfirmEDNdeposit

            To convert the EDN to TEDN, perform a transfer to the
            EdenChain server and verify that the transfer has succeeded.

            If the result value is True, it is successful and the TEDN
            balance can be confirmed.

            ::

               POST /api
                   Request
                       JSON-RPC method : "user.deposit"
                       JSON-RPC Parameter:
                           "iamtoken" (string) - IAM authentication token - value received when authentication through authentication is successful
                           "txhash" (string) - Transaction Hash - Transaction Hash value when EDN is delivered via Ethereum
                   Result
                       Status Code:
                           200  - OK
                           202  - Transaction is pending
                           404 - Requested TX not found
                   Return: inresult
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

            ::

               POST /api
                   Request
                       JSON-RPC method : "user.withdraw"
                       JSON-RPC Parameter:
                           "iamtoken" (string) - IAM authentication token
                           "ethaddress" (string) - to EDN address (may be registered to IAM profile)
                           "amount" (string) - TDEN value(18 decimals) to withdraw/ transaction fee (gas) is provided in CS wallet.
                   Result
                       Status Code:
                           200 - OK
                           403 - Your Request is forbidden
                   Return: in result
                       Return to defined API Response value
                       Success: err_code = 0, data = {'txhash': transaction_hash_value}

            | 

            .. rubric:: 3. Get CoinServer Ethereum Address
               :name: PublicInterfaceAPISpecification-3.GetCoinServerEthereumAddress

            For deposits, EDN must be delivered to the EdenChain server.
            It executes the API and receives the address to be
            transferred to the EdenChain.

            ::

               POST /api
                   Request
                       JSON-RPC method : "server.coinhdaddress"
                       JSON-RPC Parameter: 
                           "iamtoken" (string) - IAM authentication token
                   Result
                       Status Code:
                           200 - OK
                           403 - Your Request is forbidden
                   Return: in result
                       Return to the defined API Response value
                       Success: err_code=0, data={'hdaddress': coinserver_hd_address_to_transfer}

            | 

            .. rubric:: 4. Get My Balance
               :name: PublicInterfaceAPISpecification-4.GetMyBalance

            Requests the current balance of TEDN through EdenChain
            server.

            ::

               POST /api
                   Request
                       JSON-RPC method : "user.getbalance"
                       JSON-RPC Parameter:
                           "iamtoken" (string) - IAM Authentification Token
                   Result
                       Status Code:
                           200  - OK
                   Return: in result
                       Return to defined API Response value
                       Success: err_code=0, data={'amount': current_balance_of_tedn_account}

            | 

            .. rubric:: 5. List User Transaction
               :name: PublicInterfaceAPISpecification-5.ListUserTransaction

            Get transaction information of user corresponding to IAM
            token.

            Both from_addr and to_addr should be verified in the search.

            The transaction is the information completed after block
            committing in SuperNode. (This means TRANSACTIONCOMMIT.)

            ::

               POST /api
                   Request
                       JSON-RPC method : "user.lstransaction"
                       JSON-RPC Parameter:
                           "iamtoken" (string) - IAM authentification token
                           "page" (integer) - Page to be read. It is a recent value if it is 0.
                           "countperpage" (integer) - Record value to display on page 1
                   Result
                       Status Code:
                           200  - OK
                           202  - Transaction is pending
                           404 - Requested TX not found
                   Return: in result
                       Return to defined API Response Value
                       Success:
                           err_code=0,
                           data={
                               "totalcount" (integer) - Total count
                               "currentpage" (integer) - Current page requested by the request
                               "transactions" (array) - It is returned as an array of the following object.
                               [ 
                                   {
                                   "from_addr", (string) - The address where the amount is withdrawn.
                                   "to_addr", (string) - The address where the amount is deposited. 
                                   "amount", (string) - amount, decimal 18 digits
                                   "regdate", (int) -  Time, in second
                                   }
                               ]
                           }

            | 

            .. rubric:: 6. Update User Profile
               :name: PublicInterfaceAPISpecification-6.UpdateUserProfile

            ::

               POST /api
                   Request
                       JSON-RPC method : "user.update_profile"
                       JSON-RPC Parameter:
                           "iamtoken" (string) - IAM authentification token
                           "display_name" (string) - Name displayed in the screen
                   Result
                       Status Code:
                           200  - OK
                   Return: in result
                       Return to the defined API Response value
                       Success: err_code=0, data=current_user_display_name, msg=ok

            | 

            .. rubric:: 7. Get User Info
               :name: PublicInterfaceAPISpecification-7.GetUserInfo

            user.get_info is for the general user to retrieve his / her
            information, no other user information is known. A separate
            parameter is not required.

            ::

               POST /api
                   Request
                       JSON-RPC method : "user.get_info"
                       JSON-RPC Parameter:
                           "iamtoken" (string) - IAM authentification token
                   Result
                       Status Code:
                           200  - OK
                   Return: in result
                       Return to the defined API Response value
                       Success:
                           err_code=0,
                           data={
                               "display_name" (string) - Current user display name
                               "email" (string) - Current user E-mail Address
                               "eth_address" (array) - Current user Ethereum Address.
                               "phone_number" (string) - Current user phone number.
                               "tedn_public_key"(string) - Current user TEDN Address.
                           }

            | 

            .. rubric:: 8. User Sign-Up
               :name: PublicInterfaceAPISpecification-8.UserSign-Up

            Because the actual signup is handled by authentication, EIAM
            uses it for incidental processing besides email and password
            information.

            When the EIAM Server receives the signup, it creates the
            user in the NDB of the EIAM. And it creates a tedn_wallet
            for the created user, and stores it on the server.

            In the test, there is no parameter because no user
            information is input, but user information can be added
            later.

            ::

               POST /api
                   Request
                       JSON-RPC method : "user.signup"
                       JSON-RPC Parameter:
                           "iamtoken" (string) - IAM authentification token
                   Result
                       Status Code:
                           200  - OK
                   Return: in result
                       Return to the defined API Response value
                       Success:
                           err_code=0,
                           data={
                               "display_name" (string) - Current user display name
                               "email" (string) - Current user E-mail Address
                               "eth_address" (array) - Current user Ethereum Address.
                               "phone_number" (string) - Current user phone number.
                               "tedn_public_key"(string) - Current user TEDN Address.
                           }

            | 

            .. rubric:: 9. user.signin
               :name: PublicInterfaceAPISpecification-9.user.signin

            There is no parameter to call after successful sign-in in
            authentication.

            Adjusts the time of last_siginin, which causes an error when
            other users access after signed out.

            Therefore, it is possible to register multiple browsers with
            one account at the same time. However, once you sign out
            from one of the browsers, you will be signed out from all
            accounts. Therefore, login is required.

            If you do not have a wallet, signup is done internally.

            ::

               POST /api
                   Request
                       JSON-RPC method : "user.signup"
                       JSON-RPC Parameter:
                           "iamtoken" (string) - IAM authentification token
                   Result
                       Status Code:
                           200  - OK
                   Return: in result
                       Return to the defined API Response value
                       Success: err_code=0, msg=ok

            | 

            .. rubric:: 10. user.signout
               :name: PublicInterfaceAPISpecification-10.user.signout

            There is no parameter to call after successful sign-out in
            authentication. Nothing is being done at this time.

            ::

               POST /api
                   Request
                       JSON-RPC method : "user.signout"
                       JSON-RPC Parameter:
                           "iamtoken" (string) - IAM authentification token
                   Result
                       Status Code:
                           200  - OK
                   Return: in result
                       Return to the defined API Response value
                       Success: err_code=0, msg=ok

            | 

            .. rubric:: 11. Add Ethereum Address
               :name: PublicInterfaceAPISpecification-11.AddEthereumAddress

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

            ::

               POST /api
                   Request
                       JSON-RPC method : "eth.add_address"
                       JSON-RPC Parameter:
                           "iamtoken" (string) - IAM authentification token
                           "address" (string) - eth address to add (with 0x)
                           "public_key" (string) - A public key of ETH account(without 0x)
                           "signature" (string) - Signature of Signing Key(with 0x)
                   Result
                       Status Code:
                           200  - OK
                   Return: in result
                       Return to defined API Response Value
                       Success: err_code=0, msg=ok

            | 

            .. rubric:: 12. Delete Ethereum Address
               :name: PublicInterfaceAPISpecification-12.DeleteEthereumAddress

            Delete the ETH Address for the current user. The following
            parameter is equivalent to the above command,
            eth.add_address.

            ::

               POST /api
                   Request
                       JSON-RPC method : "eth.del_address"
                       JSON-RPC Parameter:
                           "iamtoken" (string) - IAM authentification token
                           "address" (string) - eth address to add (with 0x)
                           "public_key" (string) - A public key of ETH account(without 0x)
                           "signature" (string) - Signature of Signing Key(with 0x)
                   Result
                       Status Code:
                           200  - OK
                   Return: in result
                       Return to defined API Response Value
                       Success: err_code=0, msg=ok

            .. rubric:: 13. TEDN Token Transfer
               :name: PublicInterfaceAPISpecification-13.TEDNTokenTransfer

            TEDN Token Transfer between users.

            ::

               POST /api
                   Request
                       JSON-RPC method : "user.transfer"
                       JSON-RPC Parameter:
                           "iamtoken" (string) - IAM authentification token
                           "receive_tedn_address" (string) - tedn address to receive
                           "amount" (string) - Amount to send (in decimal 18)
                   Result
                       Status Code:
                           200  - OK
                   Return: in result
                       Return to defined API Response Value
                       Success: err_code=0, msg=ok, data={'tx_id'=Transaction Id of Send/Receive Transaction}

            | 

            .. rubric:: Test
               :name: PublicInterfaceAPISpecification-Test

            URL to access the test version of the Beta Release

            -  `https://api-ep-br.EdenChain.io/api/browse <https://api-ep-br.edenchain.io/api/browse/#/>`__

            | 

            URL to access the test version of Candidate Release

            -  `https://api-ep-cr.EdenChain.io/api/browse <https://api-ep-br.edenchain.io/api/browse/#/>`__

            .. rubric:: Copyright
               :name: PublicInterfaceAPISpecification-Copyright

            (c) 2018 Eden Partners, Inc. Company Confidential

   .. container::
      :name: footer

      .. container:: section footer-body

         Document generated by Confluence on Apr 29, 2019 18:42

         .. container::
            :name: footer-logo

            `Atlassian <http://www.atlassian.com/>`__




