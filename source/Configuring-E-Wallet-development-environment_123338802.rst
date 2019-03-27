======================================================
Configuring E-Wallet development environment
======================================================

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

         .. rubric:: Configuring E-Wallet development
            environment
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by Jacki Heo, last modified by Hailee Kim on Mar 26,
            2019

         .. container:: wiki-content group
            :name: main-content

            | 

             

            .. rubric:: Platform
               :name: ConfiguringE-Walletdevelopmentenvironment-Platform

            -  Mac OS Mojave or higher

            .. rubric:: Tools
               :name: ConfiguringE-Walletdevelopmentenvironment-Tools

            -  Terminal
            -  Web Browser

            .. rubric:: Nodejs & NPM Installation
               :name: ConfiguringE-Walletdevelopmentenvironment-Nodejs&NPMInstallation

            -  https://nodejs.org/ko/ download for mac 10.15.0
            -  Check Npm and node version

            .. container:: table-wrap

               +-----------------------------------------------------------------------+
               | $ npm --version                                                       |
               |                                                                       |
               | 6.4.1                                                                 |
               |                                                                       |
               | $ node --version                                                      |
               |                                                                       |
               | v10.15.0                                                              |
               +-----------------------------------------------------------------------+

            .. rubric:: npm Optional
               :name: ConfiguringE-Walletdevelopmentenvironment-npmOptional

            -  https://johnpapa.net/node-and-npm-without-sudo/

            | 

            -  global packages directory reset

            .. container:: table-wrap

               +-----------------------------------------------------------------------+
               | $ mkdir ~/.npm-packages                                               |
               |                                                                       |
               | $ npm config set prefix ~/.npm-packages                               |
               +-----------------------------------------------------------------------+

            | 

            -  Check for errors

            .. container:: table-wrap

               +-----------------------------------------------------------------------+
               | $ npm list -g --depth=0                                               |
               |                                                                       |
               | /Users/jyun/.npm-packages/lib                                         |
               |                                                                       |
               | └── (empty)                                                           |
               +-----------------------------------------------------------------------+

            | 

            -  Add the following PATH Environment Variables to ~ /
               .profile file

            .. container:: table-wrap

               +-----------------------------------------------------------------------+
               | export PATH=$PATH:$HOME/.node-packages/bin                            |
               +-----------------------------------------------------------------------+

            | 

            -  .. code:: tw-data-text

                  Restart terminal

            .. rubric:: Cordova Installation
               :name: ConfiguringE-Walletdevelopmentenvironment-CordovaInstallation

            .. container:: table-wrap

               +-----------------------------------------------------------------------+
               | $ npm install -g cordova                                              |
               |                                                                       |
               | /Users/jyun/.npm-packages/bin/cordova -> /Users/jyun/.npm-            |
               |                                                                       |
               | packages/lib/node_modules/cordova/bin/cordova                         |
               |                                                                       |
               | + cordova@8.1.2                                                       |
               |                                                                       |
               | added 594 packages from 523 contributors in 21.418s                   |
               +-----------------------------------------------------------------------+

            .. rubric:: Ionic Installation
               :name: ConfiguringE-Walletdevelopmentenvironment-IonicInstallation

            -  `Go to the installation webpage for the
               reference:https://beta.ionicframework.com/docs/ <https://beta.ionicframework.com/docs/>`__
            -  Ionic Install

            .. container:: table-wrap

               +-----------------------------------------------------------------------+
               | $ npm install -g ionic                                                |
               |                                                                       |
               | /Users/jyun/.npm-packages/bin/ionic -> /Users/jyun/.npm-              |
               |                                                                       |
               | packages/lib/node_modules/ionic/bin/ionic                             |
               |                                                                       |
               | + ionic@4.10.3                                                        |
               |                                                                       |
               | added 262 packages from 175 contributors in 11.451s                   |
               +-----------------------------------------------------------------------+

            .. rubric:: Cocoapods Installation
               :name: ConfiguringE-Walletdevelopmentenvironment-CocoapodsInstallation

            -  Refer to \ https://cocoapods.org/

            -  Installation from the terminal with the following command

            .. container:: table-wrap

               +-----------------------------------------------------------------------+
               | $ sudo gem install cocoapods                                          |
               |                                                                       |
               | $ pod repo update                                                     |
               +-----------------------------------------------------------------------+

            .. rubric:: SDK-MAN and gradle Installation
               :name: ConfiguringE-Walletdevelopmentenvironment-SDK-MANandgradleInstallation

            .. container:: table-wrap

               +-----------------------------------------------------------------------+
               | $ curl -s "\ https://get.sdkman.io\ " \| bash                         |
               |                                                                       |
               | $ sdk install gradle                                                  |
               +-----------------------------------------------------------------------+

            | 

            | 

            --------------

            --------------

            .. rubric:: Platform
               :name: ConfiguringE-Walletdevelopmentenvironment-Platform.1

            -  Mac OS Mojave 이상

            .. rubric:: 사용툴
               :name: ConfiguringE-Walletdevelopmentenvironment-사용툴

            -  Terminal
            -  Web Browser

            .. rubric:: Nodejs & NPM 설치
               :name: ConfiguringE-Walletdevelopmentenvironment-Nodejs&NPM설치

            -  https://nodejs.org/ko/ download for mac 10.15.0
            -  Npm 및node버전 확인

            .. container:: table-wrap

               ================
               $ npm --version
               
               6.4.1
               
               $ node --version
               
               v10.15.0
               ================

             

            .. rubric:: npm 부가설정( optional )
               :name: ConfiguringE-Walletdevelopmentenvironment-npm부가설정(optional)

            -  https://johnpapa.net/node-and-npm-without-sudo/

            | 

            -  global packages directory 재설정

            .. container:: table-wrap

               =======================================
               $ mkdir ~/.npm-packages
               
               $ npm config set prefix ~/.npm-packages
               =======================================

            | 

            -  오류 없는지 확인

            .. container:: table-wrap

               =============================
               $ npm list -g --depth=0
               
               /Users/jyun/.npm-packages/lib
               
               └── (empty)
               =============================

            | 

            -  ~/.profile 파일에 아래의PATH Environment Variables추가

            .. container:: table-wrap

               ==========================================
               export PATH=$PATH:$HOME/.node-packages/bin
               ==========================================

            | 

            -  터미널 재시작

            -  .. code:: tw-data-text

            .. rubric:: Cordova 설치
               :name: ConfiguringE-Walletdevelopmentenvironment-Cordova설치

            .. container:: table-wrap

               =======================================================================================================
               $ npm install -g cordova
               
               /Users/jyun/.npm-packages/bin/cordova -> /Users/jyun/.npm-packages/lib/node_modules/cordova/bin/cordova
               
               + cordova@8.1.2
               
               added 594 packages from 523 contributors in 21.418s
               =======================================================================================================

             

            | 

            .. rubric:: Ionic 설치
               :name: ConfiguringE-Walletdevelopmentenvironment-Ionic설치

            -  https://beta.ionicframework.com/docs/ 사이트로
               이동Installation참고
            -  Ionic Install

            .. container:: table-wrap

               =================================================================================================
               $ npm install -g ionic
               
               /Users/jyun/.npm-packages/bin/ionic -> /Users/jyun/.npm-packages/lib/node_modules/ionic/bin/ionic
               
               + ionic@4.10.3
               
               added 262 packages from 175 contributors in 11.451s
               =================================================================================================

            | 

            .. rubric:: Cocoapods 설치
               :name: ConfiguringE-Walletdevelopmentenvironment-Cocoapods설치

            .. rubric:: ●       https://cocoapods.org/ 참고
               :name: ConfiguringE-Walletdevelopmentenvironment-●https://cocoapods.org/참고

            .. rubric:: ●       터미널에서 아래 명령어로 설치
               :name: ConfiguringE-Walletdevelopmentenvironment-●터미널에서아래명령어로설치

             

            .. container:: table-wrap

               ============================
               $ sudo gem install cocoapods
               
               $ pod repo update
               ============================

            | 

            .. rubric:: SDK-MAN 및gradle설치
               :name: ConfiguringE-Walletdevelopmentenvironment-SDK-MAN및gradle설치

            -  https://sdkman.io/install
            -  https://gradle.org/install

             

            .. container:: table-wrap

               =============================================
               $ curl -s "\ https://get.sdkman.io\ " \| bash
               
               $ sdk install gradle
               =============================================

   .. container::
      :name: footer

      .. container:: section footer-body

         Document generated by Confluence on Mar 27, 2019 18:40

         .. container::
            :name: footer-logo

            `Atlassian <http://www.atlassian.com/>`__

