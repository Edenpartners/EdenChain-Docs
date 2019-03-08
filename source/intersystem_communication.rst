Intersystem communication
-------------------------

- Overview
서버간 통신은 TLS로 하도록 하며, 일반적인 서버단의 암호화를 통한 것 뿐아닌 인증서의 Validation을 통해 접속하는 사용자가 맞는지도 인증서를 통해 확인한다.



- Architecture
인증서는 Edenchain내부 사설 인증서를 통해 발급을 한다.



현재는 인증서의 Validation, 즉 OCSP나 CRL은 없다. 



서버는 NGINX를 통해 인증서 확인을 시도한다. 이후, 해당 데이타는 바로 Python으로 작성된 서버로 넘기도록 한다.

따라서, 



 → Client HTTP → NGINX → CS/BS



이때, Client HTTP가 인증서를 통해 TLS접속을 하며, NGINX는 해당 인증서가 EdenChain의 CA에서 발급된 인증서인지 확인한다.

우선은 인증서로 보안성을 유지하고 있으며, 추후 API Key등을 넣을지는 추후 고민한다.



해당 접속은 서버간 접속에만 해당된다.

이때, App-Engine도 서버이므로 내부 통신할 때 (Protocol buf 통신등) 해당 방식을 따라 접속한다.

Development
Client
    서버단에서 사용하는 인증서를 사용하여 인증을 수행한다.

2. Server

서버인증서는 Secret Store에 저장된다.

인증서 생성
기존 문서에 있는 방식으로 생성한다.

단, CA가 있는 서버는 1차 버전은 Compute Engine에서 작성한다. 서버 이름은 ca-server-inter-tls이며, micro 1CPU이므로 1 Month 4.28 $ 이다.



AppEngine Cert (hostname: edencore)
CS Cert (hostname: cs1)
TS Cert (hostname : ts1)
BS Cert (hostname: bs1)
DApp Cert (hostname: dapp1)


5개 인증서 생성한 후 각 인증서를 다음 Kubernetes Secret Volume에 넣는다.

AppEngine은 코드 Local에 넣으며, request로 외부 요청할 때에만 사용한다.



File 이름:

AppEngine.edencore.pem

BS.bs1.pem

CS.cs1.pem

DAppServer.dapp1.pem

TS.ts1.pem



cacert.pem



다음과 같이 생성하였으며, 위 화일은 cert-svr디렉토리에 존재한다.

kubectl create secret generic cert-secret --from-file=cert-svr



Configuration
AppEngine은 request할 때 다음과 같이 인증서를 달아서 인증한다.



Server의 경우 우선 기존 접속은 놔두고 NGINX를 통한 접속을 추가한다.

각 포트는 기존 8xxx대신 9xxx로 이며, 뒤 3자리는 동일하게 조정한다.



Certificate Authentication


같은 CA에서 발급되는 것으로만 체크하며, 추후 cert pem을 http header에 넣어서 해당 인증서의 AltSubjectName을 체크할 예정임.



현 버전에서는 client는 server의 인증서를 체크하지 않고, 서버는 CA에서 발급된  것으로만 체크함


=> BS에서는 Deposit의 용도로, AltSubjectName을 체크하고 있다. - 타서버는 추후 확장 예정임.
