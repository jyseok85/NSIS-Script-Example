# NSIS-Script-Example

; nsis의 경우 주석이 ; 이다.

;다른 프로그램으로 스크립트를 수정하면 한글이 깨져서
Unicode True 

;------------------------------------------------------------------------------------------------
;기본정보
; HM NIS Edit Wizard helper defines
!define PRODUCT_NAME "Product Name"
!define PRODUCT_VERSION "v1.0"
!define PRODUCT_PUBLISHER "발행자(회사명)"
!define PRODUCT_WEB_SITE "웹사이트"
!define PRODUCT_DIR_REGKEY "Software\Microsoft\Windows\CurrentVersion\App Paths\example.exe"
!define PRODUCT_UNINST_KEY "Software\Microsoft\Windows\CurrentVersion\Uninstall\${PRODUCT_NAME}"
!define PRODUCT_UNINST_ROOT_KEY "HKLM"

;------------------------------------------------------------------------------------------------
; 아래 정보들은 굳이 필요없는듯 한데 경고메시지가 발생하기에 기본적으로 넣어둔다.
; Version Information
; 파일설명
VIAddVersionKey /LANG=0 "FileDescription" "example"
; 제품이름
VIAddVersionKey /LANG=0 "ProductName" "example"
; 제품버전
VIProductVersion 2022.01.12.01
; 파일버전
VIAddVersionKey /LANG=0 "FileVersion" 2022.01.12.01
; 제품버전
VIAddVersionKey /LANG=0 "ProductVersion" 2022.01.12.01
; 저작권
VIAddVersionKey /LANG=0 "LegalCopyright" "Copyright @Company BizOn Co., Ltd."  
; 등록상표
VIAddVersionKey /LANG=0 "LegalTrademarks" "example.exe"

;------------------------------------------------------------------------------------------------
; 우리가 평소에 보던 일반적일 UI 를 사용한다고 선언한다. 
; MUI 1.67 compatible ------
!include "MUI.nsh"

; MUI Settings
!define MUI_ABORTWARNING
!define MUI_ICON "install.ico"
!define MUI_UNICON "${NSISDIR}\Contrib\Graphics\Icons\modern-uninstall.ico"

;시작화면 좌측 이미지
!define MUI_WELCOMEFINISHPAGE_BITMAP "Welcome.bmp"
!define MUI_UNWELCOMEFINISHPAGE_BITMAP "Welcome.bmp"

;페이지 상단 좌측 이미지
!define MUI_HEADERIMAGE
!define MUI_HEADERIMAGE_BITMAP "Header.bmp"

;------------------------------------------------------------------------------------------------
; 여기부터 표시할 UI 페이지 설정
; 1. Welcome page 
!insertmacro MUI_PAGE_WELCOME

; 2. License Page
!insertmacro MUI_PAGE_LICENSE "License.txt"

; 3. Directory Page
!insertmacro MUI_PAGE_DIRECTORY

; 4. Component Page
!insertmacro MUI_PAGE_COMPONENTS

; 5. Instfiles page 
!insertmacro MUI_PAGE_INSTFILES

; 6. Finish page
!insertmacro MUI_PAGE_FINISH

; 7. Uninstaller pages 
!insertmacro MUI_UNPAGE_INSTFILES

; ETC. Language files
!insertmacro MUI_LANGUAGE "Korean"
; MUI end ---------------------------------------------------------------------------------------------

;------------------------------------------------------------------------------------------------
; 여기부터 파일 설치 구성
Name "${PRODUCT_NAME} ${PRODUCT_VERSION}"
OutFile "Setup.exe"
InstallDir "$PROGRAMFILES\example"
ShowInstDetails show
ShowUnInstDetails show

Section "MainSection" SEC01
  SetOutPath "$INSTDIR"
  SetOverwrite try
  
  ;상대경로나 절대경로입력
  File "Build\Antlr3.Runtime.DLL"
  
  ;설치 폴더 변경
  SetOutPath "$INSTDIR\en-us"
SectionEnd

Section -Post
SectionEnd


; 옵션 추가
!include TextFunc.nsh
!include nsDialogs.nsh
!include LogicLib.nsh

!define MUI_FINISHPAGE_RUN_TEXT 'OPEN ERP 10 PRINT SETTING'
!define MUI_FINISHPAGE_RUN "$INSTDIR\ERP_PRINT_SETTING.exe"
!define MUI_FINISHPAGE_RUN_NOTCHECKED

!define MUI_FINISHPAGE_SHOWREADME "$INSTDIR\Setting\ERP10 PRINT USER MANUAL.pdf" #Used as our email checkbox
!define MUI_FINISHPAGE_SHOWREADME_TEXT "Read User Manual"
!define MUI_FINISHPAGE_SHOWREADME_NOTCHECKED

