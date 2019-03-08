Eden API Specification
======================
 
- Overview
이 문서는 EdenChain Platform에서 사용하는 API 표준을 설명하기 위한 문서이다.

이곳에서는 API Call Type, API Response 그리고 API Call 방법에 대해 설명한다.



API - JSON RPC
EdenChain Platform에서 API를 개발할때 특별한 제약사항이 없다면 JSON-RPC 2.0을 이용해서 개발한다.

블록체인계에서 JSON-RPC를 많이 사용하기 때문에 우리도 따라서 사용한다.

아래의 링크는 Flask에 JSON RPC를 쉽게 사용할 수 있는 라이브러리이다.

https://github.com/cenobites/flask-jsonrpc



API Response
모든 API의 Response는 JSON이며 다음과 같은 형태로 구성한다.

API Response
Err code
Error Code로 실행의 결과를 알려준다.
Err Message
메시지가 필요한 경우에 이곳을 이용해 메시지를 전달한다.
Data
처리 결과로 제공되어야 하는 데이터가 있는 경우 이곳에 데이터를 집어넣는다.
Example
{'code':0 , 'msg': 'ok', 'data':{} }

{'code':0 , 'msg': 'ok', 'data': { 'data1':1, 'data2':2 } }
{'code':-1 , 'msg': 'no matched data', 'data': {} }
Error code
Error Code가 '0'인 경우에는 성공을 뜻하고, 음수의 경우 Server Error, 양수인 경우 Client Error로 정의한다.

Predefined Error Code는 다음과 같다.



0	Success
-1	Server Internal Error
1	Client Parameter Missing


JSON RPC Call by javascript
JSON RPC를 Javascript에서 호출할때는 다음과 같은 방식으로 호출한다. Javascript client library를 검색해보았으나 굳이 사용할 필요를 느끼지 못해 Jquery를 이용해 호출하는 것으로 작성하였다.



    $.ajax(backendHostUrl + '/api', {

        headers: {

          'Authorization': 'Bearer ' + userIdToken

        },

        method: 'POST',

        data: JSON.stringify( {jsonrpc:'2.0',method:'add', params:[1400,2100], id:"jsonrpc"} ),

        contentType : 'application/json'

      }).then(function(response){

      console.log('btn_test success',response);

      });



호출하는 방식은 일반적인 Ajax Call과 거의 다음이 없고 Data에 위와 같은 형식으로 데이터를 지정하면 된다.

headers에 Firebase로부터 부여받은 Token ID를 서버에 전달하는 역할을 한다.

만약 Token ID를 사용할 필요가 없다면 당연히 생략해도 되는 부분이다.



참고로 아래 코드는 EIAM에서 테스트를 위해 사용한 Javascript function이다.



  function call_jsonrpc(uri,method,param,callback)

  {

  $.ajax(backendHostUrl + uri , {

        headers: {

          'Authorization': 'Bearer ' + userIdToken

        },

    method: 'POST',

    data: JSON.stringify( {"jsonrpc":"2.0", "method":method, "params":param, "id":1} ),

    contentType : 'application/json'

    }).then(function(response){

    callback(response);

    });

  }



Call Sample

      call_jsonrpc("/api","user.signin",[], function(response) {

      console.log("Sign in2 successful",response);

      });



Flask JSON RPC
아래에 나와있는 샘플코드처럼 Decorator에 반드시 Argument를 넣어주어야 한다. 너무도 당연한 이야기지만



@jsonrpc.method('server.delete_user(email=str)')

def handler_server_delete_user(email=''):

    claims = common.verify_token(HTTP_REQUEST,request)

    if not claims:

        common.unauthorized_response()

        return

    

    a_user = EUser.del_user_by_email(email)

    response = common.build_json_response(0, '', '')    

    

    return response
