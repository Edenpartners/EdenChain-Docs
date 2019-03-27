========================================================
Install SDK & Development for Javascript
========================================================

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
            #. `SDK_Docs <SDK_Docs_124813380.html>`__
            #. `Javascript <Javascript_122848134.html>`__

         .. rubric:: Install SDK & Development for
            Javascript
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by Jacki Heo, last modified by Hailee Kim on Mar 27,
            2019

         .. container:: wiki-content group
            :name: main-content

            #. .. rubric:: Development Environment
                  :name: InstallSDK&DevelopmentforJavascript-DevelopmentEnvironment

            The OS environment does not matter what you write.

            For development, Node v10 or higher, NPM v6.9 or higher
            should be installed.

            | 

            .. rubric:: 2. Install
               :name: InstallSDK&DevelopmentforJavascript-2.Install

            In the project directory, do the following.

            | 

            ::

               npm install eden-js-sdk-client

            | 

            If there is no error, installation to the relevant library
            is completed.

            .. rubric:: 3. Provided API
               :name: InstallSDK&DevelopmentforJavascript-3.ProvidedAPI

            All APIs are provided through the edensdk class.

            | 

            -  .. rubric:: Network settings 
                  :name: InstallSDK&DevelopmentforJavascript-Networksettings

            Set up the network of the Edenchain to connect through the
            SDK. There are currently three networks. You have to set up
            a network to test, which is always the first thing you need
            to do when using the SDK.

            | 

            .. rubric:: (1)  Network constant
               :name: InstallSDK&DevelopmentforJavascript-(1)Networkconstant

            ::

               EDEN_MAINNET_NETWORK      : Edenchain MainNet General Release 
               EDEN_CANDIDATE_NETWORK    : Edenchain Candidate Release
               EDEN_BETA_NETWORK         : Edenchain Beta Release

            | 

            .. rubric:: (2) Network Configuration API
               :name: InstallSDK&DevelopmentforJavascript-(2)NetworkConfigurationAPI

            The API must always be executed first.

            | 

               boolean initApp(network_id)

            ::

               parameter  :
                            network_id puts the above network constants.

               return     :
                            If true, initialization is successful; if false, initialization failed.
                             This usually happens when you set up a network that does not support it.

            | 

            -  .. rubric:: Authentication
                  :name: InstallSDK&DevelopmentforJavascript-Authentication

            It provides a sign in and sign up with Edenchain. Sign Up
            provides an Edenchain for creating accounts and wallets, and
            Sign In provides sign-in to pre-created accounts.

            | 

            .. rubric:: (1) Sign Up
               :name: InstallSDK&DevelopmentforJavascript-(1)SignUp

            Creates an account if you do not have an account

            | 

               app.auth().createUserWithEmailAndPassword( email,
               password )

            ::

               parameter  :
                            email (string): email address to use for future accounts.
                             password (string): The password of the account to be used in the future.

            | 

            .. rubric:: (2) Sign In
               :name: InstallSDK&DevelopmentforJavascript-(2)SignIn

            If you have an account, you can sign in to that account.
            Sign In fails if there is no account or the password is
            incorrect.

            | 

               app.auth().signInWithEmailAndPassword( email, password )

            ::

               parameter : 
                           email         (String)  : Account to sign In할 계정이다.
                           password  (String) :  Passsword to sing in the asccount
               return    :
                           Promise is returned. If successful, it is not handled directly, but uses the Auth Change Event defined below.
                            If it is an error, it can be processed by .catch ((error) => {}) and error can be checked by error.

            .. rubric:: (3) Sign Out
               :name: InstallSDK&DevelopmentforJavascript-(3)SignOut

            If it is currently signed in, call it and change it to Sign
            Out state. After this call, onAuthStateChanged () is not
            called and auto sign-in does not occur.

            | 

               app.auth().signOut()

            | 

            .. rubric:: (4) Authentication Change Event
               :name: InstallSDK&DevelopmentforJavascript-(4)AuthenticationChangeEvent

            Specifies the function to be called when the authentication
            state changes. This function is also called if you are
            logged in automatically after you are already logged in.

               app.auth().onAuthStateChanged( (user) => {} )

            | 

            The user specified in callback is the object that contains
            the user's information.

            The user can get token information to be used in the API.

            The call is as follows. Since getIdToken () is async, it can
            be obtained via await.

            | 

            ::

               token = await user.getIdToken()

            Therefore, the function to be called when it is needed
            should be specified as async.The function must call
            signInUser () or signOutUser () of , depending on whether
            the user is a user.

            | 

            -  .. rubric:: apis
                  :name: InstallSDK&DevelopmentforJavascript-apis

            The API instance provides the api except authentication.
            Call it from edensdk.apis.

            To use all api, you need a token that you received when
            authenticating. The token is used to invoke the api, and all
            modules in the Edenchain use the token to verify the
            authentication.

            All APIs are async.

            | 

            | 

               async getCoinServerAddress(iamtoken)

            CoinServer Returns the Ethereum Address. It returns the
            address of Ropsten Ethereum Testnet in Beta Release and
            Candidate Release, and returns the address of Ethereum
            MainNet in General Release.

            | 

            ::

               parameter  :
                            iamtoken ( String):  This is a token authenticated by getIdToken () of the user object obtained at the time of authentication.

               return     :
                            String            : Returns the Ethereum address of the Coin Server.


            ..

               async getUserBalance(iamtoken)

            Returns the user account balance of the Eden chain.

            | 

            ::

               parameter  :
                            iamtoken ( String ) :  This is a token authenticated by getIdToken () of the user object obtained at the time of authentication.

               return     :
                            int                 : Token blanace is decimal 18.

            | 

               async getUserInfo(iamtoken)

            It returns user information on the Edenchain. It mainly
            returns token address, token deposit, or ethereum address
            information to be used for withdraw.

            | 

            ::

               parameter:
                          iamtoken  ( String ) :  This is a token authenticated by getIdToken () of the user object obtained at the time of authentication.

               return:
                         {}                    : Returns an Object, with the following information:
                                email  ( String )       :  User email address
                                eth_address (String )   : It is the ethereum address of the user added / deleted by api, and may contain multiple addresses as delimiter with '|'.
                                                          withdraw or deposit of api will refuse to withdraw or deposit to unregistered address.
                                tedn_public_key (String):  Edenchain user wallet address

            | 

               async signInUser(iamtoken)

            It is an API that is used to sign in to the internal module
            separately from Authentication. When authentication ()
            succeeds, it should be called at any time. Therefore, it is
            usually called in the Authentication Event.

            | 

            ::

               parameter  :
                            iamtoken ( String ) :  This is a token authenticated by getIdToken () of the user object obtained at the time of authentication.

               return     :
                            Boolean             : signIn indicates success or failure.

            ..

               async signOutUser(iamtoken)

            This is the API that is called when signout is successful in
            the Authentication function. The call is not Mandatory and
            is usually called in an Authentication Event.

            | 

            ::

               parameter  :
                            iamtoken      ( String ) :  This is a token authenticated by getIdToken () of the user object obtained at the time of authentication.

               return     :
                            Boolean   : It indicates success or failure of signIn .

            ..

               async getTransactionList(iamtoken, page, countperpage)

            It is an API to get the transaction list of the user.
            Returns information from the transaction of the user
            corresponding to iamtoken.

            | 

            ::

               parameter  :
                            iamtoken ( String ) :  This is a token authenticated by getIdToken () of the user object obtained at the time of authentication.
                            page     (int)      : Specifies the next page transaction to return.
                            countperpage (int)  : By specifying the transaction count for each page and specifying the page, you specify how many transactions are returned.

               return:
                            [{}]        :   Returns a list of objects and each of which has the following information:
                                         from_addr  (String) :  Address in Edenchain to withdraw the amount
                                         to_addr    (String) : Address in Edenchain to deposit the amount
                                         amount     (int )   :  token amount corresponding to tx, and decimal 18.
                                         regdate    (int)    : The time at which tx was performed, in seconds.

            | 

               async addEthAddress(iamtoken,address)

            It is used to put Ethereum address in the user account. In
            order to prevent the misuse of the address of the other
            person, the address is signed and sent. The server processes
            it only when the signature is correct.

            | 

            ::

               parameter  :
                           iamtoken      ( String ) :  This is a token authenticated by getIdToken () of the user object obtained at the time of authentication.
                           address        (Object)  : An Object with the following information. The following objects are easily created by the providing api.
                           address  (String)        : Ethereum Checksum Address
                           public_key (String)      : Ethereum public key. This is used to verify the signature.
                           signature ( String )     :  It is a value signed with Ethereum private key, after the keccak256 hash of address.
               return     :
                           Boolean   : It indicates the success or failure of Ethereum address addition.

            ..

               async delEthAddress(iamtoken,address)

            It is used to delete the Ethereum address of the user
            account. In order to prevent the misuse of the address of
            the other person, the address is signed and sent. The server
            processes it only when the signature is correct.

            | 

            ::

               parameter  :
                            iamtoken      ( String ) :  This is a token authenticated by getIdToken () of the user object obtained at the time of authentication.
                            address        (Object)  : An Object with the following information. The following objects are easily created by the providing api.
                            address  (String)        : Ethereum Checksum Address
                            public_key (String)      : Ethereum public key. This is used to verify the signature.
                            signature ( String )     : It is a value signed with Ethereum private key, after the keccak256 hash of address.

               return     :
                            Boolean   : It indicates the success or failure of Ethereum address deletion.

            | 

               async depositTokenToEdenChain(iamtoken,txhash)

            It is the API that is called when the Ethereum ERC20 EDN
            Token is passed for the Edenchain service.

            | 

            ::

               parameter  :
                            iamtoken      ( String ) :  This is a token authenticated by getIdToken () of the user object obtained at the time of authentication.
                            txhash          (String)   : Transaction hash value after Ethereum transfer

               return     :
                             Boolean   : Indicates the success or failure of the API.

            ..

               async withdrawTokenToEdenChain(iamtoken,ethaddress,
               amount)

            It is the API that is called when the Ethereum ERC20 EDN
            Token is passed for the Edenchain service.

            | 

            ::

               parameter  :
                            iamtoken   ( String ):  It is the API that is called when the Ethereum ERC20 EDN Token is passed for the Edenchain service.
                            ethaddress (String ) :  The address on the Ethereum to deposit. It must be registered by addEthAddress () in advance.
                            amount     (int)     : Amount to receive and it is decimal 18.

               return      :
                            txhash  (String): Txhash value generated after Ethereum transfer in Coin Server. You can use that value to determine if the withdraw was successful.

            | 

            -  .. rubric:: utils
                  :name: InstallSDK&DevelopmentforJavascript-utils

            Called in the form of edensdk.utils, which is not related to
            api, but has the necessary utility function.

            | 

               makeAddressObject(private_key)

            This is a helper function to easily create the address
            object needed for the API that is called when the user adds
            / deletes the Ethereum address to Edenchain with the
            Ethereum private key.

            | 

            ::

               paramteter  :
                             private_key (String): Ethereum Private key.

               return      : 
                             address  (Object)  : An Object with the following information. The following objects are easily created by the providing api.
                                      address  (String)    : Ethereum Checksum Address
                                      public_key (String)  : Ethereum public key. This is used to verify the signature.
                                      signature ( String ) : After the keccak256 hash of address, it is signed with Ethereum private key.

            --------------

            --------------

            | 

            #. .. rubric:: 개발 환경
                  :name: InstallSDK&DevelopmentforJavascript-개발환경

            OS환경은 무엇을 쓰든 상관없다.

            개발을 위해서는 Node v10 이상, NPM v6.9이상을 설치하여야
            한다.

            | 

            .. rubric:: 2. 설치
               :name: InstallSDK&DevelopmentforJavascript-2.설치

            프로젝트 디렉토리에서 아래와 같이 수행한다.

            | 

            ::

               npm install eden-js-sdk-client

            | 
            | 오류가 없을 경우 관련 library까지 같이 설치가 완료된다.

            .. rubric:: 3. 제공 API
               :name: InstallSDK&DevelopmentforJavascript-3.제공API

            모든 API는 edensdk class를 통해 제공한다. 

            | 

            -  .. rubric:: 네트워크 설정 
                  :name: InstallSDK&DevelopmentforJavascript-네트워크설정

            SDK를 통해 접속할 에덴체인의 네트워크를 설정한다. 현재 3개의
            네트워크가 있으며 테스트할 네트워크를 설정하게 되는데 SDK를
            사용할 때 언제나 제일 먼저 초기화 해야 하는 작업이다.

            | 

            .. rubric:: (1)  네트워크 상수
               :name: InstallSDK&DevelopmentforJavascript-(1)네트워크상수

            ::

               EDEN_MAINNET_NETWORK      : Edenchain MainNet General Release 
               EDEN_CANDIDATE_NETWORK    : Edenchain Candidate Release
               EDEN_BETA_NETWORK         : Edenchain Beta Release

            | 

            .. rubric:: (2) 네트워크 설정 API
               :name: InstallSDK&DevelopmentforJavascript-(2)네트워크설정API

            해당 API는 언제나 처음에 수행되어야 한다.

            | 

               boolean initApp(network_id)

            ::

               parameter  :
                            network_id는 위 네트워크 상수를 넣는다.

               return     :
                            true인 경우 초기화가 성공한 것이고, false이면 초기화에 실패한 것이다. 
                            보통 지원하지 않는 네트워크를 설정하는 경우 발생된다.

            | 

            -  .. rubric:: Authentication
                  :name: InstallSDK&DevelopmentforJavascript-Authentication.1

            Edenchain으로의 Sign in과 Sign up을 지원한다.

            Sign Up은 Edenchain의 계정과 Wallet을 생성하기 위한 것을
            제공하며, Sign In은 기 생성된 계정으로의 Sign  In을
            제공한다.

            | 

            .. rubric:: (1) Sign Up
               :name: InstallSDK&DevelopmentforJavascript-(1)SignUp.1

            계정이 없을 경우 계정을 생성한다.

            | 

               app.auth().createUserWithEmailAndPassword( email,
               password )

            ::

               parameter  :
                            email       (String) :  앞으로 사용할 계정으로 사용할 email주소이다.
                            password (String) : 앞으로 사용할 계정의 암호이다.

               return     :
                            Promise를 리턴하며, 성공일 경우에는 직접 처리하지 않고, 아래에서 정의되는 Authentication Change Event를 사용한다.
                            에러일 경우에는 .catch( (error) => {} ) 에서 처리하고, error를 통해 에러를 확인할 수 있다.

            | 

            .. rubric:: (2) Sign In
               :name: InstallSDK&DevelopmentforJavascript-(2)SignIn.1

            계정이 있을 경우 해당 계정으로 Sign In이 가능하다. 없거나
            암호가 틀릴 경우 Sign In이 실패한다.

            | 

               app.auth().signInWithEmailAndPassword( email, password )

            ::

               parameter : 
                           email         (String)  : Sign In할 계정이다.
                           password  (String) :  Sign In할 계정의 암호이다.

               return    :
                           Promise를 리턴하며, 성공일 경우에는 직접 처리하지 않고, 아래에서 정의되는 Auth Change Event를 사용한다.
                           에러일 경우에는 .catch( (error) => {} ) 에서 처리하고, error를 통해 에러를 확인할 수 있다.

            | 

            .. rubric:: (3) Sign Out
               :name: InstallSDK&DevelopmentforJavascript-(3)SignOut.1

            현재 Sign In되어 있는 경우 호출하여 Sign Out상태로 변경한다.
            호출하면 이후에는  onAuthStateChanged()가 호출되지 않아 자동
            Sign In이 되지 않는다.

            | 

               app.auth().signOut()

            | 

            .. rubric:: (4) Authentication Change Event
               :name: InstallSDK&DevelopmentforJavascript-(4)AuthenticationChangeEvent.1

            인증 상태가 변경될 때 호출되는 함수를 지정한다. 이미
            로그인되어 있을 경우 자동으로 로그인을 하는 경우에도 역시
            호출되는 함수이다.

            | 

               app.auth().onAuthStateChanged( (user) => {} )

            | 

            callback에서 지정되는 user는 사용자의 정보를 가지고 있는
            object이다.

            user에서 API에서 사용될 token정보를 얻을 수 있다. 

            아래와 같이 호출하게 되는데, getIdToken()이 async이므로
            await를 통해 얻을 수 있다.

            | 

            ::

               token = await user.getIdToken()

            | 
            | 따라서, 필요시  호출되는 함수는 async로 지정하여야 한다.

            해당 함수에서는 user의 여부에 따라 apis의 signInUser()또는
            signOutUser()을 호출하여야 한다.

            | 

            -  .. rubric:: apis
                  :name: InstallSDK&DevelopmentforJavascript-apis.1

            API instance는 인증을 제외한 나머지 api를 제공한다.
            edensdk.apis에서 호출한다.

            모든 api의 사용을 위해서는 인증시 받은 token이 필요하다. 이
            token을 사용하여 api를 호출하게 되는데, 에덴체인 내의 모든
            모듈은 해당 token을 사용하여  인증을 확인한다.

            모든 API는 async 형태이다.

            | 

               async getCoinServerAddress(iamtoken)

            CoinServer Ethereum Address를 리턴한다. Beta Release와
            Candidate Release까지는 Ropsten Ethereum Testnet의 주소를
            리턴하며, General Release시에는 Ethereum MainNet상의 주소를
            리턴한다.

            | 

            ::

               parameter  :
                            iamtoken ( String):  Authentication시 얻어지는 user object의 getIdToken()에 의해 인증되는 token이다.

               return     :
                            String            : Coin Server의 Ethereum 주소를 리턴한다.


            ..

               async getUserBalance(iamtoken)

            에덴체인의 사용자 계정 Balance를 리턴한다.

            | 

            ::

               parameter  :
                            iamtoken ( String ) :  Authentication시 얻어지는 user object의 getIdToken()에 의해 인증되는 token이다.

               return     :
                            int                 : decimal 18인 Token balance이다.

            | 

               async getUserInfo(iamtoken)

            에덴체인 상의 사용자 정보를 리턴한다. 주로 token address
            또는 token deposit, withdraw에 사용될 ethereum 주소 정보
            등을 리턴한다.

            | 

            ::

               parameter:
                          iamtoken  ( String ) :  Authentication시 얻어지는 user object의 getIdToken()에 의해 인증되는 token이다.

               return:
                         {}                    : Object를 리턴하며, 다음 정보를 가진다.
                                email  ( String )       :  사용자 email주소
                                eth_address (String )   : api에 의해 추가/삭제된 사용자 ethereum주소이며, '|'로 Delimiter로 하여 여러 주소가 들어 있을 수 있다. 
                                                          api중 withdraw나 deposit은 등록되지 않은 주소로의 withdraw나 deposit은 거부한다.
                                tedn_public_key (String):  에덴체인 사용자 Wallet 주소

            | 

               async signInUser(iamtoken)

            Authentication과는 별도로 내부 모듈에 Signin할 때 쓰이는
            API로, Authentication()에 성공하면 언제나 호출하여야 한다. 

            따라서, 보통 Authentication Event상에서 호출하게 된다.

            | 

            ::

               parameter  :
                            iamtoken ( String ) :  Authentication시 얻어지는 user object의 getIdToken()에 의해 인증되는 token이다.

               return     :
                            Boolean             : signIn성공, 실패여부를 나타낸다.

            ..

               async signOutUser(iamtoken)

            Authentication함수에서 signout이 성공할 때 호출되는 API이다.
            호출이 Mandatory는 아니며, 보통 Authentication Event상에서
            호출된다.

            | 

            ::

               parameter  :
                            iamtoken      ( String ) :  Authentication시 얻어지는 user object의 getIdToken()에 의해 인증되는 token이다.

               return     :
                            Boolean   : signIn성공, 실패여부를 나타낸다.

            ..

               async getTransactionList(iamtoken, page, countperpage)

            사용자의 Transaction List를 얻는 API이다. iamtoken에
            해당하는 사용자의 Transaction에서 정보를 리턴한다.

            | 

            ::

               parameter  :
                            iamtoken ( String ) :  Authentication시 얻어지는 user object의 getIdToken()에 의해 인증되는 token이다.
                            page     (int)      : 몇번 째 페이지의 transaction을 리턴하는지 지정한다.
                            countperpage (int)  : 각 페이지의 transaction count를 지정해서 , page를 지정하게 되면 몇번째 transaction이 몇개 리턴되는지 지정하게 된다.

               return:
                            [{}]        :   Object의 리스트를 리턴하며, 각 오브젝트는 다음 정보를 가진다.
                                         from_addr  (String) :  amount가 빠져나가는 에덴체인 상의 주소
                                         to_addr    (String) : amount가 들어가는 에덴체인 상의 주소
                                         amount     (int )   :  tx에 해당하는 token amount이며, decimal 18이다.
                                         regdate    (int)    : tx가 수행된 시간이며, 초단위이다.

            | 

               async addEthAddress(iamtoken,address)

            사용자 계정에 ethereum 주소를 넣는데 사용된다. 남의
            address의 도용을 방지하기 위해서 address를 sign하여 보내게
            되어 있으며, sign이 맞을 경우에만 서버에서 처리한다.

            | 

            ::

               parameter  :
                           iamtoken      ( String ) :  Authentication시 얻어지는 user object의 getIdToken()에 의해 인증되는 token이다.
                           address        (Object)  : 다음 정보를 가지는 Object이다. 아래 object는 제공하는 api에 의해 쉽게 만들 수 있다.
                           address  (String)        : Ethereum Checksum Address
                           public_key (String)      : Ethereum public Key.이며, signature를 verify할 때 사용한다.
                           signature ( String )     : address를 keccak256 해시 후 이를 ethereum private key로 sign한 값이다.

               return     :
                           Boolean   : Ethereum 주소 추가의 성공, 실패여부를 나타낸다.

            ..

               async delEthAddress(iamtoken,address)

            사용자 계정의  ethereum 주소를 삭제하는 데 사용된다. 남의
            address의 도용을 방지하기 위해서 address를 sign하여 보내게
            되어 있으며, sign이 맞을 경우에만 서버에서 처리한다.

            | 

            ::

               parameter  :
                            iamtoken      ( String ) :  Authentication시 얻어지는 user object의 getIdToken()에 의해 인증되는 token이다.
                            address        (Object)  : 다음 정보를 가지는 Object이다. 아래 object는 제공하는 api에 의해 쉽게 만들 수 있다.
                            address  (String)        : Ethereum Checksum Address
                            public_key (String)      : Ethereum public Key.이며, signature를 verify할 때 사용한다.
                            signature ( String )     : address를 keccak256 해시 후 이를 ethereum private key로 sign한 값이다.

               return     :
                            Boolean   : Ethereum 주소 삭제의 성공, 실패여부를 나타낸다.

            | 

               async depositTokenToEdenChain(iamtoken,txhash)

            Ethereum의 ERC20 EDN Token을 에덴체인 서비스를 위해 넘기는
            경우 호출되는 API이다.

            | 

            ::

               parameter  :
                            iamtoken      ( String ) :  Authentication시 얻어지는 user object의 getIdToken()에 의해 인증되는 token이다.
                            txhash          (String)   : Ethereum transfer후의 transaction hash값 

               return     :
                             Boolean   : API의 성공, 실패여부를 나타낸다.

            ..

               async withdrawTokenToEdenChain(iamtoken,ethaddress,
               amount)

            에덴체인 서비스의 token을 Ethereum의 ERC20 EDN Token으로
            넘기는 경우 호출되는 API이다.

            | 

            ::

               parameter  :
                            iamtoken   ( String ):  Authentication시 얻어지는 user object의 getIdToken()에 의해 인증되는 token이다.
                            ethaddress (String ) :  받고자 하는 ethereum상의 주소이며, 이는 미리 addEthAddress()에 의해 등록되어 있어야 한다.
                            amount     (int)     : 받고자 하는 양이며, decimal 18이다.

               return      :
                            txhash  (String): Coin Server에서 Ethereum Transfer후 발생된 txhash값. 해당 값을 사용하여 withdraw가 성공했는지 알 수 있다.

            | 

            -  .. rubric:: utils
                  :name: InstallSDK&DevelopmentforJavascript-utils.1

            edensdk.utils의 형태로 호출하며, api와는 상관없지만 필요한
            Utiltity함수를 가지고 있다.

            | 

               makeAddressObject(private_key)

            ethereum private key를 가지고 사용자가 에덴체인에 etheruem
            주소를 추가/삭제할 때 호출하는 API에 필요한 address object를
            쉽게 생성하기 위한 도움함수이다.

            | 

            ::

               paramteter  :
                             private_key (String): Ethereum Private key.

               return      : 
                             address  (Object)  : 다음 정보를 가지는 Object이다. 아래 object는 제공하는 api에 의해 쉽게 만들 수 있다.
                                      address  (String)    : Ethereum Checksum Address
                                      public_key (String)  : Ethereum public Key.이며, signature를 verify할 때 사용한다.
                                      signature ( String ) : address를 keccak256 해시 후 이를 ethereum private key로 sign한 값이다.

   .. container::
      :name: footer

      .. container:: section footer-body

         Document generated by Confluence on Mar 27, 2019 18:40

         .. container::
            :name: footer-logo

            `Atlassian <http://www.atlassian.com/>`__

