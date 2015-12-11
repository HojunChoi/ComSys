***********************


Secret Paper
======

***********************

Documents
-------------


### Contents

 *Procedure*|  *Content*|
-------- | ---
01 | 문제정의|
02 | 해결방안  |
03 | 유사사례 |
04 | 시스템 설계|
05 | 핵심 콤포넌트 상세 설계 |
06  | 구현에 필요한 기술 및 개발환경에 대한 조사|
***********************


> **<문제정의>**
> 
 우리 팀은 기업들을 위한 프로그램을 만들었다.  과거에는 회사 내에서 프로그램을 개발하여 정보 유출을 방지하였다. 현재는 중소기업은 Cloudium, Secure Disk 등을 이용하며 대기업은 회사 자체 프로그램으로 정보유출을 방지하고 있다. 그리고 미래는 우리가 개발할 Secret paper를 미래 시스템으로 설정하였다. 브레인스토밍을 통하여 현재 상용되고 있는 프로그램의 불편한 점을 발견할 수 있었다. 첫 번째로는 과거에는 해커 등 외부의 침입으로 인한 정보유출의 사례가 주를 이루었지만 현재는 해당 기업 내부 조직원들에 의한 정보유출이 심각성이 커지고 있다는 점이다. 두 번째로는 기업 내 주요정보가 유출될 수 있는 수단이 늘어났다는 점이다. 그래서 그 문제점을 해결 할 방안을 찾다가 기업 내 내부정보 유출방지를 위한 솔루션이 없다는 문제를 인식하였다.
![](http://postfiles10.naver.net/20151211_25/k120cj_1449803459925Nezn2_PNG/1.png?type=w3)


***********************

> 
**<해결방안>**
>1) 모든 문서를 웹에서 작업 및 관리
> 2) 문서동시 편집 기능 제공 (협업)
> 3) 보안 등급 설정 (S등급/N등급)
> 4) S등급의 파일 읽기/수정/인쇄는 메시지를 통한 승인
> 5) 열람할 때 누가 언제 무엇을 열람하는지, 인쇄 여부 등 문서에 대한 모든 로그를 DB에 저장 


*********************

>**<유사사례 : Cloudium, SecureDisk>**
1) 공통점 : 
① 서버 중앙화
② 외장하드에 직접 저장, 화면 캡처 불가능
2) 차별점 :
① 문서마다 보안등급을 지정하여 등급이 높은 문서를 접근하기 위해서는 작성자의 허가가 필요하다.
② Ms Office, 한글 Office를 실행하는 것이 아닌 구글의 docs처럼 웹으로 개발한다.
③ 협력업체와 프로젝트를 진행할 경우를 대비해 문서 동시 편집기능다.도 제공한다. 


***********************
> 
**<시스템 설계> **

> 1) User Interface
> ![](http://postfiles7.naver.net/20151211_22/k120cj_1449804357623NTkcj_PNG/2.png?type=w3)

> 2) 전체적인 시스템

>![](http://postfiles6.naver.net/20151211_213/k120cj_1449804561645jq299_PNG/3.png?type=w3)


>3) 새 문서 및 보안등급 설정
>![](http://postfiles13.naver.net/20151211_268/k120cj_1449804745117h41Ki_PNG/4.png?type=w3)


>4) N등급 문서 작업
>                                 ![](http://postfiles4.naver.net/20151211_83/k120cj_1449804838970MU52M_PNG/5.png?type=w3)


>5) S등급 문서 작업
>                                 ![](http://postfiles15.naver.net/20151211_142/k120cj_1449804891435Gc72J_PNG/6.png?type=w3)


>6) 문서 저장
>                                 ![](http://postfiles8.naver.net/20151211_55/k120cj_1449804950157gKaAL_PNG/7.png?type=w3)


>7) DB 관계도
>                                 ![](http://postfiles7.naver.net/20151211_294/k120cj_1449805045882Nr7vG_PNG/8.png?type=w3)



***************
**<핵심 콤포넌트 상세설계> **


|              함수명    | 매개변수                |기능         |
 ----------------- | ---------------------------- | ------------------
| void SdocCall() | int docnum : 문서번호, int enum : 작성자사원번호,  int work : 작업종류| 요청자의 브라우저에서 S등급 문서 열기/ 수정/ 인쇄 요청하는 함수 호출|
| void InsertLog()  | int enum : 요청자사원번호 int docnum , int work , int time : 작업시간           |웹 서버에서 DB에 로그 기록하는 함수 호출 |
| int SendMessage() |int docnum : 문서번호, int enum : 작성자 번호, int work : 작업종류|웹 서버에서 작성자에게 메시지를 전송하는 함수 호출|
| void SendPermission() |int docnum, int enum, int permission : 승인/거부|작성자의 브라우저에서 허가 / 거부 메시지를 전송하는 함수 호출|
|void SendReject() |int docnum : 문서번호, int enum : 작성자 번호,  int work : 작업종류|거부일 경우 웹 서버가 사원의 브라우저에 거절 메시지를 전송하는 함수 호출|
| void GetDoc() |int docnum|승인일 경우 웹 서버가 파일서버에서 문서를 로드하여 사원의 브라우저에 전송하는 함수 호출|
|void SendDoc()|int enum|승인일 경우 웹 서버가 파일서버에서 문서를 로드하여 사원의로드하여 사원의 브라우저에 전송하는 함수 호출|


```C,C++
//[요청자 브라우저]
SdocCall(1000, 2013136150, "PRINT"); 			// 1. S등급 문서 열기 요청
```


```Web Server
//[Web Server]
InsertLog(2013136149, 1000, "PRINT", "2015-12-10 23:48:47") // 2. DB에 로그 기록
int i = SendMessage(1000, 2013136150, "PRINT");	// 3. 작성자에게 메세지 전송
```

```C,C++
//[작성자 브라우저]
SendPermission(1000, 2013136149,, "Approval");	// 4. 웹서버에 승인/거부 메세지를 전송
```

```Web Server
//[Web Server]
if ( i == 0 )
	SendReject();				// 5.1 거절하는 함수 
else if ( i == 1 ) {
	GetDoc(1000);				// 5.2 파일 서버에서 문서를 Get
	SendDoc(2013136149);			// 5.2 요청자의 브라우저에 Send
}
```

```DB
//[DB]
create Table EMP(   					 // 사원 테이블
	Enum number(8) not null,
	Ename varchar(50) not null,
	Position varchar(10) not null,
	Rnum varchar(13) not null,
	Phone varchar(30) not null,
	Email varchar(40) null,
	ID varchar(20) not null,
	PW varchar(20) not null,
	Primary key(Enum) );

create Table Log(					//로그 테이블
	Enum number(8) not null,
	Dnum varchar(10) not null,
	Work varchar(20) not null,
	Worktime date_time datetime not null,
	FOREIGN KEY (`Enum`) REFERENCES `EMP` (`Enum`) );

create Table Document(					//문서 테이블
	Dnum number(10) not null,
	Dname varchar(50) not null,
	SecurityLevel varchar(10) not null,
	Primary Key(Dnum) ) ;
```
> 1) 개발환경
>                                                                   ![](http://postfiles4.naver.net/20151211_51/k120cj_1449805492516pHBk7_PNG/9.png?type=w3)


> 2) 구현에 필요한 기술1
>                                                                              ![](http://postfiles11.naver.net/20151211_266/k120cj_1449805572942vVAhq_PNG/10.png?type=w3)
>3) 구현에 필요한 기술2
>                                                                              ![](http://postfiles4.naver.net/20151211_67/k120cj_1449806090990sLwd5_PNG/noname01.png?type=w3)