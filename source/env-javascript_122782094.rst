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

            Created by Jacki Heo, last modified on Mar 28, 2019

         .. container:: wiki-content group
            :name: main-content

            .. rubric:: 1. Development Environment
               :name: InstallSDK&DevelopmentforJavascript-1.DevelopmentEnvironment

            The OS environment does not matter what you write.

            For development, Node v10 or higher, NPM v6.9 or higher
            should be installed.

            | 

            .. rubric:: 2. Install
               :name: InstallSDK&DevelopmentforJavascript-2.Install

            In the project directory, do the following.

            | 

            ::

               npm install edenchain-client-sdk

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

               EDENCHAIN_MAINNET_NETWORK      : Edenchain MainNet General Release 
               EDENCHAIN_CANDIDATE_RELEASE    : Edenchain Candidate Release
               EDENCHAIN_BETA_RELEASE         : Edenchain Beta Release

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
                            email    (String): email address to use for future accounts.
                            password (String): The password of the account to be used in the future.

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
                           email     (String)  : Account to sign In
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

               onAuthChanged( (idtoken) => {} )

            | 

            The idtoken specified in callback is the access token for
            API.

            idtoken will be used in the other API.

            | 

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
                            iamtoken  (String):  This is a token authenticated by getIdToken () of the user object obtained at the time of authentication.

               return     :
                            string            : Returns the Ethereum address of the Coin Server.


            ..

               async getUserBalance(iamtoken)

            Returns the user account balance of the Eden chain.

            | 

            ::

               parameter  :
                            iamtoken  (String) :  This is a token authenticated by getIdToken () of the user object obtained at the time of authentication.

               return     :
                            int                : Token blanace is decimal 18.

            | 

               async getUserInfo(iamtoken)

            It returns user information on the Edenchain. It mainly
            returns token address, token deposit, or ethereum address
            information to be used for withdraw.

            | 

            ::

               parameter:
                          iamtoken    (String) :  This is a token authenticated by getIdToken () of the user object obtained at the time of authentication.

               return:
                         {}                    : Returns an Object, with the following information:
                                email           (String):  User email address
                                eth_address     (String): It is the ethereum address of the user added / deleted by api, and may contain multiple addresses as delimiter with '|'.
                                                          withdraw or deposit of api will refuse to withdraw or deposit to unregistered address.
                                tedn_public_key (String):  Edenchain user wallet address

            | 

               async signInUser(iamtoken)

            It is an API that is used to sign in to the internal module
            separately from Authentication. This function is called
            automatically, so does not need to call.

            | 

            ::

               parameter  :
                            iamtoken (String) :  This is a token authenticated by getIdToken () of the user object obtained at the time of authentication.

               return     :
                            Boolean           : signIn indicates success or failure.

            ..

               async signOutUser(iamtoken)

            This is the API that is called when signout is successful in
            the Authentication function. This function is called
            automatically, so does not need to call.

            | 

            ::

               parameter  :
                            iamtoken (String) :  This is a token authenticated by getIdToken () of the user object obtained at the time of authentication.

               return     :
                            Boolean           : It indicates success or failure of signIn .

            ..

               async getTransactionList(iamtoken, page, countperpage)

            It is an API to get the transaction list of the user.
            Returns information from the transaction of the user
            corresponding to iamtoken.

            | 

            ::

               parameter  :
                            iamtoken     (String) :  This is a token authenticated by getIdToken () of the user object obtained at the time of authentication.
                            page         (int)    : Specifies the next page transaction to return.
                            countperpage (int)    : By specifying the transaction count for each page and specifying the page, you specify how many transactions are returned.

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
                           iamtoken       (String)  :  This is a token authenticated by getIdToken () of the user object obtained at the time of authentication.
                           address        (Object)  : An Object with the following information. The following objects are easily created by the providing api.
                                                      address        (String)  : Ethereum Checksum Address
                                                      public_key     (String)  : Ethereum public key. This is used to verify the signature.
                                                      signature      (String)  :  It is a value signed with Ethereum private key, after the keccak256 hash of address.
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
                            iamtoken   (String) :  This is a token authenticated by getIdToken () of the user object obtained at the time of authentication.
                            address    (Object) : An Object with the following information. The following objects are easily created by the providing api.
                                                  address    (String)     : Ethereum Checksum Address
                                                  public_key (String)     : Ethereum public key. This is used to verify the signature.
                                                  signature  (String)     : It is a value signed with Ethereum private key, after the keccak256 hash of address.

               return     :
                            Boolean   : It indicates the success or failure of Ethereum address deletion.

            | 

               async depositTokenToEdenChain(iamtoken,txhash)

            It is the API that is called when the Ethereum ERC20 EDN
            Token is passed for the Edenchain service.

            | 

            ::

               parameter  :
                            iamtoken      (String) :  This is a token authenticated by getIdToken () of the user object obtained at the time of authentication.
                            txhash        (String) : Transaction hash value after Ethereum transfer

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
                            iamtoken   (String):  It is the API that is called when the Ethereum ERC20 EDN Token is passed for the Edenchain service.
                            ethaddress (String):  The address on the Ethereum to deposit. It must be registered by addEthAddress () in advance.
                            amount     (int)   : Amount to receive and it is decimal 18.

               return      :
                            txhash     (String): Txhash value generated after Ethereum transfer in Coin Server. You can use that value to determine if the withdraw was successful.

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
                             address   (Object)  : An Object with the following information. The following objects are easily created by the providing api.
                                      address  (String)    : Ethereum Checksum Address
                                      public_key (String)  : Ethereum public key. This is used to verify the signature.
                                      signature ( String ) : After the keccak256 hash of address, it is signed with Ethereum private key.

            ::

   .. container::
      :name: footer

      .. container:: section footer-body

         Document generated by Confluence on Mar 28, 2019 12:30

         .. container::
            :name: footer-logo

            `Atlassian <http://www.atlassian.com/>`__

