===============================================
Urlscheme및 Universal Link 정의
===============================================

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

         .. rubric:: Urlscheme및 Universal Link 정의
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by Jacki Heo, last modified by Hailee Kim on Mar 26,
            2019

         .. container:: wiki-content group
            :name: main-content

            .. rubric:: Jira Link
               :name: Urlscheme및UniversalLink정의-JiraLink

            `|image0|\ EW-342 <https://edenchain.atlassian.net/browse/EW-342>`__\ -
            e-Wallet연동 URLScheme정의 Done

            .. rubric:: Define Urlscheme and Universal Link
               :name: Urlscheme및UniversalLink정의-DefineUrlschemeandUniversalLink

            .. rubric:: Overview
               :name: Urlscheme및UniversalLink정의-Overview

            Define urlscheme to work with the app on the web server. Be
            sure to leave the urlscheme for the installation as it is.
            The withdraw or deposit should be processed as a universal
            link, so that if there is no app, the installation screen is
            displayed through the browser.

            .. rubric:: Description
               :name: Urlscheme및UniversalLink정의-Description

            userid_hash changes the user id to a hex string after sha256
            () and sends only the first 32 digits.

            Here is an example of what python does.

            .. container:: code panel pdl

               .. container:: codeContent panelContent pdl

                  .. code:: syntaxhighlighter-pre

                     import hashlib
                     hashlib.sha256('id@id.com'.encode()).hexdigest()[:32]

            | 

            이때,  Web에서 Login한 ID와 APP의  ID가 혹시 틀릴 수
            있으므로 앱에서는 요청을 받았을 때 id를 비교해서 틀리면 다른
            계정으로 로그인해달라고 알람을 표시하는데 목적이 있다.

            id가 없을 경우에는 체크하지 않고 띄워준다. (예:
            edenwallet://?action=withdraw)

            Here, the logged-in ID on the web and the APP ID may not be
            matched. In this case, the app compares the id and displays
            an alarming message to log in with a different account.

            If id does not exist, it is displayed without checking.
            (Example: edenwallet://?action=withdraw)

            -  Android

               -  Connect to
                  Play:  \ ``market://details?id=``\ io.edenchain.ewallet
               -  Withdraw:
                  https://ewallet.edenchain.io/withdraw/?id=userid_hash
               -  Deposit:
                  https://ewallet.edenchain.io/deposit/?id=userid_hash

            -  iOS

               -  Connect to AppStore:
                  itms-apps://itunes.apple.com/app/id1234567890
                  (appstore에 upload후 app id가 결정되면 업데이트.)
               -  Withdraw:
                  https://ewallet.edenchain.io/withdraw/\ ?id=userid_hash
               -  Deposit:
                  https://ewallet.edenchain.io/deposit/?id=userid_hash

            | 

            In the E-garden, play and app store connections remain the
            same on the initial installation screen.

            Thereafter, the withdraw and deposit changes as defined
            above.

            DApp modifies the url in the call to withdraw and deposit as
            above.

            In case of E-Wallet App, it is modified to process only each
            withdraws or deposits.

            | 

            | 

            --------------

            --------------

            .. rubric:: Overview
               :name: Urlscheme및UniversalLink정의-Overview.1

            Web서버에서 앱에 연동할 수 있도록 urlscheme을 정의한다.
            이때, 설치를 위한 urlscheme은 그대로 두고, withdraw나
            deposit은 universal link로 처리하여 앱이 없을 경우
            브라우저를 통해 설치 화면이 나올 수 있도록 정리한다.

            .. rubric:: Description
               :name: Urlscheme및UniversalLink정의-Description.1

            userid_hash는 사용자 id를 sha256()후 hex string로 바꾼 후 앞
            32자리만 보낸다. 

            다음은 python에서 수행하는 예이다.

            .. container:: code panel pdl

               .. container:: codeContent panelContent pdl

                  .. code:: syntaxhighlighter-pre

                     import hashlib
                     hashlib.sha256('id@id.com'.encode()).hexdigest()[:32]

            | 

            이때,  Web에서 Login한 ID와 APP의  ID가 혹시 틀릴 수
            있으므로  앱에서는 요청을 받았을 때 id를 비교해서 틀리면
            다른 계정으로 로그인해달라고 알람을 표시하는데 목적이 있다.

            id가 없을 경우에는 체크하지 않고 띄워준다. (예:
            edenwallet://?action=withdraw)

            | 

            -  Android

               -  Play 연결
                  :  ``market://details?id=``\ io.edenchain.ewallet
               -  Withdraw 수행 :
                  https://ewallet.edenchain.io/withdraw/?id=userid_hash
               -  Deposit 수행 :
                  https://ewallet.edenchain.io/deposit/?id=userid_hash

            -  iOS

               -  AppStore 연결:
                  itms-apps://itunes.apple.com/app/id1234567890
                  (appstore에 upload후 app id가 결정되면 업데이트.)
               -  Withdraw 수행 :
                  https://ewallet.edenchain.io/withdraw/?id=userid_hash
               -  Deposit 수행 :
                  https://ewallet.edenchain.io/deposit/?id=userid_hash

            | 

            E-garden에서는 초기 설치화면에서는 그대로 play,
            appstore연결은 동일하게 그대로 둔다.

                                       이후, withdraw, deposit은 위에서
            정의한 대로 변경한다.

            DApp은 withdraw, deposit호출시의 url을 위와 같이 수정한다.

            E-Wallet App의 경우는 각 withdraw 수행, deposit수행만
            처리하도록 수정한다.

            | 

            | 

   .. container::
      :name: footer

      .. container:: section footer-body

         Document generated by Confluence on Mar 27, 2019 18:40

         .. container::
            :name: footer-logo

            `Atlassian <http://www.atlassian.com/>`__

.. |image0| image:: https://edenchain.atlassian.net/secure/viewavatar?size=xsmall&avatarId=10318&avatarType=issuetype
   :class: icon

