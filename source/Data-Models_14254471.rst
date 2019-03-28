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

            Created by Jacki Heo, last modified on Mar 28, 2019

         .. container:: wiki-content group
            :name: main-content

            .. rubric:: Overview
               :name: DataModels-Overview

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

            .. rubric:: Messages
               :name: DataModels-Messages

            syntax uses proto3.

            The package is defined as edenchain.

            | 

            1. TransactionBody

            This is the message used when all transactions of TEDN are
            processed.

            | 

            ::

               message TransactionBody  {
                  enum TXMessageType{
                   TRANSACTION_DEPOSIT = 0;
                   TRANSACTION_WITHDRAW = 1;
                   TRANSACTION_TRANSFER = 2;
                  }

                  TXMessageType tx_type = 1;
                  string to_addr = 2;
                  string from_addr  = 3;
                  string amount=4;
                  string dapp_name = 5;
                  string transfer_type = 6;

               }​

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

            ::

               message Transaction {
                  string version = 1;
                  TransactionBody transaction = 2;
                  string from_sign_hashed = 10;
               }​

            | 

            | 

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

            ::

               message BalanceRequest {
                  string addr = 1;
               }​

            | 

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

            | 

            ::

               message BalanceResponse {
                  string addr = 1;
                  string amount = 2;
               }​

            | 

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

            ::

               message BalanceResponseList{
                   repeated BalanceResponse response=1;
                   string txkey=2;
               }

            | 

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

            ::

               message TransactionHash{
                  string txhash=1;
               }​

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

            | 

            ::

               message WithdrawRequest{
                  string eth_address=1;
                  string tedn_amount=2;
               }

            | 

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

            | 

            ::

               message HDAddress{
                  string hd_address=1;
               }

            | 

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

            | 

            ::

               ​message TransactionListRequest{
                  int32 page = 1;
                  int32 countperpage = 2;
               }

            | 

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

            | 

            ::

               message TransactionReceipt{
                  string to_addr = 1;
                  string from_addr  = 2;
                  string amount=3;
                  Timestamp regdate=4;
               }​

            | 

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

            ::

               message TransactionListResponse{
                  int32 totalcount=1;
                  int32 currentpage=2;
                  repeated TransactionReceipt transactions=3;
               }

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

            ::

               message TransactionStream{
                  int32 version=1;
                  string data=2;
                  Transaction tx=3;
               }

            | 

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

            ::

               message BalanceRollbackRequest {
                  string txkey = 1; 
               }

            | 

            -  Explanation

            .. container:: table-wrap

               +------------+-----+---------------------------------------------------+
               | Field Name | Typ | Explanation                                       |
               |            | e   |                                                   |
               +============+=====+===================================================+
               | ​txkey     | str | The txkey sent by the BS. This is not txhash.     |
               |            | ing |                                                   |
               +------------+-----+---------------------------------------------------+

   .. container::
      :name: footer

      .. container:: section footer-body

         Document generated by Confluence on Mar 28, 2019 12:30

         .. container::
            :name: footer-logo

            `Atlassian <http://www.atlassian.com/>`__

