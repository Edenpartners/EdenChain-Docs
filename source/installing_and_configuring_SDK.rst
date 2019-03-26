JavaScript
==========

개발 환경
-----------

OS환경은 무엇을 쓰든 상관없다.
개발을 위해서는 Node v10 이상, NPM v6.9이상을 설치하여야 한다.


설치
-------


프로젝트 디렉토리에서 아래와 같이 수행한다.
::

	npm install eden-js-sdk-client

오류가 없을 경우 관련 library까지 같이 설치가 완료된다.

제공 API
--------

모든 API는 edensdk class를 통해 제공한다.

네트워크 설정 
SDK를 통해 접속할 에덴체인의 네트워크를 설정한다. 현재 3개의 네트워크가 있으며 테스트할 네트워크를 설정하게 되는데 SDK를 사용할 때 언제나 제일 먼저 초기화 해야 하는 작업이다.

네트워크 상수
^^^^^^^^^^^^^^^

::

    EDEN_MAINNET_NETWORK:   MainNet General Release
    EDEN_CANDIDATE_NETWORK:    Edenchain Candidate Release
    EDEN_BETA_NETWORK:  Edenchain Beta Release


네트워크 설정 API
^^^^^^^^^^^^^^^^^

해당 API는 언제나 처음에 수행되어야 한다.

*boolean initApp(network_id)*

    :parameter:
        network_id는 위 네트워크 상수를 넣는다.
    :return:
        true인 경우 초기화가 성공한 것이고, false이면 초기화에 실패한 것이다. 보통 지원하지 않는 네트워크를 설정하는 경우 발생된다.


Authentication
^^^^^^^^^^^^^^^^

Edenchain으로의 Sign in과 Sign up을 지원한다.
Sign Up은 Edenchain의 계정과 Wallet을 생성하기 위한 것을 제공하며, Sign In은 기 생성된 계정으로의 Sign In을 제공한다.


(1) Sign Up
"""""""""""""

계정이 없을 경우 계정을 생성한다.

*app.auth().createUserWithEmailAndPassword( email, password )*

    :parameter:
        - email(String) :  앞으로 사용할 계정으로 사용할 email주소이다.
        - password(String) : 앞으로 사용할 계정의 암호이다.
    :return:
        - Promise를 리턴하며, 성공일 경우에는 직접 처리하지 않고, 아래에서 정의되는 Authentication Change Event를 사용한다.
        - 에러일 경우에는 .catch( (error) => {} ) 에서 처리하고, error를 통해 에러를 확인할 수 있다.

(2) Sign In
"""""""""""""

계정이 있을 경우 해당 계정으로 Sign In이 가능하다. 없거나 암호가 틀릴 경우 Sign In이 실패한다.

*app.auth().signInWithEmailAndPassword( email, password )*

    :parameter:
        - email(String) : Sign In할 계정이다.
        - password(String) : Sign In할 계정의 암호이다.
    :return:
        - Promise를 리턴하며, 성공일 경우에는 직접 처리하지 않고, 아래에서 정의되는 Auth Change Event를 사용한다.
        - 에러일 경우에는 .catch( (error) => {} ) 에서 처리하고, error를 통해 에러를 확인할 수 있다.

(3) Sign Out
"""""""""""""""

현재 Sign In되어 있는 경우 호출하여 Sign Out상태로 변경한다. 호출하면 이후에는 onAuthStateChanged()가 호출되지 않아 자동 Sign In이 되지 않는다.

*app.auth().signOut()*


(4) Authentication Change Event
""""""""""""""""""""""""""""""""""""

인증 상태가 변경될 때 호출되는 함수를 지정한다. 이미 로그인되어 있을 경우 자동으로 로그인을 하는 경우에도 역시 호출되는 함수이다.

*app.auth().onAuthStateChanged( (user) => {} )*

callback에서 지정되는 user는 사용자의 정보를 가지고 있는 object이다.
user에서 API에서 사용될 token정보를 얻을 수 있다. 
아래와 같이 호출하게 되는데, getIdToken()이 async이므로 await를 통해 얻을 수 있다.
::

    token = await user.getIdToken()

따라서, 필요시  호출되는 함수는 async로 지정하여야 한다.
해당 함수에서는 user의 여부에 따라 apis의 signInUser()또는 signOutUser()을 호출하여야 한다.

apis
^^^^^^

API instance는 인증을 제외한 나머지 api를 제공한다. edensdk.apis에서 호출한다.
모든 api의 사용을 위해서는 인증시 받은 token이 필요하다. 이 token을 사용하여 api를 호출하게 되는데, 에덴체인 내의 모든 모듈은 해당 token을 사용하여  인증을 확인한다.
모든 API는 async 형태이다.

(1) async getCoinServerAddress(iamtoken)
""""""""""""""""""""""""""""""""""""""""""

CoinServer Ethereum Address를 리턴한다. Beta Release와 Candidate Release까지는 Ropsten Ethereum Testnet의 주소를 리턴하며, General Release시에는 Ethereum MainNet상의 주소를 리턴한다.

    :parameter:
        - iamtoken(String) : Authentication시 얻어지는 user object의 getIdToken()에 의해 인증되는 token이다.
    :return:
        - String : Coin Server의 Ethereum 주소를 리턴한다.



(2)  async getUserBalance(iamtoken)
"""""""""""""""""""""""""""""""""""""

에덴체인의 사용자 계정 Balance를 리턴한다.

    :parameter:
        - iamtoken(String) : Authentication시 얻어지는 user object의 getIdToken()에 의해 인증되는 token이다.
    :return:
        - int : decimal 18인 Token balance이다.

(3) async getUserInfo(iamtoken)
""""""""""""""""""""""""""""""""

에덴체인 상의 사용자 정보를 리턴한다. 주로 token address 또는 token deposit, withdraw에 사용될 ethereum 주소 정보 등을 리턴한다.

    :parameter:
        - iamtoken(String) : Authentication시 얻어지는 user object의 getIdToken()에 의해 인증되는 token이다.
    :return:
        - {} : Object를 리턴하며, 다음 정보를 가진다.
        - email  (String) :  사용자 email주소
        - eth_address (String) : 
            - api에 의해 추가/삭제된 사용자 ethereum주소이며, '|'로 Delimiter로 하여 여러 주소가 들어 있을 수 있다.
            - api중 withdraw나 deposit은 등록되지 않은 주소로의 withdraw나 deposit은 거부한다.
        - tedn_public_key (String) : 에덴체인 사용자 Wallet 주소

(4) async signInUser(iamtoken)
"""""""""""""""""""""""""""""""

Authentication과는 별도로 내부 모듈에 Signin할 때 쓰이는 API로, Authentication()에 성공하면 언제나 호출하여야 한다. 
따라서, 보통 Authentication Event상에서 호출하게 된다.

    :parameter:
        - iamtoken(String) : Authentication시 얻어지는 user object의 getIdToken()에 의해 인증되는 token이다.
    :return:
        - Boolean : signIn성공, 실패여부를 나타낸다.

(5) async signOutUser(iamtoken)
""""""""""""""""""""""""""""""""

Authentication함수에서 signout이 성공할 때 호출되는 API이다. 호출이 Mandatory는 아니며, 보통 Authentication Event상에서 호출된다.

    :parameter:
        - iamtoken(String) : Authentication시 얻어지는 user object의 getIdToken()에 의해 인증되는 token이다.
    :return:
        - Boolean : signIn성공, 실패여부를 나타낸다.

(6) async getTransactionList(iamtoken, page, countperpage)
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

사용자의 Transaction List를 얻는 API이다. iamtoken에 해당하는 사용자의 Transaction에서 정보를 리턴한다.

    :parameter:
        - iamtoken(String) : Authentication시 얻어지는 user object의 getIdToken()에 의해 인증되는 token이다.
        - page(int) : 몇번 째 페이지의 transaction을 리턴하는지 지정한다.
        - countperpage(int) : 각 페이지의 transaction count를 지정해서 page를 지정하게 되면 몇번째 transaction이 몇개 리턴되는지 지정하게 된다.
    :return:
        - [{}] : Object의 리스트를 리턴하며, 각 오브젝트는 다음 정보를 가진다.
        - from_addr(String) : amount가 빠져나가는 에덴체인 상의 주소
        - to_addr(String) : amount가 들어가는 에덴체인 상의 주소
        - amount(int) : tx에 해당하는 token amount이며, decimal 18이다.
        - regdate(int) : tx가 수행된 시간이며, 초단위이다.

(7) async addEthAddress(iamtoken,address)
""""""""""""""""""""""""""""""""""""""""""

사용자 계정에 ethereum 주소를 넣는데 사용된다. 남의 address의 도용을 방지하기 위해서 address를 sign하여 보내게 되어 있으며, sign이 맞을 경우에만 서버에서 처리한다.

    :parameter:
        - iamtoken(String) : Authentication시 얻어지는 user object의 getIdToken()에 의해 인증되는 token이다.
        - address(Object) : 다음 정보를 가지는 Object이다. 아래 object는 제공하는 api에 의해 쉽게 만들 수 있다.
        - address(String) : Ethereum Checksum Address
        - public_key (String) : Ethereum public Key.이며, signature를 verify할 때 사용한다.
        - signature ( String ) : address를 keccak256 해시 후 이를 ethereum private key로 sign한 값이다.
    :return:
        - Boolean : Ethereum 주소 추가의 성공, 실패여부를 나타낸다.

(8) async delEthAddress(iamtoken,address)
"""""""""""""""""""""""""""""""""""""""""""""

사용자 계정의  ethereum 주소를 삭제하는 데 사용된다. 남의 address의 도용을 방지하기 위해서 address를 sign하여 보내게 되어 있으며, sign이 맞을 경우에만 서버에서 처리한다.

    :parameter:
        - iamtoken(String) : Authentication시 얻어지는 user object의 getIdToken()에 의해 인증되는 token이다.
        - address(Object) : 다음 정보를 가지는 Object이다. 아래 object는 제공하는 api에 의해 쉽게 만들 수 있다.
        - address(String) : Ethereum Checksum Address
        - public_key (String) : Ethereum public Key.이며, signature를 verify할 때 사용한다.
        - signature (String) : address를 keccak256 해시 후 이를 ethereum private key로 sign한 값이다.
    :return:
        - Boolean : Ethereum 주소 삭제의 성공, 실패여부를 나타낸다.

(9) async depositTokenToEdenChain(iamtoken,txhash)
""""""""""""""""""""""""""""""""""""""""""""""""""""

Ethereum의 ERC20 EDN Token을 에덴체인 서비스를 위해 넘기는 경우 호출되는 API이다.

    :parameter:
        - iamtoken(String) :  Authentication시 얻어지는 user object의 getIdToken()에 의해 인증되는 token이다.
        - txhash(String) : Ethereum transfer후의 transaction hash값
    :return:
        - Boolean : API의 성공, 실패여부를 나타낸다.

(10) async withdrawTokenToEdenChain(iamtoken,ethaddress, amount)
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

에덴체인 서비스의 token을 Ethereum의 ERC20 EDN Token으로 넘기는 경우 호출되는 API이다.

    :parameter:
        - iamtoken(String) : Authentication시 얻어지는 user object의 getIdToken()에 의해 인증되는 token이다.
        - ethaddress(String) : 받고자 하는 ethereum상의 주소이며, 이는 미리 addEthAddress()에 의해 등록되어 있어야 한다.
        - amount(int) : 받고자 하는 양이며, decimal 18이다.
    :return:
        - txhash(String) : Coin Server에서 Ethereum Transfer후 발생된 txhash값. 해당 값을 사용하여 withdraw가 성공했는지 알 수 있다.

utils
^^^^^^^

edensdk.utils의 형태로 호출하며, api와는 상관없지만 필요한 Utiltity함수를 가지고 있다.

(1) makeAddressObject(private_key)
""""""""""""""""""""""""""""""""""""

ethereum private key를 가지고 사용자가 에덴체인에 etheruem 주소를 추가/삭제할 때 호출하는 API에 필요한 address object를 쉽게 생성하기 위한 도움함수이다.

    :paramteter:
        - private_key(String): Ethereum Private key.
    :return: 
        - address(Object) : 다음 정보를 가지는 Object이다. 아래 object는 제공하는 api에 의해 쉽게 만들 수 있다.
        - address(String) : Ethereum Checksum Address
        - public_key(String): Ethereum public Key.이며, signature를 verify할 때 사용한다.
        - signature(String) : address를 keccak256 해시 후 이를 ethereum private key로 sign한 값이다.

Python
=======

개발 환경
---------
Linux/Windows/Mac OS
python3용으로만 개발되어 있다.

설치
-----

프로젝트 디렉토리에서 아래와 같이 수행한다.
::

    pip install  eden_client_api

오류가 없을 경우 관련 library까지 같이 설치가 완료된다.

제공 API
----------

아래 모든 API는 EdenClientApi() class의 method이다.

네트워크 설정 
^^^^^^^^^^^^^^

SDK를 통해 접속할 에덴체인의 네트워크를 설정한다. 현재 3개의 네트워크가 있으며 테스트할 네트워크를 설정하게 되는데 SDK를 사용할 때 언제나 제일 먼저 초기화 해야 하는 작업이다.

네트워크 상수
"""""""""""""""
::

    EDEN_MAINNET_NETWORK : MainNet General Release 
    EDEN_CANDIDATE_NETWORK : Edenchain Candidate Release
    EDEN_BETA_NETWORK : Edenchain Beta Release

네트워크 설정 
"""""""""""""

API Class 초기화할 때 사용되며, 이 후 모든 API는 해당 class를 이용하여 호출된다.

*EdenClientApi(network_id)*

    :parameter:
        - network_id는 위 네트워크 상수를 넣는다.
    :return:
        - Python class가 생성된다. 이 instance를 사용하여 API를 호출한다.


Authentication
^^^^^^^^^^^^^^^

Edenchain으로의 Sign in만을 지원한다. Sign In은 기 생성된 계정으로의 Sign  In을 제공한다.

(1) authenticate_user( email, password)
"""""""""""""""""""""""""""""""""""""""""

    :parameter:
        - email(String) : 앞으로 사용할 계정으로 사용할 email주소이다.
        - password (String) : 앞으로 사용할 계정의 암호이다.
    :return:
        - token(String) : API호출에 사용될 사용자의 인증 token을 리턴한다.

Synchronous apis
^^^^^^^^^^^^^^^^^^

각 api는 async버전과 기본 sync버전이 있으며, async의 경우는 함수 이름 뒤에 _async가 붙어있는 형태로 되어 있다.

(1) get_user_info(token='')
""""""""""""""""""""""""""""

에덴체인 상의 사용자 정보를 리턴한다.  dictionary형태로 리턴하며  token address 또는 token deposit, withdraw에 사용될 ethereum 주소 정보 등을 리턴한다.

    :parameter:
        - token(String) : Authentication시 얻어지는 token 값이다.
    :return:
        - {} : Dictionary를 리턴하며, 다음 정보를 가진다.
        - email(String) : 사용자 email주소
        - eth_address(String) : 
            - api에 의해 추가/삭제된 사용자 ethereum주소이며, '|'로 Delimiter로 하여 여러 주소가 들어 있을 수 있다.
            - api중 withdraw나 deposit은 등록되지 않은 주소로의 withdraw나 deposit은 거부한다.
        - tedn_public_key(String) : 에덴체인 사용자 Wallet 주소

(2) get_user_balance(token='')
"""""""""""""""""""""""""""""""

에덴체인의 사용자 계정 Balance를 리턴한다.

    :parameter:
        - token(String) : Authentication시 얻어지는 token 값이다.
    :return:
        - int : decimal 18인 Token balance이다

(3) get_user_transaction(token='', page = 0, countperpage = 0)
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

사용자의 Transaction List를 얻는 API이다. iamtoken에 해당하는 사용자의 Transaction에서 정보를 리턴한다.

    :parameter:
        - token(String) : Authentication시 얻어지는 token 값이다.
        - page(int) : 몇번 째 페이지의 transaction을 리턴하는지 지정한다.
        - countperpage(int): 각 페이지의 transaction count를 지정해서 , page를 지정하게 되면 몇번째 transaction이 몇개 리턴되는지 지정하게 된다.
    :return:
        - [{}] : Object의 리스트를 리턴하며, 각 오브젝트는 딕셔너리 형태로 다음 정보를 가진다.
        - from_addr(String) : amount가 빠져나가는 에덴체인 상의 주소
        - to_addr(String) : amount가 들어가는 에덴체인 상의 주소
        - amount(int) : tx에 해당하는 token amount이며, decimal 18이다.
        - regdate(int) : tx가 수행된 시간이며, 초단위이다.

(4) get_coin_server_address(token='')
"""""""""""""""""""""""""""""""""""""""

CoinServer Ethereum Address를 리턴한다. Beta Release와 Candidate Release까지는 Ropsten Ethereum Testnet의 주소를 리턴하며, General Release시에는 Ethereum MainNet상의 주소를 리턴한다.

    :parameter:
        - token(String) : Authentication시 얻어지는 token 값이다.
    :return:
        - String : Coin Server의 Ethereum 주소를 리턴한다.

(5) add_eth_address(token='', private_key='')
""""""""""""""""""""""""""""""""""""""""""""""

사용자 계정에 ethereum 주소를 넣는데 사용된다. 남의 address의 도용을 방지하기 위해서 address를 sign하여 보내게 되어 있으며, sign이 맞을 경우에만 서버에서 처리한다.

    :parameter:
        - token(String) : Authentication시 얻어지는 token 값이다.
        - private_key(String) : Etheruem Private Key이며, 해당 key를 사용하여 아래 address object를 생성하여 서버쪽에 전달한다.
    :address Object:
        - address(String) : Ethereum Checksum Address
        - public_key(String) : Ethereum public Key.이며, signature를 verify할 때 사용한다.
        - signature(String) : address를 keccak256 해시 후 이를 ethereum private key로 sign한 값이다.
    :return:
        - Boolean : Ethereum 주소 추가의 성공, 실패여부를 나타낸다.

(6) del_eth_address( token='', private_key='')
"""""""""""""""""""""""""""""""""""""""""""""""

사용자 계정에 ethereum 주소를 넣는데 사용된다. 남의 address의 도용을 방지하기 위해서 address를 sign하여 보내게 되어 있으며, sign이 맞을 경우에만 서버에서 처리한다.

    :parameter:
        - token(String) : Authentication시 얻어지는 token 값이다.
        - private_key(String) : Etheruem Private Key이며, 해당 key를 사용하여 아래 address object를 생성하여 서버쪽에 전달한다.
    :address Object:
        - address(String) : Ethereum Checksum Address
        - public_key(String) : Ethereum public Key.이며, signature를 verify할 때 사용한다.
        - signature(String) : address를 keccak256 해시 후 이를 ethereum private key로 sign한 값이다.
    :return:
        - Boolean : Ethereum 주소 삭제의 성공, 실패여부를 나타낸다.

(7) deposit_token(token='', txhash='')
""""""""""""""""""""""""""""""""""""""""

Ethereum의 ERC20 EDN Token을 에덴체인 서비스를 위해 넘기는 경우 호출되는 API이다.

    :parameter:
        - token(String) :  Authentication시 얻어지는 token 값이다.
        - txhash(String) : Ethereum transfer후의 transaction hash값
    :return:
        - Boolean : API의 성공, 실패여부를 나타낸다.

(8) withdraw_token(token='', ethaddress='',amount=0)
"""""""""""""""""""""""""""""""""""""""""""""""""""""""

에덴체인 서비스의 token을 Ethereum의 ERC20 EDN Token으로 넘기는 경우 호출되는 API이다.

    :parameter:
        - token(String) : Authentication시 얻어지는 token 값이다.
        - ethaddress(String) : 받고자 하는 ethereum상의 주소이며, 이는 미리 addEthAddress()에 의해 등록되어 있어야 한다.
        - amount(int) : 받고자 하는 양이며, decimal 18이다.
    :return:
        - txhash(String) : Coin Server에서 Ethereum Transfer후 발생된 txhash값. 해당 값을 사용하여 withdraw가 성공했는지 알 수 있다.

Asynchronous apis
^^^^^^^^^^^^^^^^^^^^^
각 api는 async버전과 기본 sync버전이 있으며, async의 경우는 함수 이름 뒤에 _async가 붙어있는 형태로 되어 있다.

(1) get_user_info_async(token='')
""""""""""""""""""""""""""""""""""

에덴체인 상의 사용자 정보를 리턴한다.  dictionary형태로 리턴하며  token address 또는 token deposit, withdraw에 사용될 ethereum 주소 정보 등을 리턴한다.

    :parameter:
        - token(String) : Authentication시 얻어지는 token 값이다.
    :return:
        - {} : Dictionary를 리턴하며, 다음 정보를 가진다.
        - email(String) : 사용자 email주소
        - eth_address(String) : 
            - api에 의해 추가/삭제된 사용자 ethereum주소이며, '|'로 Delimiter로 하여 여러 주소가 들어 있을 수 있다. 
            - api중 withdraw나 deposit은 등록되지 않은 주소로의 withdraw나 deposit은 거부한다.
        - tedn_public_key(String) : 에덴체인 사용자 Wallet 주소

(2) get_user_balance_async(token='')
"""""""""""""""""""""""""""""""""""""""

에덴체인의 사용자 계정 Balance를 리턴한다.

    :parameter:
        - token(String) : Authentication시 얻어지는 token 값이다.
    :return:
        - int : decimal 18인 Token balance이다

(3) get_user_transaction_async(token='', page = 0, countperpage = 0)
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

사용자의 Transaction List를 얻는 API이다. iamtoken에 해당하는 사용자의 Transaction에서 정보를 리턴한다.

    :parameter:
        - token(String) : Authentication시 얻어지는 token 값이다.
        - page(int) : 몇번 째 페이지의 transaction을 리턴하는지 지정한다.
        - countperpage(int): 각 페이지의 transaction count를 지정해서 , page를 지정하게 되면 몇번째 transaction이 몇개 리턴되는지 지정하게 된다.
    :return:
        - [{}]  :   Object의 리스트를 리턴하며, 각 오브젝트는 딕셔너리 형태로 다음 정보를 가진다.
        - from_addr(String):  amount가 빠져나가는 에덴체인 상의 주소
        - to_addr(String) : amount가 들어가는 에덴체인 상의 주소
        - amount(int) : tx에 해당하는 token amount이며, decimal 18이다.
        - regdate(int) : tx가 수행된 시간이며, 초단위이다.

(4) get_coin_server_address_async(token='')
"""""""""""""""""""""""""""""""""""""""""""""""

CoinServer Ethereum Address를 리턴한다. Beta Release와 Candidate Release까지는 Ropsten Ethereum Testnet의 주소를 리턴하며, General Release시에는 Ethereum MainNet상의 주소를 리턴한다.

    :parameter:
        - token(String) : Authentication시 얻어지는 token 값이다.
    :return:
        - String : Coin Server의 Ethereum 주소를 리턴한다.

(5) add_eth_address_async(token='', private_key='')
"""""""""""""""""""""""""""""""""""""""""""""""""""""

사용자 계정에 ethereum 주소를 넣는데 사용된다. 남의 address의 도용을 방지하기 위해서 address를 sign하여 보내게 되어 있으며, sign이 맞을 경우에만 서버에서 처리한다.

    :parameter:
        - token(String) : Authentication시 얻어지는 token 값이다.
        - private_key(String) : Etheruem Private Key이며, 해당 key를 사용하여 아래 address object를 생성하여 서버쪽에 전달한다.
    :address Object:
        - address(String) : Ethereum Checksum Address
        - public_key(String) : Ethereum public Key.이며, signature를 verify할 때 사용한다.
        - signature(String) : address를 keccak256 해시 후 이를 ethereum private key로 sign한 값이다.
    :return:
        - Boolean : Ethereum 주소 추가의 성공, 실패여부를 나타낸다.

(6) del_eth_address_async( token='', private_key='')
""""""""""""""""""""""""""""""""""""""""""""""""""""""""

사용자 계정에 ethereum 주소를 넣는데 사용된다. 남의 address의 도용을 방지하기 위해서 address를 sign하여 보내게 되어 있으며, sign이 맞을 경우에만 서버에서 처리한다.

    :parameter:
        - token(String) : Authentication시 얻어지는 token 값이다.
        - private_key(String) : Etheruem Private Key이며, 해당 key를 사용하여 아래 address object를 생성하여 서버쪽에 전달한다.
    :address Object:
        - address(String) : Ethereum Checksum Address
        - public_key(String) : Ethereum public Key.이며, signature를 verify할 때 사용한다.
        - signature(String) : address를 keccak256 해시 후 이를 ethereum private key로 sign한 값이다.
    :return:
        - Boolean : Ethereum 주소 삭제의 성공, 실패여부를 나타낸다.

(7) deposit_token_async(token='', txhash='')
"""""""""""""""""""""""""""""""""""""""""""""""

Ethereum의 ERC20 EDN Token을 에덴체인 서비스를 위해 넘기는 경우 호출되는 API이다.

    :parameter:
        - token(String) : Authentication시 얻어지는 token 값이다.
        - txhash(String) : Ethereum transfer후의 transaction hash값
    :return:
        - Boolean : API의 성공, 실패여부를 나타낸다.

(8) withdraw_token_async(token='', ethaddress='',amount=0)
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

에덴체인 서비스의 token을 Ethereum의 ERC20 EDN Token으로 넘기는 경우 호출되는 API이다.

    :parameter:
        - token(String) : Authentication시 얻어지는 token 값이다.
        - ethaddress(String) : 받고자 하는 ethereum상의 주소이며, 이는 미리 addEthAddress()에 의해 등록되어 있어야 한다.
        - amount(int) : 받고자 하는 양이며, decimal 18이다.
    :return:
        - txhash(String) : Coin Server에서 Ethereum Transfer후 발생된 txhash값. 해당 값을 사용하여 withdraw가 성공했는지 알 수 있다.