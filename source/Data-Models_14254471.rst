===========================
Data Models
===========================

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
            #. `dApp Development <dApp-Development_124780598.html>`__

         .. rubric:: Data Models
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by Jacki Heo, last modified by Hailee Kim on Mar 27,
            2019

         .. container:: wiki-content group
            :name: main-content

            .. rubric:: Jira Link
               :name: DataModels-JiraLink

            .. rubric:: `|image0|\ EW-65 <https://edenchain.atlassian.net/browse/EW-65>`__\ -
               Data Models Specification Done
               :name: DataModels-SystemJIRAkey,summary,type,created,updated,due,assignee,reporter,priority,status,resolutiond606b66f-45a3-32f1-8426-452683fe0d6eEW-65

            Overview

            Communication between internal core components is done in a
            Restful form, and the content-type is application /
            x-protobuf.

            That is, communication is performed with Protocol Buffers.
            Since this is a definition of internal communication, this
            part does not have to be concerned by the developer or the
            user.

            Since the Protocol Buffers Message to be communicated should
            be the same, the related part is defined here.

            | 

            | 

            .. rubric:: Messages
               :name: DataModels-Messages

            syntax uses proto3.

            The package is defined as edenchain.

            | 

            1. TransactionBody

            This is the message used when all transactions of TEDN are
            processed.

            | 

            .. container:: table-wrap

               ==========================
               message TransactionBody  {
               
               | 
               
               enum TXMessageType{
               
               TRANSACTION_DEPOSIT = 0;
               
               TRANSACTION_WITHDRAW = 1;
               
               TRANSACTION_TRANSFER = 2;
               
               }
               
               | 
               
               TXMessageType tx_type = 1;
               
               string to_addr = 2;
               
               string from_addr  = 3;
               
               string amount=4;
               
               string dapp_name = 5;
               
               string transfer_type = 6;
               
               }​
               ==========================

            -  Explanation

                      enum TXMessageType

            It summarizes the kind of message required for the
            transaction.

            .. container:: table-wrap

               ==================== =============================================
               Enum Name            Explanation
               ==================== =============================================
               ​TRANSACTION_DEPOSIT ​CS→Change EDN to TEDN with BS
               TRANSACTION_WITHDRAW CS→Change TEDN to EDN with BS
               TRANSACTION_TRANSFER dAppServer→betting or paying out TEDN with BS
               ==================== =============================================

                    

            .. container:: table-wrap

               ============= =============== ==========================================================================================
               Field Name    Type            Explanation
               ============= =============== ==========================================================================================
               tx_type       TXMessageType ​ This is transaction operation
                                            
                                             Among the defined enum,
                                            
                                             TRANSACTION_DEPOSIT/
                                            
                                             TRANSACTION_WITHDRAW/
                                            
                                             TRANSACTION_TRANSFER
                                            
                                             are used.
               to_addr       string          TEDN base58 encoded public key
               from_addr     string          TEDN base58 encoded public key
               amount        string          It is a token that is decimal (18). When processing internally, it is converted to bignum.
                                            
                                             In the protocol buffer, 64 is the maximum size, so it is converted to string.
               dapp_name     string          DAPP name
               transfer_type string          It means payout, betting, or the transfer type between dapp and the user afterwards.
                                            
                                             pr means payout rollback and br means betting rollback.
               ============= =============== ==========================================================================================

            | 

            2. Transaction

            It is a message used to exchange transactions, and has
            version information and signed values ​​for TransactionBody.

            | 

            .. container:: table-wrap

               ================================
               message Transaction {
               
               string version = 1;
               
               TransactionBody transaction = 2;
               
               string from_sign_hashed = 10;
               
               }​
               ================================

            -  Explanation

            .. container:: table-wrap

               ================ =========== =================================================================================================================================================================
               Field Name       Type        Explanation
               ================ =========== =================================================================================================================================================================
               ​version         string​     It is set to "1.0" and can be changed if the transaction itself changes later.
               transaction      Transaction It is TransactionBody of the process for TEDN.
               from_sign_hashed string      This is the base58 encoded value of the signing after SerializeToString of the transaction that was transferred with private key of from_addr of TransactionBody.
               ================ =========== =================================================================================================================================================================

            | 

            3. BalanceRequest

            It is used to get a balance of TEDN users.

            | 

            .. container:: table-wrap

               ========================
               message BalanceRequest {
               
               string addr = 1;
               
               }​
               ========================

            -  Explanation

            .. container:: table-wrap

               ========== ======== ===================================================
               Field Name Type     Explanation
               ========== ======== ===================================================
               ​addr      string ​ Base58 encoded public key of TEDN user with Balance
               ========== ======== ===================================================

            | 

            | 

            4. BalanceResponse

            This is the response to the TEDN user's BalanceRequest.
            Returns the current balance value of the TEDN user.

            .. container:: table-wrap

               =========================
               message BalanceResponse {
               
               string addr = 1;
               
               string amount = 2;
               
               }​
               =========================

            -  Explanation

            .. container:: table-wrap

               ========== ======= ==================================================================================
               Field Name Type    Explanation
               ========== ======= ==================================================================================
               ​addr      string​ base58 encoded public key of TEDN user with addrstring balance
               amount     string  It is a token that is decimal (18). When processing internally, convert to bignum.
                                 
                                  In the protocol buffer, 64 is the maximum size, so convert it to string.
               ========== ======= ==================================================================================

            5. BalanceResponseList

            The value for the balance of from_addr and to_addr when
            TRANSACTION completes normally.

            .. container:: table-wrap

               ====================================
               message BalanceResponseList{
               
               repeated BalanceResponse response=1;
               
               string txkey=2;
               
               }​
               ====================================

            -  Explanation

            .. container:: table-wrap

               ========== ================= ================================================================================================================================================================================================================================================================================================================================================================================================================================================
               Field Name Type              Explanation
               ========== ================= ================================================================================================================================================================================================================================================================================================================================================================================================================================================
               ​response  BalanceResponse ​ repeated type, the first element is the balance of from_addr, and the next element is the balance of to_addr.
               txkey      string            In later txkey.cs used for the rollback in the BS, tx of Ethereum should be checked and the value processed at bs should be rolled back when the corresponding tx value is rolled back. However, because cs is not a user, there is no token value.When returning the corresponding txkey, when the rollback is performed, the corresponding txkey is sent to the http header and the corresponding value is executed even if there is no token.
                                           
                                            However, in this case, only a simple rollback is performed.
                                           
                                            If a server other than bs receives the tx, just '' is returned.
                                           
                                            | 
               ========== ================= ================================================================================================================================================================================================================================================================================================================================================================================================================================================

            6. TransactionHash

            It is a hash value for transaction search, and when an EDN
            is deposited in an E-Wallet, it sends a hash value for the
            transaction. The user is identified by the IAM Token, which
            is in the HTTP Header.

            .. container:: table-wrap

               ========================
               message TransactionHash{
               
               string txhash=1;
               
               }​
               ========================

            -  Explanation

            .. container:: table-wrap

               ========== ======= ===================================================================================================================================================================================
               Field Name Type    Explanation
               ========== ======= ===================================================================================================================================================================================
               ​txhash    string​ If you transfer from E-Wallet to Ethereum network, hash of the transaction appears. This allows you to check whether the transaction succeeded or not with the corresponding value.
               ========== ======= ===================================================================================================================================================================================

            7. WithdrawRequest

            It is used to ask CS to deduct TEDN and increase EDN from
            E-Wallet.

            The user is identified by the IAM Token, which is in the
            HTTP Header.

            .. container:: table-wrap

               ========================
               message WithdrawRequest{
               
               string eth_address=1;
               
               string tedn_amount=2;
               
               }
               ========================

            -  Explanation

            .. container:: table-wrap

               ============ ======= ==================================================================================================================================
               Field Name   Type    Explanation
               ============ ======= ==================================================================================================================================
               ​eth_address string​ The Ethereum address to receive the EDN.
               tedn_amount  string  This value is the TEDN value to be subtracted. This value is converted to EDN with 1:1 and is passed to the specified eth_address.
               ============ ======= ==================================================================================================================================

            8. HDAddress

            This is the message returned when the E-Wallet requests
            CoinServer's HDAddress with CoinServer.

            .. container:: table-wrap

               ====================
               message HDAddress{
               
               string hd_address=1;
               
               }​
               ====================

            -  Explanation

            .. container:: table-wrap

               =========== ======= =================================================================================
               Field Name  Type    Explanation
               =========== ======= =================================================================================
               ​hd_address string​ Coin Server's HD Address, which is the address the E-Wallet will receive the EDN.
               =========== ======= =================================================================================

            9. TransactionListRequest

            It is a message used by E-Wallet to request the transaction
            of the current user to TransactionServer.

            .. container:: table-wrap

               ================================
               ​message TransactionListRequest{
               
               int32 page = 1;
               
               int32 countperpage = 2;
               
               }
               ================================

            -  Explanation

            .. container:: table-wrap

               ============ ====== ==============================================================================================
               Field Name   Type   Explanation
               ============ ====== ==============================================================================================
               ​page        int​32 Requests which page to return when requesting transaction list from E-Wallet or other servers.
               countperpage int32  The row count to display per page.
               ============ ====== ==============================================================================================

            10. TransactionReceipt 

            In addition to the actual Transaction, TransactionReceipt
            has additional information such as the creation date, and
            reads the db value generated by the block commit of the
            supernode.

            .. container:: table-wrap

               ===========================
               message TransactionReceipt{
               
               string to_addr = 1;
               
               string from_addr  = 2;
               
               string amount=3;
               
               Timestamp regdate=4;
               
               }​
               ===========================

            -  Explanation

            .. container:: table-wrap

               ========== ========= ====================================
               Field Name Type      Explanation
               ========== ========= ====================================
               ​to_addr   string​   Address to receive TEDN​
               from_addr  string    Address to send TEDN
               amount     string    The decimal 18 delivered TEDN value.
               regdate    Timestamp The time took to deliver TEDN.
               ========== ========= ====================================

            11. TransactionListResponse

            This is the response to the transaction of the current user
            requested by TransactionListRequest. The Type of the
            internal TransctionBody is always defined as
            RANSACTION_TRANSFER.

            .. container:: table-wrap

               ===========================================
               ​message TransactionListResponse{
               
               int32 totalcount=1;
               
               int32 currentpage=2;
               
               repeated TransactionReceipt transactions=3;
               
               }
               ===========================================

            -  Explanation

            .. container:: table-wrap

               ============ ================== ==========================================================================================
               Field Name   Type               Explanation
               ============ ================== ==========================================================================================
               ​totalcount  int32​             total transaction count​
               currentpage  int32              This is the page requested in the transaction request and is the page currently requested.
               transactions TransactionReceipt An array of TransactionReceipt, containing transaction content.
               ============ ================== ==========================================================================================

            12. TransactionStream

            It is used to send Binary Transaction to TS in DApp server
            etc.

            Since the data is data that internally has been completed
            until signing, the TS forwards the data to the SuperNode as
            a proxy.

            .. container:: table-wrap

               ===========================
               ​message TransactionStream{
               
               int32 version=1;
               
               string data=2;
               
               Transaction tx=3;
               
               }
               ===========================

            -  Explanation

            .. container:: table-wrap

               +------------+-----+---------------------------------------------------+
               | Field Name | Typ | Explanation                                       |
               |            | e   |                                                   |
               +============+=====+===================================================+
               | ​version   | int | The data version, depending on the version, the   |
               |            | 32​ | type of data can be different. The current value  |
               |            |     | is 1.                                             |
               +------------+-----+---------------------------------------------------+
               | data       | str | For version 1, it is a json string.               |
               |            | ing |                                                   |
               +------------+-----+---------------------------------------------------+

            13. BalanceRollbackRequest

            It is used when you want to roll back related tx after
            ethereum tx fails when Ethereum transaction is done in
            Coinserver. This information is sent from the BS in the
            BalanceListResponse.

            .. container:: table-wrap

               =================================
               ​message BalanceRollbackRequest {
               
               string txkey = 1; 
               
               }
               =================================

            -  Explanation

            .. container:: table-wrap

               +------------+-----+---------------------------------------------------+
               | Field Name | Typ | Explanation                                       |
               |            | e   |                                                   |
               +============+=====+===================================================+
               | ​txkey     | str | The txkey sent by the BS. This is not txhash.     |
               |            | ing |                                                   |
               +------------+-----+---------------------------------------------------+

            --------------

            --------------

            | 

            Overview

            내부 core component들 간의 통신은 Restful 형태로 통신을 하고
            있으며 Content-Type은 application/x-protobuf 이다.

            즉, Protocol Buffers로 통신을 하도록 한다.  내부 통신에 대한
            정의이므로 이 부분은 개발사나 사용자 측에서  값을 보거나
            호출할 일은 없다. 

            이때 통신하는 Protocol Buffers Message는 동일해야 하므로
            관련 부분을 정의한다. 

            | 

            | 

            .. rubric:: Messages
               :name: DataModels-Messages.1

            syntax는 proto3를 사용한다.

            package는 edenchain으로 정의한다.

            | 

            1. TransactionBody

            TEDN에 대한 모든 Transaction이 처리될 때 사용되는
            메세지이다.

            .. container:: table-wrap

               ==========================
               message TransactionBody  {
               
               | 
               
               enum TXMessageType{
               
               TRANSACTION_DEPOSIT = 0;
               
               TRANSACTION_WITHDRAW = 1;
               
               TRANSACTION_TRANSFER = 2;
               
               }
               
               | 
               
               TXMessageType tx_type = 1;
               
               string to_addr = 2;
               
               string from_addr  = 3;
               
               string amount=4;
               
               string dapp_name = 5;
               
               string transfer_type = 6;
               
               }​
               ==========================

            -  설명

                      enum TXMessageType

            Transaction에 필요한 메세지 종류 를 정리한다.

            .. container:: table-wrap

               ==================== =====================================================
               Enum 이름            설명
               ==================== =====================================================
               ​TRANSACTION_DEPOSIT ​CS→BS로 EDN을 TEDN으로 바꿀 경우
               TRANSACTION_WITHDRAW CS→BS로 TEDN을 EDN으로 바꿀 경우
               TRANSACTION_TRANSFER dAppServer→BS 로 TEDN을 betting하거나 payout하는 경우
               ==================== =====================================================

                    

            .. container:: table-wrap

               ============= =============== =============================================================================
               Field 이름    Type            설명
               ============= =============== =============================================================================
               tx_type       TXMessageType ​ transaction operation이다.
                                            
                                             정의된 enum중
                                            
                                             TRANSACTION_DEPOSIT/
                                            
                                             TRANSACTION_WITHDRAW/
                                            
                                             TRANSACTION_TRANSFER
                                            
                                              가  쓰인다.
               to_addr       string          TEDN base58 encoded public key
               from_addr     string          TEDN base58 encoded public key
               amount        string          decimal(18) 인 token이다. 내부적으로 처리할 때에는 bignum으로 변환한다. 
                                            
                                             protocol buffer에서는 64가 최대크기이므로 string으로 변환하여 처리한다.
               dapp_name     string          DAPP 이름이다.
               transfer_type string          payout인지, betting, 또는 이후 있을 dapp과 사용자간 transfer type을 의미한다.
                                            
                                             pr은 payout rollback, br은 betting rollback을 의미한다.
               ============= =============== =============================================================================

            | 

            2. Transaction

            Transaction을 주고 받을 때 사용하는 메세지이며,   버전
            정보와 TransactionBody에 대한 sign된 값을 가지고 있다.

            | 

            .. container:: table-wrap

               ================================
               message Transaction {
               
               string version = 1;
               
               TransactionBody transaction = 2;
               
               string from_sign_hashed = 10;
               
               }​
               ================================

            -  설명

            .. container:: table-wrap

               ================ =========== ===============================================================================================================================
               Field 이름       Type        설명
               ================ =========== ===============================================================================================================================
               ​version         string​     "1.0"​으로 설정하며, 추후 transaction자체가 변경되는 경우 변경 가능하다.
               transaction      Transaction TEDN 처리에 대한 TransactionBody이다.
               from_sign_hashed string      TransactionBody의 from_addr의 private key로 전달된 transaction을 SerializeToString후 signing한 값을 base58로 encoding한 값이다.
               ================ =========== ===============================================================================================================================

            | 

            3. BalanceRequest

            TEDN사용자의 Balance를 얻을 때 사용한다.

            | 

            .. container:: table-wrap

               ========================
               message BalanceRequest {
               
               string addr = 1;
               
               }​
               ========================

            -  설명

            .. container:: table-wrap

               ========== ======== =========================================================
               Field 이름 Type     설명
               ========== ======== =========================================================
               ​addr      string ​ Balance를 가진 TEDN사용자의 base58 encoding된 public key​
               ========== ======== =========================================================

            | 

            | 

            4. BalanceResponse

            TEDN 사용자의 BalanceRequest에 대한 응답이다. TEDN사용자의
            현재 balance값을 return한다.

            .. container:: table-wrap

               =========================
               message BalanceResponse {
               
               string addr = 1;
               
               string amount = 2;
               
               }​
               =========================

            -  설명

            .. container:: table-wrap

               ========= ======= ========================================================================
               Field이름 Type    설명
               ========= ======= ========================================================================
               ​addr     string​ ​addrstring ​Balance를 가진 TEDN사용자의 base58 encoding된 public key​​
               amount    string  decimal(18) 인 token이다. 내부적으로 처리할 때에는 bignum으로 변환한다. 
                                
                                 protocol buffer에서는 64가 최대크기이므로 string으로 변환하여 처리한다.
               ========= ======= ========================================================================

            5. BalanceResponseList

            TRANSACTION이 정상적으로 완료되는 경우 from_addr과 to_addr의
            잔액에 대한 값이다.

            .. container:: table-wrap

               ====================================
               message BalanceResponseList{
               
               repeated BalanceResponse response=1;
               
               string txkey=2;
               
               }​
               ====================================

            -  설명

            .. container:: table-wrap

               ========== ================= =================================================================================================================
               Field 이름 Type              설명
               ========== ================= =================================================================================================================
               ​response  BalanceResponse ​ repeated type이며, 처음 element는 from_addr의 잔액, 다음 element는 to_addr의 잔액이다.​
               txkey      string            추후 BS에서 rollback을 위해 사용되는 txkey. cs에서 ethereum의 tx를 체크하고 해당 tx값이 rollback되었을 때 bs에서 
                                           
                                            처리된 값도 rollback되어야 한다. 한데, cs는 사용자가 아니므로 token값이 없으므로 해당 txkey를 return하면
                                           
                                            rollback할 때 해당 txkey를 http header에 실어서 보내면 token이 없어도 해당 값을 수행한다.
                                           
                                            단, 이경우는 단순 rollback만 수행하게 된다.
                                           
                                            만약 bs가 아닌 다른 서버가 해당 tx를 받았을 경우는 그냥 ''가 리턴된다.
               ========== ================= =================================================================================================================

            6. TransactionHash

            Transaction을 찾기 위한 Hash값이며, 보통 E-Wallet에서 EDN을
            Deposit을 하였을 경우 해당 Transaction에 대한 hash값을
            보낸다.

            사용자는 IAM Token으로 구분하는데, 이값은 HTTP Header에
            들어있다.

            .. container:: table-wrap

               ========================
               message TransactionHash{
               
               string txhash=1;
               
               }​
               ========================

            -  설명

            .. container:: table-wrap

               ========== ======= =============================================================================================================================
               Field 이름 Type    설명
               ========== ======= =============================================================================================================================
               ​txhash    string​ E-Wallet에서 Ethereum network으로 transfer를 하면 해당 transaction에 대한 hash가 나오는데 해당 값을 가지고 추후 Transaction이
                                 
                                  성공했는지 실패했는지 확인이 가능하다.​
               ========== ======= =============================================================================================================================

            7. WithdrawRequest

            E-Wallet에서 CS에게 TEDN을 차감하고 EDN을 늘려달라는 요청을
            할 때 쓰인다.

            사용자는 IAM Token으로 구분하는데, 이값은 HTTP Header에
            들어있다.

            .. container:: table-wrap

               ========================
               message WithdrawRequest{
               
               string eth_address=1;
               
               string tedn_amount=2;
               
               }
               ========================

            -  설명

            .. container:: table-wrap

               ============ ======= ===============================================================================
               Field 이름   Type    설명
               ============ ======= ===============================================================================
               ​eth_address string​ ethereum address​, EDN을 받을 주소를 의미한다.
               tedn_amount  string  차감할 TEDN값이며, 이값은 1:1로 EDN으로 변환되어 지정한 eth_address로 전달된다.
               ============ ======= ===============================================================================

            8. HDAddress

            E-Wallet이 CoinServer로 CoinServer의 HDAddress를 요청하였을
            경우 리턴되는 메세지이다.

            .. container:: table-wrap

               ====================
               message HDAddress{
               
               string hd_address=1;
               
               }​
               ====================

            -  설명

            .. container:: table-wrap

               =========== ======= ==================================================================
               Field 이름  Type    설명
               =========== ======= ==================================================================
               ​hd_address string​ Coin Server의 HD Address로, E-Wallet이 EDN을 줄 떄 받는 주소이다.​
               =========== ======= ==================================================================

            9. TransactionListRequest

            E-Wallet이 TransactionServer로 현재 사용자의 Transaction을
            요청할 때 쓰이는 메세지이다.

            .. container:: table-wrap

               ================================
               ​message TransactionListRequest{
               
               int32 page = 1;
               
               int32 countperpage = 2;
               
               }
               ================================

            -  설명

            .. container:: table-wrap

               ============ ====== ========================================================================================
               Field 이름   Type   설명
               ============ ====== ========================================================================================
               ​page        int​32 transaction리스트를 E-Wallet이나 기타 서버에서 요청할 때 어느 page를 리턴할지 요청한다.​
               countperpage int32  1page당 표시될 row count이다.
               ============ ====== ========================================================================================

            10. TransactionReceipt 

            TransactionReceipt는 실제 Transaction외에도 생성 날짜등의
            부가 정보를 가지고 있으며, supernode의 block commit에 의해
            생성된 db값을 읽어 들인다.

            .. container:: table-wrap

               ===========================
               message TransactionReceipt{
               
               string to_addr = 1;
               
               string from_addr  = 2;
               
               string amount=3;
               
               Timestamp regdate=4;
               
               }​
               ===========================

            -  설명

            .. container:: table-wrap

               ========== ========= ================================
               Field 이름 Type      설명
               ========== ========= ================================
               ​to_addr   string​   TEDN을 받은 주소​
               from_addr  string    TEDN을 전달한 주소
               amount     string    decimal 18인 전달된 TEDN 값이다.
               regdate    Timestamp TEDN이 전달된 시간이다.
               ========== ========= ================================

            11. TransactionListResponse

            TransactionListRequest로 요청된 현재 사용자의 Transaction에
            대한 응답이다. 내부 TransctionBody의 Type은
            언제나 RANSACTION_TRANSFER로 정의된다.

            .. container:: table-wrap

               ===========================================
               ​message TransactionListResponse{
               
               int32 totalcount=1;
               
               int32 currentpage=2;
               
               repeated TransactionReceipt transactions=3;
               
               }
               ===========================================

            -  설명

            .. container:: table-wrap

               ============ ================== ==============================================================
               Field 이름   Type               설명
               ============ ================== ==============================================================
               ​totalcount  int32​             전체 tranction count​
               currentpage  int32              transaction request에서 요청된 page이며, 현재 요청된 page이다.
               transactions TransactionReceipt TransactionReceipt의 array이며, transaction내용을 가지고 있다.
               ============ ================== ==============================================================

            12. TransactionStream

            DApp서버 등에서 Binary Transaction을 TS로 보낼때 쓴다. 

            해당 데이타는 내부적으로는 Signing까지 완료된 데이타 이므로
            TS에서는 proxy처럼 해당 데이타를 SuperNode에 포워딩한다.

            .. container:: table-wrap

               ===========================
               ​message TransactionStream{
               
               int32 version=1;
               
               string data=2;
               
               Transaction tx=3;
               
               }
               ===========================

            -  설명

            .. container:: table-wrap

               +------------+-----+---------------------------------------------------+
               | Field 이름 | Typ | 설명                                              |
               |            | e   |                                                   |
               +============+=====+===================================================+
               | ​version   | int | data ​version으로, 버전에 따라 data의 형태가      |
               |            | 32​ | 달라질 수 있다. 현재 값은 1이다.                  |
               +------------+-----+---------------------------------------------------+
               | data       | str | version 1인 경우 json string이다.                 |
               |            | ing |                                                   |
               +------------+-----+---------------------------------------------------+

            13. BalanceRollbackRequest

            Coinserver에서 ethereum transaction을 수행 후 ethereum tx가
            실패하여 관련 tx를 rollback하고 싶을 때 사용한다. 해당
            정보는 BalanceListResponse에서 BS에서 보내는 정보이다.

            .. container:: table-wrap

               =================================
               ​message BalanceRollbackRequest {
               
               string txkey = 1; 
               
               }
               =================================

            -  설명

            .. container:: table-wrap

               +------------+-----+---------------------------------------------------+
               | Field 이름 | Typ | 설명                                              |
               |            | e   |                                                   |
               +============+=====+===================================================+
               | ​txkey     | str | BS가 보낸 txkey이다. 이는, txhash가 아니다.       |
               |            | ing |                                                   |
               +------------+-----+---------------------------------------------------+

   .. container::
      :name: footer

      .. container:: section footer-body

         Document generated by Confluence on Mar 27, 2019 18:14

         .. container::
            :name: footer-logo

            `Atlassian <http://www.atlassian.com/>`__

.. |image0| image:: https://edenchain.atlassian.net/secure/viewavatar?size=xsmall&avatarId=10316&avatarType=issuetype
   :class: icon

