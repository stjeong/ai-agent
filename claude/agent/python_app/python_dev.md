# Python General App Plan

## Overview

확장 가능한 범용 Python 프레임워크. 실행 시 인터페이스를 지정할 수 있으며, 기본값은 CLI.

## Project Structure

- 프로젝트 생성 위치는 plan md 파일이 있는 디렉터리가 아닌 현재 디렉터리를 기준

```
python_app/
├── main.py                 # 진입점 역할만 수행 (lib_func.py에 구현한 함수를 호출)
├── lib_func.py             # 모든 코드 구현
└── requirements.txt        # 의존성
```

---

## Usage Examples

```bash
# 기본 CLI 모드
python main.py
```

---

## Implementation Order

1. `main.py` - 진입점
2. `lib_func.py` - 작업 코드
3. `requirements.txt` - 의존성

---
