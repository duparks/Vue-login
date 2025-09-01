# 로그인 앱

Cursor AI의 추천 UI로 제작된 회원가입 및 로그인 시스템입니다.
프론트 개발은 Vue 3와 Tailwind CSS를 활용하여 반응형 디자인과 다크모드를 지원하고,
백엔드 개발은 Python Fastapi 기반으로 JWT 토큰 인증과 OAuth2 로그인을 처리합니다.

🌐 **Login Web App**: [https://parkscamp.net:4710](https://parkscamp.net:4710)

# [프론트엔드]

## 주요 기능

- **로그인/회원가입**: 이메일과 비밀번호를 통한 기본 인증
- **소셜 로그인**: Google, Kakao, Naver OAuth2 지원
- **반응형 디자인**: 모든 디바이스에서 최적화된 사용자 경험
- **다크모드**: 사용자 선호도에 따른 테마 선택
- **Vue 3 + Composition API**: 최신 Vue.js 기능 활용
- **Tailwind CSS**: 유틸리티 우선 CSS 프레임워크
- **Pinia**: Vue 3 공식 상태 관리 라이브러리
- **자동 토큰 갱신**: 액세스 토큰 만료 시 자동 갱신 및 401 에러 처리
- **토큰 만료 모니터링**: 실시간 토큰 상태 모니터링 및 만료 경고
- **사용자 프로필**: 로그인된 사용자 정보를 그리드 형식으로 표시

## 기술 스택

- **Frontend**: Vue 3, Vue Router 4
- **상태 관리**: Pinia
- **스타일링**: Tailwind CSS
- **빌드 도구**: Vite
- **인증**: JWT, OAuth 2.0

## 설치 및 실행

```bash
# 의존성 설치
npm install

# 개발 서버 실행
npm run dev

# 프로덕션 빌드
npm run build

# 빌드 미리보기
npm run preview
```

## 프로젝트 구조

```
src/
├── components/                # 재사용 가능한 컴포넌트
│   ├── HeaderNavi.vue         # 헤더 네비게이션
│   └── SocialLogin.vue        # 소셜 로그인 처리
├── router/                    # 라우팅 설정
│   └── index.js               # Vue Router 설정
├── stores/                    # Pinia 상태 관리
│   └── AuthStore.js           # 인증 상태 스토어
├── utils/                     # 유틸리티 함수
│   ├── HttpClient.js          # HTTP 인터셉터 및 API 클라이언트
│   ├── NotificationDevice.js  # 알림 장치 확인
│   ├── NotificationInit.js    # 알림 초기화
│   ├── NotificationManager.js # 알림 관리
│   ├── OauthConfig.js         # OAuth 설정
│   └── TokenMonitor.js        # 토큰 만료
├── views/                     # 페이지 컴포넌트
│   ├── AboutView.vue          # 로그인 앱 정보
│   ├── HomeView.vue           # 홈 페이지
│   ├── LoginView.vue          # 로그인 페이지
│   ├── LogoutView.vue         # 로그아웃
│   ├── ProfileAlterView.vue   # 비밀번호 변경 페이지
│   ├── ProfileEditView.vue    # 회원정보 변경 페이지
│   ├── ProfileFindView.vue    # 아이디 패스워드 찾기 페이지
│   ├── ProfileView.vue        # 회원정보 페이지
│   └── SignupView.vue         # 회원가입 페이지
├── App.vue                    # 루트 컴포넌트
└── main.js                    # 앱 진입점
```

## API 엔드포인트

### 인증 관련
- `POST /auth/register`: 사용자 회원가입
- `POST /auth/login`: 사용자 로그인
- `POST /auth/logout`: 사용자 로그아웃
- `POST /auth/refresh`: 액세스 토큰 갱신
- `GET /auth/me`: 사용자 토큰 조회

### OAuth 관련
- `GET /oauth/:provider/callback`: 소셜 로그인 콜백 처리
- `POST /auth/:provider/logout`: 소셜 로그아웃 처리

### 사용자 관련
- `GET /auth/profile`: 사용자 정보 조회
- `PUT /auth/profile`: 사용자 정보 변경
- `DELETE /auth/profile`: 사용자 정보 삭제
- `PUT /auth/profile/alter`: 사용자 비밀번호 변경
- `POST /auth/profile/findid`: 사용자 ID 찾기
- `POST /auth/profile/findpw`: 사용자 PASSWORD 찾기

## Pinia 상태 관리

이 프로젝트는 Pinia를 사용하여 전역 상태를 관리합니다.

### 인증 스토어 (AuthStore.js)

```javascript
import { useAuthStore } from '../stores/AuthStore'

export default {
  setup() {
    const authStore = useAuthStore()
    return { authStore }
  }
}
```

#### 주요 상태
- `isLoggedIn`: 로그인 상태
- `accessToken`: 액세스 토큰
- `oauthToken`: Oauth 토큰
- `userData`: 사용자 정보
- `isLoading`: 로딩 상태

#### 주요 액션
- `initializeAuth()`: 앱 시작 시 인증 상태 초기화
- `loginSuccess(token, userData)`: 로그인 성공 시 상태 업데이트
- `appLogout()`: 로그아웃 처리
- `clearAuth()`: 인증 정보 정리
- `fetchUserInfo()`: `/auth/profile` 엔드포인트에서 사용자 정보 가져오기
- `refreshToken()`: 토큰 갱신
- `checkTokenExpiration()`: 토큰 만료 감지 및 처리
- `validateToken()`: 토큰 유효성 검사
- `startTokenMonitoring()`: 토큰 모니터링 시작
- `stopTokenMonitoring()`: 토큰 모니터링 중지
- `showNotification()`: 브라우저 알림 표시
- `showToastNotification()`: 안전한 토스트 (XSS 방지)
- `vibrateDevice()`: 진동 알림
- `playNotificationSound()`: 사운드 알림

### 사용 예시

```javascript
// 로그인 성공 시
this.authStore.loginSuccess(data.accessToken, data.userData)

// 로그아웃 시
await this.authStore.appLogout()

// 로그인 상태 확인
if (this.authStore.isUserLoggedIn) {
  // 로그인된 상태
}
```

## OAuth 설정

### Google OAuth
1. Google Cloud Console에서 프로젝트 생성
2. OAuth 2.0 클라이언트 ID 발급
3. `src/utils/OauthConfig.js`의 `clientId` 업데이트

### Kakao OAuth
1. Kakao Developers에서 애플리케이션 생성
2. REST API 키 발급
3. `src/utils/OauthConfig.js`의 `clientId` 업데이트

### Naver OAuth
1. Naver Developers에서 애플리케이션 생성
2. 클라이언트 ID 발급
3. `src/utils/OauthConfig.js`의 `clientId` 업데이트

## 환경 변수

프로덕션 환경에서는 다음 환경 변수를 설정하세요:

```bash
# API 기본 URL
VITE_API_BASE_URL=https://your-production-domain.com

# OAuth 클라이언트 ID들
VITE_GOOGLE_CLIENT_ID=your_google_client_id
VITE_KAKAO_CLIENT_ID=your_kakao_client_id
VITE_NAVER_CLIENT_ID=your_naver_client_id
```

### 토큰 설정

```bash
# 토큰 만료 시간 설정 (밀리초)
VITE_TOKEN_EXPIRY_WARNING=180000   # 3분 전 경고
VITE_TOKEN_CHECK_FREQUENCY=60000   # 1분마다 체크

# 브라우저 알림 설정
VITE_ENABLE_NOTIFICATIONS=true     # 알림 활성화
```

## 라이센스

MIT License

## 라우팅 구조

- **`/`**: 홈 페이지 - 앱 소개 및 기능 설명
- **`/login`**: 로그인 페이지 - 로그인 폼 및 소셜 로그인
- **`/signup`**: 회원가입 페이지 - 사용자 등록 폼
- **`/oauth/:provider/callback`**: 소셜 로그인 처리 페이지 - OAuth 콜백 처리 및 결과 표시
  - `:provider`는 동적 경로 파라미터 (google, kakao, naver)

## 소셜 로그인 플로우

### SPA 기반 구조
1. **로그인 페이지**에서 소셜 로그인 버튼 클릭
2. **현재 페이지**에서 해당 플랫폼의 OAuth 인증 페이지로 이동
3. **사용자 인증 완료** 후 `/oauth/{provider}/callback?code={code}` 로 리다이렉트
4. **SocialLoginView**에서 인증 코드를 서버로 전송
5. **성공 시**: 3초 카운트다운 후 자동으로 홈 화면으로 이동
6. **실패 시**: 로그인 화면으로 이동 또는 재시도 옵션

### 장점
- **SPA 장점 활용**: 새 창 없이 부드러운 페이지 전환
- **사용자 경험**: 팝업 차단 문제 해결
- **모바일 친화적**: 모바일에서도 안정적인 동작
- **자동 이동**: 성공 시 자동으로 홈 화면으로 이동
- **코드 통합**: 모든 소셜 로그인을 하나의 컴포넌트에서 처리
- **RESTful URL**: 깔끔하고 직관적인 URL 구조 (`/oauth/google/callback` vs `/socialLogin?provider=google`)

## 토큰 만료 및 자동 갱신 시스템

### HTTP 인터셉터 (`src/utils/HttpClient.js`)
- 모든 API 요청에 대해 401 에러 자동 감지
- 토큰 만료 시 자동으로 토큰 갱신 시도
- 갱신 성공 시 원래 요청 자동 재시도
- 갱신 실패 시 자동 로그아웃 및 로그인 페이지 리다이렉트

### 토큰 모니터링 (`src/utils/TokenMonitor.js`)
- 주기적인 토큰 유효성 검사 (1분마다)
- 만료 3분 전 사용자 경고 알림
- 브라우저 알림 지원 (권한 허용 시)
- 자동 토큰 갱신 및 로그아웃 처리

### 주요 기능
- **자동 토큰 갱신**: 백그라운드에서 토큰 갱신 처리
- **사용자 알림**: 만료 예정 시 브라우저 알림
- **중복 요청 방지**: 동시에 여러 갱신 요청 방지
- **실패 요청 큐**: 갱신 중 실패한 요청들을 자동 재시도
- **에러 처리**: 네트워크 오류 및 서버 오류에 대한 안전한 처리

### 사용 예시

```javascript
// HTTP 클라이언트 사용
import { api } from '../utils/HttpClient'

// 자동으로 401 에러 처리 및 토큰 갱신
const response = await fetch('/api/protected-data')
const data = await response.json()

// 토큰 모니터링 수동 제어
import { startTokenMonitoring, stopTokenMonitoring } from '../utils/TokenMonitor'

// 모니터링 시작
startTokenMonitoring()

// 모니터링 중지
stopTokenMonitoring()
```



# [백엔드]

## 주요 기능

- JWT 토큰 기반 인증 (Access Token + Refresh Token)
- 사용자 등록 및 로그인/로그아웃
- 토큰/블랙리스트 DB 관리(`token` 테이블)
- 토큰 로테이션(리프레시 재발급 시 기존 토큰 회수)
- 자동 API 문서 생성 (Swagger UI)
- 구글 OAuth2 소셜 로그인 지원

## 프로젝트 구조

```
Fastcamp/
├── auth/
│   ├── auth_bearer.py
│   ├── jwt_handler.py
│   └── oauth_handler.py
├── config/
│   └── settings.py
├── database/
│   └── connection.py
├── models/
│   ├── token.py
│   └── user.py
├── routers/
│   └── auth.py
├── schemas/
│   └── auth.py
├── utils/
│   ├── email_sender.py
│   └── password_generator.py
├── main.py
└── requirements.txt
```

## 설치 및 실행

### 1. 가상환경 생성 및 활성화

```bash
# 가상환경 생성
python -m venv pyenv

# 가상환경 활성화 (linux)
pyenv\Scripts\activate

# 가상환경 활성화 (Linux)
source pyenv/bin/activate
```

### 2. 패키지 설치

```bash
pip install -r requirements.txt
```

### 3. 환경 변수 설정(.env)

`.env` 파일을 생성하고 다음 내용을 추가하세요:

```env
# Google OAuth2
GOOGLE_CLIENT_ID=your-google-client-id.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=your-google-client-secret
GOOGLE_REDIRECT_URI=google-redirect-uri

# Naver OAuth2
NAVER_CLIENT_ID=your-naver-client-id
NAVER_CLIENT_SECRET=your-naver-client-secret
NAVER_REDIRECT_URI=naver-redirect-uri

# Kakao OAuth2
KAKAO_CLIENT_ID=your-kakao-client-id
KAKAO_CLIENT_SECRET=your-kakao-client-secret
KAKAO_REDIRECT_URI=kakao-redirect-uri

# JWT
SECRET_KEY=your-super-secret-key-here-change-in-production
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=min
REFRESH_TOKEN_EXPIRE_DAYS=days

# MySQL
MYSQL_HOST=yourhost
MYSQL_PORT=yourport
MYSQL_USER=youruser
MYSQL_PASSWORD=yourpassword
MYSQL_DATABASE=yourdatabase

# 포트
PORT=port
```



# [시스템]

## 하드웨어

- ZOTAC ZBOX-RI531NANO

## OS

- Ubuntu 24.04.2 LTS

## 주요 서비스

- **Certbot**: SSL 인증 관리
- **Cron**: 백그라운드 프로세싱
- **Fastcamp**: 로그인 앱 백엔드 서버
- **Mariadb**: MySQL 데이터베이스 서버
- **Nginx**: 웹서버 및 리버스 프록시
- **Ssh**: Secure Shell 서버
- **Vsftpd**: SFTP 서버
