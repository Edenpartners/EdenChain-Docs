Data models
-----------
1. Overview

내부 core component들 간의 통신은 Restful 형태로 통신을 하고 있으며 Content-Type은 application/x-protobuf 이다.
즉, Protocol Buffers로 통신을 하도록 한다.
이때 통신하는 Protocol Buffers Message는 동일해야 하므로 관련 부분을 정의한다.

2. Messages

syntax는 proto3를 사용한다.
package는 edenchain으로 정의한다.


 * TransactionBody
 TEDN에 대한 모든 Transaction이 처리될 때 사용되는 메세지이다.


.. literalinclude:: protos/transaction.proto
   :language: protobuf
   :caption: File: protos/transaction.proto
   :linenos:


- 설명
   enum TXMessageType

   Transaction에 필요한 메세지 종류 를 정리한다.
   
.. list-table::
   :widths: 30, 50
   :header-rows: 1
   
   * - Enum 이름
     - Description 
   * - TRANSACTION_DEPOSIT
     - CS→BS로 EDN을 TEDN으로 바꿀 경우
   * - TRANSACTION_WITHDRAW
     - CS→BS로 TEDN을 EDN으로 바꿀 경우
   * - TRANSACTION_TRANSFER
     - dAppServer→BS 로 TEDN을 betting하거나 payout하는 경우
     
     
.. list-table::
   :widths: 10, 20, 50
   :header-rows: 1
   
   * - Field 
     - Type
     - | Description
   * - tx_type
     - TXMessageType
     - | transaction operation이다.
       | 정의된 enum중 
       | TRANSACTION_DEPOSIT / 
       | TRANSACTION_WITHDRAW/ 
       | TRANSACTION_TRANSFER가  쓰인다.
   * - to_addr
     - string 
     - | TEDN base58 encoded public key    
   * - from_addr
     - string
     - | TEDN base58 encoded public key
   * - amount
     - string
     - | decimal(18) 인 token이다. 
       | 내부적으로 처리할 때에는 bignum으로 변환한다. 
       | protocol buffer에서는 64가 최대크기이므로 
       | string으로 변환하여 처리한다.
   * - dapp_name
     - string
     - | DApp 이름
   * - trasnfer_type
     - string
     - | payout인지, betting, 또는 
       | 이후 있을 dapp과 사용자간 transfer type을 의미한다.
       | pr은 payout rollback, br은 betting rollback을 의미한다.
       
       
* Transaction
Transaction을 주고 받을 때 사용하는 메세지이며,   버전 정보와 TransactionBody에 대한 sign된 값을 가지고 있다.

- 설명
.. list-table::
   :widths: 10, 20, 50
   :header-rows: 1
   
   * - Field 
     - Type
     - | Description
   * - version
     - string
     - | "1.0"​으로 설정하며, 추후 transaction자체가 
       | 변경되는 경우 변경 가능하다.
   * - transaction
     - Transaction
     - | TEDN 처리에 대한 TransactionBody이다.
   * - from_sign_hashed
     - string
     - | TransactionBody의 from_addr의 private key로 
       | 전달된 transaction을 SerializeToString후 
       | signing한 값을 base58로 encoding한 값이다.
       
3. BalanceRequest

TEDN사용자의 Balance를 얻을 때 사용한다.      

설명
​addr	string ​	Balance를 가진 TEDN사용자의 base58 encoding된 public key​

4. BalanceResponse

TEDN 사용자의 BalanceRequest에 대한 응답이다. TEDN사용자의 현재 balance값을 return한다.

설명
​addr	string​	​addrstring ​Balance를 가진 TEDN사용자의 base58 encoding된 public key​​
amount	string	
decimal(18) 인 token이다. 내부적으로 처리할 때에는 bignum으로 변환한다. 

protocol buffer에서는 64가 최대크기이므로 string으로 변환하여 처리한다.

5. BalanceResponseList

TRANSACTION이 정상적으로 완료되는 경우 from_addr과 to_addr의 잔액에 대한 값이다.

설명
​response	BalanceResponse ​	repeated type이며, 처음 element는 from_addr의 잔액, 다음 element는 to_addr의 잔액이다.​
txkey	string	
추후 BS에서 rollback을 위해 사용되는 txkey. cs에서 ethereum의 tx를 체크하고 해당 tx값이 rollback되었을 때 bs에서 

처리된 값도 rollback되어야 한다. 한데, cs는 사용자가 아니므로 token값이 없으므로 해당 txkey를 return하면

rollback할 때 해당 txkey를 http header에 실어서 보내면 token이 없어도 해당 값을 수행한다.

단, 이경우는 단순 rollback만 수행하게 된다.

만약 bs가 아닌 다른 서버가 해당 tx를 받았을 경우는 그냥 ''가 리턴된다.

6. TransactionHash

Transaction을 찾기 위한 Hash값이며, 보통 E-Wallet에서 EDN을 Deposit을 하였을 경우 해당 Transaction에 대한 hash값을 보낸다.

사용자는 IAM Token으로 구분하는데, 이값은 HTTP Header에 들어있다.

설명
​txhash	string​	
E-Wallet에서 Ethereum network으로 transfer를 하면 해당 transaction에 대한 hash가 나오는데 해당 값을 가지고 추후 Transaction이

성공했는지 실패했는지 확인이 가능하다.​


7. WithdrawRequest

E-Wallet에서 CS에게 TEDN을 차감하고 EDN을 늘려달라는 요청을 할 때 쓰인다.

사용자는 IAM Token으로 구분하는데, 이값은 HTTP Header에 들어있다.

설명
​eth_address	string​	ethereum address​, EDN을 받을 주소를 의미한다.
tedn_amount	string	차감할 TEDN값이며, 이값은 1:1로 EDN으로 변환되어 지정한 eth_address로 전달된다.


8. HDAddress

E-Wallet이 CoinServer로 CoinServer의 HDAddress를 요청하였을 경우 리턴되는 메세지이다.

설명
​hd_address	string​	Coin Server의 HD Address로, E-Wallet이 EDN을 줄 떄 받는 주소이다.​


9. TransactionListRequest

E-Wallet이 TransactionServer로 현재 사용자의 Transaction을 요청할 때 쓰이는 메세지이다.

설명
​page	int​32	transaction리스트를 E-Wallet이나 기타 서버에서 요청할 때 어느 page를 리턴할지 요청한다.​
countperpage	int32	1page당 표시될 row count이다.

10. TransactionReceipt 

TransactionReceipt는 실제 Transaction외에도 생성 날짜등의 부가 정보를 가지고 있으며, supernode의 block commit에 의해 생성된 db값을 읽어 들인다.

설명
​to_addr	string​	TEDN을 받은 주소​
from_addr	string	TEDN을 전달한 주소
amount	string 	decimal 18인 전달된 TEDN 값이다.
regdate	Timestamp	TEDN이 전달된 시간이다.


11. TransactionListResponse

TransactionListRequest로 요청된 현재 사용자의 Transaction에 대한 응답이다. 내부 TransctionBody의 Type은 언제나 RANSACTION_TRANSFER로 정의된다.

설명
​totalcount	int32​	전체 tranction count​
currentpage	int32	transaction request에서 요청된 page이며, 현재 요청된 page이다.
transactions	TransactionReceipt	TransactionReceipt의 array이며, transaction내용을 가지고 있다.


12. TransactionStream

DApp서버 등에서 Binary Transaction을 TS로 보낼때 쓴다. 

해당 데이타는 내부적으로는 Signing까지 완료된 데이타 이므로 TS에서는 proxy처럼 해당 데이타를 SuperNode에 포워딩한다.

설명
​version	int32​	data ​version으로, 버전에 따라 data의 형태가 달라질 수 있다. 현재 값은 1이다.
data	string	version 1인 경우 json string이다.

13. BalanceRollbackRequest

Coinserver에서 ethereum transaction을 수행 후 ethereum tx가 실패하여 관련 tx를 rollback하고 싶을 때 사용한다. 해당 정보는 BalanceListResponse에서 BS에서 보내는 정보이다.

설명
​txkey	string	BS가 보낸 txkey이다. 이는, txhash가 아니다.

