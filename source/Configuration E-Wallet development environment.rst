============================================
Configuring E-Wallet development environment
============================================


Platform
--------

-  Mac OS Mojave 이상

 

사용툴
------

-  Terminal

-  Web Browser

Nodejs & NPM 설치
-----------------

-  https://nodejs.org/ko/ download for mac 10.15.0

-  Npm 및 node 버전 확인

   ::

      $ npm --version

      6.4.1

      $ node --version
   
      v10.15.0


..

    

npm 부가설정 ( optional )
-------------------------

-  https://johnpapa.net/node-and-npm-without-sudo/

-  global packages directory 재설정

   ::

      $ mkdir ~/.npm-packages

      $ npm config set prefix ~/.npm-packages

..

-  오류 없는지 확인

   ::

      $ npm list -g --depth=0

      /Users/jyun/.npm-packages/lib

      └── (empty)

..


-  ~/.profile 파일에 아래의 PATH Environment Variables 추가

   ::
    
     export PATH=$PATH:$HOME/.node-packages/bin

..

-  터미널 재시작

Cordova 설치
------------

 

   ::

      $ npm install -g cordova

      /Users/jyun/.npm-packages/bin/cordova -> /Users/jyun/.npm-packages/lib/node_modules/cordova/bin/cordova/ + cordova@8.1.2

      added 594 packages from 523 contributors in 21.418s

   ..

 

Ionic 설치
----------

-  https://beta.ionicframework.com/docs/ 사이트로 이동 Installation 참고

-  Ionic Install

 

   ::

      $ npm install -g ionic

      /Users/jyun/.npm-packages/bin/ionic -> /Users/jyun/.npm-packages/lib/node_modules/ionic/bin/ionic + ionic@4.10.3

      added 262 packages from 175 contributors in 11.451s

   ..

Cocoapods 설치
--------------

-  https://cocoapods.org/ 참고

-  터미널에서 아래 명령어로 설치


 

   ::

      $ sudo gem install cocoapods

      $ pod repo update

   ..

SDK-MAN 및 gradle 설치
----------------------

-  https://sdkman.io/install
-  https://gradle.org/install



    

   ::

      $ curl -s "https://get.sdkman.io" \| bash

      $ sdk install gradle

   ..

