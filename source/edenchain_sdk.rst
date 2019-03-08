EdenChain SDK
=============

Overview
DApp Server에서 Balance Server나 Transaction Server에 연동하는 API 가이드 문서이다.

Architecture
DApp Server에서 내부 서버와 통신하는 경우 Protocol Buffers를 통해 통신한다.

단, EIAM과의 통신은 JSON RPC 2.0으로 수행한다.



이때, 통신하는 서버는 Balance Server와 Transaction Server로, Balance Server에서는 잔액 및 TEDN Token Transfer 관련 Handling을 하고,

Transaction Server는 Super Node에 Transaction Handling을 수행한다.

또한,  서명이나 Token확인을 위해 EIAM과의 연동도 필요하다.

이를 위해 SDK에서는  Sync API와 Async API 형태로 제공한다.



이 때 서버간 통신은 TLS로 동작하고 있으므로 SDK에서도 TLS를 통해 서버로 접속을 수행하며, 각 서버는 Client인증서를 확인하고 있으므로 Eden Chain에서 발급한 인증서를 사용하여 API 호출을 하여야 한다.



google cloud logging에 관련된 부분이 추가 되었다. 각 서버는 해당 API를 사용하여 log를 쓰든지, 아니면 logging guide 문서를 참고하여 직접 구글 API를 참고하여 log를 쓰도록 한다.

Description
SDK에서는 서버 Wallet관련 Private Key와 Public Key를 생성하며, 이를 이용한 서명을 수행한다.

또한, 사용자 TEDN을 전달하는 경우 사용자 Private Key를 사용하여 서명을 하여야 하는데 현재 개인 TEDN Private Key는 EIAM서버에 존재하므로, EIAM의 sign transaction api를 사용하여 서명을 수행하도록 되어 있다.

모든 API는 접속한 사용자의 IdToken (Access Token)이 존재하지 않으면 에러를 내도록 되어 있으므로 Sign-In이 성공한 상태에서 받은 Token을  API에서 언제나 사용하도록 한다.

Reference Guide
package import: 



from ec_server_api.api import EcServerConnect
from ec_server_api import api

from ec_server_api.logging.logger import ECLogger



Class EcServerConnect 가 기본 class이며, 모든 API를 가지고 있다.

api package는 Transfer token을 수행할 때 BETTING이나 PAYOUT에 대한 Action을 지정하는 값을 가지고 있다.



ec_server_api.api package
​api.DAPP_BETTING	​사용자가 LOSE하는 경우 token transfer를 호출할 때  설정되는 값
api.DAPP_PAYOUT	사용자가 WIN하는 경우 token transfer를 호출할 때  설정되는 값


ec_server_api.api package의 EcServerConnect class
네트워크 에러등에 의해 Exception이 raise될 수 있다.

Method	설명	인자	기본값	인자 설명	리턴
​EcServerConnect 생성자

생성자	​iam_url	​''	EIAM URL	
bs_url	''	Balance Server URL
ts_url	''	Transaction Server URL
server_type	dapp	
서버 Type

해당 이름으로 ~/.keys 아래에  해당 이름으로 public key, private key를 서버 Type 이름으로 생성한다.

cert	None	
각 서버로 접속할 때 사용하는 인증서 이며,  모든 서버와 접속할 때 해당 인증서를 사용한다.

현재 TS, BS의 경우 EdenChain CA에서 발급되지 않은 인증서로 접속하는 경우 거부한다.

EIAM은 적용되지 않았다.

logger	None	ECLogger를 넘겨서 api내부에서 구글 로그를 쓸 수 있도록 한다.
is_token_valid	EIAM에 현재 IdToken이 정상인지 문의할 때 쓰인다.	token	
token은 idtoken을 의미한다.

정상 : True

비정상: False

is_token_valid_async	asyncio를 사용하여 EIAM에 idToken문의한다.	token	
token은 idtoken을 의미한다.

정상 : True

비정상: False

get_balance	Balance Server에 접속하여 token사용자의 현재 Balance를 문의한다.	token	''	
token은 idtoken을 의미한다.

정상: TEden Amount (decimal : 18)

에러: None

get_balance_async	asyncio를 사용하여 Balance Server에 접속하여 token사용자의 현재 Balance를 문의한다.	token	''	token은 idtoken을 의미한다.	
정상: TEden Amount  (decimal : 18)

에러: None

transfer_token	
TEDN이체할 때 쓰인다.

사용자의 Wallet주소는 token에서 자동으로 얻어서 사용하고,

서버의  Wallet주소는 현 클래스에서 초기화할 때 자동으로 생성되는 public key를 사용한다.



따라서, ~/.keys/서버type.*는 중요한 화일이므로 백업을 해 놓아야 한다.

amount	0	이체할 TEDN amount이며, decimal은 18이다.	
정상 완료: True

비정상 에러: False

               Exception raise

direction	DAPP_BETTING	
transfer의 방향이다.

DAPP_BETTING이라면 사용자가 LOSE되는 경우이고 따라서 사용자 계정에서 서버 계정으로 TEDN이 Transfer된다.

DAPP_PAYOUT이라면 사용자가 WIN되는 경우이고 서버 계정에서 사용자 계정으로 TEDN이 Transfer된다.

token	''	사용자의 token이다. 만약 token이 없거나 token과 관련된 TEDN wallet address가 없다면 HTTP Status가 403으로 리턴된다.
transfer_token_async 	
asyncio를 사용하여 TEDN이체할 때 쓰인다.

사용자의 Wallet주소는 token에서 자동으로 얻어서 사용하고,

서버의  Wallet주소는 현 클래스에서 초기화할 때 자동으로 생성되는 public key를 사용한다.



따라서, ~/.keys/서버type.*는 중요한 화일이므로 백업을 해 놓아야 한다.

amount	0	이체할 TEDN amount이며, decimal은 18이다.	
정상 완료: True

비정상 에러: False

               Exception raise

direction	''	
transfer의 방향이다.

DAPP_BETTING이라면 사용자가 LOSE되는 경우이고 따라서 사용자 계정에서 서버 계정으로 TEDN이 Transfer된다.

DAPP_PAYOUT이라면 사용자가 WIN되는 경우이고 서버 계정에서 사용자 계정으로 TEDN이 Transfer된다.

token	''	사용자의 token이다. 만약 token이 없거나 token과 관련된 TEDN wallet address가 없다면 HTTP Status가 403으로 리턴된다.








ec_server_api.logging.logger  package의 ECLogger class
구글 로그를 사용할 때 쓰이는 Class이며, EcServerConnect 초기화시 넘길 수도 있고, 단독으로 쓰일 수도 있다.

Method	설명	인자	기본값	인자 설명	리턴
​ECLogger	​ 생성자	name​	''​	로그이름​	​
service_json	None	
로그 서비스를 접근하기 위한 키 화일 

None일 경우 환경 변수

GOOGLE_APPLICATION_CREDENTIALS 를 통해 설정도 가능하다

log_text	text형태로 로그 쓰기	text	''	로그 message	
severity	'INFO'	로그 레벨
log_struct	json형태로 로그 쓰기	json	{}	로그 json	
severity	'INFO'	로그 레벨



