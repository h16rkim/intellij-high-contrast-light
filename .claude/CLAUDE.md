# intellij-high-contrast-light 프로젝트 가이드

## 프로젝트 개요

이 프로젝트는 IntelliJ IDEA를 위한 **Light Mode High Contrast 테마 플러그인**입니다.

### 핵심 특징
- **Light 테마 기반**: IntelliJ 기본 Light Theme (`IntelliJ Light`)를 baseTheme으로 상속받아 기본 UI 유지
- **High Contrast 테두리**: UI 테두리를 진한 검정색(`#000000`)으로 표시하여 접근성 향상
- **선택 색상**: 선택된 항목을 명확하게 파란색(`#0000FF`) + 흰 텍스트(`#FFFFFF`)로 표시
- **최소한의 오버라이드**: 필요한 색상만 정의하고 나머지는 모두 Light 테마에서 상속

---

## 테마 구현 구조

### 생성된 파일 위치

```
src/main/resources/themes/
├── HighContrastLight.json          # 테마 설정 (UI 컴포넌트 색상)
└── HighContrastLightEditor.xml     # 에디터 색상 스키마 (문법 강조)
```

### 핵심 파일 설명

#### 1. **HighContrastLight.json** (`/src/main/resources/themes/HighContrastLight.json`)
- **용도**: 메인 테마 설정 파일 - UI 컴포넌트 색상 정의
- **주요 구조**:
  ```json
  {
    "name": "High Contrast Light",
    "dark": false,
    "author": "h16rkim",
    "baseTheme": "IntelliJ Light",        // IntelliJ 기본 Light 테마 상속
    "editorScheme": "/themes/HighContrastLightEditor.xml",
    "colors": { ... },                    // 전역 색상 정의
    "ui": { ... }                         // UI 컴포넌트별 색상 정의
  }
  ```

- **colors 섹션** - 전역 색상 정의:
  - `"Borders": "#000000"` - 모든 테두리 (High Contrast 검정)
  - `"Component.borderColor": "#000000"` - 컴포넌트 테두리
  - `"Selection.background": "#0000FF"` - 선택 배경 (파란색)
  - `"Selection.foreground": "#FFFFFF"` - 선택 텍스트 (흰색)
  - Tree, List, Table, TextField 등의 선택 색상 정의

- **ui 섹션** - 컴포넌트별 세부 설정:
  - `"*": { "borderColor": "#000000" }` - 모든 컴포넌트에 검은 테두리 적용
  - `"Tree"`, `"List"`, `"Table"` - 선택 배경/텍스트 색상 지정

#### 2. **HighContrastLightEditor.xml** (`/src/main/resources/themes/HighContrastLightEditor.xml`)
- **용도**: 코드 에디터 색상 스키마
- **주요 내용**:
  - `parent_scheme="Default"` - 기본 Light 에디터 스키마 상속
  - `version="1"` - 스키마 버전
  - **colors 섹션** - 최소한의 설정으로 대부분 상속됨:
    - `BORDER_LINES_COLOR` - 에디터 테두리 및 구분선을 High Contrast Black (`#000000`)으로 설정
  - 나머지 코드 문법 강조 색상은 Default 스키마에서 상속

#### 3. **plugin.xml 수정** (`/src/main/resources/META-INF/plugin.xml`)
```xml
<themeProvider id="HighContrastLight" path="/themes/HighContrastLight.json"/>
```
- 플러그인에 테마를 등록하는 확장(extension) 선언
- `id`: 테마의 고유 식별자 (UI에서 표시됨)
- `path`: JSON 테마 파일의 경로

---

## 색상 팔레트

### 핵심 색상 (HighContrastLight.json 기준)

| 요소 | 색상 | 용도 |
|------|------|------|
| 테두리 (모든 컴포넌트) | `#000000` | High Contrast 검정 |
| 선택 배경 | `#0000FF` | 파란색 (UI 선택) |
| 선택 텍스트 | `#FFFFFF` | 흰색 |
| Component 테두리 | `#000000` | 기본 컴포넌트 테두리 |
| Panel 테두리 | `#000000` | 패널 테두리 |
| Table 테두리 | `#000000` | 테이블 테두리 |
| Popup 테두리 | `#000000` | 팝업 테두리 |

### 상속되는 색상 (baseTheme: "IntelliJ Light"에서 상속)
- 배경: 밝은 회색 계열 (기본 Light 테마)
- 텍스트: 검정색 (기본 Light 테마)
- 기타 UI 요소: IntelliJ 기본 Light 테마의 모든 설정

---

## 지원하는 UI 컴포넌트

다음 모든 IntelliJ UI 컴포넌트에 High Contrast 테두리가 적용됩니다:

- **기본 컴포넌트**: Button, CheckBox, RadioButton, ToggleButton
- **입력 필드**: TextField, TextArea, PasswordField, FormattedTextField
- **선택 컴포넌트**: ComboBox, Spinner, Slider
- **목록/테이블**: List, Table, Tree
- **메뉴**: Menu, MenuItem, MenuBar, PopupMenu
- **레이아웃**: Panel, ScrollPane, SplitPane, TabbedPane
- **기타**: ToolBar, ToolTip, ProgressBar, InternalFrame 등

---

## 개발 시 참고사항

### 테마 수정 방법

테마는 **baseTheme: "IntelliJ Light"**를 상속받기 때문에, 필요한 부분만 오버라이드하면 됩니다.

1. **전역 색상 수정**: `HighContrastLight.json`의 `colors` 섹션 수정
   ```json
   "colors": {
     "Borders": "#000000",           // 모든 테두리
     "Selection.background": "#0000FF",
     "Selection.foreground": "#FFFFFF"
   }
   ```

2. **특정 컴포넌트 수정**: `HighContrastLight.json`의 `ui` 섹션 수정
   ```json
   "ui": {
     "Tree": { "selectionBackground": "#0000FF" },
     "List": { "selectionBackground": "#0000FF" },
     "*": { "borderColor": "#000000" }  // 모든 컴포넌트에 적용
   }
   ```

3. **에디터 색상 수정**: `HighContrastLightEditor.xml`의 `colors` 섹션 수정
   - Default 에디터 스키마를 상속받으므로 필요한 색상만 오버라이드

4. **빌드 및 테스트**:
   ```bash
   ./gradlew clean build
   ```

### 색상 추가/수정 시 주의사항

- **16진수 색상 형식**: `#RRGGBB` 형식 사용 (예: `#000000`, `#0000FF`)
- **전역 설정 우선**: `colors` 섹션의 설정이 `ui` 섹션보다 먼저 적용됨
- **와일드카드 사용**: `"*"` 키를 사용하면 모든 컴포넌트에 일괄 적용
- **IDE 캐시 삭제**: 색상 변경 후 IntelliJ `Invalidate Caches / Restart` 실행

### 새로운 색상 추가 방법

`HighContrastLight.json`의 `colors` 섹션에 추가:

```json
"colors": {
  "Borders": "#000000",
  "NewColor.property": "#XXXXXX",
  "Component.newProperty": "#XXXXXX"
}
```

또는 `ui` 섹션에 추가:

```json
"ui": {
  "NewComponent": {
    "borderColor": "#000000",
    "background": "#FFFFFF",
    "foreground": "#000000"
  }
}
```

---

## 빌드 및 배포

### 빌드
```bash
./gradlew build
```

생성 위치: `/build/libs/intellij-high-contrast-light-0.0.1.jar`

### 로컬 테스트
1. IntelliJ IDEA 실행
2. Settings/Preferences > Appearance & Behavior > Appearance
3. Theme 드롭다운에서 "High Contrast Light" 선택

### 플러그인 마켓플레이스 배포
- [JetBrains 플러그인 마켓플레이스 문서](https://plugins.jetbrains.com/docs/intellij/publishing-plugin.html) 참조

---

## 문제 해결

### 테마가 Appearance 탭에 나타나지 않는 경우
1. `plugin.xml`에 `<themeProvider id="HighContrastLight" path="/themes/HighContrastLight.json"/>` 올바르게 등록되었는지 확인
2. 테마 파일이 JAR에 포함되었는지 확인:
   ```bash
   unzip -l build/libs/intellij-high-contrast-light-0.0.1.jar | grep theme
   ```
3. 파일 경로 확인:
   - `themes/HighContrastLight.json` 존재 확인
   - `themes/HighContrastLightEditor.xml` 존재 확인
4. 플러그인 재설치 후 IDE 재시작

### baseTheme 설정이 유효하지 않은 경우
- **오류**: `"parentTheme": "Light"` 설정 시 에러 발생
- **해결**: `"baseTheme": "IntelliJ Light"` 사용
- IntelliJ 2024.1 이상에서는 `baseTheme`를 사용하는 것이 권장됨

### 색상이 예상과 다른 경우
1. `HighContrastLight.json` 색상값 확인 (16진수 형식: `#RRGGBB`)
   - 색상 값이 큰따옴표로 감싸져 있는지 확인: `"#000000"` (O), `#000000` (X)
2. `colors` 섹션과 `ui` 섹션의 우선순위 확인
   - `colors` 섹션의 전역 설정이 먼저 적용됨
3. IDE 캐시 삭제 후 재시작: `Invalidate Caches / Restart`

### 특정 UI 요소가 겹쳐 보이는 경우
- 예: 설정 창의 버튼들이 겹쳐서 표시됨
- **확인 사항**:
  - 테마 상속 구조 확인 (baseTheme이 올바른지)
  - 불필요한 색상 오버라이드가 없는지 확인
  - `*` 와일드카드 설정이 모든 컴포넌트에 의도치 않게 적용되지 않았는지 확인
- **해결**: 필요한 색상만 최소한으로 설정하고 나머지는 상속받도록 구성

---

## 관련 문서

- [IntelliJ Platform 테마 개발 문서](https://plugins.jetbrains.com/docs/intellij/themes-intro.html)
- [UI 테마 색상 정의](https://plugins.jetbrains.com/docs/intellij/themes.html)
- [에디터 색상 스키마](https://plugins.jetbrains.com/docs/intellij/syntax-highlighting.html)
