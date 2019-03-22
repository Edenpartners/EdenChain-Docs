.. image:: images/Architecture/Hyper_Node.png
    :width: 750px
    
Hyper Node
==========

Hyper node means Edenchain's blockchain. In Edenchain's architecture,
transaction handling is mainly done by super node, and hyper node's
major role is anchoring data and processing its native coin.

Edenchain does not put all the data produced by super node into its
blockchain for scalability. In this picture there is a possibility super
node might manipulate data, so to prevent from such kinds of cheating,
hyper nodes does anchoring. Whenever super nodes generate new
transaction data, anchoring will be done automatically by anchoring
server in conjunction with transaction server. 

Anchoring
---------

Coin server and Transaction server are supposed to do anchoring whenever
it processes transactions. 

Hyper node stores anchoring value to dedicated internal namespace. It is
only accessible by special transaction processor. 

The transaction processor has writing function only and the processor
puts written time into global state.

Data to be anchored
-------------------

By Coin server
~~~~~~~~~~~~~~

Coin Server가 Deposit하거나 Withdraw하였을 때 해당 Audit 데이타는
저장소에 저장된다. 이를 통해 나중에 사용자의 요청을 처리하게 되는데,
해당 값이 임의로 변경되어 찾을 수 없거나, 다른 이유로 찾지 못할 경우가
있다.

이런 경우가 발생되면, 어디까지가 믿을 수 있는 값이고 어디까지가 믿을 수
없는 값인지 확인이 불가능하다.

이때, 해당 값을 특정 단위로 잘라서 Hyper Node에 Anchoring을 하여 놓았을
경우, 나중에 문제가 생겼을 경우 비교하여 특정 부분에 대한 문제를 파악이
가능하다. 

10개 단위씩 잘라서 Hash후 Tx하나로 하고 1개의 배치는 10개의 
Transaction으로 하여 서버에 올리게 되며, 따라서 Anchoring은 1-10까지를
1개의 Transaction, 배치는 10개의 Transaction이므로 총 100개의
Transaction을 1개로 하여 올리게 된다. (자르는 단위는 Record ID로 한다.)

    10 records → 1 TX  =>  10TX → 1 Batch

서버 리스타트등으로 마지막 Anchor한 데이타를 모르는 경우 마지막 자리
수를 가지고 NameSpace가 존재하는지 확인하여 존재하지 않는 경우를
최신으로 한다.

데이타의 Hash는 모든 값을 붙인 후 (숫자도 string처럼 취급) 해당 값을
sha512로 hash한 값으로 삼는다.

By Transaction Server
~~~~~~~~~~~~~~~~~~~~~

Transaction Server는  TX를 받아 저장하는 것과 해당 값을 Super Node에
insert한 후 해당 Super Node에서 성공하였을 경우 해당 event를 받아서
저장하는 작업을 처리한다.

따라서,  무엇을 Anchoring할 지는 Transaction을 처리하는 시차때문에
처리되기 전의 데이터와 처리된 후의 데이터 2가지가 있을 수 있다.

TX와 배치의 구성도 10개 record를 1개의 tx로, 10개의 tx가 1개의 batch로
구성되는 것은 Coin Server와 동일하다.

Data의 hash의 값은 위에서 설명한 것 처럼 join후 나온 레코드의 모든 값을
붙여서 해당 값을 sha512로 hash한 값을 사용한다.

Incremental Anchor
~~~~~~~~~~~~~~~~~~

Anchor TP에 해당 계산된 Sha512를 넣기 위해서는  ID값은 계속 증가하게
되므로 Anchor Server는 일정시간마다 증가한 ID를 체크하여 지속적으로
Hash를 만든 후 push를 하게 된다.

이 때, 기존 push한 것들은 skip하고 새로 추가된 것들만 할 수 있어야
하는데 우선 어디까지 insert했는지 last_id는 저장 한다.
