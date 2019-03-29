=======================================
Wallet Sequence Diagram
=======================================

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
            #. `Wallet <Wallet_124780582.html>`__

         .. rubric:: Wallet Sequence Diagram
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by James Ahn, last modified by Jay Lee on Mar 29,
            2019

         .. container:: wiki-content group
            :name: main-content

            ASequence diagram is a useful tool to study howasystem works
            fromtheprocess' viewpoint. Itshows theexact processesand
            order of the processeswhen a function is being triggered.

            AWallet's sequence diagram is composed ofeightsequences such
            as importing, creating wallet and so on.

            -  Importing EDN wallet
            -  Creating EDN wallet
            -  TEDN Deposit
            -  TEDN Withdrawal
            -  Pin Code Creation
            -  Adding EDN by KeyberSwap
            -  Adding EDN by IDEX
            -  EDN Transfer

            Each sequence diagram for the corresponding functionshow
            theinteraction among wallet componentswithin thesequence. So
            it is a good starting point to develop an understanding of
            how Eden's wallet works.

            .. rubric:: Importing EDN Wallet
               :name: WalletSequenceDiagram-ImportingEDNWallet

            The sequence diagram shows the process required to import or
            restore an EDN wallet.

            |image2019-3-25_12-58-18.png|

            .. rubric:: Creating EDN Wallet
               :name: WalletSequenceDiagram-CreatingEDNWallet

            Thediagramexplains the steps foranEDN wallet creation. It is
            based on BIP 39, the process starts with seed phrase.

            |image2019-3-25_12-59-2.png|

            .. rubric:: TEDN Deposit
               :name: WalletSequenceDiagram-TEDNDeposit

            This process is relatively complex because it deals with
            EDN/TEDN exchange behind the scene. TEDN deposit is only
            allowed by EDN to TEDN exchange. 

            |image2019-3-25_15-42-17.png|

            .. rubric:: TEDN Withdrawal
               :name: WalletSequenceDiagram-TEDNWithdrawal

            TEDN withdrawal takes place only inasuper node,ahyper node
            is supposed to have anchoring data. 

            |image2019-3-25_15-43-24.png|

            .. rubric:: Pin Code Creation
               :name: WalletSequenceDiagram-PinCodeCreation

            For better usability, Eden's wallet encouragesusersto use
            pincodesinstead ofpasswords.

            |image2019-3-25_15-44-10.png|

            .. rubric:: Adding EDN through KyberSwap
               :name: WalletSequenceDiagram-AddingEDNthroughKyberSwap

            Userscan convert ETH to EDN through kyberswap. 

            |image2019-3-25_15-51-2.png|

            .. rubric:: Adding EDN by IDEX
               :name: WalletSequenceDiagram-AddingEDNbyIDEX

            ETH to EDN is also possible through IDEX.

            |image2019-3-25_15-52-4.png|

            .. rubric:: EDN Transfer
               :name: WalletSequenceDiagram-EDNTransfer

            At this moment, EDN isanERC20 token so EDNtransfers needto
            access Ethereum. The sequencediagramexplains this process.

            |image2019-3-25_15-52-37.png|

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

            | 

         .. container:: pageSection group

            .. container:: pageSectionHeader

               .. rubric:: Attachments:
                  :name: attachments
                  :class: pageSectionTitle

            .. container:: greybox

              .. |image2019-3-25_12-58-18.png| image:: images/122848028/122880821.png

              .. |image2019-3-25_12-59-2.png| image:: images/122848028/122815353.png

              .. |image2019-3-25_13-2-17.png| image:: images/122848028/122848038.png

              .. |image2019-3-25_15-42-17.png| image:: images/122848028/122815366.png

              .. |image2019-3-25_15-43-24.png| image:: images/122848028/123371526.png

              .. |image2019-3-25_15-44-10.png| image:: images/122848028/122946595.png

              .. |image2019-3-25_15-51-2.png| image:: images/122848028/122815374.png

              .. |image2019-3-25_15-52-4.png| image:: images/122848028/122946600.png

              .. |image2019-3-25_15-52-37.png| image:: images/122848028/122815379.png


   .. container::
      :name: footer

      .. container:: section footer-body

         Document generated by Confluence on Mar 29, 2019 14:48

         .. container::
            :name: footer-logo

            `Atlassian <http://www.atlassian.com/>`__

.. |image0| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image1| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image2| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image3| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image4| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image5| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image6| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image7| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image8| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px



