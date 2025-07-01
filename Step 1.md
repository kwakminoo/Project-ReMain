# ✅ Step 1. REMAIN 런처 UI 구성 - 테마 선택 화면

사용자가 테마를 선택하고 "적용" 버튼을 누르면, 선택한 테마가 저장되고  
다음 부팅 시 해당 테마로 부팅 애니메이션과 HUD가 실행됩니다.

---

## 🎯 목표

- 씬 이름: `LauncherScene`
- 사용자에게 설치된 테마 목록을 보여주고 선택할 수 있게 하기
- 선택된 테마 정보를 저장 (PlayerPrefs 사용)

## 🛠️ 1. Unity 프로젝트 생성Add commentMore actions

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
    }Add commentMore actions
}


---

## 🛠️ 1-1. 폴더 및 씬 구성

Assets/
├── Scenes/
│ └── LauncherScene.unity
├── Scripts/
│ ├── ThemeSelector.cs
│ └── ThemeItem.cs
├── Resources/
│ └── UI/
│ └── theme_item.prefab
├── StreamingAssets/
│ └── themes/
│ ├── Default/
│ │ ├── intro.mp4
│ │ ├── preview.png
│ │ └── theme.json
│ ├── Futuristic/
│ │ ├── intro.mp4
│ │ ├── preview.png
│ │ └── theme.json

markdown
복사
편집

---

## 🖥️ 1-2. Unity UI 구성

### ✅ Canvas
- Render Mode: Screen Space - Overlay
- UI Scale Mode: Scale With Screen Size

### ✅ UI 구조 예시
- **ScrollView** (`ThemeScrollView`)  
  - 테마 리스트 표시
- **Panel** (오른쪽 미리보기)
  - `RawImage`: `preview.png` 표시
  - `Text`: 테마 이름
  - `Button`: “적용” 버튼

---

## 🧱 1-3. ThemeItem 프리팹 만들기

1. `UI > Button` 생성
2. 안에:
   - `RawImage` (썸네일)
   - `Text` (테마 이름)
3. `ThemeItem.cs` 스크립트 추가
4. 프리팹으로 저장: `Resources/UI/theme_item.prefab`

### 🔧 ThemeItem.cs

```csharp
using UnityEngine;
using UnityEngine.UI;

public class ThemeItem : MonoBehaviour
{
    public RawImage thumbnail;
    public Text themeNameText;
    private string themeName;

    public void Initialize(string name, Texture2D thumbnailTex, System.Action<string> onClickCallback)
    {
        themeName = name;
        themeNameText.text = name;
        thumbnail.texture = thumbnailTex;

        GetComponent<Button>().onClick.AddListener(() => onClickCallback(themeName));
    }
}
💻 1-4. ThemeSelector.cs (전체 관리 스크립트)
csharp
복사
편집
using UnityEngine;
using UnityEngine.UI;
using System.IO;

public class ThemeSelector : MonoBehaviour
{
    public Transform scrollContent;
    public GameObject themeItemPrefab;
    public RawImage previewImage;
    public Text themeTitleText;
    public Button applyButton;

    private string selectedTheme = "";

    void Start()
    {
        string themesPath = Path.Combine(Application.streamingAssetsPath, "themes");
        string[] themeDirs = Directory.GetDirectories(themesPath);

        foreach (string dir in themeDirs)
        {
            string themeName = Path.GetFileName(dir);
            string previewPath = Path.Combine(dir, "preview.png");

            Texture2D tex = new Texture2D(2, 2);
            tex.LoadImage(File.ReadAllBytes(previewPath));

            GameObject item = Instantiate(themeItemPrefab, scrollContent);
            item.GetComponent<ThemeItem>().Initialize(themeName, tex, OnThemeSelected);
        }

        applyButton.onClick.AddListener(OnApplyClicked);
    }

    void OnThemeSelected(string themeName)
    {
        selectedTheme = themeName;
        themeTitleText.text = themeName;

        string previewPath = Path.Combine(Application.streamingAssetsPath, "themes", themeName, "preview.png");
        Texture2D tex = new Texture2D(2, 2);
        tex.LoadImage(File.ReadAllBytes(previewPath));
        previewImage.texture = tex;
    }

    void OnApplyClicked()
    {
        if (!string.IsNullOrEmpty(selectedTheme))
        {
            PlayerPrefs.SetString("SelectedTheme", selectedTheme);
            PlayerPrefs.Save();
            Debug.Log("테마 적용됨: " + selectedTheme);
        }
    }
}
