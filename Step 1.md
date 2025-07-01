# ✅ Step 1. REMAIN 런처 UI 구성 - 테마 선택 화면

사용자가 테마를 선택하고 "적용" 버튼을 누르면, 선택한 테마가 저장되고  
다음 부팅 시 해당 테마로 부팅 애니메이션과 HUD가 실행됩니다.

---

## 🎯 목표

- 씬 이름: `LauncherScene`
- 사용자에게 설치된 테마 목록을 보여주고 선택할 수 있게 하기
- 선택된 테마 정보를 저장 (PlayerPrefs 사용)

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
