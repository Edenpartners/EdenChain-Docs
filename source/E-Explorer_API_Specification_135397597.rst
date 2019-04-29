============================================
E-Explorer API Specification
============================================

.. container::
   :name: page

   .. container:: aui-page-panel
      :name: main

      .. container::
         :name: main-header

         .. container::
            :name: breadcrumb-section

            #. `Eden Platform <index.html>`__

         .. rubric:: E-Explorer API Specification
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by Jacki Heo, last modified by Jay Lee on Apr 29,
            2019

         .. container:: wiki-content group
            :name: main-content

            .. rubric:: Overview
               :name: E-ExplorerAPISpecification-Overview

            E-Explorer is Edenchain Explorer. For its candidate release,
            the function is limited to handling blocks and transactions,
            excluding assets and smart contracts.

            For its 1\ :sup:`st` release, E-Explorer will reflect data
            received by AJAX collected only when a person clicks. Later,
            it will be able to stream real-time data via WebSocket.

            The document is to define API for the aforementioned
            function.

            .. rubric:: API List
               :name: E-ExplorerAPISpecification-APIList

            From each server, data are transmitted is from a browser
            using Json RPC 2.0

            | The default API URL is as follows.
            | https://<\ **server** dns>/api

            The default interface is as follows.

            The result value of JSON-RPC consists of JSON with a unified
            response form.

            -  API Response

               -  Err code

                  -  Informs the result of the execution with the error
                     code. 

               -  Err Message

                  -  Use it to deliver the message, if needed.

               -  Data

                  -  If there is data that should be provided as a
                     result of processing, insert data here.

            -  Example

               -  {'err_code':0 , 'msg': 'ok', 'data':{} }

               -  {'err_code':0 , 'msg': 'ok', 'data': { 'data1':1,
                  'data2':2 } }
               -  {'err_code':-1 , 'msg': 'no matched data', 'data': {}
                  }

            -  Error code

            If the error code is '0', it indicates success, and if it is
            negative, it is defined as Server Error and if it is
            positive, it is defined as Client Error.

            The predefined error codes are as follows.

            .. container:: table-wrap

               .. container:: table-wrap

                  .. container:: table-wrap

                     ============== ========================
                     **Error Code** **Description**
                     0              Success
                     -1             Server Internal Error
                     1              Client Parameter Missing
                     ============== ========================

                  .. rubric:: 1. Get EdenChain Block Height
                     :name: E-ExplorerAPISpecification-1.GetEdenChainBlockHeight
                     :class: auto-cursor-target

            Return block height committed now.

            ::

               POST /api
                   Request
                       JSON-RPC method : "block.getheight"
                   Result
                       Status Code: 
                           200 - OK
                   Return: in result
                       Return to defined API Response value
                       Success: err_code=0, data={'height': current_block_height_of_EdenChain}

            | 

            .. rubric:: 2. Get EdenChain Total Transaction count
               :name: E-ExplorerAPISpecification-2.GetEdenChainTotalTransactioncount

            Return the number of transaction committed now.

            ::

               POST /api
                   Request
                       JSON-RPC method : "transaction.totalcount"
                       JSON-RPC Parameter:
                   Result
                       Status Code:
                           200 - OK
                   Return: in result
                       Return to defined API Response value
                       Success: err_code=0, data={'total_count': total_count_of_committed_transactions}

            | 

            .. rubric:: 3.  Get EdenChain Latest TPS
               :name: E-ExplorerAPISpecification-3.GetEdenChainLatestTPS

            Return the most recent transaction TPS. This must be the
            most current information. If there has been a transaction in
            the last one minute, then show TPS of the transaction.

            ::

               POST /api
                   Request
                       JSON-RPC method : "transaction.tps"
                       JSON-RPC Parameter:
                   Result
                       Status Code:
                           200  - OK
                   Return: in result
                       Return to defined API Response value
                       Success: err_code=0, data={'tps': latest_tps_of_committed_transactions}

            | 

            .. rubric:: 4.  Get EdenChain Latest Block Commit Time
               :name: E-ExplorerAPISpecification-4.GetEdenChainLatestBlockCommitTime

            Return block time committed most recently.

            ::

               POST /api
                   Request
                       JSON-RPC method : "block.latest_commit_time"
                       JSON-RPC Parameter:
                   Result
                       Status Code:
                           200  - OK
                   Return: in result
                       Return to defined API Response value
                       Success: err_code=0, data={'time': block_commit_time_of_int_time_of_epoch}

            | 

            .. rubric:: 5. Get EdenChain Highest TPS
               :name: E-ExplorerAPISpecification-5.GetEdenChainHighestTPS

            Return the Transaction Highest TPS of the given day. The
            marker time can be modified later.

            ::

               POST /api
                   Request
                       JSON-RPC method : "transaction.highest_tps"
                       JSON-RPC Parameter:
                   Result
                       Status Code:
                           200  - OK
                   Return: in result
                       Return to defined API Response value
                       Success: err_code=0, data={'tps': highest_tps_of_committed_transactions}

            | 

            .. container:: table-wrap

               .. rubric:: 6.  Get EdenChain Transaction Time Series
                  Groups
                  :name: E-ExplorerAPISpecification-6.GetEdenChainTransactionTimeSeriesGroups
                  :class: table-wrap

            Get transaction group per condition. Return this per time
            series. For now, we only offer groups by dates and times. If
            it is by the hour, it offers groups of last 1 day. If it is
            by date, it covers the last seven dates.

            ::

               POST /api
                   Request
                       JSON-RPC method : "transaction.count_by_time_series"
                       JSON-RPC Parameter:
                           time_series_type (String) : "hour", "day"
                   Result
                       Status Code:
                           200  - OK
                   Return: in result
                       Return to defined API Response value
                       Success: err_code=0, data={ 'hour_series' : [{'<hour>': transaction_count},.... ] } or
                                            data={ 'day_series'  : [{'<day>' : transaction_count}k,....] }

            | 

            .. container:: table-wrap

               .. rubric:: 7.  Get EdenChain Transaction Block Series
                  Groups
                  :name: E-ExplorerAPISpecification-7.GetEdenChainTransactionBlockSeriesGroups
                  :class: table-wrap

            Get block group per condition. Return this per time series.
            For now, we only offer groups by dates and times. If it is
            by the hour, it offers groups of last 1 day. If it is by
            date, it covers the last seven dates.

            ::

               POST /api
                   Request
                       JSON-RPC method : "block.count_by_time_series"
                       JSON-RPC Parameter:
                           time_series_type (String) : "hour", "day"
                   Result
                       Status Code:
                           200  - OK
                   Return: in result
                       Return to defined API Response value
                       Success: err_code=0, data={ 'hour_series' : [{'<hour>': block_count},.... ] } or
                                            data={ 'day_series'  : [{'<day>' : block_count},....] }

            | 

            .. container:: table-wrap

               .. rubric:: 8.  Get Detail Block Information
                  :name: E-ExplorerAPISpecification-8.GetDetailBlockInformation
                  :class: table-wrap

            When a request is received regarding a specific block
            height, return the information pertinent to the block.

            ::

               POST /api
                   Request
                       JSON-RPC method : "block.information"
                       JSON-RPC Parameter:
                           height (String) : "Number"
                   Result
                       Status Code:
                           200  - OK
                       Return: in result
                           Return to defined API Response value
                           Success: err_code=0, data=<refer block model>

            | 

            .. container:: table-wrap

               .. rubric:: 9.  Get Detail Transaction Information
                  :name: E-ExplorerAPISpecification-9.GetDetailTransactionInformation
                  :class: table-wrap

            When requested of a specific transaction ID, return the
            information pertinent to the block.

            ::

               POST /api
                   Request
                       JSON-RPC method : "transaction.information"
                       JSON-RPC Parameter:
                           tx_id (String) : "TX ID"
                   Result
                       Status Code:
                           200  - OK
                   Return: in result
                       Return to defined API Response value
                       Success: err_code=0, data=<refer transaction model>

            | 

            .. container:: table-wrap

               .. rubric:: 10.  Search by Keyword
                  :name: E-ExplorerAPISpecification-10.SearchbyKeyword
                  :class: table-wrap

            When a search is made with keywords, then refers to TX ID,
            Block Height, and User Address. This process only supports
            the complete match. Partial Match is currently not
            supported.

            ::

               POST /api
                   Request
                       JSON-RPC method : "search.keyword"
                       JSON-RPC Parameter:
                           keyword (String) : "an arbitrate value"
                           page    (int) : "number" - Only used when bringing only page after returning user address.
                           count_per_page (int) : "number" - Only used when bringing only page after returning user address.
                   Result
                       Status Code:
                           200  - OK
                   Return: in result
                       Return to defined API Response value
                           Success: err_code=0, data={ type: "block" , data: <refer block model >} or
                                                data={ type: "transaction" , data: <refer transaction model>}
                                                data={ type: "user_address", data: {total_count: number, page: number, count_per_page: number,  [<refer transaction model> .....] }}

            | 

            .. container:: table-wrap

               .. rubric:: 11.  Block List
                  :name: E-ExplorerAPISpecification-11.BlockList
                  :class: table-wrap

            Return Block List. Return it in reverse chronological order.
            The dashboard is set to show page 1-20 only to bring a
            portion of the information.

            ::

               POST /api
                   Request
                       JSON-RPC method : "block.list"
                       JSON-RPC Parameter:
                           page    (int) : "number" - Only used when bringing only page after returning user address.
                           count_per_page (int) : "number" - Only used when bringing only page after returning user address.
                   Result
                       Status Code:
                           200  - OK
                   Return: in result
                       Return to defined API Response value
                       Success: err_code=0, data={ total_count: number, page: number, count_per_page: number,  'blocks': [<refer block model> .....] }

            | 

            .. container:: table-wrap

               .. rubric:: 12.  Transaction List
                  :name: E-ExplorerAPISpecification-12.TransactionList
                  :class: table-wrap

            Return the transaction list. Return it in reverse
            chronological order. The dashboard is set to show page 1-20
            only to bring a portion of the information.

            ::

               POST /api
                   Request
                       JSON-RPC method : "transaction.list"
                       JSON-RPC Parameter:
                           page    (int) : "number" - Only used when bringing only page after returning user address.                                 
                                           Only used when bringing the next page after return.
                           count_per_page (int) : "number" - Only used when bringing only page after returning user address.
                   Result
                       Status Code:
                           200  - OK
                   Return: in result
                           Return to defined API Response value
                           Success: err_code=0, data={ total_count: number, page: number, count_per_page: number, 'transactions': [<refer transaction model> .....] }

            | 

            .. rubric:: Data Model
               :name: E-ExplorerAPISpecification-DataModel

            .. rubric:: 1.  Transaction
               :name: E-ExplorerAPISpecification-1.Transaction

            Define information pertinent to each transaction

            ::

               id                (string): transaction tx id.
               timestamp         (int) : time elapsed from epoch in second. (decimal point includes 1/10 second)
               associated_asset  (string) : return asset info id only (plan to receive asset information from asset info ID in future)
               operation         (string) : CREATE or TRANSFER
               before_owner      (array of string) : the owner before the completion of the transaction (from address)
               request_transaction_amount [array of {"address":receiver address, "amount":amount}] :list of the token receivers and the respective amount.

            | 

            .. container:: table-wrap

               .. rubric:: 2.  Block
                  :name: E-ExplorerAPISpecification-2.Block
                  :class: table-wrap

            Define information pertinent to each datum

            ::

               height        (int) : block height.
               timestamp (int):  time elapsed from epoch in second. (The decimal point includes 1/10 secon)
               hash           (string) : previous block + hash of the current block.
               transactions (array of string) : tx_id list

   .. container::
      :name: footer

      .. container:: section footer-body

         Document generated by Confluence on Apr 29, 2019 16:56

         .. container::
            :name: footer-logo

            `Atlassian <http://www.atlassian.com/>`__




