Dapp development guideline
==========================

Overview
Edenchain Platform상에서 운영되는 DApp 서버 및 Dapp Client의 역할에 대해 기술한 문서이다.

Edenchain은 API 기반의 블록체인 서비스 개발을 적극 권장하고 있기 때문에 서비스 개발은 일반적인 서비스 개발과 크게 다르지 않다.

차이점이 있다면 블록체인이기 때문에 Coin, Transaction 처리에 대한 부분이라고 할 수 있으며, 이러한 것들 역시 API를 이용해 처리하기 때문에 전체적인 그림에서 보면 API기반의 프로그래밍과 유사하다.

이 문서는 보다 명확한 가이드라인의 제시를 위해 블록체인의 특성에 의해 필요한 몇가지 필수적인 기능과 2가지 형태의 Dapp 개발방법과 각각의 방법에 따른 Dapp Server, Dapp client의 역할에 대해 기술한다.

Features from Blockchain Technology
블록체인 서비스 역시 사용자들을 위해 존재하며, 사용자들이 필요한 어떤 기능을 제공하기 위해 서비스를 개발한다. 하지만 Non-blockchain server와 달리 기술 특성상 반드시 가져가야 하는 기능들이 있으며, 이것들은 Dapp 서비스에 녹아있어야 Blockchain Service라고 이야기 할 수 있다.

Blockchain Service를 개발할 때 필요한 기능들을 기술하면 다음과 같다.



Logic for handling general tasks
Signup, Signin, Storing data ....
Transaction Management
Token withdraw, transfer and anchoring data...
Key Management
A Key used to manage blockchain things especially signing transaction.
처음에 언급한 것은 Dapp 자체에서 데이터 처리를 위해 필요한 것으로 일반적인 서비스에서 서버가 하는 역할이라고 생각하면 된다.

Transaction Management는 Token Withdraw, Deposit등의 기능을 의미하는 것으로 이러한 작업들은 블록체인과 직접적으로 연관되어 있고, 탈중앙화를 위해 Dapp Provider가 독자적으로 필요한 로직을 개발해 사용한다. 한가지 유의해야 할 사항은 블록체인과 직접적으로 연관된 작업들은 Dapp provider가 반드시 제공되는 SDK를 이용해 Signing을 하고 Transaction을 실행시켜야 한다는 점이다. 

Key management는 블록체인 기능을 사용하기 위해 필요한 Key를 관리하는 것으로 Dapp Provider가 키를 생성해서 사용하는 것을 의미한다. 특히 Private Key의 경우 Edenchain Platform에서 관리하지 않고 Dapp Provider가 독자적으로 관리하기 때문에 철저한 주의가 필요하다. Edenchain은 Platform Provider로서 필요한 기능을 제공하고, Key를 가지고 있기 때문에 Dapp Provider의 데이터를 조작하거나 삭제할 수 없다.

Transaction Management, Key Management가 블록체인 서비스가 가지는 Non-blockchain service와의 차이이며, 반드시 Dapp 개발시 구현되어야 한다.

2 Type of Dapp Development
API를 이용한 Dapp 개발은 크게 2가지 형태로 나누어서 이야기 할 수 있다.



Type 1 : Dapp client + Dapp Server
Type 2 : Dapp Client 
Type 1
Type 1은 가장 일반적인 형태로 UI를 담당하는 Dapp Client와 데이터처리등을 위한 Dapp Server로 구성된다.

이 경우 앞서 언급한 2가지 블록체인 기능은 Dapp Server에서 담당하는 것을 원칙으로 한다.

즉 2개 모듈은 다음과 같이 기능을 나누어 가지게 된다.

Dapp Server
Logics for Dapp thing
Transaction Management
Key Management
Dapp Client
UI thing
 이 방식은 가장 일반화된 서비스 개발방법으로 기존 서비스 개발방법과 큰 차이가 없다. 다른 부분은 TX mgt와 Key Mgt의 2개부분이다. 

Type 2
Type2는 Dapp Server를 없애고, 모든 역할을 Dapp Client에서 담당하는 방식이다. 즉 Dapp Client는 다음과 같은 기능을 모두 담당한다.

Dapp Client
Logics for Dapp thing
Transaction Management
Key Management
UI Thing
Dapp Client가 Dapp 실행등에 필요한 모든 기능을 담당하기 때문에 별도의 Dapp Server가 필요없다. 

Dapp Client는 Edenchain에서 제공하는 API와 서버들을 이용해 서비스 프로그램을 개발할 수 있다.



Type1 Vs Type2


Type1	
다양한 비지니스 로직 구현 및 기능 사용 가능 
보다 안전하게 시스템을 사용할 수 있음 
Dapp Server, Dapp client를 모두 개발해야 함
Dapp Server 서비스 운영이 필요함
Type2	
Dapp Server 개발이 필요치 않음
별도의 서비스 운영이 필요치 않음
주요한 값들이 노출되기 때문에 보안에 좋지 않음
제한적인 형태의 Dapp 만 개발할 수 있음


위의 표에서 알 수 있듯이 일반적인 Dapp이라면 Type1 형태로 서비스를 개발한다.



Key Management
Eden SDK를 이용해 Public Key/Private Key를 생성한다.

생성한 Private Key는 Dapp Provider가 저장하고, Transaction Signing등 필요한 시점에 활용하면 된다.

Transaction Management
Transaction 역시 Key Management와 동일하게 Eden SDK를 이용해 처리한다.

Dapp Provider 관점에서 보면 Transaction mgt는 withdraw, deposit등의 작업을 처리하기 위해 Edenchain Platform에 request를 보내는 것이기 때문에 Signed Transaction을 만들어 Edenchain Platform에 전송하는 것으로 설명할 수 있다.



Transaction Management는 다음과 같은 순서로 처리된다.

Building a transaction using Eden SDK
Signing the transaction with private key
Submitting the signed transaction to corresponding Eden API server


Eden SDK
Eden SDK는 Key management와 Transaction mgt에 필요한 기능들을 제공하며, Eden API와 더불어 Dapp 서비스 개발에 필요한 여러 기능들을 제공한다.

현재는 Python SDK만을 제공하며, 추후 지원언어가 확장될 것이다.



Dapp Registration
Dapp Provider는 Dapp 개발을 위해 회사정보와 Namespace등을 먼저 등록해야 한다. Google API, Facebook 개발자 등록과 유사한 과정이다.

Edenchain은 Permissioned blockchain이기 때문에 Dapp Provider는 Edenchain Governance를 통해 사전에 허가를 받아야 하고, 이를 위해 기본적인 정보를 제공해야 한다.

Developer Portal인 E-Edge의 메뉴를 이용해 Dapp provider가 서비스를 신청하고 승인받을 수 있고, 승인을 받으면 Custom Key를 EIAM 서버로부터 부여받는다.

아래의 테이블은 Dapp 등록을 위해 필요한 정보들이다.

* 아래의 정보들은 E-Garden에 포함되어야 할 것이다.



Dapp Name	

Dapp description	

Dapp Category	

Dapp Icon	

Dapp Link	Either HTML5 link or mobile app link	
Dapp Provider Name	

Dapp Provider Contact info	

Dapp Provider web site	

Namespace	

Computing Zone	
2 options

Proprietary
Edenchain

White ip list	Allowed IPs for Edenchain Interaction	


서비스를 성공적으로 등록하면 E-Garden Server로부터 1) API Key, 2) Secret Key 2개의 키값을 돌려받는다.

Dapp Server는 2개의 키를 이용해 Edenchain Platform과 데이터를 주고 받으며 처리한다.



Components
​HTML5 Game Plinko	​Single
HTML5 Game Keno	Single
DApp Server	Serving HTML 5 Game
Interfaces
DApp 서버는 내부 서버와 Protocol Buffer를 통해 통신한다.

해당 부분의 문서는 따로 제공한다.



현재 E-Wallet과 서버간의 통신은 JSON-RPC로 통신하고 있으며 관련 부분 참고는 다음 Link를 참고한다.

https://edencore-end-point-dot-manifest-ivy-213501.appspot.com/api/browse/



DApp 서버와 Client간 통신은 원하는 방식으로 구현한다.



Technical Documents
Data+Models
DApp+Server+Product+Requirements
Keno+Product+Requirements
Plinko+Product+Requirements
Keno+Product+Specification
Plinko_+Product+Specification
Plinko+UX+Guideline
Keno+UX+Guideline
위 문서는 다음 Link에서 확인이 가능하다.

https://drive.google.com/file/d/1IcZMzg8WMo-eTCsDKfVMMvc5_X2BbAlQ/view?usp=sharing



DApp Server에서 필요한 interface문서는 다시 update할 예정임.

Game Code Link
https://drive.google.com/file/d/1GUB7UuiZkUZpK-pFmyFdxyc2z3O1UOGx/view?usp=sharing



Dapp Transaction Handling

