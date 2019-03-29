=====================================
Transaction Processor
=====================================

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
            #. `EdenChain
               Introduction <EdenChain-Introduction_120161393.html>`__

         .. rubric:: Transaction Processor
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by James Ahn, last modified by Jay Lee on Mar 29,
            2019

         .. container:: wiki-content group
            :name: main-content

            .. rubric:: Overview 
               :name: TransactionProcessor-Overview

            EdenChain can develop block-chain applications using APIs.
            It is an effective development method for blockchain
            application developers as it is relatively easy and quick to
            develop, and the available manpower can be secured easily. 

            From a developer perspective, developing an application
            using the EdenChain API can be described as developing a
            custom TP (Transaction Processor). TP stands for a software
            module that literally has business logic to process a
            specific task, such as checking the balance of an account or
            implementing functions associated with blockchain, such as
            remittance.

            It is a principle that TP implements one function.
            Therefore, if many blockchain functions are required, it is
            necessary to develop TPs corresponding to the number of
            functions. EdenChain is, therefore, a blockchain application
            that is a collection of multiple TPs. Therefore, developing
            an application based on EdenChain can be explained by the
            development of a TP using the EdenChain API, and the
            consolidation of the TP is a software module implemented
            using the EdenChain API.

            |image2018-10-31_11-42-54.png|

            .. rubric:: TP Handling
               :name: TransactionProcessor-TPHandling

            Except for the fact that the EdenChain API is applied, TPs
            are no different from common software modules and no special
            explanation is needed. However, understanding how EdenChain
            processes TPs is required to create optimized TPs.

            On the EdenChain platform, once the TP is executed, it
            blocks the related tasks until the TP is completed and
            secures data consistency. Blocking is especially needed to
            handle write-related tasks rather than read-related tasks.
            Because blocking is maintained until the start and end of a
            TP, TP should be developed to handle the process in as short
            a time as possible.

            |image2018-10-31_11-44-10.png|

            Especially, in the case of a TP that changes state, other
            work should not be performed concurrently. Therefore, the
            code should be optimized so that it can be processed in the
            shortest possible time. TP can be implemented either as a
            sync method or async method or as both methods. However, it
            can not be expected to improve much unless the TP is
            developed in accordance with EdenChain s processing method.
            This means that if methods are grouped together the
            Performance Bottleneck eventually affects EdenChain.
            Therefore, we can expect better performance by breaking
            tasks into smaller units and dividing them into multiple TPs
            rather than processing them all at once by putting all the
            functions in a single TP.

            .. rubric:: TP Deployment
               :name: TransactionProcessor-TPDeployment

            The developed TP can be deployed in three major ways as
            needed.

            .. container:: table-wrap

               ======================== ============================================================ ===============
                                        Description                                                  Comments
               ======================== ============================================================ ===============
               Dedicated Namespace Zone Deploy a TP in EdenChain's cloud computing zone.             High speed
                                                                                                    
                                                                                                     More secured
               Legacy Zone              Deploy a TP in your computing zone                           Slow speed
                                                                                                    
                                                                                                     Less secured
               Hybrid                   Deploy in both Dedicated Namespace Zone and the Legacy Zone. Well optimized 
                                                                                                    
                                                                                                     More Complex
               ======================== ============================================================ ===============

            | TP interacts directly with the EdenChain Platform and has
              many interactions inherent to its design. Therefore, the
              network speed between the TP and the EdenChain platform
              affects the overall performance of the application.
            | For faster and safer applications, deploying TPs in the
              EdenChain cloud computing zone would produce the best
              results. Because it is located in the same cloud zone, the
              network latency is low and TP can operate securely in the
              internal computing zone.

            | However, there may be cases where it is not possible to
              deploy TPs in the cloud due to policies or data ownership
              of application developers. In this case, the TP can be
              operated using the existing computing zone. In this case,
              however, it is much slower and more likely to have
              potential security problems.
            | The Hybrid approach can be used in combination with some
              of the advantages of both schemes, i.e. some in the
              Dedicated Namespace Zone and some in the Legacy Zone.

            | 

         .. container:: pageSection group

            .. container:: pageSectionHeader

               .. rubric:: Attachments:
                  :name: attachments
                  :class: pageSectionTitle

            .. container:: greybox

              .. |image2018-10-31_11-42-54.png| image:: images/120783203/120881553.png

              .. |image2018-10-31_11-44-10.png| image:: images/120783203/120881557.png


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



