# Project-ReMain

# AIDESK - J.A.R.V.I.S-style Desktop Environment Engine

AIDESK는 Windows 부팅 후 자동 실행되어 영화 '아이언맨'의 J.A.R.V.I.S.처럼  
시네마틱한 부팅 연출과 함께 AI 스타일의 데스크탑 HUD를 적용하는 차세대 데스크탑 커스터마이징 툴입니다.

- [Cursor]([https://glama.ai/mcp/servers/@cjo4m06/mcp-shrimp-task-manager?locale=ko-KR](https://cursor.com/downloads))
  ~~~C
  "shrimp-task-manager": {
      "command": "node",
      "args": ["/path/to/mcp-shrimp-task-manager/dist/index.js"],
      "env": {
        "DATA_DIR": "/path/to/project/data",
        "MCP_PROMPT_PLAN_TASK": "Custom planning guidance...",
        "MCP_PROMPT_EXECUTE_TASK_APPEND": "Additional execution instructions...",
        "TEMPLATES_USE": "en",
        "ENABLE_GUI": "false"
      }
    }
  ~~~
- Auto Rename Tag
- Korean Lenguage
- Live Server
- Material Icon Theme
- Vue - Official
- [NodeJs](https://nodejs.org/ko/download)

### MCP
- context7
- Sequential Thinking

### Claude Code
- [설치](https://www.youtube.com/watch?v=J0IWxZXczxs)
1. WSL 설치 명령어 (영상 1:15, 1:43)
    ~~~
    wsl --install
    ~~~
2. Node JS + Claude Code 설치 명령어 (영상 2:18)
~~~
  curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
  sudo apt update && sudo apt install -y nodejs
  sudo npm install -g @anthropic-ai/claude-code
~~~

---

## ✨ Features (MVP)

- 🔹 **Cinematic Boot Sequence**: Windows 부팅 직후 영화 같은 전체화면 애니메이션 연출
- 🔹 **Seamless Transition to Desktop**: 연출 후 배경 + HUD 테마로 자연스럽게 전환
- 🔹 **AI-style HUD Interface**: 파형, 시계, 텍스트 박스 등 반응형 HUD 표시
- 🔹 **Custom Theme Support**: 사용자 제작 테마(.json + assets) 불러오기
- 🔹 **Window Overlay Effects**: 사용자가 지정한 앱이 창모드로 실행되면 테두리에 푸른빛 오버레이 효과 적용

---

## 🧱 Architecture Overview

[Windows Boot]
↓
[AIDESK Launcher (Autostart)]
↓
[Boot Animation (Unity Fullscreen)]
↓
[HUD UI + Desktop Theme Activation]
↓
[Target App Window Detection]
↓
[AI-style Overlay Applied to Window Border]

yaml
복사
편집

---

## 🛠️ Tech Stack

- **Engine**: Unity (2022.x or later)
- **Language**: C#, JSON
- **Overlay Detection**: Win32 API (GetWindowRect, FindWindow, etc.)
- **Installer**: Inno Setup (or NSIS for later packaging)

---

## 🔄 File Structure (예시)

/themes/
├── Default/
│ ├── intro.mp4
│ ├── hud_config.json
│ ├── bg_image.jpg
│ └── boot_sfx.wav
/main/
├── AIDESK_Launcher.exe
├── BootAnimationPlayer.unity
├── HUDOverlay.unity

yaml
복사
편집

---

## 📌 Project Status

🚧 **In Development (Early MVP stage)**  
🔜 목표: 부팅 후 시네마틱 애니메이션 → HUD 전환까지 구현

---

## ✅ TODO List (Dev Roadmap)

### 📍 Phase 1: Planning & Spec
- [x] 기능 요구사항 정리
- [x] 전체 시스템 구조 설계
- [x] 테마 파일 구조 정의 (.json + 리소스)

---

### 📍 Phase 2: Boot Animation System
- [ ] Unity 프로젝트 생성 및 부팅용 전체화면 씬 설계
- [ ] `intro.mp4` 또는 Timeline 기반 애니메이션 연출 구현
- [ ] 부팅 연출 후 HUD로 자연스럽게 전환되는 구조 제작

---

### 📍 Phase 3: Desktop HUD Theme Loader
- [ ] HUD 요소 구현 (파형, 시계, 텍스트 등)
- [ ] 테마 설정값(json) 불러오기 시스템
- [ ] 화면 고정 위치 및 크기 조절 기능 추가

---

### 📍 Phase 4: App Window Overlay
- [ ] Win32 API로 창 감지 모듈 제작
- [ ] Unity 오버레이 창 제작 및 감지된 위치에 테두리 적용
- [ ] 사용자 지정 앱 리스트 기반으로 효과 제한

---

### 📍 Phase 5: Launcher & Integration
- [ ] Windows 부팅 시 자동 실행 설정
- [ ] 테마 선택 및 저장 기능 구현
- [ ] 전체 통합 흐름 테스트 및 예외 처리

---

## 📬 Contact

- Project Lead: [Your Name or GitHub Handle]
- License: MIT (추후 변경 가능)

---

## 🎬 목표 데모 영상 스타일

> https://youtu.be/Example_Jarvis_Demo (예시 링크 넣기)

---

## 💡 미래 계획 (선택적 확장)

- 음성 인터페이스 및 GPT 연동 (보류됨)
- 사용자 테마 제작 에디터 추가
- 테마 마켓/커뮤니티 연동
- 마우스 커서 커스터마이징 및 상호작용 애니메이션
