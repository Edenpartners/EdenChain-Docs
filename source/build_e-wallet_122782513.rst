===============================
Build E-Wallet 
===============================

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

         .. rubric:: Build E-Wallet 
            :name: title-heading
            :class: pagetitle

      .. container:: view
         :name: content

         .. container:: page-metadata

            Created by Jacki Heo, last modified on Mar 28, 2019

         .. container:: wiki-content group
            :name: main-content

            .. rubric:: Environment
               :name: BuildE-Wallet-Environment

            -  MacOS Mojave
            -  NodeJS

               -  13.0

            -  NPM

               -  4.1

            -  Ionic

               -  Framework 4.0.0
               -  CLI 4.10.3

            -  IDE

               -  Xcode 10.1

            | 

            .. container:: table-wrap

               ==========================================================================================================================
               $ ionic info
               
               | 
               
               Ionic:
               
                  ionic (Ionic CLI)             : 4.10.3 (/Users/jyun/.nvm/versions/node/v10.13.0/lib/node_modules/ionic)
               
                  Ionic Framework               : @ionic/angular 4.0.0
               
                  @angular-devkit/build-angular : 0.12.4
               
                  @angular-devkit/schematics    : 7.2.4
               
                  @angular/cli                  : 7.2.4
               
                  @ionic/angular-toolkit        : 1.4.0
               
               | 
               
               Cordova:
               
                  cordova (Cordova CLI) : 8.1.2 (cordova-lib@8.1.1)
               
                  Cordova Platforms     : android 7.1.4, browser 5.0.4, ios 4.5.5
               
                  Cordova Plugins       : cordova-plugin-ionic-keyboard 2.1.3, cordova-plugin-ionic-webview 3.1.2, (and 17 other plugins)
               
               | 
               
               System:
               
                  ios-deploy : 1.9.4
               
                  NodeJS     : v10.13.0 (/Users/jyun/.nvm/versions/node/v10.13.0/bin/node)
               
                  npm        : 6.4.1
               
                  OS         : macOS Mojave
               
                  Xcode      : Xcode 10.1 Build version 10B61
               ==========================================================================================================================

            .. rubric:: E-Wallet Build Environment Setting
               :name: BuildE-Wallet-E-WalletBuildEnvironmentSetting

            .. rubric:: Overview
               :name: BuildE-Wallet-Overview

            The e-wallet GIT source has removed the necessary firebase,
            facebook, twitter and google config settings for the app. If
            you do not take any action due to setting conflicts of Ionic
            Plugins, a build error occurs when creating the Android /
            iOS Platform environment.

            Build environment should be set up using a separate script
            which is designed to automatically remove Android, iOS,
            firebase related conflicts and build conflicts.

            .. rubric:: E-Wallet Project Reset
               :name: BuildE-Wallet-E-WalletProjectReset

             for //debug 

            .. container:: table-wrap

               ================================
               .. container:: table-wrap
               
                  =============================
                  $ npm run reset:ewallet-debug
                  =============================
               ================================

            | 

             for //product

            .. container:: table-wrap

               ============================
               $ npm run reset:ewallet-prod
               ============================

            .. rubric::  
               :name: BuildE-Wallet-

            .. rubric:: Building Product for Android
               :name: BuildE-Wallet-BuildingProductforAndroid

            Access to Project root folder in Terminal before build and
            reflect source edit history

            .. container:: table-wrap

               =========================================
               | 
               
               .. container:: table-wrap
               
                  ======================================
                  $ ionic cordova prepare android --prod
                  ======================================
               =========================================

            #. Launch Android Studio
            #. Select Import project (Gradle, Eclipse ADT, etc.) 
            #. 'e-wallet/platforms/android' Open
            #. .. code:: tw-data-text

                  Build apk via Build / Generate Signed APK

            | 

            .. rubric:: Building Product for iOS
               :name: BuildE-Wallet-BuildingProductforiOS

            Access to Project root folder in Terminal before build
            and reflect source edit history

            .. container:: table-wrap

               =====================================
               | 
               
               .. container:: table-wrap
               
                  ==================================
                  $ ionic cordova prepare ios --prod
                  ==================================
               =====================================

            #. Launch Xcode
            #. 'e-wallet/platforms/ios/E-Wallet.xcworkspace' Open
            #. For Product Build, launch XCode / Product / Archive

            | 

            .. rubric:: Build error patch
               :name: BuildE-Wallet-Builderrorpatch

            Because of dependency conflicts, duplicate symbol errors
            occur during Xcode build. Therefore, you should remove
            specific .frameworks for E-Wallet / Frameworks / at first
            open with Xcode.

            | ● nanopb.framework
            | ● FirebaseAnalytics.framework
            | ● FirebaseAuth.framework
            | ● FirebaseCore.framework
            | ● FirebaseInstanceID.framework
            | ● FirebaseABTesting.framework
            | ● GoogleUtilities.framework
            | ● GTMSessionFetcher.framework
            | ● GoogleToolboxForMac.framework
            | ● Protobuf.framework
            | ● FirebaseMessaging.framework
            | ● FirebasePerformance.framework
            | ● FirebaseRemoteConfig.framework

            .. image:: images/122782513/123338815.png

            .. rubric:: Universal link Configuration
               :name: BuildE-Wallet-UniversallinkConfiguration

            For Universal link operation, Xcode configuration is
            necessary as below, assuming the relevant settings of the
            Signing Profile and the Server in Apple Development already
            exist.

            -  Set Xcode / E-Wallet / Capabilities / Associated Domains
            -  After clicking the‘+’ button, type
               ‘applinks:\ `ewallet.edenchain.io <http://ewallet.edenchain.io>`__ 

            .. image:: images/122782513/122782520.png

         .. container:: pageSection group

            .. container:: pageSectionHeader

               .. rubric:: Attachments:
                  :name: attachments
                  :class: pageSectionTitle

            .. container:: greybox

               |image0|
               `image2019-3-25_12-34-7.png <images/122782513/123338815.png>`__
               (image/png)
               |image1|
               `image2019-3-25_12-34-43.png <images/122782513/122782520.png>`__
               (image/png)

   .. container::
      :name: footer

      .. container:: section footer-body

         Document generated by Confluence on Mar 28, 2019 14:29

         .. container::
            :name: footer-logo

            `Atlassian <http://www.atlassian.com/>`__

.. |image0| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px
.. |image1| image:: images/icons/bullet_blue.gif
   :width: 8px
   :height: 8px

