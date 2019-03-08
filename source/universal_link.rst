Universal Link 정의
===================

Overview
Web서버에서 앱에 연동할 수 있도록 urlscheme을 정의한다. 이때, 설치를 위한 urlscheme은 그대로 두고, withdraw나 deposit은 universal link로 처리하여 앱이 없을 경우 브라우저를 통해 설치 화면이 나올 수 있도록 정리한다.



Description
userid_hash는 사용자 id를 sha256()후 hex string로 바꾼 후 앞 32자리만 보낸다. 

다음은 python에서 수행하는 예이다.

import hashlib
hashlib.sha256('id@id.com'.encode()).hexdigest()[:32]


이때,  Web에서 Login한 ID와 APP의  ID가 혹시 틀릴 수 있으므로  앱에서는 요청을 받았을 때 id를 비교해서 틀리면 다른 계정으로 로그인해달라고 알람을 표시하는데 목적이 있다.

id가 없을 경우에는 체크하지 않고 띄워준다. (예: edenwallet://?action=withdraw)



Android
Play 연결 :  market://details?id=io.edenchain.ewallet
Withdraw 수행 : https://ewallet.edenchain.io/withdraw/?id=userid_hash
Deposit 수행 : https://ewallet.edenchain.io/deposit/?id=userid_hash
iOS
AppStore 연결: itms-apps://itunes.apple.com/app/id1234567890 (appstore에 upload후 app id가 결정되면 업데이트.)
Withdraw 수행 : https://ewallet.edenchain.io/withdraw/?id=userid_hash
Deposit 수행 : https://ewallet.edenchain.io/deposit/?id=userid_hash


E-garden에서는 초기 설치화면에서는 그대로 play, appstore연결은 동일하게 그대로 둔다.

                           이후, withdraw, deposit은 위에서 정의한 대로 변경한다.

DApp은 withdraw, deposit호출시의 url을 위와 같이 수정한다.

E-Wallet App의 경우는 각 withdraw 수행, deposit수행만 처리하도록 수정한다
