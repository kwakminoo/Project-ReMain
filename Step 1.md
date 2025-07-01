# ✅ Step 1. Unity 프로젝트 초기 세팅 - AIDESK

AIDESK는 Windows 로그인 직후 자동 실행되는 Unity 기반 데스크탑 연출 시스템입니다.  
이 문서는 Unity 프로젝트 생성 및 초기 구성 단계의 작업 절차를 정리한 것입니다.

---

## 🛠️ 1. Unity 프로젝트 생성

1. Unity Hub 실행 → [New Project] 클릭
2. 템플릿 선택: `3D Core` (또는 2D, 목적에 따라 선택)
3. 프로젝트 이름: `AIDESK`
4. 저장 경로: 원하는 위치 지정 (예: `C:\Projects\AIDESK`)
5. Unity 버전: **2022.3 LTS 이상** 권장
6. [Create Project] 클릭

---

## ⚙️ 2. Player 설정 변경 (전체화면 모드)

> 전체화면 부팅 애니메이션 연출을 위해 설정 필요

1. `Edit > Project Settings > Player` 이동
2. 아래 항목 수정:
   - `Fullscreen Mode`: **Fullscreen Window**
   - `Default Screen Width`: `1920`
   - `Default Screen Height`: `1080`
   - `Display Resolution Dialog`: **Disabled**
   - `Run In Background`: ✅ 체크

---

## 🎮 3. 입력 설정 (HUD 방해 방지)

> HUDScene에서 사용자 입력 방해 방지를 위한 기반

- Input System 필요 시 `Project Settings > Input Manager` 조절
- HUD UI 요소에 `Raycast Target = false` 설정 가능

---

## 🌀 4. DOTween 설치 (애니메이션 플러그인)

> 부팅 연출 및 HUD 애니메이션 구현을 위한 툴

1. [Asset Store](https://assetstore.unity.com/packages/tools/animation/dotween-hotween-v2-27676)에서 "DOTween" 다운로드 및 Import
2. 또는 [GitHub](https://github.com/Demigiant/dotween)에서 zip 다운로드 후 수동 설치
3. Unity 메뉴바 → `Tools > DOTween Utility Panel` 클릭
4. [Setup DOTween...] 클릭 → 자동 설정 완료

---

## 🗂️ 5. 씬 구성

> 부팅 연출용 씬과 HUD 씬 분리 구성

1. `Assets > Scenes` 폴더 생성
2. `BootScene` 씬 생성 (부팅 연출용)
3. `HUDScene` 씬 생성 (HUD 상시 표시용)
4. 현재는 `BootScene`을 열어 작업 준비

---

## 🔄 6. BootScene → HUDScene 자동 전환 로직

> 부팅 애니메이션이 끝난 후 자동으로 HUD 화면으로 전환

```csharp
// BootManager.cs
using UnityEngine;
using UnityEngine.SceneManagement;

public class BootManager : MonoBehaviour
{
    public float bootDuration = 5.0f;

    void Start()
    {
        Invoke(nameof(LoadHUDScene), bootDuration);
    }

    void LoadHUDScene()
    {
        SceneManager.LoadScene("HUDScene");
    }
}
