Core Components
---------------
*  describes core components in Edenchain platform to build e-garden ecosystem as well as supporting Dapps.


* Component List

.. list-table::
   :widths: 5, 16, 60
   :header-rows: 1
   
   * - Name
     - Description
     - | Task
   * - Hyper Node
     - Sawtooth Native EDN Management
     - | Anchoring
       | TEDN Anchoring
       | EDN Transaction
   * - Super Node
     - Transaction Stroage
     - | To record TEDN related transactions
   * - Coin Server
     - Exchange EDN to TEDN
     - | Convert EDN to TEDN
       | Convert TEDN to EDN
     
   * - Balance Server
     - To manage TEDN balance
     - | To handle deposit/withdraw TEDN
     
   * - TX Server
     - To handle blockchain related tasks
     - | TEDN transaction cache
       | To write transactions in super node
       | To read transactions from local DB 
       | To write anchor data into hyper node
       
   * - E-Garden
     - Dapp Store
     - | Dapp management
       | Sign up/sign in/sign out
       | TEDN balance check
       | Dapp discovery
       | Dapp listing management
       
   * - Wallet
     - Mobile App
     - | Exchange Function
       | TEDN deposit
       | TEDN withdraw
       | EDN/TEDN balance
       | To browse TEDN transaction
       | To browse EDN transaction
       | Sign up/Sign in
       
   * - App Server
     - To manage Dapp
     - | To bet TEDN
       | To do payut TEDN
       | Business logics
       | TEDN balance checks
       | Sign in/Sign out
       
   * - IAM
     - To store identity information
     - | Sign up
       | Sign in / Sign out
       | To send identity information 
       |   to requester
       | To sign TEDN transaction
       | To create TEDN wallet address
       | To manage token
