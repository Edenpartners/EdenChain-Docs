Public Interface Specification
==============================

Overview
E-Wallet과의 연동을 위한 interface를 설명한다.


Interfaces
모든 interface는 JSON-RPC 2.0으로 작성되어 있다.  모든 Amount의 decimal은 18이다. 

JSON-RPC의 result값은 통일된 Response 형태의 JSON으로 구성된다.



API Response
Err code
Error Code로 실행의 결과를 알려준다.
Err Message
메시지가 필요한 경우에 이곳을 이용해 메시지를 전달한다.
Data
처리 결과로 제공되어야 하는 데이터가 있는 경우 이곳에 데이터를 집어넣는다.
Example
{'err_code':0 , 'msg': 'ok', 'data':{} }

{'err_code':0 , 'msg': 'ok', 'data': { 'data1':1, 'data2':2 } }
{'err_code':-1 , 'msg': 'no matched data', 'data': {} }
Error code
Error Code가 '0'인 경우에는 성공을 뜻하고, 음수의 경우 Server Error, 양수인 경우 Client Error로 정의한다.

Predefined Error Code는 다음과 같다.

Error Code
Description
0	Success
-1	Server Internal Error
1	Client Parameter Missing


Confirm EDN deposit
EDN을 TEDN으로 변환하기 위해 EdenChain서버로 transfer를 수행한 후 해당 transfer가 성공했는지 확인한다.

result값이 True일 경우 성공한 것이며, 이 때 TEDN balance Check를 하여 잔액을 확인가능하다.



POST /api

Request

JSON-RPC method : "user.deposit"

JSON-RPC Parameter:

"iamtoken" (string) - IAM 인증 토큰   - Firebase를 통한 인증성공시 받아지는 값

"txhash" (string) - Transaction Hash - Ethereum을 통해 EDN을 전달했을때의 Transaction Hash값



Result

        Status Code:

200  - OK

202  - Transaction is pending

404 - Requested TX not found

Return: inresult

정의된 API Response값으로 리턴

성공: err_code=0



2. Withdraw TEDN
TEDN을 EDN으로 변환할 때 쓰이는 API이다. 

해당 API를 받으면 TEDN잔액이 충분하면 요청한 금액만큼 decrease되며, 해당 TEDN은 EDN과 1:1 pegging되므로 해당 금액 만큼 사용자에게 transfer된다. 

리턴으로는 해당 tx hash값이 전달되므로, 해당 값을 이용하여 tx가 성공했는지 Pending중인지 확인이 가능하다. 

BlockChain의 특성상 실패할 수가 있으므로 이 경우 서버 내부에서는 금액이 rollback되나, 사용자에게는 전달되는 항목이 없으므로 따로 확인해야 한다.



POST /api

Request

JSON-RPC method : "user.withdraw"

JSON-RPC Parameter:

"iamtoken" (string) - IAM 인증 토큰

"ethaddress" (string) - to EDN address (may be registered to IAM profile)

"amount" (string) - TDEN value(18 decimals) to withdraw/ transaction fee (gas) is provided in CS wallet.

Result

Status Code:

200 - OK

403 - Your Request is forbidden

Return: in result

정의된 API Response값으로 리턴

성공: err_code=0, data={'txhash': transaction_hash_value }



3. Get CoinServer Ethereum Address
Deposit을 위해서는 Edenchain서버로 EDN을 전달해야 한다.  해당 API를 수행하여 EdenChain으로 전달할 주소를 전달 받는다.



POST /api

Request

JSON-RPC method : "server.coinhdaddress"

JSON-RPC Parameter: 

"iamtoken" (string) - IAM 인증 토큰

Result

Status Code:

200 - OK

403 - Your Request is forbidden

Return: in result

정의된 API Response값으로 리턴

성공: err_code=0, data={'hdaddress': coinserver_hd_address_to_transfer}



4. Get My Balance
Edenchain서버로 TEDN의 현재 잔액을 요청한다.



POST /api

Request

JSON-RPC method : "user.getbalance"

JSON-RPC Parameter:

"iamtoken" (string) - IAM 인증 토큰

Result

        Status Code:

200  - OK

Return: in result

정의된 API Response값으로 리턴

성공: err_code=0, data={'amount': current_balance_of_tedn_account}



5. List User Transaction
IAM token에 해당되는 사용자의 Transaction정보를 가져온다.

from_addr과 to_addr모두 검색에서 확인되어야 한다.

해당 Transaction은 SuperNode에서 Block Commit후 완료된 정보이다. (TRANSACTIONCOMMIT을 의미한다.)



POST /api

Request

JSON-RPC method : "user.lstransaction"

JSON-RPC Parameter:

"iamtoken" (string) - IAM 인증 토큰

"page" (integer) - 읽어들일 page. 0이면 최근값이다.

"countperpage" (integer) - 1페이지에 보여줄 레코드 값



Result

        Status Code:

200  - OK

202  - Transaction is pending

404 - Requested TX not found

Return:  in result

정의된 API Response값으로 리턴

성공: err_code=0, data={

"totalcount" (integer) - 모든 count

"currentpage" (integer) - request에서 요청한 현재 page

"transactions" (array) - 다음 object의 array형태로 리턴된다.

 [ {"from_addr", (string) - amount가 빠져나가는 주소

    "to_addr", (string) - amount가 들어가는 주소

    "amount", (string) - amount, decimal 18자리 

   "regdate", (int) -  시간, 초단위

  }]

}



6. user.update_profile
POST /api
Parameter
Name
Type
Description
iamtoken	String	IAM 인증 토큰
display_name	String	화면에 표시할때 사용하는 이름
Response
err_code
msg
data
display_name


7. user.get_info
POST /api
user.get_info는 일반 사용자가 자신의 정보를 가져오기 위한 것으로 자신의 정보만 가져올 수 있으며, 다른 사용자정보는 알수가 없다. 그래서 별도의 파라메터가 필요없다.

Parameter
​iamtoken	String​	IAM 인증 토큰​
Response
err_code
msg
data
email
address
eth_address
eth_address1
eth_address2
8. user.signup
POST /api
실질적인 Signup은 firebase에서 처리가 되기 때문에 EIAM에서는 email, password의 정보외에 부수적인 처리를 하기 위해 사용한다.

EIAM Server는 signup을 받으면 해당 사용자를 EIAM의 NDB에 생성하고, 생성된 사용자에게 tedn_wallet를 생성하고 이것을 서버에 저장한다.

Parameter
iamtoken	String 	IAM 인증 토큰
테스트에서는 별도의 사용자정보를 입력받지 않았기 때문에 파라메터가 없는데, 추후 간단하게 사용자정보를 추가할 수 있다.

Response
err_code
msg
data
9. user.signin
POST /api
Firebase에서 성공적으로 sign-in을 한후에 호출하는 것으로 파라메터가 없다. 현재는 별다른 처리를 하고 있지 않다.

Parameter
iamtoken	String	IAM 인증 토큰
Response
err_code
msg
data
10. user.signout
POST /api
Firebase에서 성공적으로 sign-out을 한후에 호출하는 것으로 파라메터가 없다. 현재는 별다른 처리를 하고 있지 않다.

Parameter
iamtoken	String	IAM 인증 토큰
Response
err_code
msg
data
11. eth.add_address
POST /api
현재 사용하고 있는 사용자에 ETH Address를 추가한다.

Parameter
Name
Type
Description
iamtoken	String	IAM 인증 토큰
address	string	추가하려고 하는 eth address
Response
err_code
msg
data


12. eth.del_address
POST /api
현재 사용하고 있는 사용자에 ETH Address를 삭제한다.

Parameter
Name
Type
Description
iamtoken	string	IAM 인증 토큰
address	string	삭제하려고 하는 eth address
Response
err_code
msg
data
