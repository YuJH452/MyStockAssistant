# 프로세스 설명

- 모든 프로세스는 크롬으로 돌아갑니다. 크롬 없으면 에러납니다.


#### 1. 개별 보고서 다운로드
프로세스를 실행하면 stockrow.com에서 해당 종목코드를 검색해서 재무제표와 손익계산서(모두 영문)를 다운받습니다.
검색할 종목코드는 입력인수 str_inputCode에 값을 넣어주어야합니다.
실행테스트를 위해 기본적으로 마이크로소프트("msft")가 str_inputCode에 들어가있습니다. 

(프로세스 실행 후 생성되는 파일)
 재무제표 : (종목코드)_연월일_balancesheet.xlsx 
 손익계산서 : (종목코드)_연월일_income.xlsx
![개별보고서다운](https://user-images.githubusercontent.com/58212594/111160029-0c909280-85dd-11eb-8ef8-8a483f1a2018.gif)
![밸런스 인컴](https://user-images.githubusercontent.com/58212594/111162731-c852c180-85df-11eb-9089-f07aac324a06.gif)
#### 2. 개별 종목 분석 - 엑셀 필요
메인기능1 입니다.
프로세스를 실행하면 investing.com에서 해당 종목코드를 검색해서 기본적인 회사 분석 정보를 스크래핑해 엑셀템플릿에 붙여넣습니다.
검색할 종목코드는 입력인수 JongMok에다가 값을 넣어주어야 합니다.
실행 전 반드시 가장 마지막 엑티비티(copy file)에서 따옴표 부분의 경로를 자신이 저장하고자 하는 경로로 바꿔주세요.
따옴표 부분은 반드시 \로 끝나야 합니다.

프로세스가 완성 된 후에는 완성된 엑셀 파일이 지정한 경로에 저장됩니다.
OneDrive같이 일반 윈도우 폴더처럼 경로를 가지는 네트워크 드라이브를 사용하면 클라우드에 연동하는 것도 가능합니다.
원드라이브 이외에는 클라우드 연동을 테스트 하지 않았으므로 타 클라우드 드라이브에 저장하고자 하는 경우에는 안될 수 있습니다.

또한 본 프로세스는 타 프로세스나 UiPath App 등의 연동을 위해 아래와 같은 string 출력 인수들이 생성됩니다.

변수명		설명				결과 예시
Beadang		배당여부				(한다/안한다)
Pe		재무건전성 정도			(매우불량/불량/좋음)
YoYack		기술분석 종합 의견		(적극매도/매도/매수/적극매수)
str_conclusion	상기 3개 인수의 내용에 따라 변동	(매도 하세요/유지 하세요/매수 타이밍 입니다)

(프로세스 실행 후 생성되는 파일)
Analysis_(종목코드)_연월일.xlsx

![개별종목분석](https://user-images.githubusercontent.com/58212594/111160145-2b8f2480-85dd-11eb-9753-0488ba8a8c79.gif)
![개별종목분석 에셀](https://user-images.githubusercontent.com/58212594/111162711-c2f57700-85df-11eb-8ed1-6c6ed8f40789.gif)
#### 3. 구성종목 일괄 분석 - 엑셀 필요
메인기능2 입니다.
프로세스를 실행하면 프로젝트 내 codeset 엑셀파일로부터 종목 코드 및 url를 세트로 크롬 브라우저에서 각 url을 순차적으로 방문하여 필요한 내용을 스크래핑해옵니다.
입력인수 str_Selected_Indices에서 분석하고 싶은 그룹 번호를 지정해주세요.
현재 지원하는 종목 그룹은 아래 3가지밖에 없습니다.
(1: KOSPI 50, 2: DOW 30, 3: 국내 코로나 테마주)

추후에는 DB 업데이트 느낌의 프로세스를 따로 추가하여 거기서 작성된 종목코드 및 url 세트를 읽어오는 것으로 수정해볼 생각이 있으나 언제 만들지는 모릅니다.

스크래핑이 완료되면 엑셀템플릿에 붙여 넣은 후 완성된 엑셀파일을 PC에 저장합니다.
2번 프로세스와 마찬가지로 실행 전에 맨 마지막 copy file 엑티비티에서 따옴표 부분의 경로를 자신이 저장하고자하는 폴더 경로로 바꿔주세요.
역시 따옴표 부분은 반드시 \로 끝나야 합니다.

2번과 마찬가지로 경로명을 원드라이브 경로로 하면 클라우드 연동이 가능합니다.
원드라이브 말고 다른 클라우드 드라이브와의 테스트도 역시 안해봤습니다.
엑셀파일만 생성하고 별도의 출력인수는 없습니다.

(프로세스 실행 후 생성되는 파일)
Analysis_(종목코드)_연월일.xlsx

![일괄분석](https://user-images.githubusercontent.com/58212594/111160290-4bbee380-85dd-11eb-9928-d01e1a5634c1.gif)
![일괄분석 엑셀](https://user-images.githubusercontent.com/58212594/111162749-cbe64880-85df-11eb-9790-d361cae955fb.gif)
#### 4. 환율 끌어오기
investing.com의 환율 위젯에서 환율정보 끌어올 뿐입니다. 
엑셀파일도 생성하지 않으며 출력 인수 하나 있습니다.
	타입: 	DataTable
	인수명:	Dt_hwan


각 프로세스에서 산출되는 엑셀파일이 어떤지 예시를 보고싶은 경우에는 엑셀 결과 샘플 폴더에서 확인하세요.
UiPath App과 연동 시켰을 경우에는 어떤식으로 활용할 수 있는지 예시를 보고싶은 경우에는 영상자료 폴더에서 동영상을 확인하세요
![환율업데이트](https://user-images.githubusercontent.com/58212594/111160277-495c8980-85dd-11eb-9d53-5aa5f75d9d76.gif)




