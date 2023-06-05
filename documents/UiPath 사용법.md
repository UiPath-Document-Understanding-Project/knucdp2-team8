# UiPath 사용법

# UiPath 사용법

UiPath은 RPA(Robotic Process Automation) 툴로, 비즈니스 프로세스 자동화를 위한 다양한 기능을 제공합니다. UiPath을 사용하기 위해서는 다음과 같은 단계가 필요합니다.

1. UiPath Studio 설치
2. 프로젝트 생성 및 설정
3. 워크플로우 디자인
4. 디버깅 및 실행

## 1. UiPath Studio 설치

UiPath Studio는 UiPath의 메인 개발 도구입니다. UiPath Studio를 설치하려면 다음 단계를 따르십시오.

1. UiPath 공식 웹 사이트에서 UiPath Studio를 다운로드합니다.
    1. [https://download.uipath.com/UiPathStudio.msi](https://download.uipath.com/UiPathStudio.msi) 다운로드 후 설치 
2. UiPath Studio 설치 파일을 실행합니다.
    1. Enterprise 선택 하고 모든 사용자 옵션 선택 
3. 지침에 따라 설치를 완료합니다.

## 2. 프로젝트 생성 및 설정

UiPath Studio에서 프로젝트를 생성하려면 다음 단계를 따르십시오.

1. UiPath Studio를 엽니다.
2. "새 프로젝트" 버튼을 클릭합니다.
3. 프로젝트 이름을 입력하고 프로젝트 위치를 선택합니다.
4. 프로젝트 설정을 구성합니다.

## 3. 워크플로우 디자인

UiPath Studio에서 워크플로우를 디자인하려면 다음 단계를 따르십시오.

우리가 만들 결과물을 테스트 할 수 있는 샘플 프로젝트 공유 예정이며, 공유된 프로젝트에서 개발한 액티비티(Visual Studio 2022, C#)를 적용 함 

## 4. 디버깅 및 실행

UiPath Studio에서 디버깅 및 실행을 위해서는 다음 단계를 따르십시오.

1. UiPath Studio에서 프로젝트를 엽니다.
2. 디버그 모드로 설정하여 워크플로우를 디버깅합니다.
3. 워크플로우를 실행합니다.

이상입니다. 위의 단계를 따라 UiPath을 사용하여 비즈니스 프로세스를 자동화할 수 있습니다.

## 5. UiPath Orchestrator를 활용한 프로젝트 관리

UiPath Orchestrator는 UiPath의 웹 기반 관리 툴로, 프로젝트 관리, 로봇 관리, 스케줄링 등 다양한 기능을 제공합니다. UiPath Orchestrator를 사용하여 프로젝트를 관리하려면 다음과 같은 단계가 필요합니다.

1. UiPath Orchestrator 설치 및 설정
2. 프로젝트 등록 및 배포
3. 로봇 등록 및 할당
4. 스케줄링 및 모니터링

이상입니다. 위의 단계를 따라 UiPath을 사용하여 비즈니스 프로세스를 자동화하고 UiPath Orchestrator를 활용하여 프로젝트를 관리할 수 있습니다.

## 샘플 프로젝트 활용 방법

1. UiPath Studio 에서 [github.com/](https://github.com/javaos74/KNUDU.git)javaos74/KNUDU.git 에서 프로젝트는 내려 받는다 

![Untitled](UiPath%20%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8%207991afc9542b47e6b02976a6e32eb988/Untitled.png)

1. UploadData 폴더에 있는 워크플로우를 실행 한다. 파일 오픈 후 현재 파일 실행(디버깅)

![Untitled](UiPath%20%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8%207991afc9542b47e6b02976a6e32eb988/Untitled%201.png)

![Untitled](UiPath%20%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8%207991afc9542b47e6b02976a6e32eb988/Untitled%202.png)

1. 실행하고 나면 Queue에 데이터 업로드 됨 
2. 업로드된 문서를 처리하는 부분은 Main-ActionCenter.xaml 실행 ( 더블클릭 후 실행 버턴 누르기)

![Untitled](UiPath%20%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8%207991afc9542b47e6b02976a6e32eb988/Untitled%203.png)

1. 실행 이후 다시 로그 메세지에 아래와 같이 나오면 [cloud.uipath.com](http://cloud.uipath.com) 에서 Action 메뉴로 진입 함 

![Untitled](UiPath%20%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8%207991afc9542b47e6b02976a6e32eb988/Untitled%204.png)

![ ](UiPath%20%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8%207991afc9542b47e6b02976a6e32eb988/Untitled%205.png)

 

1. 액션센터에서 조치할 액션을 선택 함 

![Untitled](UiPath%20%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8%207991afc9542b47e6b02976a6e32eb988/Untitled%206.png)

1. 문서 타입을 결정 해 줌 ( 샘플은 전부 invoice 문서임) 

![Untitled](UiPath%20%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8%207991afc9542b47e6b02976a6e32eb988/Untitled%207.png)

![Untitled](UiPath%20%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8%207991afc9542b47e6b02976a6e32eb988/Untitled%208.png)

1. 완료 후 UiPath Studio에서 재시작 버튼 누름

![Untitled](UiPath%20%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8%207991afc9542b47e6b02976a6e32eb988/Untitled%209.png)

1. 문서 추출을 시도하면서 동일하게 사용자 검증을 기다리게 됨, 액션센터 메뉴로 가서 문서를 검증해줌 

![Untitled](UiPath%20%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8%207991afc9542b47e6b02976a6e32eb988/Untitled%2010.png)

1. 프로젝트 Data 폴더에 추출된 파일을 확인 함 

![Untitled](UiPath%20%E1%84%89%E1%85%A1%E1%84%8B%E1%85%AD%E1%86%BC%E1%84%87%E1%85%A5%E1%86%B8%207991afc9542b47e6b02976a6e32eb988/Untitled%2011.png)