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

            Created by James Ahn, last modified on Mar 25, 2019

         .. container:: wiki-content group
            :name: main-content

            Sequence diagram is a useful tool to study how system works
            from process' viewpoint. It tells exact process and order of
            the process when a function is being triggered.

            Wallet's sequence diagram is composed of 8 sequences such as
            importing, creating wallet and so on.

            -  Importing EDN wallet
            -  Creating EDN wallet
            -  TEDN Deposit
            -  TEDN Withdrawal
            -  Pin Code Creation
            -  Adding EDN by KeyberSwap
            -  Adding EDN by IDEX
            -  EDN Transfer

            Each sequence diagram for the corresponding function are
            showing interaction among wallet components with sequence.
            So it is a good starting point to take a look to understand
            how Eden's wallet works.

            .. rubric:: Importing EDN Wallet
               :name: WalletSequenceDiagram-ImportingEDNWallet

            The sequence diagram shows the process required to import or
            restore EDN wallet.

            .. rubric:: Creating EDN Wallet
               :name: WalletSequenceDiagram-CreatingEDNWallet

            The digram explains the steps for EDN wallet creation. It is
            based on BIP 39, the process starts with seed phrase.

            .. rubric:: TEDN Deposit
               :name: WalletSequenceDiagram-TEDNDeposit

            This process is relatively complex because it deals with
            EDN/TEDN exchange behind the scene. TEDN deposit is only
            allowed by EDN to TEDN exchange. 

            .. rubric:: TEDN Withdrawal
               :name: WalletSequenceDiagram-TEDNWithdrawal

            TEDN withdrawal takes place only in super node, hyper node
            is supposed to have anchoring data. 

            .. rubric:: Pin Code Creation
               :name: WalletSequenceDiagram-PinCodeCreation

            For better usability, Eden's wallet encourages user to use
            pin code instead of password.

            .. rubric:: Adding EDN through KyberSwap
               :name: WalletSequenceDiagram-AddingEDNthroughKyberSwap

            User can convert ETH to EDN through kyberswap. 

            .. rubric:: Adding EDN by IDEX
               :name: WalletSequenceDiagram-AddingEDNbyIDEX

            ETH to EDN is also possible through IDEX.

            .. rubric:: EDN Transfer
               :name: WalletSequenceDiagram-EDNTransfer

            At this moment, EDN is ERC20 token so EDN transfer needs to
            access Ethereum. The sequence digram explains this process.

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

               |image0|
               `image2019-3-25_12-58-18.png <attachments/122848028/122880821.png>`__
               (image/png)
               |image1|
               `image2019-3-25_12-59-2.png <attachments/122848028/122815353.png>`__
               (image/png)
               |image2|
               `image2019-3-25_13-2-17.png <attachments/122848028/122848038.png>`__
               (image/png)
               |image3|
               `image2019-3-25_15-42-17.png <attachments/122848028/122815366.png>`__
               (image/png)
               |image4|
               `image2019-3-25_15-43-24.png <attachments/122848028/123371526.png>`__
               (image/png)
               |image5|
               `image2019-3-25_15-44-10.png <attachments/122848028/122946595.png>`__
               (image/png)
               |image6|
               `image2019-3-25_15-51-2.png <attachments/122848028/122815374.png>`__
               (image/png)
               |image7|
               `image2019-3-25_15-52-4.png <attachments/122848028/122946600.png>`__
               (image/png)
               |image8|
               `image2019-3-25_15-52-37.png <attachments/122848028/122815379.png>`__
               (image/png)

   .. container::
      :name: footer

      .. container:: section footer-body

         Document generated by Confluence on Mar 27, 2019 18:40

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

