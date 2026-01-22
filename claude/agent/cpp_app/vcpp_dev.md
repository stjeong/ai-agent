# Visual C++ 프로그램 개발 플랜

## 개요
사용자의 요구사항에 맞는 Visual C++ 프로그램을 설계하고 구현합니다.

## 기본 설정
- **기본 프로그램 유형**: CLI (명령줄 인터페이스)
- **지원 유형**: CLI, 게임, 시스템 유틸리티, 네트워크 애플리케이션
- **컴파일러**: Visual C++ (MSVC)
- **빌드 도구**: MSBuild
- **플랫폼**: Windows

---

## 구현 워크플로우

### 1단계: 요구사항 수집
- 만약 프로젝트 이름을 사용자가 제공하지 않았다면 에이전트 실행 전 요구한다.
- 사용자에게 프로그램의 목적과 기능 확인
- 프로그램 유형 확인 (미지정 시 CLI 기본)
- 외부 라이브러리 사용 여부 확인

### 2단계: 프로젝트 구조 설계
```
project_name/
├── src/
│   ├── main.cpp          # 진입점
│   ├── app.cpp           # 핵심 애플리케이션 로직
│   └── app.h
├── include/              # 헤더 파일
├── tests/                # 테스트 코드
├── project_name.vcxproj  # Visual Studio 프로젝트 파일
└── README.md             # 프로젝트 문서
```
 - 프로젝트 생성 디렉터리는 현재 디렉터리를 기준으로 함
 - Visual Studio 2022용 솔루션 파일도 함께 생성

### 3단계: 코드 구현
1. **.vcxproj 생성** - MSBuild용 프로젝트 파일 설정
2. **main.cpp 작성** - 프로그램 진입점
3. **핵심 클래스/함수 구현** - 요구사항에 따른 기능 구현
4. **에러 처리 추가** - 예외 처리 및 입력 검증

### 4단계: 빌드 및 테스트
1. MSBuild로 컴파일 및 링크
2. 기능 테스트 실행
3. 버그 수정

### 5단계: 문서화
- README.md에 사용법 작성

---

## 프로그램 유형별 추가 고려사항

### CLI 프로그램 (기본)
- 명령줄 인자 파싱 (argc, argv)
- 사용법 출력 (--help)
- 표준 입출력 처리
- 종료 코드 반환

### 게임
- 게임 루프 구현
- 입력 처리 시스템
- 상태 관리
- (선택) 그래픽 라이브러리 연동

### 시스템 유틸리티
- 파일 시스템 접근
- 프로세스/스레드 관리
- Windows API 호출
- 권한 처리

### 네트워크 애플리케이션
- Winsock 소켓 프로그래밍
- 프로토콜 구현 (TCP/UDP)
- 비동기 I/O 처리
- 연결 관리

---

## 빌드 환경
- **컴파일러**: Visual C++ (MSVC)
- **빌드 도구**: MSBuild.exe
- **C++ 표준**: C++17 이상 권장

## 빌드 명령어
```bash
# Release 빌드
msbuild.exe project_name.vcxproj /p:Configuration=Release /p:Platform=x64

# Debug 빌드
msbuild.exe project_name.vcxproj /p:Configuration=Debug /p:Platform=x64

# 실행
.\x64\Release\project_name.exe [옵션]
```
