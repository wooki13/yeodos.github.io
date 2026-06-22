[README.md](https://github.com/user-attachments/files/29209194/README.md)# 🎯 QR 스탬프 홈페이지 - 사용 설명서

학교 행사/수행평가용 QR 스탬프 적립 시스템입니다.
6개 부스를 방문하며 QR을 스캔하면 자동으로 스탬프가 적립됩니다.

## 📦 파일 구성

```
qr_stamp/
├── index.html                         ← 홈페이지 본체 (이 파일을 호스팅)
├── qr_codes/
│   ├── booth_1.png ~ booth_6.png      ← 부스 1~6번 QR (행사장 각 부스에 부착)
│   ├── homepage.png                   ← 홈페이지 진입 QR (입구에 부착)
│   ├── print_sheet_booths.png         ← 부스 QR 6개를 한 장에 모은 인쇄용 시트
│   └── print_sheet_homepage.png       ← 홈페이지 진입 QR 인쇄용 시트
└── README.md
```

## 🚀 사용 방법

### 1단계 — 홈페이지 호스팅 (무료)

가장 쉬운 방법: **GitHub Pages**
1. github.com 가입 → 새 저장소 생성 (예: `qr-stamp`)
2. `index.html` 업로드
3. Settings → Pages → main 브랜치 선택 → Save
4. 몇 분 뒤 `https://본인아이디.github.io/qr-stamp/` 주소가 생성됨

다른 방법: **Vercel**, **Netlify**, **학교 서버** 등도 가능

### 2단계 — QR코드의 URL을 본인 주소로 교체

`qr_codes/` 안의 QR코드는 데모용 `https://example.com/qr-stamp/index.html` URL을 가리킵니다.
1단계에서 만든 본인 주소로 다시 만들어야 합니다.

**다시 만드는 법:**
- 무료 QR 생성 사이트 사용 (qr-code-generator.com 등)
- 부스 1번 QR: `https://본인주소/index.html?booth=1`
- 부스 2번 QR: `https://본인주소/index.html?booth=2`
- ... (booth=6까지)
- 홈페이지 진입 QR: `https://본인주소/index.html`

### 3단계 — 인쇄 후 부스에 부착

- `print_sheet_booths.png` 를 A4 컬러로 인쇄 → 6칸을 잘라 각 부스에 부착
- `print_sheet_homepage.png` 를 행사장 입구에 부착

### 4단계 — 행사 진행

1. 참여자가 입구에서 홈페이지 QR을 찍어 사이트 접속
2. 이메일로 로그인
3. 각 부스의 QR을 스캔하면 자동 적립
4. "내 스탬프판"에서 진행 현황 확인 (6/6 완료 시 축하 메시지)

## 🛠 주요 기능

| 기능 | 설명 |
|---|---|
| 이메일 로그인 | 이메일별로 데이터 분리 저장 |
| QR 자동 적립 | URL에 `?booth=1~6` 파라미터가 있으면 자동 적립 |
| 카메라 QR 스캔 | "QR 스캔하기" 버튼으로 카메라로 직접 인식 |
| 중복 방지 | 같은 부스 QR을 다시 찍어도 한 번만 기록 |
| 개인 스탬프판 | 6칸 격자에 ✓ 표시 + "○/6" 진행률 |
| 관리자 페이지 | 비밀번호 `admin1234` 로 전체 참여자 현황 조회 |
| CSV 내보내기 | 관리자 페이지에서 전체 데이터 CSV로 다운로드 |

## ⚙️ 설정 변경

`index.html` 안에서 수정 가능:
```javascript
const ADMIN_PASSWORD = "admin1234";   // ← 관리자 비밀번호
const BOOTH_NAMES = {                  // ← 부스 이름
  1: "1번 부스 - 과학",
  ...
};
```

## 🔐 진짜 구글 OAuth 로그인 적용 방법

현재는 **데모 모드**(이메일 직접 입력)로 동작합니다.
실제 구글 로그인을 쓰려면:

1. [Google Cloud Console](https://console.cloud.google.com)에서 OAuth 2.0 클라이언트 ID 발급
2. `index.html`의 `<head>`에 추가:
   ```html
   <script src="https://accounts.google.com/gsi/client" async defer></script>
   ```
3. 로그인 페이지에 다음 추가:
   ```html
   <div id="g_id_onload" data-client_id="발급받은ID"
        data-callback="handleGoogleLogin"></div>
   <div class="g_id_signin" data-type="standard"></div>
   ```
4. JS에서 `handleGoogleLogin(response)` 함수를 만들어
   JWT 토큰에서 email을 꺼낸 뒤 `setUser(email)` 호출

자세한 가이드는 `index.html` 상단의 주석 참고.

## 💾 데이터 저장 위치

현재는 **브라우저 localStorage**에 저장됩니다.
- 장점: 서버가 필요 없어 즉시 동작
- 단점: 다른 기기에서는 데이터 공유 안 됨

여러 기기 공유가 필요하면 **Google Sheets + Apps Script**, **Firebase Firestore** 등으로 업그레이드 가능합니다. (학생 수행평가 수준에서는 localStorage로 충분)

## ❓ 자주 묻는 문제

| 문제 | 해결 |
|---|---|
| 카메라가 안 열림 | HTTPS 환경에서만 작동. GitHub Pages는 자동 HTTPS |
| QR 인식이 안 됨 | 조명을 밝게, 손떨림 줄이고, 거리 조절 (15~30cm) |
| 부정 적립 우려 | QR을 부스에서만 보여주거나, 시간대 안내 활용 |
| 데이터가 사라짐 | 브라우저 캐시 삭제 시 사라짐. 주기적으로 CSV 백업 |

---
만든이: 학생 수행평가 프로젝트

