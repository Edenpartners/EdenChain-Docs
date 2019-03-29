========================================================================
Message Encryption/Decryption for Internal communication
========================================================================

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

         .. rubric:: Message Encryption/Decryption for
            Internal communication
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by James Ahn, last modified by Jay Lee on Mar 29,
            2019

         .. container:: wiki-content group
            :name: main-content

            | 

            To serve business applications, EdenChain has many internal
            components such as an identity server, a transaction server,
            transaction processors and so on.

            The nature of the existing architecture demands close
            internal communication among the components. Since EdenChain
            is a permissioned blockchain platform, the internal network
            is secured due to its architecture and network topology -
            the possibility of accessing the internal network is
            relatively low. We cannot, however, be sure about advanced
            hacking possibilities. This situation underscores the
            importance of having a safe internal communication
            mechanism.

            The internal communication mechanism must have 1) safe
            communication, and 2) low computation power. Yes, there is a
            trade-off: if we want strong safety, we need more
            computation power. 

            We must, therefore, choose a solution that balances this
            trade-off between safety and computation power. A few
            experiments led us to the conclusion that the
            "Diffie-Hellman key exchange" seems to be a sound choice for
            our objective. The Diffie-Hellman key exchange is used in
            many internet services according to WIKI. It is safe while
            using low computation power and, furthermore, it is
            considered to be a very solid approach among leading tech
            companies.

            .. rubric:: DHKE(Diffie-Hellman key exchange)
               :name: MessageEncryption/DecryptionforInternalcommunication-DHKE(Diffie-Hellmankeyexchange)

            The basic concept of DHKE is that a shared secret will be
            used for encryption and decryption, so only members with the
            shared secret can communicate with one another. The beauty
            of DHKE is that it does not require prior information except
            for the shared secret. DHKE is easy and clear to understand,
            making implementation simple. 

            The following image explain how DHKE works intuitively. 

            | 

            Image result for Diffie-Hellman key exchange

            |https://upload.wikimedia.org/wikipedia/commons/thumb/4/46/Diffie-Hellman_Key_Exchange.svg/250px-Diffie-Hellman_Key_Exchange.svg.png|

            Source - Wiki

            | 

            To understand the actual process of DHKE, consider the
            following. 

            #. `Alice and
                  Bob <https://en.wikipedia.org/wiki/Alice_and_Bob>`__\  publicly
                  agree to use a modulus \ *p*\  = 23 and
                  base \ *g*\  = 5 (which is a primitive root modulo
                  23).

            #. Alice chooses a secret integer \ **a**\  = 4, then sends
                  Bob \ *A*\  = \ *g\ a*\  mod \ *p*

                  -  *A*\  = 5\ :sup:`4`\  mod 23 = 4

            #. Bob chooses a secret integer \ **b**\  = 3, then sends
                  Alice \ *B*\  = \ *g\ b*\  mod \ *p*

                  -  *B*\  = 5\ :sup:`3`\  mod 23 = 10

            #. Alice computes \ **s**\  = \ *B\ a*\  mod \ *p*

                  -  **s**\  = 10\ :sup:`4`\  mod 23 = 18

            #. Bob computes \ **s**\  = \ *A\ b*\  mod \ *p*

                  -  **s**\  = 4\ :sup:`3`\  mod 23 = 18

            #. Alice and Bob now share a secret (the number 18).

            #. Source - WIKI

            | 

            .. rubric:: DHKE Module in EdenChain
               :name: MessageEncryption/DecryptionforInternalcommunication-DHKEModuleinEdenChain

            We created a DHKE module and are using it for internal
            communication in the EdenChain platform. 

            Most of the internal communication in the EdenChain platform
            is based on the DHKE module. 

            Below is sample code showing how EdenChain uses the DHKE
            module for message encryption/decryption

                alice = EAuthKey()

                bob = EAuthKey()

                alice.get_salt()

                bob.get_salt()  

                dh_alice = alice.calc_dh_value()

                dh_bob = bob.calc_dh_value()

               alice.calc_shared_secret(dh_bob)

               bob.calc_shared_secret(dh_alice)

               a_encrypted = alice.to_ascii(alice.salt)    

               a_enc = alice.encode("hello")

               a_dec = bob.decode(a_enc)

            Although DHKE is used as the basis for secure internal
            communication in EdenChain, certain sensitive messages such
            as identity information and coin-related transactions invoke
            additional methods for protection and security.

            | 

            | 

   .. container::
      :name: footer

      .. container:: section footer-body

         Document generated by Confluence on Mar 29, 2019 14:48

         .. container::
            :name: footer-logo

            `Atlassian <http://www.atlassian.com/>`__



