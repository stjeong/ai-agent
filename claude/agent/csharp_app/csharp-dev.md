# C# 프로그램 생성 에이전트 플랜

## 개요

이 에이전트는 사용자의 요청에 따라 C# 프로젝트를 생성합니다.

---

## 핵심 컴포넌트

| 컴포넌트 | 역할 |
|---------|------|
| **AgentEngine** | 사용자 요청 해석 및 작업 조율 |
| **ProjectGenerator** | 프로젝트 구조 생성 |
| **CodeGenerator** | C# 코드 파일 생성 |
| **TemplateManager** | 프로젝트 유형별 템플릿 관리 |
| **ConfigurationManager** | 설정 및 옵션 관리 |

---

## 지원 프로젝트 유형

프로젝트 유형을 지정하지 않으면 **CLI (Console Application)**를 기본으로 사용합니다.

| 유형 | dotnet 템플릿 | 설명 | 추가 패키지 |
|-----|--------------|------|------------|
| **CLI** (기본값) | `console` | Console Application | - |
| **WPF** | `wpf` | Windows Presentation Foundation | - |
| **WinForms** | `winforms` | Windows Forms | - |
| **WebAPI** | `webapi` | ASP.NET Core Web API | - |
| **Blazor** | `blazorwasm` / `blazorserver` | Blazor WebAssembly/Server | - |
| **MAUI** | `maui` | .NET MAUI 크로스플랫폼 | - |
| **WindowsService** | `worker` | Windows NT Service | `Microsoft.Extensions.Hosting.WindowsServices` |
| **LinuxDaemon** | `worker` | Linux Daemon (systemd) | `Microsoft.Extensions.Hosting.Systemd` |
| **WorkerService** | `worker` | 크로스플랫폼 백그라운드 서비스 | 위 두 패키지 모두 |

---

## 워크플로우

```
[사용자 입력] → 만약 프로젝트 이름을 사용자가 제공하지 않았다면 에이전트 실행 전 요구한다.
    ↓
[요청 파싱] → 프로그램 유형 확인 (기본: CLI)
    ↓
[템플릿 선택] → 해당 유형 템플릿 로드
    ↓
[코드 생성] → 요구사항에 맞는 코드 생성
    ↓
[프로젝트 출력] → .csproj, .cs 파일들 생성, 그리고 Visual Studio 2022용 솔루션 파일도 함께 생성
```

---

## 에이전트 실행 지침

### 1. 프로젝트 유형 확인
- 사용자가 유형을 지정하면 해당 유형 사용
- 지정하지 않으면 **CLI** 기본 적용

### 2. 프로젝트 생성 명령어

- 모든 프로젝트 생성 시 "Do not use top-level statements" 옵션 기본 적용

```bash
# 솔루션 생성
dotnet new sln -n {프로젝트명}

# 프로젝트 유형별 생성
dotnet new {템플릿} -n {프로젝트명}

# 솔루션에 프로젝트 추가
dotnet sln add {프로젝트경로}
```

### 3. 유형별 생성 예시

#### CLI (기본)
```bash
dotnet new console -n MyApp
```

#### WPF
```bash
dotnet new wpf -n MyApp
```

#### WinForms
```bash
dotnet new winforms -n MyApp
```

#### WebAPI
```bash
dotnet new webapi -n MyApp
```

#### Blazor Server
```bash
dotnet new blazorserver -n MyApp
```

#### Blazor WebAssembly
```bash
dotnet new blazorwasm -n MyApp
```

#### MAUI
```bash
dotnet new maui -n MyApp
```

#### Windows Service
```bash
dotnet new worker -n MyApp
cd MyApp
dotnet add package Microsoft.Extensions.Hosting.WindowsServices
```

#### Linux Daemon
```bash
dotnet new worker -n MyApp
cd MyApp
dotnet add package Microsoft.Extensions.Hosting.Systemd
```

#### Worker Service (크로스플랫폼)
```bash
dotnet new worker -n MyApp
cd MyApp
dotnet add package Microsoft.Extensions.Hosting.WindowsServices
dotnet add package Microsoft.Extensions.Hosting.Systemd
```

---

## 서비스 유형별 Program.cs 설정

### Windows Service
```csharp
var builder = Host.CreateApplicationBuilder(args);
builder.Services.AddHostedService<Worker>();
builder.Services.AddWindowsService();
builder.Build().Run();
```

### Linux Daemon
```csharp
var builder = Host.CreateApplicationBuilder(args);
builder.Services.AddHostedService<Worker>();
builder.Services.UseSystemd();
builder.Build().Run();
```

### Worker Service (크로스플랫폼)
```csharp
var builder = Host.CreateApplicationBuilder(args);
builder.Services.AddHostedService<Worker>();

if (OperatingSystem.IsWindows())
{
    builder.Services.AddWindowsService();
}
else if (OperatingSystem.IsLinux())
{
    builder.Services.UseSystemd();
}

builder.Build().Run();
```

---

## 빌드 및 실행

```bash
# 빌드
dotnet build

# 실행
dotnet run

# 릴리스 빌드
dotnet publish -c Release
```

---

## 참고사항

- .NET 버전: 10.0 이상 권장
- 모든 프로젝트는 nullable reference types 활성화
- 암시적 using 사용 (`ImplicitUsings`)
