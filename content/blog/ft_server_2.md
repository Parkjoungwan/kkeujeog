---
title: "[Docker, ft_server]FT_SERVER 튜토리얼"
date: 2021-05-17T18:17:41+09:00
slug: ""
description: ""
keywords: [Docker, ft_server, 42Seoul]
draft: true
tags: [Docker, ft_server, 42Seoul]
math: false
toc: false
---
# FT_SERVER

# 안 읽어도 되는 부분(전후상황)

드디어, 지난 주 목요일 EXAM02를 통과했다. 라 피씬 이후 첫 EXAM 통과였다.

EXAM02는 CIr1에 해당하는 내용을 다루고 있었고, GNL을 구현하는 문제가 나와 어렵사리 통과할 수 있었다.

그렇다.

이제 난 당당히 Cir2에 해당하는 서브젝트들을 풀어볼 수 있는 자격을 얻게 된 것이다.

하하하(사악한 악당 웃음)

# ft_server 분석

ft_server는 도커 사용에 초점을 맞춘 서브젝트인 것 같다.

도커를 사용해 데비안 버스터라는 OS의 이미지를 가져오고 그 위에 Nginx라는 웹 서버 소프트웨어를 가동시키고 그리고 그 위에 wordpress, php-fpm, MariaDB(mysql), phpMyAdmin등을 얹어 특정한 하나의 OS와 웹 서버를 만들어 보는 것 이다.

물론, 웹 서버의 작동에 관련된 개념도 알아야 한다. 어떻게 SSL 인증서를 통해 인터넷 세계는 안전한 웹 환경을 만들고 있는가 라던가. url을 통해서 어떻게 한 서버안에서 각 앱들을 구동시킨다 라던가, autoindex를 통해서 우리가 원하는 페이지를 사용자에게 제공하는가 등이 담겨져 있는 것 같다.

# docker 시작

위 서브젝트는, docker라는 아주 편리한 tool을 사용하기 떄문에 명령어만 숙지하고 있다면 빠르게 환경을 구축해 나갈 수 있다. 코드를 작성하거나 수정해야하는 부분은 생각보다 적었다.

그럼, 우리가 Terminal에 작성해야하는 docker 명령어들과 그 명령어들이 무엇을 의미하는지에 대해서 정리해보자.

```docker
docker pull debian:buster
```

시작은 이거다. 위에도 얘기했지만, 이 서브젝트는 debian buster라는 리눅스기반(?)의 OS 위에 우리가 사용할 앱들을 얹어내는 과제이다.

그러기 위해서 우리가 가장 먼저 행해야할 작업은 Docker 위에 얹을 OS를 먼저 찾는 것이다.

'docker pull [우리가 얹을 OS 이미지]' 라는 명령어를 통해서 OS이미지를 찾고 우리가 사용할 수있게 다운로드한다.

```docker
docker run -it --name con_debian -p 80:80 -p 443:443 debian:buster
```

자, 우리가 사용할 OS 이미지를 얻었으니 이제 그 이미지를 통해서 가상머신을 구동시켜보자. (정확히는 가상머신을 구동시키는건 아니고 컨테이너를 실행시키는 것인데 Docker의 원리는 나중에 따로 공부하고 간단히 생각해서 우리가 원하는 OS의 가상머신을 구동한다고 생각하자.)

docker run 은 가상머신을 구동시킬 떄 사용하는 명령어이다. 여기에 몇가지 옵션들을 부쳐서 우리가 원하는 환경으로 실행 시킬 수 있다.

-it 은 컨테이너를 종료 시키지 않은체로, 터미널에 계속 입력을 받아 컨테이너로 전달해줄 수 있도록 해주는 옵션.

—name은 우리가 실행시키기는 컨테이너에 이름을 붙여주는 것이다. 컨테이너 ID로 다시 구동시키면 귀찮기 떄문에 나중에 찾기 편하려고 이름을 붙인다고 생각하면 될 것 같다.

-p 는 실행된 컨테이너와 우리, 다시 말해 HOST간의 port publish/bind를 위해서 사용되는데, 쉽게 말해 컨테이너의 실행을 우리의 컴퓨터에서 관찰할 수 있기 위해서 우리 컴퓨터의 포트와 컨테이너를 연결한다고 생각하면 될 것 같다. 위 명령어에서는 80번과 443번에 연결했다.

```docker
apt-get -y install nginx curl
```

위에서 이제 컨테이너를 실행시켰으니, 그 위에 웹 서버를 가동시켜줄 nginx를 얹어야 할 차례이다.

apt-get이라는 다운로드 툴을 사용해서 nginx와 curl을 다운로드하는 명령어 이다.

-y옵션은 다운로드 하는 과정에서 질문하는 모든 질문에 yes로 대답하겠다는 옵션이다.

'apt-get install [다운로드 할 앱]' 이 명령어를 통해서 다운로드를 시작한다.

아, apt-get 사용이 제대로 작동되지 않는다면, 'apt-get update', 'apt-get upgrade' 명령어를 통해서 업데이트해보자.

```docker
service nginx start
```

다운로드가 완료되었다면, nginx를 실행해 서버가 제대로 작동되는가를 확인해보자.

[](http://localhost:80)

위 url로 접속했을 떄 'Welcome's to nginx!'가 뜬다면 제대로 작동되고 있는 것 이다.

```docker
curl localhost
```

위 명령어를 통해서 현재 localhost에서 어떤 페이지를 제공하고 있는지 html 코드로도 확인해볼 수 있다.

```docker
service nginx stop
```

그럼 잠시 서버를 중단 시켜놓자.

여기 까지가, docker를 이용해 OS를 설치하고 컨테이너를 실행시키고 웹 서버를 작동시킬 앱을 다운로드 받는 과정이다. 다음은, HTTPS 프로토콜에 인증성에 대해서 알아보자.

---

# SSL 인증서 생성

HTTPS(Hypertext Transfer Protocol over Secure Socket Layer)는 SSL 위에서 돌아가는 HTTP처럼 평문을 전송하는 것이아닌 평문을 암호화한 후 통신하는 암호화 프로토콜이다.

그런데, HTTPS 통신 서버를 구현하기 위해서는 **"신뢰할 수 있는 상위 기업"**이 발급한 인증서, CA(Certificate authority)라는 것이 필요한데. **이게 유료다;**

그래도 우리는 HTTPS 통신을 하고 싶으니, 대채제로 self-signed SSL 인증서를 사용한다. 말 그대로 자체 발급 인증서이며, 로그인 정보 및 기타 계인 인증 정보를 암호화할 수 있다. 물론 유효한 인증서로 인정되는 것은 아니여서 브라우저로 실행 했을 때 보안 경고가 발생하는건 어쩔 수 없다.

self-singed SSL를 만드는 방법은 여러 경로가 있지만, 여기서는 오픈소스인 openssl을 사용해서 만들어 보려한다.

openssl은 HTTPS 서버 구현을 위한, .key(개인키), .csr(서면요청파일), .crt(인증서 파일)을 발급해준다.

```docker
apt-get -y install openssl
```

openssl을 apt-get 명령어를 통해 다운로드한다.

```docker
openssl req -newkey rsa:4096 -days 365 -nodes -x509 -subj "/C=KR/ST=Seoul/L=Seoul/O=42Seoul/OU=joupark/CN=localhost" -keyout localhost.dev.key -out localhost.dev.crt
```

자, 처음으로 엄청나게 긴 명령어가 하나 나왔다.

하나 하나 뜯어 설명해보자.

- req: 인증서 요청서 및 인증서 생성
- -newkey rsa:4096: 개인키 생성 옵션 4096-bit의 개인키가 생성된다.
- -days 365: 인증서의 유효기간을 작성하는 옵션
- -nodes: 개인키를 암호화하지 않는 옵션
- -x509: 인증서를 만드는 대신에 자체 사인한 인증서를 만드는 옵션
- -subj: DN(Distinguished Name)을 작성한다.

DN이란, 인증서에 들어갈 특정 값들이다. 아래 링크를 참고하자.

[What is a Distinguished Name (DN)?](https://knowledge.digicert.com/generalinformation/INFO1745.html)

- -keyout <키 파일 이름>: 키 파일 이름을 지정해 키 파일 생성해주는 옵션
- -out <인증서 이름>: 인증서 이름을 지정해 인증서를 생성해주는 옵션

결론적으로 저 명령어는

"인증서 요청서나 인증서를 생성하려고 하는데요. 4096-bit짜리 개인키를 생성해주시고, 유효기간은 365일로 해주시고, 개인키는 암호화 하지 마시고요. 아, 인증 요청서 말고 self singed 인증서 만들거고요. DN에는 국가: 한국, 거리:서울, 위치:서울, 기관:42서울, 기관유닛:joupark, Common Name(:?이건 뭐라하지):localhost로 해주세요."

라고 할 수 있을 것 같다.

```docker
mv localhost.dev.crt etc/ssl/certs/
mv localhost.dev.key etc/ssl/private/
chmod 600 etc/ssl/certs/localhost.dev.crt etc/ssl/private/localhost.dev.key
```

이렇게 생성된 개인 키와 인증서를 서버를 실행할 때, SSL인증서를 체크하는 폴더로 옮겨주고 권한 설정을 해줍니다. 여기서 chmod 600은 r w -|- - -|- - -로, 소유자만 읽고 쓸 수 있게 권한을 준다는 뜻이다.

# 내 SSL을 알까?

이렇게 생성된 SSL을 nginx에서 사용하려면, nginx가 우리가 특정한 폴더에서 ssl 인증서와 개인키를 읽을 수 있어야한다.

그 설정을 위해서 etc/nginx/sites-available/default 파일을 수정해준다.

그 전에 파일 수정할 때 사용할 vim 에디터를 설치하자.

```docker
apt-get -y install vim
vim etc/nginx/sites-available/default
```

수정완료된 코드를 보자.

```docker
server {
	listen 80;
	listen [::]:80;

	return 301 https://$host$request_uri;
}

server {
	listen 443 ssl;
	listen [::]:442 ssl;

	# ssl set
	ssl on;
	ssl_certificate /etc/ssl/certs/localhost.dev.crt;
	ssl_certificate_key /etc/ssl/private/localhost.dev.key;

	# set root dir
	root /var/www/html;

	# autoindex
	index index.html index.htm index.nginx-debian.html;

	server_name ft_server;
	location / {
		try_files $uri $uri/ =404;
	}
}
```

80번 포트로 접속하면 301을 반환하는데 여기서 301은 Moved permanently 상태코드로,

> HTTP 응답 상태 코드 301 Moved Permanently는 영구적인 URL 리다이렉션을 위해 사용되며, 즉 응답을 수신하는 URL을 사용하는 현재의 링크나 레코드가 업데이트되어야 함을 의미한다. 이 새 URL은 응답에 포함된 위치 필드에 지정되어야 한다. 301 리다이렉트는 사용자가 HTTP를 HTTPS로 업그레이드하게 만드는 최상의 방법으로 간주된다

라고 위키에서 긁어왔다. HTTPS를 만들기 위한 방법이라고 생각하면 될 것 같다.

```docker
service nginx reload
service nginx start
```

바뀐 설정을 reload로 적용하고, 다시 서버를 실행시켰을 때, chrome에서 "Your connection is not private" 경고문구가 뜬다면 성공이다.

잠깐, 그럼 경고문구밖에 안 뜨는데 제대로 작동되는지는 어떻게 확인하지?

*여기서 부터는 안전하지 않은 사이트를 접속하는 방법이니, 지금 경우 우리가 작동시킨 서버이기 때문에 괜찮지만 안전한 사이트가 아니라면 이 방법은 사용하지 말자. (mac에서는 고급 설정에서 안전하지 않은 사이트임을 인지하고 접속하는 버튼이 없기 때문에 아래 방법을 사용한다.)

1. NET::ERR_CERT_REVOKED 화면의 빈 공간의 아무 곳에서 마우스 좌클릭.
2. 키보드로 `thisisunsafe` 문자열 입력. (화면에 보이지 않으니 그냥 입력)
3. 접속하고자 하는 화면이 보이면 성공. 보이지 않으면 화면 Refresh 하시고 다시 시도.

# php-fpm 설치 및 nginx 설정

nginx는 웹 서버이기 때문에 정적인 페이지만을 보여준다. 그래서 동적 페이지(데이터를 통해서 유동적으로 페이지를 변경할 수 있는 페이지)를 구성하기 위해서는 데이터를 읽고 html로 변환해서 웹 서버에게 전달해줄 외부 프로그램이 필요한데.

그게 php모듈이다. 이런 연결과정의 방법 or 규약을 정의한 것을 CGI라고 부른다.

```docker
apt-get install php-fpm
```

위에서와 마찬가지로 apt-get으로 php-fpm를 설치한 후, nginx에서 php파일을 처리해주기 위한 설정을 추가해야한다.

```docker
vim /etc/nginx/sites-available/default
```

vim에디터를 통해서 설정파일을 열어주자.

```docker
server {
	listen 80;
	listen [::]:80;

	return 301 https://$host$request_uri;
}

server {
	listen 443 ssl;
	listen [::]:442 ssl;

	# ssl setting
	ssl on;
	ssl_certificate /etc/ssl/certs/localhost.dev.crt;
	ssl_certificate_key /etc/ssl/private/localhost.dev.key;

	# Set root dir of server
	root /var/www/html;

	# Auto index
	-index index.html index.htm index.nginx-debian.html
	+index index.html index.htm index.nginx-debian.html index.php;

	server_name ft_server;
	location / {
		try_files $uri $uri/ =404;
	}

	# PHP
	+location ~ \.php$ {
	+  include snippets/fastcgi-php.conf;
	+ fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
	+}
```

**이전 설정파일에서 바뀐부분은 +와 -로 표시했다. 복붙할 땐 지워주자.**

# autoindex가 뭔데

여기서 설정파일을 적용하기 전에, autoindex도 추가해줄 것이다.

auto index는 웹서버가 어떻게 우리가 원하는 페이지를 띄워주는가에 대한 문제인데 다음 인용글을 보자.

> **웹 서버는 어떻게 수 많은 리소스 중 요청에 알맞은 콘텐츠를 제공할까?**
일반적으로 웹 서버 파일 시스템의 특별한 한 폴더를 웹 콘텐츠를 위해 사용한다. 이 폴더를 문서루트 혹은 docroot라고 부른다. 리소스 매핑의 가장 단순한 형태는 요청 URI를 dotroot 안에 있는 파일의 이름으로 사용하는 것이다.
만약 파일이 아닌 디렉토리를 가리키는 url에 대한 요청을 받았을 때는, 요청한 url에 대응되는 디렉토리 안에서 index.html 혹은 index.htm으로 이름 붙은 파일을 찾아 그 파일의 콘텐츠를 반환한다. 이를 **autoindex** 라고 부른다.

즉, 우리가 특정 폴더에 접근했을 때, 자동으로 index.html, index.htm으로 시작하는 파일을 찾아 페이지를 띄워주는 기능이라고 이해하면 될 것 같다.

따라서 다시 설정파일을 좀 만져주자.

```docker
server {
	listen 80;
	listen [::]:80;

	return 301 https://$host$request_uri;
}

server {
	listen 443 ssl;
	listen [::]:442 ssl;

	# ssl setting
	ssl on;
	ssl_certificate /etc/ssl/certs/localhost.dev.crt;
	ssl_certificate_key /etc/ssl/private/localhost.dev.key;

	# Set root dir of server
	root /var/www/html;

	# Auto index
	index index.html index.htm index.nginx-debian.html index.php;

	server_name ft_server;
	location / {
	+ autoindex on;
		try_files $uri $uri/ =404;
	}

	# PHP
	location ~ \.php$ {
	  include snippets/fastcgi-php.conf;
	 fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
	}
```

그럼 일단은 설정파일을 적용시키자.

```docker
service nginx reload
service nginx start
```

# MariaDB(mysql) 얹기

```docker
apt-get -y install mariadb-server php-mysql php-mbstring
```

이제 좀 익숙하다. mariadb-server는 MariaDB설치, php-mysql은 php에서 mysql에 접근할 수 있게 해주는 모듈, php-mbstring은 2바이트 확장 문자를 읽을 수 있게 해주는 모듈이다. 모두 설치해주자.

이거 입력하면 끝이다. MariaDB 설치 끝

# Wordpress 얹기

```docker
apt-get -y install wget
wget https://wordpress.org/latest.tar.gz
tar -xvf latest.tar.gz
mv wordpress/ var/www/html/
```

왠지는 모르겠는데, 리눅스 기본 명령어인 wget이 작동을 안했다.

그래서 apt get으로 설치해줬다.

wget은 web-get의 준말로 웹 주소로 해당 파일을 다운로드해주는 역할을 한다.

즉, 위에 적힌 아래서 3개의 명령어는 wget으로 압축파일을 다운로드받고, tar로 압축을 풀어, wordpress를 nginx의 루트폴더로 옮겨주는 과정이다.

```docker
chown -R www-data:www-data /var/www/html/wordpress
```

wordpress의 사용자를 바꿔줄 필요가 있다.

따라서 chown 명령어를 통해서 사용자 유저를 nginx.conf에 적혀있는 사용자로 변경해준다.

-R은 재귀적용을 위한 옵션으로, 해당 폴더 내에 있는 모든 폴더와 파일에 사용자 변경을 적용 시켜준다.

```docker
cp -rp var/www/html/wordpress/wp-config-sample.php var/www/html/wordpress/wp-config.php
```

자, 이제 wordpress의 초기 설정을 해줘야한다.

설정파일의 이름은 wp-config.php인데, wordpress를 처음 설치했을 때 제공하는 wp-config-sample.php를 cp 명령어를 통해서 복사해서 사용하면 된다.

그럼 복사해온 설정파일에서 우리가 수정해야되는 부분은 DB관련 정보이다.

```docker
define( 'DB_NAME', 'wordpress' );

/** MySQL database username */
define( 'DB_USER', 'joupark' );

/** MySQL database password */
define( 'DB_PASSWORD', 'joupark' );
```

wordpress 에서 사용할 데이터베이스의 이름과 사용자 그리고 비밀번호를 설정해주는 과정이다.

이름은 wordpress, 사용자는 joupark, 비밀번호는 joupark로 설정했다.

자, 그럼 이제 mysql을 이용해서 위 정보에 맞게 데이터베이스와 사용자를 설정해보자.

```docker
service mysql start
```

mysql을 실행시켜준다.

```docker
mysql
# 이제 mysql이 실행됐을 것이다.

create database wordpress;
# wordpress 데이터베이스 생성

create user 'joupark'@'localhost' identified by 'joupark';
# 유저 생성 @'localhost'는 로컬 접속만 허용한다는 뜻이다. @ %로 입력하면 외부 접속을 허용한다는 뜻이 된다.
# identified by '비밀번호'이다. 내 경우는 joupark를 비밀번호로 설정해줬다.

GRANT ALL PRIVILEGES ON wordpress.* TO 'joupark'@'localhost' WITH GRANT OPTION;
# 'joupark'에게 wordpress 데이터베이스의 권한을 넘겨주는 과정.

use wordpress;
show tables;
# 설정을 확인

exit
# mysql 탈출
```

다음과 같은 과정으로, mysql 설정도 ok 이다.

이제, php 모듈을 한 번 실행시켜주면, 설정 적용이 된다.

```docker
service php7.3-fpm start
service php7.3-fpm restart
```

[https://localhost/wordpress/](https://localhost/wordpress/wp-admin/)

위 url로 접속했을 때, wordpress 사이트가 보인다면 성공!

# phpMyAdmin 얹기

```docker
wget https://files.phpmyadmin.net/phpMyAdmin/5.0.2/phpMyAdmin-5.0.2-all-languages.tar.gz
tar -xvf phpMyAdmin-5.0.2-all-languages.tar.gz
mv phpMyAdmin-5.0.2-all-languages phpmyadmin
mv phpmyadmin /var/www/html/
```

wordpress를 설치할 때와 같다.ui

wget을 통해서 압축파일을 다운받고, tart을 통해서 압축파일을 해제하고 필요한 파일들을 쓰일 장소로 옮겨준다.

```docker
cp -rp var/www/html/phpmyadmin/config.sample.inc.php var/www/html/phpmyadmin/config.inc.php
```

이번에도 phpMyAdmin 설정파일을 만들어줘야한다.

wordpress와 동일한 과정이다.

```docker
vim var/www/html/phpmyadmin/config.inc.php
```

설정파일을 vim 에디터로 수정해주자.

여기서 우리가 설정해줘야하는 부분은 'blowfish_secret'라는 부분인데,

> 일반적으로 디지털 서명이나 암호화 복호화에 사용되는 패스워드보다 긴 문자열로 된 비밀번호

라고 네이버 지식백과에 적혀있다.

아무튼 아주 긴 비밀번호를 입력해줘야한다고 생각하면 된다. 그런데 이 비밀번호를 만들어주는 사이트가 있으니 그 사이트를 사용하자.

[phpMyAdmin blowfish secret generator](https://phpsolved.com/phpmyadmin-blowfish-secret-generator/?g=%5Binsert_php%5Decho%20$code;%5B/insert_php%5D)

```docker
$cfg['blowfish_secret'] = ''; /* YOU MUST FILL IN THIS FOR COOKIE AUTH! */
```

생성된 비밀번호를 위 코드와 교체해주면 된다.

다음으로, phpMyAdmin의 정보를 담기위한 데이터베이스를 생성해줘야하는데, wordpress와 다르게 phpMyadmin은 데이터베이스 생성을 위한 sql문을 따로 제공한다. 그걸 사용하자.

```docker
mysql < var/www/html/phpmyadmin/sql/create_tables.sql
```

mysql에 var/www/html/phpmyadmin/sql/create_tables.sql 파일을 리다이렉션해줘서 데이터베이스를 만들어주자.

자, 모든 설정이 끝났다.

```docker
service nginx reload
```

nginx를 리로드하고 로컬로 접속해보자.

[](https://127.0.0.1/phpmyadmin)

아, 이 설명을 빼먹었네.

phpMyadmin은 데이터베이스를 GUI(graphical user interface)로 편하게 관리하게 해주는 페이지이다. 따라서 이전에 wordpress를 얹을 때 사용했던, 사용자 ID와 사용자 PW를 입력하면 해당 사용자가 관리할 수 있는 권한이 있는 DB를 GUI로 편하게 관리할 수 있다.

여기서 설정한 ID와 PW는 joupark, joupark이다.

# 끝(?)

여기까지가 docker를 이용해서, 데비안 버스터 OS 이미지를 가져와서 컨테이너를 실행시키고 그 위에 nginx 웹서버를 구동시키고 동적 페이지 작성을 위해 php-fpm를 연결하고 openssl을 통해 인증서를 받고, 데이터베이스(mariaDB), wordpress, phpmyadmin을 설치해서 웹서버에서 구동시키는 과정을 진행했다.

그런데, 이렇게 긴 과정을 자동화해주는 Dockerfile이라는 파일이 있다.

그걸 이제 작성해서 위 과정을 자동화 해보자.

# Docker file

자, 위 과정의 모든 과정을 자동화 할 수 있도록 Docker file과 쉘 스크립트를 작성해보자.

먼저, Docker file

```docker
FROM	debian:buster
# set OS image

LABEL	maintainer="joupark@student.42seoul.kr"
# set image matadata

RUN		apt-get update && apt-get install -y \
		nginx \
		mariadb-server \
		php-mysql \
		php-mbstring \
		openssl \
		vim \
		wget \
		php7.3-fpm
# install what we need

COPY	./srcs/run.sh ./
COPY	./srcs/default ./tmp
COPY	./srcs/wp-config.php ./tmp
COPY	./srcs/config.inc.php ./tmp
COPY	./srcs/wpsql.sql ./tmp
# COPY file hostos to container

EXPOSE	80 443
# telling user to this port will used

CMD		bash run.sh
# runing shellscript that will starting build container
```

from, run, copy, expose, cmd 명령어가 사용되는데 각각 설명을 붙여보자.

- FROM [이름] : 베이스가 되는 OS 이미지를 설정
- LABEL [데이터] : 만들어진, 이미지에 메타데이터를 설정 위에서는 maintainer에 내 이메일을 넣어줬다.
- RUN [명령어] : 이미지를 생성할 때, 추가할 패키지를 설치하고 권한을 설정할 때 사용한다.
- COPY [파일] : 만들어진 이미지에 특정 파일을 추가해서 만들어질 수 있게 된다.
- EXPOSE [포트] : 만들어질 컨테이너와 호스트가 연결될 포트를 설정해준다.
- CMD [명령어] : 컨테이너가 시작됐을 때 실행할 스크립트 혹은 명령어를 설정해준다. 여기서는 우리가 넣어준 run.sh를 실행하게 된다.

즉, 위 dockerfile에서는 "debian:buster" 이미지를 베이스로 한 이미지를 만들 것이고,

메타데이터에 "maintainer="joupark@student.42seoul.kr"를 설정해주고,

이미지를 생성할 때 패키지를 "apt-get update && apt-get install -y \
		nginx \
		mariadb-server \
		php-mysql \
		php-mbstring \
		openssl \
		vim \
		wget \
		php7.3-fpm"

다음과 같이 추가해주고

nginx 설정파일인 "default" wordpress 설정파일인 "wp-config.php" 그리고 phpmyadmin 설정파일인  "config.inc.php" 그리고 wordpress 데이터베이스를 만들어주기 위해 따로 작성한 "wpsql.sql" 그리고 마지막으로 컨테이너가 실행될 때 실행될 쉘스크립트문 "run.sh"를 이미지에 추가해준다.

그 후에, 호스트와 연결될 포트를 80포트와 443포트를 설정해주고, 마지막으로 컨테이너가 생성될 때 실행될 run.sh를 설정해준다.

자, 다른 파일들은 이전에 다뤘지만 컨테이너가 생성될 때, 실행될 run.sh는 다루지 않았다.

### run.sh

```docker
#!/bin/sh

# set right
chmod 775 /run.sh
chown -R www-data:www-data /var/www/
chmod -R 755 /var/www/

# starting get certificate
openssl req -newkey rsa:4096 -days 365 -nodes -x509 -subj "/C=KR/ST=Seoul/L=Seoul/O=42Seoul/OU=joupark/CN=localhost" -keyout localhost.dev.key -out localhost.dev.crt
mv localhost.dev.crt etc/ssl/certs/
mv localhost.dev.key etc/ssl/private/
chmod 600 etc/ssl/certs/localhost.dev.crt etc/ssl/private/localhost.dev.key

# starting get wordpress
wget https://wordpress.org/latest.tar.gz
tar -xvf latest.tar.gz
mv wordpress/ var/www/html/
chown -R www-data:www-data /var/www/html/wordpress
cp /tmp/wp-config.php var/www/html/wordpress/wp-config.php

# run mysql
service mysql start
mysql < /tmp/wpsql.sql

# starting get phpMyadmin
wget https://files.phpmyadmin.net/phpMyAdmin/5.0.2/phpMyAdmin-5.0.2-all-languages.tar.gz
tar -xvf phpMyAdmin-5.0.2-all-languages.tar.gz
mv phpMyAdmin-5.0.2-all-languages phpmyadmin
mv phpmyadmin /var/www/html/
cp -rp var/www/html/phpmyadmin/config.sample.inc.php var/www/html/phpmyadmin/config.inc.php
cp /tmp/config.inc.php var/www/html/phpmyadmin/config.inc.php
mysql < /var/www/html/phpMyAdmin/sql/create_tables.sql

# edit nginx setting file
cp /tmp/default etc/nginx/sites-available/default

# run server
service php7.3-fpm start
service nginx start
service mysql restart

bash
```

각각의 명령어는 이미 위에서 다룬 부분이다.

다른 점이 있다면, 우리가 이미지에 dockerfile에서 COPY 명령어로 복사해준 설정파일들을 cp 명령어를 통해서 새로 만들어진 설정파일에 그대로 복사해주는 부분이 다르다.

# 이제 진짜 끝.

이미지를 만들 때 사용할 dockerfile과 이미지에 추가할 src 폴더 내의 파일들, 그리고 run.sh를 준비했다면 이제 진짜 자동화는 완료됐다. docker 명령어를 통해서 이미지를 만들고 그 이미지를 사용해 컨테이너를 만들어보자.

```docker
docker build -t ft_server .
```

도커파일과 src폴더가 있는 곳에서 위 명령어를 실행한다.

docker build 명령어는 이미지를 만들 때 사용하는 명령어다 -t 옵션은 이름을 설정할 때 사용한다.

즉, ft_server라는 이름을 가진 이미지를 현재 폴더에 있는(.) 파일들로 만든다는 뜻이다.

문제없이 이미지가 작성됐다면 이제 컨테이너를 만들고 컨테이너를 실행시켜보자.

```docker
docker run --name ft_con -it -p80:80 -p443:443 ft_server
```

자 이전에도 다룬 명령어다. ft_con이라는 이름을 가진 컨테이너 생성후 종료하지 않고, 80포트와 443포트가 연결된 ft_server이미지로 작성된 컨테이너라는 뜻이다.

위 두 명령어를 실행하고,

[http://localhost:80](http://localhost:80) 으로 접속했을 때 자동으로 [https://localhost:443](https://localhost:443으로) 으로 자동으로 리다이렉트 되면서 https 보안이 잘 되는지 확인하고, wordpress와 phpmyadmin이 작동되는지 확인한다.

그리고 마지막으로 데이터베이스에 우리가 만든 post나 다른 값들이 제대로 저장되는지 확인한다면, 성공한 것이다. 기뻐하고 보람을 느껴보자.

# 추가 ft_server

현재 docker 컨테이너 확인

```docker
docker ps -a
```

현재 존재하는 컨테이터 삭제

```docker
docker rm [컨테이너ID]
```

현재 존재하는 이미지 확인

```docker
docker images
```

현재 존재하는 이미지 삭제

```docker
docker rmi [이미지ID]
```

