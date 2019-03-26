Testing Environment Guideline
=============================

Javascript
----------

xSample application을 clone후 수행한다.

javascript application은 reactjs를 사용하여 개발되었으며, 모든 API기능을
사용할 수 있도록 하였다.

주요 기능은 Signup, SignIn, Address추가 삭제, Balance확인 등이다.

Sample Application수행 환경은 Linux/Windows/MacOS 모두 가능하다. 단,
Node 버전 10.0이상, NPM 6.9이상이어야 한다.

다음은 수행 방법이다.

1. GitHub Sample Application Clone
	::

		git clone https://github.com/Edenpartners/sample-react-api.git

2. 해당 디렉토리로 이동
	::

		cd sample-react-api

3. npm을 사용하여 패키지 설치
	::

		npm install

4. 샘플 어플리케이션 수행
	::

		npm start

5. 샘플 어플리케이션 접속
	::

		웹 브라우저에서 http://localhost:3000을 접속한다.

Python
------

Sample application을 clone후 수행한다.

python3상에서 수행되며,  모든 API기능을 사용할 수 있도록 하였다.

주요 기능은  SignIn, Address추가 삭제, Balance확인 등이다.

Sample Application수행 환경은 Linux/Windows/MacOS 모두 가능하다.

다음은 수행 방법이다.

1. GitHub Sample Application Clone
	::

		git clone https://github.com/Edenpartners/sample-python-api.git

2. 해당 디렉토리로 이동
	::

		cd sample-python-api

3. virtual environment 설정
	::

		python3 -m venv env
		env/bin/activate

4. 패키지 설치
	::

		ip install -r requirements.txt

5. 테스트 프로그램 수행
	::

		python3 main.py
