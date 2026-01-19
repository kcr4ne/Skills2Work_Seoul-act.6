# 취약점 분석 방법론 (공격)

## 웹 취약점 분석 실습의 시작
- 보안 관점 전환<br>
  사용자 -> 공격자의 관점으로 전환
- 개념 -> 실습<br>
  개념 이론 후 바로 실습으로 연결
- 도구 적응 훈련<br>
  Burp Suite 환경 세팅 및 실습

## 웹 취약점 분석, 무엇일까?
**웹 서비스의 동작 구조를 이해하고 그 안에 숨겨진 보안 취약점을 식별&이용하는 과정**<br>
- ### 공격자 관점 | Attacker's Perspective
  보안 분석은 공격자가 시스템을 어떻게 이용할 수 있는지 상상하는 것에서 시작
- ### 논리적 사고 | Logical Exploitation
  로직의 허점을 찾아내는 문제 해결 중심 사고
- ### 웹 구조 이해 | UNderstanding the Web
  요청 -> 응답 -> 세션 -> 권한 흐름 등, 웹 애플리케이션의 구조를 이해하는 것이 공격 포인트를 발견하는 열쇠

## 정적 분석 VS 동적 분석
- ### 정적 분석
  코드, 파일, 설정 등 **실행 전의 정보를 분석**하여 취약점을 식별하는 방식
  - 파일 기반 분석
  - 구조적 로직 분석
  - 보안 규칙 검토
- ### 동적 분석
  웹 애플리케이션의 요청/응답을 조작하면서 실행 중인 시스템의 반응을 분석하는 방식
  - 요청 조작
  - 응답 분석
  - 실습 도구 활용

## 실제 공격 흐름과 분석 방법론 적용 단계
1. 정보 수집 : 타겟 웹 사이트의 구조, 입력 포인트, 서버 정보 등 탐색
2. 취약점 식별 : 수집한 정보를 바탕으로 입력 포인트, 파라미터, 인증 흐름 등을 분석하고 취약해 보이는 부분을 식별
3. 분석 및 익스플로잇 : 취약점을 실제로 공격
4. 권한 상승 및 피봇팅 : 획득한 세션 쿠키, 관리자 힌트 등을 이용해 권한 상승 또는 내부 자산 접근을 시도
5. 후속 조치 : 정보 탈취, 백도어 삽입, 웹 쉘 설치, 로그 삭제 등의 행동을 모의하여 전체 흐름을 마무리

## 핵심 웹 취약점
- #### SQL Injection
  입력 값 조작으로 쿼리를 변조해 DB 정보를 탈취
- #### 인증 우회
  세션, 토큰, 힌트를 이용해 로그인 절차를 우회
- #### IDOR
  URL/파라미터를 바꿔 타인 리소스에 접근
- #### 정보 공개
  에러 메시지, 백업 파일, 서버 정보 노출
- #### 접근 제어 우회
  권한 없이 관리자 메뉴 접근 가능
- #### OWASP TOP 10 연계
  - A01 : Broken Access Control
  - A03 : Injection
  - A05 : Security Misconfiguration 등과 연계

## 웹 보안 분석의 시작 Burp Suite란?
**웹 보안에서 가장 많이 사용되는 도구, Burp Suite는 공격자의 시선으로 요청과 응답은 분석할 수 있는 핵심 툴**<br>

## Burp Suite의 필요성과 역할
- 웹 보안 전문가의 필수 도구
- 클라이언트 - 서버 통신 가시화
- 공격 자동화와 반복 테스트

## 정적 분석, 어떻게 접근할까?
**웹 애플리케이션의 실행 없이도, 노출된 코드,  파일, 경로 등 사전 정보를 바탕으로 취약점을 분석할 수 있음**<br>
- #### 파일 노출 기반 분석
  서버가 의도치 않게 노출한 백업, 설정, 코드 파일을 정벅 분석으로 탐지하는 실습
- #### 내부 정보 확인 흐름
  소스 코드 구조나 설정 내용을 통해 시스템 동작 방식과 잠재적 위험 요소를 파악할 수 있음
- #### 사고 중심 접근법
  공격자의 관점에서 '정보 노출 -> 악용' 흐름을 역으로 추론하는 분석 훈련

## 정적 분석 실습

## 1. Lab-infoleak-via-backup-files
<img width="1919" height="1199" alt="image" src="https://github.com/user-attachments/assets/9ad1273c-9aa8-4b21-87c1-0ba5f24abb04" />
<br><br>
아래는 위 사이트의 구조를 Burp Suite를 사용해 확인한 사진이다.<br><br>
<img width="453" height="302" alt="image" src="https://github.com/user-attachments/assets/9d4b226b-8c54-4041-811e-ec1129647815" />
<br><br>
이 웹 사이트의 URL 주소 뒤에 robots.txt를 붙여보면 아래와 같이 나타나게 된다.<br><br>
<img width="865" height="166" alt="image" src="https://github.com/user-attachments/assets/de574b8f-a2c5-4871-8ab2-8ac8a09dcdcf" />
<br><br>
모든 유저 에이전트에 대하여 /backup에 대한 접근을 막고 있다.<br>
하지만, robots.txt는 봇의 크롤링을 방지하기 위해 봇에게 알려주는 역할만 하기에 직접 접근할 수 있다.<br>
/backup으로 이동하면 웹 사이트가 아래와 같이 나타난다.
<br><br>
<img width="1919" height="1199" alt="image" src="https://github.com/user-attachments/assets/fc71e817-ccdd-4e6d-bbca-2ca125f80e9e" />
<br><br>
하이퍼텍스트를 클릭해 이동하면 아래와 같은 이미지가 나오고 이 중에 플래그 값이 들어있는 것을 확인할 수 있다.
<br><br>
<img width="1919" height="1199" alt="image" src="https://github.com/user-attachments/assets/31472f52-7d79-4e89-83f8-6634bdca80f2" />
<br><br>

## 동적 분석, 어떻게 접근할까?
**웹 애플리케이션이 실제 동작할 때 발생하는 요청과 응답 흐름을 실시간으로 관찰하고 조작하는 방식**<br>
- #### 입력 값 기반 공격 분석
- #### 서버 응답 흐름 추적
- #### 공격 체인 이해 훈련

## 2. Lab-login-bypass
<img width="1124" height="440" alt="image" src="https://github.com/user-attachments/assets/d24edcc6-9193-434a-a9fa-d29b06834e2c" />
<br><br>
**administrator** 라는 계정을 사용해 SQLi 인증 우회를 해야한다.<br>
메인 화면의 우측에 있는 `My account`를 클릭해 나온 화면이다.<br><br>
<img width="1919" height="1092" alt="image" src="https://github.com/user-attachments/assets/ca0135fe-1324-49a0-bc54-c628c6a4a5dc" />
<br><br>
간단한 SQL Injection 쿼리를 삽입했다.<br>
유저 네임 필드에 입력 값으로 `administrator`를 넣고 그 뒤는 주석 처리했으니 패스워드는 아무거나 입력했다.<br><br>
<img width="1369" height="700" alt="image" src="https://github.com/user-attachments/assets/987995ba-b4e6-47bf-9f34-3e4acabd28c5" />
<br><br>
<img width="1241" height="671" alt="image" src="https://github.com/user-attachments/assets/21302c6c-dd2c-43f0-8a5f-76429abeaa33" />
<br><br>

## 3. Stored XSS into HTML context with nothing encoded
<img width="1086" height="461" alt="image" src="https://github.com/user-attachments/assets/6f50b393-696b-4eae-9916-439a311a360b" />
<br><br>
alert 만 실행하면 되는 간단한 문제이다.<br>
입력 필드를 찾고 입력 값으로

```html
<script>alert("sexy")</script>
```

을 입력하면 된다.<br><br>
<img width="1919" height="1199" alt="image" src="https://github.com/user-attachments/assets/7057a44b-32f2-4a77-b036-1a028b30d64f" />
<br><br>
입력 필드를 찾았고 이곳에 아까의 페이로드를 입력하면 될 것이다.<br><br>
<img width="1197" height="836" alt="image" src="https://github.com/user-attachments/assets/8d155994-3c81-4df0-87d0-738737b4bf52" />
<br><br>
<img width="1317" height="390" alt="image" src="https://github.com/user-attachments/assets/06bf8b0d-64bf-4e5c-9a85-621b6f8f0a6f" />
<br><br>

## 4. OS command injection, simple case
<img width="1274" height="561" alt="image" src="https://github.com/user-attachments/assets/55247105-2268-41c0-a62e-c7bc037d14bc" />
<br><br>
whoami 명령을 실행시키는 것이 목표이다.<br>
XSS와 마찬가지로 입력 필드에 삽입하면 될 것이다.<br><br>
<img width="1919" height="1199" alt="image" src="https://github.com/user-attachments/assets/03fc4a6f-9458-40bb-873a-dadc3138dd29" />
<br><br>
위 사진을 보면 입력 필드가 없다.<br>
하지만, 개발자 도구를 사용해 페이로드를 삽입하는 방법이 있다.<br><br>
<img width="1895" height="666" alt="image" src="https://github.com/user-attachments/assets/d359d4fd-0226-40fc-8116-b4b180dafa87" />
<br><br>
<img width="441" height="114" alt="image" src="https://github.com/user-attachments/assets/d3dfa6aa-6439-479d-aafa-4f7f44504033" />
<br><br>
<img width="783" height="200" alt="image" src="https://github.com/user-attachments/assets/f3bd620f-d72e-48e7-b093-945a11802fd8" />
<br><br>
개발자 도구로 조작 후 Check stock을 누르니 값이 익스플로잇에 성공했다.<br>
아래는 Burp Suite를 활용해 익스플로잇 하는 방법이다.<br><br>
<img width="237" height="124" alt="image" src="https://github.com/user-attachments/assets/8a94e3c9-b5c7-4c4d-ac7d-1c73df91eb12" />
<br><br>
인터셉트를 켜서 요청을 가로챌 준비를 한다.<br><br>
<img width="465" height="121" alt="image" src="https://github.com/user-attachments/assets/f72b80e2-0a18-4acc-be7e-416475eaddc5" />
<br><br>
Check Stock을 눌러 요청을 보낸다.<br>
이러면 Burp Suite에서 요청을 가로채 내가 서버로 보낸 요청을 보여준다.<br><br>
<img width="1329" height="846" alt="image" src="https://github.com/user-attachments/assets/8e819838-dacb-4a95-a875-bf63f9cffd97" />
<br><br>
20번째 줄에 서버로 전송하는 데이터가 있다.<br><br>
<img width="469" height="169" alt="image" src="https://github.com/user-attachments/assets/0ecfc2ea-f948-4327-8866-0f11a395998f" />
<br><br>
20번째 줄에 | whoami 를 붙여준다.<br><br>
<img width="257" height="68" alt="image" src="https://github.com/user-attachments/assets/d445be8b-84ae-484d-a8ba-efd5e6502ca1" />
<br><br>
forward를 클릭해 변조한 요청을 서버로 보낸다.<br><br>
<img width="307" height="109" alt="image" src="https://github.com/user-attachments/assets/e11f4936-e9e5-4d75-bd9b-392e43c36dfd" />
<br><br>

# 디지털 포렌식 실습 (방어)

## 1. 디지털 포렌식 개요
### **접촉하는 두 물체 간에는 반드시 흔적이 남는다."**
- Locard's exchange principle | 로카르드 교환 법칙

## 디지털 포렌식 흔적을 탐구하다
- ### 절차
  사전 준비-증거 수집-포장 및 이송-조사 분석-정밀검토-보고서 작성 총 6개의 단계를 거치며 데이터를 디지털 증거로 가공한다
- ### 대상
  디지털 포렌식의 대상은 디지털 흔적을 남길 수 있는 모든 전자기기를 포함한다
- ### 발전 방향
  - 침해 사고 포렌식 : 침해 사고의 원인과 영향을 분석
  - 악성 코드 포렌식 : 악성 코드의 흔적을 분석
  - 감사 포렌식 : 기업의 내부 감사를 위한 분석
  - 회계 포렌식 : 회계 데이터의 은닉과 조직을 판단

## Digital Forensics?
**디지털 기기를 매채게로 하여 발생한 특정 행위의 사실 관계를 법정에서 규명하고 증명하기 위한 절차와 방법**
- 주요 분야
  컴퓨터, 모바일, 네트워크, 악성 코드, 로그 등 디지털 기기에서 증거를 추출하고 분석
- 중요성
  사이버 범죄를 수사하고 증거를 확보하거나, 보안 사고 발생 시 원인을 파악하고 피해를 최소화 시킴

## 디지털 포렌식 절차
- 사전 준비 단계
  사건이 일어나기 전에 행동하는 것으로 증거 수집 단계에서 사용해야 할 도구, 장비 등을 준비 함으로써 도구와 기술의 한계 등 교육과 물리적인 준비를 하는 단계
- 증거 수집 단계
  사건 현장에서 조사해야 하는 대상을 수집하는 단계. 영장 집행에 따른 압수 절차와 수집 대상의 구체화 등을 주의 해야함, 연계 보관성(COC) 유지를 위해 이 단계부터 모든 것을 기록 해야함
- 포장 및 이송 단계
  디지털 입수물의 원본 혹은, 복제본을 포장하여 이송하는 과정. 외부의 요인으로 인한 변조를 막기 위해 여러 조치를 통한 안전 보관 및 이송을 위한 단계
- 조사 분석
  수집한 디지털 기기들을 분석하는 과정. 메모리 분석, 파일 복구, 로그 분석 등의 증거 분석 기술을 활용하여 분석 시 원본 파일을 무결성을 위해 사본 이용을 권장하기도 함
- 정밀 검토
  각 단계의 검증을 비롯하여 분석 결과가 정확한지 검토하는 단계. 분석 결과는 법정 증거로 채택될 수 있기 때문에 재현 가능성에 대해 확인한다.
- 보고서 작성
  정밀 검토를 마친 결과를 바탕으로 문서화를 위한 보고서 작성의 과정. 보고서 작성 시 절차 연속성 과정을 육하원칙에 따라 작성한다. 비전공자 혹은 제3자의 전문가 등의 검증이 가능하도록 기술적인 용어를 최소화 한다.

## 디지털 포렌식 도구 소개
- 종합 포렌식 도구 : Encase, FTK, X-Ways, Autopsy 등
- 이미징 도구 : FTK Imager, Encase Imager, Ubuntu, X-WaysImager 등
- 메모리 분석 : Redline, Volatility, Memorize & Audit Viewer 등
- 타임 라인 분석 : Log2timeline, plaso 등
- 레지스트리 분석 : REGA, Registry Recon 등
- 로그 분석 : Event Log Explorer, Log Parser, NTFS Log Tracker 등

## 디지털 포렌식의 위치
- #### 예방 (방어)
  공격이 일어나기 전에 취약점을 줄이고 보안을 강화하는 단계
- #### 침해 (공격)
  해커나 악성 코드에 의해 시스템이 침입당하거나 손상되는 순간
- #### **대응 (포렌식)**
  공격 후에 무슨 일이 있었는지 분석하고 증거를 확보하는 과정
- #### 복구
  손상된 시스템을 원래 상태로 되돌리고, 재발 방지책을 세우는 단계

## 공격자의 흔적
- #### 악성 코드 실행
  사용자가 악성 프로그램을 실행하면, Windows는 해당 실행 정보를 자동으로 기록함
- #### 관리자 계정 탈취
  사용자가 관리자 계정으로 로그인하거나 해커가 탈취한 경우, Windows는 인증 및 로그인 정보를 자동으로 기록
- #### 파일 복사 / 삭제
  중요 파일을 복사하거나 삭제하면, Windows는 파일 접근 및 변경 기록을 여러 위치에 남김
- #### 로그 지우기 시도
  공격자가 자신의 흔적을 숨기기 위해 로그를 지워도, 로그 삭제 시도 자체가 시스템에 기록됨

## IR ( Incident Response) 포렌식
- ## 공격자 활동 로그 추적
  누가 로그인 했는지, 어떤 명령을 실행하였는지 로그로 파악
- ## 이상 징후 탐지
  새벽 시간대 로그인, 평소에 없던 파일 이동 등 이상 행위 발견
- ## 데이터 유출 분석
  USB 사용 기록, 클라우드 전송 흔적 등 외부로 빠져나간 정보 추적

## 포렌식 Artifacts
**운영체제나 애플리케이션을 사용하면서 생성되는 흔적**<br>

#### 아티펙트의 구분
- 생성 증거
  시스템이나 애플리케이션이 자동으로 생성한 데이터
- 보관 증거
  사람의 사상이나 감정을 표현하기 위해 작성한 데이터

## 아티펙트의 종류
- LNK File : 사용자가 [바로 가기 생성]을 하였을 때 생성
- Dumplist : 사용자가 자주 사용하너 최근에 사용한 문서 또는 프로그램을 관리
- Shellbags : 사용자가 로컬, 네트워크 및 이동식 저장장치에서 접근한 폴더 정보를 Shellbags 레지스트리에 기록
- Open / Save MRU : Windows 탐색기 공용 대화 상자를 통해 최근에 열거나 저장된 파일 정보 저장
- Recent Files : 사용자가 Windows 탐색기를 통해서 열거나 실행한 파일, 폴더 정보 저장
- User Assist : 최근에 실행한 프로그램 목록, 실행 횟수, 마지막 실행 시간 등 기록
- Prefetch : 응용 프로그램의 이름, 실행 횟수, 마지막 실행 시간, 볼륨 정보 등 저장
- ShimCache : 모든 실행 파일의 경로, 크기, 마지막 수정시간, 마지막 실행 시간 등의 정보를 저장
- AmCache : 응용 프로그램의 실행 경로, 최초 실행 시간, 삭제 시간 정보 등 저장
- MUICache : 다중 언어를 지원하기 위해 프로그램 이름을 캐쉬화 하는 기능
- Event Logs : Windows 운용 과정에서 발생하는 특정 이벤트를 종합적이고 체계적으로 로그 기록

## 로그 분석
**로그는 방대한 양의 데이터를 포함**<br>
- 필터링
- 추출
- 정렬 / 중복 제거

## 로그 분석 실습

## 분석 방법
- 원본 편집 금지
- 정보 탐색 가능

# 포렌식 CTF
> 2등(2400점, 16/17)

로그 파일의 크기가 4.3gb라 ctf 시작 후 한동안 아무도 로그 파일을 열지 못했다.<br>
이대로 가다간 파일을 열지도 못하고 ctf를 끝낼 것 같아 최후의 수단을 쓰기로 했다.<br>
(주)지오유에 가서 알아낸 Antigravity라는 IDE에 WSL을 연결하고 문제 파일을 다운받은 뒤,<br><br>
<img width="319" height="154" alt="image" src="https://github.com/user-attachments/assets/4b0ed873-43fa-486a-8529-5ff6644c3a7c" />
<br><br>
<img width="800" height="301" alt="image" src="https://github.com/user-attachments/assets/29618c2e-55f8-4929-9944-c15160eb7b52" />
<img width="425" height="1020" alt="image" src="https://github.com/user-attachments/assets/4a6c7772-6ca8-4980-b286-22ad6b44cf65" />
<br><br>
현대 기술의 집합체인 Gemini를 사용해 풀었다.<br>
파일의 크기가 커 gemini도 읽어내는데 시간이 오래 걸려 ctf 시작 후 30분 만에 문제를 풀어내기 시작했다.<br>
리스트 형식으로 한번에 여러 문제의 정답을 주었기에 그대로 입력하느라 연속적으로 정답을 맞추는 모습을 연출하기도 하였다.<br>
500점인 보너스 문제만 못 푼게 아쉬웠고, 이 보너스 문제 때문에 2등을 뺏길 뻔 했지만 시간 제한으로 ctf가 종료되어 2등을 유지했다.<br>
여러 노트북들이 로그 파일의 크기를 못 이기고 터져나가는 모습을 보니, 공유기를 하나 더 사서 데스크탑에 원격으로 접속할 수 있는 환경을 마련해야 할 것 같다는 생각이 들었다.<br>

## 정답
```md
🏷️ 100점
100-0 : 박우진
100-1 : sqlmap/1.5.1#stable
100-2 : 198.51.100.200
100-3 : report_final.php
100-4 : 192.0.2.77
100-5 : whoami
100-6 : 200
100-7 : 1141625
100-8 : 198.51.100.23
100-9 : 192.168.0.219
🏷️ 200점
200-1 : %252F
200-2 : stolen_files.tar.gz
200-3 : 58765432
200-4 : 31337
🏷️ 300점
300-1 : /var/www/html/.env
300-2 : rm /var/www/html/uploads/assignments/20241123/stolen_files.tar.gz; rm /var/www/html/uploads/assignments/20241123/report_final.php
🎁 BONUS (500점) - 못 풀었음
500-1 : Skills2WorkSkills2WorkSkills2WorkSkills2WorkSkills2WorkSkills2WorkSkills2Work.php
```
