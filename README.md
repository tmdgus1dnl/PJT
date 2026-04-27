# 1444820-성주희
# 1441050-한승현

## Smart Dashboard

사용자 위치 기반 지도, 날씨 예보, 개인 포트폴리오, 텔레매틱스 로그 조회, AI 챗봇 기능을 하나의 대시보드 화면에서 사용할 수 있도록 구성한 웹 프로젝트입니다.

기본 화면은 태블릿 형태의 UI로 구성되어 있으며, 좌측 또는 우측 네비게이션 버튼을 통해 Home, Weather, My Page, Logs, Chat 화면으로 이동할 수 있습니다. 각 화면은 별도의 HTML partial과 JavaScript 모듈로 분리되어 있고, `main.js`가 화면 전환과 페이지 초기화를 담당합니다.

---

## 프로젝트 개요

이 프로젝트는 정적 HTML/CSS/JavaScript 기반의 프론트엔드와, OpenAI API 호출을 중계하는 Node.js Express 백엔드로 구성되어 있습니다.

### 핵심 목표

- 지도 기반 위치 검색 및 경로 표시
- 현재 위치 기반 날씨 요약 및 시간대별 예보 제공
- 개인 포트폴리오 페이지 제공
- Firebase Firestore 기반 텔레매틱스 로그 조회
- 최근 로그 데이터를 참고하는 AI 챗봇 제공
- 음성 입력과 음성 출력이 가능한 대화형 UI 제공

---

## 주요 기능

### 1. Home / Navigation

Home 화면은 현재 위치를 기준으로 지도와 날씨 요약 정보를 제공합니다.

주요 기능은 다음과 같습니다.

- 현재 위치 기반 날씨 요약 표시
- 위치 권한이 없거나 실패할 경우 서울 기준 날씨 정보 표시
- Kakao Map 기반 지도 출력
- 장소 키워드 검색
- 검색 결과 마커 표시
- 목적지 선택 시 현재 위치에서 목적지까지 경로 표시

Home 화면의 초기화는 `navigation.js`의 `HomePage.init()`에서 수행됩니다.

---

### 2. Weather

Weather 화면은 OpenWeatherMap API를 사용하여 시간대별 예보 정보를 카드 형태로 보여줍니다.

주요 기능은 다음과 같습니다.

- 현재 위치 기반 3시간 단위 예보 조회
- 최대 5개의 예보 카드 표시
- 위치 접근 실패 시 서울 좌표 기준 예보 표시
- 가로 보기 / 세로 보기 모드 전환
- 선택한 보기 모드를 `localStorage`에 저장
- 30분 주기 자동 갱신
- 날씨 로딩 중 skeleton UI 표시

---

### 3. Portfolio / My Page

Portfolio 화면은 개인 소개와 기술 스택을 보여주는 페이지입니다.

주요 기능은 다음과 같습니다.

- 자기소개 영역
- 이미지 슬라이드
- Skill 진행률 바
- Stack 표기
- Closing 섹션
- `IntersectionObserver`를 이용한 progress bar 애니메이션

---

### 4. Logs

Logs 화면은 Firebase Firestore의 `telematics_logs` 컬렉션을 기반으로 로그 데이터를 조회합니다.

주요 기능은 다음과 같습니다.

- 실시간 로그 구독
- INFO / WARN / CRITICAL 레벨 필터
- 사용자 필터
- 검색어 기반 텍스트 필터
- Last 1h / Last 24h / Last 7d / Custom 기간 필터
- 로그 레벨 비율을 도넛 차트 형태로 표시
- WARN / CRITICAL 로그의 경우 `Event=value` 패턴에서 이벤트 값 추출

---

### 5. Chatbot

Chat 화면은 AI 챗봇 기능을 제공합니다.

주요 기능은 다음과 같습니다.

- 사용자 입력 메시지 전송
- OpenAI API 기반 응답 생성
- Express 백엔드 프록시를 통한 API 호출
- Firebase Firestore에 채팅 기록 저장
- 기존 채팅 기록 불러오기
- 최근 `telematics_logs` 데이터를 AI 컨텍스트로 사용
- Chrome Speech API를 이용한 음성 입력
- `speechSynthesis`를 이용한 AI 응답 음성 출력

챗봇은 단순 질의응답뿐 아니라, 최근 텔레매틱스 로그를 기반으로 주행 데이터 요약과 권장 조치를 제공하도록 구성되어 있습니다.

---

## 화면 구성

### main
![img](./assets/main.png)

### weather
![img](./assets/weather.png)

### portfolio
![img](./assets/portfolio.png)

### log
![img](./assets/log.png)

---

## 기술 스택

### Frontend

- HTML
- CSS
- JavaScript ES Modules
- Bootstrap / Bootstrap Icons
- Axios
- Kakao Maps API
- Kakao Mobility Directions API
- OpenWeatherMap API
- Firebase Firestore
- Web Speech API
  - SpeechRecognition
  - speechSynthesis

### Backend

- Node.js
- Express
- CORS
- dotenv
- Morgan
- OpenAI SDK

---

## 프로젝트 구조

```bash
PJT/
├── README.md
├── main.html                 # 전체 앱의 진입 HTML
├── main.css                  # 전체 UI 스타일
├── main.js                   # SPA 화면 전환 및 라우팅 담당
├── navigation.js             # Home 지도 / 날씨 요약 기능
├── weather.js                # 시간대별 날씨 예보 기능
├── portfolio.js              # 포트폴리오 progress bar 애니메이션
├── log.js                    # Firestore 텔레매틱스 로그 조회
├── chatbot.js                # AI 챗봇 / 음성 입력 / 음성 출력
├── home_navigation.html      # Home 화면 partial
├── weather.html              # Weather 화면 partial
├── portfolio.html            # Portfolio 화면 partial
├── log.html                  # Logs 화면 partial
├── chatbot.html              # Chatbot 화면 partial
├── assets/                   # 이미지 리소스
├── backend/
│   ├── package.json          # 백엔드 의존성 정보
│   └── server.js             # OpenAI API 프록시 서버
└── 요구사항명세서.xlsx
```

---

## 전체 동작 흐름

```text
main.html
  └── main.js
        ├── 화면 버튼 이벤트 등록
        ├── fetchPartial()로 partial HTML 로드
        ├── main-ui 영역에 HTML 삽입
        ├── slide-in / slide-out 애니메이션 적용
        └── routeInit()으로 현재 화면에 맞는 init 함수 실행
              ├── HomePage.init()
              ├── WeatherPage.init()
              ├── PortfolioPage.init()
              ├── LogPage.init()
              └── ChatbotPage.init()
```

`main.js`는 사용자가 네비게이션 버튼을 클릭할 때마다 해당 HTML 파일을 불러와 `main-ui` 영역에 삽입합니다. 이후 현재 화면에 맞는 JavaScript 모듈의 `init()` 함수를 실행합니다.

각 페이지의 `init()` 함수는 이벤트 리스너, API 호출, Firebase 구독 등을 설정하고, 화면을 벗어날 때 필요한 정리 작업을 수행할 수 있도록 cleanup 함수를 반환합니다.

---

## 실행 방법

### 1. 저장소 클론

```bash
git clone https://github.com/tmdgus1dnl/PJT.git
cd PJT
```

---

### 2. 프론트엔드 실행

이 프로젝트는 `fetch()`로 partial HTML을 불러오기 때문에 `file://` 방식으로 직접 HTML 파일을 여는 것보다 로컬 서버로 실행하는 것이 안전합니다.

Python이 설치되어 있다면 다음 명령어로 간단히 실행할 수 있습니다.

```bash
python -m http.server 5500
```

브라우저에서 아래 주소로 접속합니다.

```text
http://localhost:5500/main.html
```

VS Code를 사용한다면 Live Server 확장을 사용해도 됩니다.

---

### 3. Firebase 설정

`log.js`와 `chatbot.js`는 `firebase.js`에서 Firebase 설정값을 가져옵니다.

보안상 `firebase.js`는 `.gitignore`에 포함되어 있으므로, 프로젝트 루트에 직접 생성해야 합니다.

```javascript
// firebase.js
export const firebaseConfig = {
  apiKey: "YOUR_FIREBASE_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

Firestore에는 다음 컬렉션을 사용합니다.

```text
telematics_logs
chat_messages
```

---

### 4. 백엔드 실행

챗봇 기능을 사용하려면 백엔드 서버를 실행해야 합니다.

```bash
cd backend
npm install
```

`.env` 파일을 생성하고 OpenAI API Key를 입력합니다.

```bash
OPENAI_API_KEY=YOUR_OPENAI_API_KEY
```

서버를 실행합니다.

```bash
node server.js
```

기본 서버 주소는 다음과 같습니다.

```text
http://localhost:8080
```

프론트엔드의 `chatbot.js`는 기본적으로 아래 API 주소로 요청을 보냅니다.

```text
POST http://localhost:8080/api/chat
```

---

## API 및 외부 서비스

### OpenWeatherMap API

사용 위치:

- `navigation.js`
- `weather.js`

역할:

- 현재 날씨 조회
- 3시간 단위 예보 조회

---

### Kakao Map / Kakao Mobility API

사용 위치:

- `navigation.js`

역할:

- 지도 표시
- 장소 검색
- 현재 위치에서 목적지까지 경로 표시

---

### Firebase Firestore

사용 위치:

- `log.js`
- `chatbot.js`

역할:

- 텔레매틱스 로그 조회
- 채팅 기록 저장 및 조회

---

### OpenAI API

사용 위치:

- `backend/server.js`
- `chatbot.js`

역할:

- 사용자 메시지와 최근 로그 컨텍스트를 기반으로 AI 응답 생성

---

## 데이터 구조 예시

### telematics_logs

`log.js` 기준으로 로그 문서는 다음 필드를 사용합니다.

```javascript
{
  createdAt: Timestamp,
  level: "INFO" | "WARN" | "WARNING" | "CRITICAL" | "ERROR",
  user: "user name",
  message: "Event=SomeEvent ..."
}
```

`WARN` 또는 `CRITICAL` 로그의 경우, 화면에서는 `message` 전체가 아니라 `Event=value` 형태에서 추출한 이벤트 값을 우선 표시합니다.

---

### chat_messages

`chatbot.js` 기준으로 채팅 문서는 다음 필드를 사용합니다.

```javascript
{
  role: "user" | "assistant",
  content: "message text",
  timestamp: Date
}
```

---

## 주의사항

### 1. API Key 관리

현재 일부 외부 API Key가 프론트엔드 코드에 직접 포함되어 있습니다. 프론트엔드 코드는 브라우저에서 누구나 확인할 수 있으므로, 실제 배포 환경에서는 API Key를 백엔드 프록시 또는 환경변수 기반 구조로 분리하는 것이 좋습니다.

특히 다음 키는 배포 전 관리 방식 개선이 필요합니다.

- OpenWeatherMap API Key
- Kakao REST API Key
- Firebase 설정값
- OpenAI API Key

OpenAI API Key는 반드시 백엔드 `.env`에서만 관리해야 합니다.

---

### 2. 로컬 서버 필요

`main.js`는 `fetch()`를 사용해 HTML partial을 불러옵니다. 따라서 브라우저에서 `main.html`을 직접 여는 경우 일부 기능이 정상 동작하지 않을 수 있습니다.

로컬 서버 또는 배포 서버 환경에서 실행하는 것을 권장합니다.

---

### 3. 브라우저 권한

다음 기능은 브라우저 권한 또는 브라우저 지원 여부에 영향을 받습니다.

- 위치 정보 접근
- 음성 인식
- 음성 출력

음성 인식은 주로 Chrome 계열 브라우저에서 안정적으로 동작합니다.

---

## 개선 가능 사항

- API Key를 백엔드 프록시로 이동
- `backend/package.json`에 `start` 스크립트 추가
- Firebase 인덱스 설정 문서화
- 로그 데이터 샘플 추가
- 배포 방법 추가
- 화면별 담당 기능과 담당자 구분 추가
- 에러 메시지 UI 개선
- 모바일 반응형 레이아웃 보완
- `temp.js`와 현재 모듈 구조의 관계 정리
- README에 실제 시연 GIF 추가

---

## 담당자

| 학번 | 이름 |
|---|---|
| 1444820 | 성주희 |
| 1441050 | 한승현 |
