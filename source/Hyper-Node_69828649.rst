==========================
Hyper Node
==========================

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
            #. `Architecture <Architecture_78413825.html>`__

         .. rubric:: Hyper Node
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by Jacki Heo, last modified by Jay Lee on Mar 29,
            2019

         .. container:: wiki-content group
            :name: main-content

            Hypernode means EdenChain's blockchain. In EdenChain 's
            architecture, transaction handling is mainly done by the
            supernode, and the hypernode's major role is anchoring data
            and processing its native coin.

            EdenChain does not put all the data produced by a supernode
            into its blockchain for scalability reasons. In this picture
            there is a possibility a supernode might manipulate data, so
            to prevent such kinds of cheating, hypernodes allow for
            anchoring. Whenever supernodes generate new transaction
            data, anchoring will be done automatically by the anchoring
            server in conjunction with the transaction server. 

            .. rubric:: Anchoring
               :name: HyperNode-Anchoring

            TheCoin server and Transaction server are supposed to do
            anchoring whenever it processes transactions. 

            Thehypernodestoresananchoring value toadedicated internal
            namespace. It is only accessible byaspecial transaction
            processor. 

            The transaction processoronlyhas write capability and it
            imprints a timestampintotheglobal state.

            .. rubric:: Data to be anchored
               :name: HyperNode-Datatobeanchored

            .. rubric:: By Coin server
               :name: HyperNode-ByCoinserver

            When the Coin Server deposits or Withdraws, the
            corresponding audit data is stored in the repository. Here,
            it allows processing the user's request at a later time.
            However, the valuecannotbe foundwhenit has been arbitrarily
            changed or for other reasons.

            When this happens, it is impossible to determine whether the
            value is reliable or not.

            At this time, if the corresponding value is put on a Hyper
            Node by cutting it into a specific unit and anchoring, it is
            possible to identify a problem in a specific part when a
            problem occurs later.

            It is cut bytenunits,hashed and becomes one Tx, and one
            batch is uploaded to the server by 10 transactions.
            Therefore, anchoring is one transaction for 1-10 and 10
            transactions for batch. Therefore, a total of 100
            transactions are uploaded as one. (The unit to be cut is
            Record ID.)

               10 records → 1 TX  =>  10TX → 1 Batch

            If you do not know the last anchor data due toaserver
            restart, check if the NameSpace exists with the last digit.
            Update the case if it does not exist.

            The hash of the data is made by attaching all the values
            ​​and then hashing the value to sha512. ​This includes the
            numbers as a string. 

            .. rubric:: By Transaction Server
               :name: HyperNode-ByTransactionServer

            The Transaction Server handles storing and receiving the TX
            and inserts the corresponding value into the Super Node and
            then receiving and storing the corresponding event when the
            Super Node succeeds.

            There could be two data results: the data before processing
            and the data after processing due to the time lag in
            processing the transaction.

            TX and the layout configurationarethe same as Coin
            Serverinthat 10 records are composed of 1 tx and 10 tx is
            composed of 1 batch.

            As described above, the hash value of the data is the
            hashing value to sha512 after attaching all the values ​​of
            the record after joining.

            .. rubric:: Incremental Anchor
               :name: HyperNode-IncrementalAnchor

            In order to insert the calculated Sha512 into the Anchor TP,
            the Anchor Server checks the ID that has been incrementedat
            regular intervalsand continuously hashes after every push.
            This is because the ID value continues to increase.

            Atthattime, it should be able to skip the existing pushes
            and add only new ones.Noteit savesthelast-idupuntil the
            point where itwasinserted. 

            | 

            | 

   .. container::
      :name: footer

      .. container:: section footer-body

         Document generated by Confluence on Mar 29, 2019 14:48

         .. container::
            :name: footer-logo

            `Atlassian <http://www.atlassian.com/>`__



