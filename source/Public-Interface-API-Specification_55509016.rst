==================================================
Eden Platform : Public Interface API Specification
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
            #. `dApp Development <dApp-Development_124780598.html>`__

         .. rubric:: Eden Platform : Public Interface API Specification
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by Jacki Heo, last modified by Hailee Kim on Mar 27,
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

            --------------

            --------------

            .. rubric:: Overview
               :name: PublicInterfaceAPISpecification-Overview.1

            E-Wallet과의 연동을 위한 interface를 설명한다.

            | 

            .. rubric:: Interfaces
               :name: PublicInterfaceAPISpecification-Interfaces.1

            모든 interface는 JSON-RPC 2.0으로 작성되어 있다.  모든
            Amount의 decimal은 18이다. 

            JSON-RPC의 result값은 통일된 Response 형태의 JSON으로
            구성된다.

            | 

            -  API Response

               -  Err code

                  -  Error Code로 실행의 결과를 알려준다.

               -  Err Message

                  -  메시지가 필요한 경우에 이곳을 이용해 메시지를
                     전달한다.

               -  Data

                  -  처리 결과로 제공되어야 하는 데이터가 있는 경우
                     이곳에 데이터를 집어넣는다.

            -  Example

               -  {'err_code':0 , 'msg': 'ok', 'data':{} }

               -  {'err_code':0 , 'msg': 'ok', 'data': { 'data1':1,
                  'data2':2 } }
               -  {'err_code':-1 , 'msg': 'no matched data', 'data': {}
                  }

            -  Error code

            Error Code가 '0'인 경우에는 성공을 뜻하고, 음수의 경우
            Server Error, 양수인 경우 Client Error로 정의한다.

            Predefined Error Code는 다음과 같다.

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
                  :name: PublicInterfaceAPISpecification-ConfirmEDNdeposit.1

            EDN을 TEDN으로 변환하기 위해 EdenChain서버로 transfer를
            수행한 후 해당 transfer가 성공했는지 확인한다.

            result값이 True일 경우 성공한 것이며, 이 때 TEDN balance
            Check를 하여 잔액을 확인가능하다.

            | 

            POST /api

            **Request**

            **JSON-RPC method : "user.deposit"**

            **JSON-RPC Parameter:**

            "iamtoken" (string) - IAM 인증 토큰   - Firebase를 통한
            인증성공시 받아지는 값

            "txhash" (string) - Transaction Hash - Ethereum을 통해 EDN을
            전달했을때의 Transaction Hash값

            | 

            **Result**

            **        Status Code:**

            200  - OK

            202  - Transaction is pending

            404 - Requested TX not found

            **Return: inresult**

            정의된 API Response값으로 리턴

            성공: err_code=0

            | 

            .. rubric:: 2. Withdraw TEDN
               :name: PublicInterfaceAPISpecification-2.WithdrawTEDN.1

            TEDN을 EDN으로 변환할 때 쓰이는 API이다. 

            해당 API를 받으면 TEDN잔액이 충분하면 요청한 금액만큼
            decrease되며, 해당 TEDN은 EDN과 1:1 pegging되므로 해당 금액
            만큼 사용자에게 transfer된다. 

            리턴으로는 해당 tx hash값이 전달되므로, 해당 값을 이용하여
            tx가 성공했는지 Pending중인지 확인이 가능하다. 

            BlockChain의 특성상 실패할 수가 있으므로 이 경우 서버
            내부에서는 금액이 rollback되나, 사용자에게는 전달되는 항목이
            없으므로 따로 확인해야 한다.

            | 

            POST /api

            **Request**

            **JSON-RPC method : "user.withdraw"**

            **JSON-RPC Parameter:**

            "iamtoken" (string) - IAM 인증 토큰

            "ethaddress" (string) - to EDN address (may be registered to
            IAM profile)

            "amount" (string) - TDEN value(18 decimals) to withdraw/
            transaction fee (gas) is provided in CS wallet.

            **Result**

            **Status Code:**

            200 - OK

            403 - Your Request is forbidden

            **Return: in result**

            정의된 API Response값으로 리턴

            성공: err_code=0, data={'txhash': transaction_hash_value }

            | 

            .. rubric:: 3. Get CoinServer Ethereum Address
               :name: PublicInterfaceAPISpecification-3.GetCoinServerEthereumAddress.1

            Deposit을 위해서는 Edenchain서버로 EDN을 전달해야 한다. 
            해당 API를 수행하여 EdenChain으로 전달할 주소를 전달 받는다.

            | 

            POST /api

            **Request**

            **JSON-RPC method : "server.coinhdaddress"**

            **JSON-RPC Parameter: **

            "iamtoken" (string) - IAM 인증 토큰

            **Result**

            **Status Code:**

            200 - OK

            403 - Your Request is forbidden

            **Return: in result**

            정의된 API Response값으로 리턴

            성공: err_code=0, data={'hdaddress':
            coinserver_hd_address_to_transfer}

            | 

            .. rubric:: 4. Get My Balance
               :name: PublicInterfaceAPISpecification-4.GetMyBalance.1

            Edenchain서버로 TEDN의 현재 잔액을 요청한다.

            | 

            POST /api

            **Request**

            **JSON-RPC method : "user.getbalance"**

            **JSON-RPC Parameter:**

            "iamtoken" (string) - IAM 인증 토큰

            **Result**

            **        Status Code:**

            200  - OK

            **Return: in result**

            정의된 API Response값으로 리턴

            성공: err_code=0, data={'amount':
            current_balance_of_tedn_account}

            | 

            .. rubric:: 5. List User Transaction
               :name: PublicInterfaceAPISpecification-5.ListUserTransaction.1

            IAM token에 해당되는 사용자의 Transaction정보를 가져온다.

            from_addr과 to_addr모두 검색에서 확인되어야 한다.

            해당 Transaction은 SuperNode에서 Block Commit후 완료된
            정보이다. (TRANSACTIONCOMMIT을 의미한다.)

            | 

            POST /api

            **Request**

            **JSON-RPC method : "user.lstransaction"**

            **JSON-RPC Parameter:**

            "iamtoken" (string) - IAM 인증 토큰

            "page" (integer) - 읽어들일 page. 0이면 최근값이다.

            "countperpage" (integer) - 1페이지에 보여줄 레코드 값

            | 

            **Result**

            **        Status Code:**

            200  - OK

            202  - Transaction is pending

            404 - Requested TX not found

            **Return:  in result**

            정의된 API Response값으로 리턴

            성공: err_code=0, data={

            "totalcount" (integer) - 모든 count

            "currentpage" (integer) - request에서 요청한 현재 page

            "transactions" (array) - 다음 object의 array형태로 리턴된다.

             [ {"from_addr", (string) - amount가 빠져나가는 주소

                "to_addr", (string) - amount가 들어가는 주소

                "amount", (string) - amount, decimal 18자리 

               "regdate", (int) -  시간, 초단위

              }]

            }

            | 

            .. rubric:: 6. user.update_profile
               :name: PublicInterfaceAPISpecification-6.user.update_profile.1

            -  POST /api

            .. rubric:: Parameter
               :name: PublicInterfaceAPISpecification-Parameter.7

            .. container:: table-wrap

               .. container:: table-wrap

                  ======================================= ======================================= =======================================
                  .. container:: tablesorter-header-inner .. container:: tablesorter-header-inner .. container:: tablesorter-header-inner
                                                                                                 
                     Name                                    Type                                    Description
                  ======================================= ======================================= =======================================
                  iamtoken                                String                                  IAM 인증 토큰
                  display_name                            String                                  화면에 표시할때 사용하는 이름
                  ======================================= ======================================= =======================================

            .. rubric:: Response
               :name: PublicInterfaceAPISpecification-Response.7

            -  err_code
            -  msg
            -  data

               -  display_name

            | 

            .. rubric:: 7. user.get_info
               :name: PublicInterfaceAPISpecification-7.user.get_info.1

            -  POST /api

            user.get_info는 일반 사용자가 자신의 정보를 가져오기 위한
            것으로 자신의 정보만 가져올 수 있으며, 다른 사용자정보는
            알수가 없다. 그래서 별도의 파라메터가 필요없다.

            .. rubric:: Parameter
               :name: PublicInterfaceAPISpecification-Parameter.8

            .. container:: table-wrap

               ========= ======= ==============
               Name      Type    Description
               ========= ======= ==============
               ​iamtoken String​ IAM 인증 토큰​
               ========= ======= ==============

            .. rubric:: Response
               :name: PublicInterfaceAPISpecification-Response.8

            -  err_code
            -  msg
            -  data

               -  email
               -  address
               -  eth_address

                  -  eth_address1
                  -  eth_address2

            .. rubric:: 8. user.signup
               :name: PublicInterfaceAPISpecification-8.user.signup.1

            -  POST /api

            실질적인 Signup은 firebase에서 처리가 되기 때문에 EIAM에서는
            email, password의 정보외에 부수적인 처리를 하기 위해
            사용한다.

            EIAM Server는 signup을 받으면 해당 사용자를 EIAM의 NDB에
            생성하고, 생성된 사용자에게 tedn_wallet를 생성하고 이것을
            서버에 저장한다.

            .. rubric:: Parameter
               :name: PublicInterfaceAPISpecification-Parameter.9

            .. container:: table-wrap

               .. container:: table-wrap

                  ======== ======= =============
                  Name     Type    Description
                  ======== ======= =============
                  iamtoken String  IAM 인증 토큰
                  ======== ======= =============

            테스트에서는 별도의 사용자정보를 입력받지 않았기 때문에
            파라메터가 없는데, 추후 간단하게 사용자정보를 추가할 수
            있다.

            .. rubric:: Response
               :name: PublicInterfaceAPISpecification-Response.9

            -  err_code
            -  msg
            -  data

            .. rubric:: 9. user.signin
               :name: PublicInterfaceAPISpecification-9.user.signin.1

            -  POST /api

            Firebase에서 성공적으로 sign-in을 한후에 호출하는 것으로
            파라메터가 없다. 

            last_siginin시간을 조정하며, 이를 통해 signout되었을 때 다른
            사용자가 access하는 것을 에러가 나게 한다.

            따라서, 만약 1개의 계정으로 동시에 여러 Browser에 등록하는
            것은 가능하나, 그 중 하나에서 signout하였을 경우 모든
            계정에서 signout되므로 다시 로그인하여야 한다.

            또한, signin을 하였을때 만약 wallet이 없다면 내부적으로
            signup을 수행하도록 한다.

            .. rubric:: Parameter
               :name: PublicInterfaceAPISpecification-Parameter.10

            .. container:: table-wrap

               .. container:: table-wrap

                  ======== ====== =============
                  Name     Type   Description
                  ======== ====== =============
                  iamtoken String IAM 인증 토큰
                  ======== ====== =============

            .. rubric:: Response
               :name: PublicInterfaceAPISpecification-Response.10

            -  err_code
            -  msg
            -  data

            .. rubric:: 10. user.signout
               :name: PublicInterfaceAPISpecification-10.user.signout.1

            -  POST /api

            Firebase에서 성공적으로 sign-out을 한후에 호출하는 것으로
            파라메터가 없다. 현재는 별다른 처리를 하고 있지 않다.

            .. rubric:: Parameter
               :name: PublicInterfaceAPISpecification-Parameter.11

            .. container:: table-wrap

               .. container:: table-wrap

                  ======== ====== =============
                  Name     Type   Description
                  ======== ====== =============
                  iamtoken String IAM 인증 토큰
                  ======== ====== =============

            .. rubric:: Response
               :name: PublicInterfaceAPISpecification-Response.11

            -  err_code
            -  msg
            -  data

            .. rubric:: 11. eth.\ *add_address*
               :name: PublicInterfaceAPISpecification-11.eth.add_address.1

            -  POST /api

            현재 사용하고 있는 사용자에 ETH Address를 추가한다.

            ETH Address를 추가하려는 사람이 진짜 해당 계좌의 주인인지를
            확인하기 위해 Signature값과 ETH Address 값을 비교한다.

            이렇게 하는 가장 주된 이유는 보안문제로, 특정 ETH address를
            추가하려는 사람이 진짜 해당 계좌의 주인인지를 확인한 후에
            추가하기 위함이다.

            이러한 이유로 client에서는 다음과 순서로 진행해야 한다.

            - Private Key를 이용해서 Signing Key 생성 

            - Signing Key에 ETH Address를 메시지로 넣어서 사인

            - 생성된 Signature, ETH Address, Public Key를 파라메터로
            서버에 전송

            서버에서 Signature를 확인후 올바르다고 확인되면 해당 ETH
            Address를 서버에 저장한다.

            .. rubric:: Parameter
               :name: PublicInterfaceAPISpecification-Parameter.12

            .. container:: table-wrap

               .. container:: table-wrap

                  ======================================= ======================================= ========================================
                  .. container:: tablesorter-header-inner .. container:: tablesorter-header-inner .. container:: tablesorter-header-inner
                                                                                                 
                     Name                                    Type                                    Description
                  ======================================= ======================================= ========================================
                  iamtoken                                String                                  IAM 인증 토큰
                  address                                 string                                  추가하려고 하는 eth address (0x가 있다.)
                  public_key                              string                                  ETH account의 public key (0x가 없다.)
                  signature                               string                                  Signing Key의 Signature (0x가 있다.)
                  ======================================= ======================================= ========================================

            .. rubric:: Response
               :name: PublicInterfaceAPISpecification-Response.12

            -  err_code
            -  msg
            -  data

            | 

            .. rubric:: 12. eth.del\ *\_address*
               :name: PublicInterfaceAPISpecification-12.eth.del_address.1

            -  POST /api

            현재 사용하고 있는 사용자에 ETH Address를 삭제한다. 아래
            parameter는 위 명령 eth.add_address와 동일하다.

            .. rubric:: Parameter
               :name: PublicInterfaceAPISpecification-Parameter.13

            .. container:: table-wrap

               .. container:: table-wrap

                  ======================================= ======================================= =======================================
                  .. container:: tablesorter-header-inner .. container:: tablesorter-header-inner .. container:: tablesorter-header-inner
                                                                                                 
                     Name                                    Type                                    Description
                  ======================================= ======================================= =======================================
                  iamtoken                                string                                  IAM 인증 토큰
                  address                                 string                                  삭제하려고 하는 eth address
                  public_key                              string                                  ETH account의 public key
                  signature                               string                                  Signing Key의 Signature
                  ======================================= ======================================= =======================================

            .. rubric:: Response
               :name: PublicInterfaceAPISpecification-Response.13

            -  err_code
            -  msg
            -  data

            .. rubric:: Test
               :name: PublicInterfaceAPISpecification-Test.1

            #. 테스트 용 Beta Release 접속 URL

            `https://api-ep-br.edenchain.io/api/browse <https://api-ep-br.edenchain.io/api/browse/#/>`__

            .. rubric:: Copyright
               :name: PublicInterfaceAPISpecification-Copyright.1

            (c) 2018 Eden Partners, Inc. Company Confidential

            | 

            | 

   .. container::
      :name: footer

      .. container:: section footer-body

         Document generated by Confluence on Mar 27, 2019 15:01

         .. container::
            :name: footer-logo

            `Atlassian <http://www.atlassian.com/>`__
