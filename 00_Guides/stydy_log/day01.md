# 📅 Day 1 (2025-12-17)

> **주제:** 웹이 실제로 동작하는 길(네트워크/HTTP)과 브라우저가 화면을 그리는 길(렌더링 파이프라인)을 **선 그리듯** 이해하기.
> 목표는 "말로 술술 설명 + DevTools로 증명 + 작은 실험으로 재현"

---

## 1. 오늘의 학습 목표 (완료 기준)
- [ ] 주소창 입력부터 화면이 나오기까지(네트워크 경로)를 단계별로 **암기 없이 설명**할 수 있다.
- [ ] HTTP의 **요청/응답 구조**, **메서드/상태코드**, **캐시 헤더**의 역할을 **예시로 설명**할 수 있다.
- [ ] 브라우저의 **CRP(Critical Render Path)** 과 **DOMContentLoaded vs load** 차이를 말하고, `defer/async`가 왜 중요한지 **실험 결과로 증명**할 수 있다.

---

## 2. 이론 핵심 요약 (오늘 반드시 이해할 목록)

### 2.1 주소창 이후 무슨 일이?
1) **URL 파싱** -> 스킴/호스트/경로/쿼리 분리
2) **DNS 조회** -> 브라우저/OS 캐시 -> 로컬 리졸버 -> 권한있는 네임서버로 **재귀 질의**
3) ** TCP 3-Way Handshake** -> SYN -> SYN/ACK -> ACK (HTTP1.1, HTTP/2는 TCP 기반, HTTP/3는 QUIC/UDP)
4) **TLS 1.3 핸드셰이크***(HTTPS) -> 서버 인증서 검증, ALPN(HTTP/2/3 선택), SNI(가상 호스팅)
5) **HTTP 요청/응답** -> 헤더/바디, **TTFB**(첫 바이트 도달 시간), **Content-Encoding**(gzip/br)
6) **연결 재사용** -> HTTP/1.1 keep-alive / HTTP/2 멀티플렉싱 / HTTP/3(QUIC) 지연 내성

> **면접 한 줄 설명:** "브라우저는 DNS->(TCP/TLS)->HTTP로 서버와 대화하고, 받은 HTML/CSS/JS를 파싱·계산·그려서 화면을 만듭니다.”

---

### 2.2 HTTP 한 장 요약
- **메서드 & 멱등성**:
  - GET(멱등/조회)
  - POST(비멱등/생성)
  - PUT(멱등/전체 치환)
  - PATCH(부분 수정)
  - DELETE(멱등/삭제)
  > 멱등성 : 수학이나 컴퓨터 과학에서 어떤 연산을 여러 번 반복해도 결과가 최초 한 번 실행했을 때와 동일하게 유지되는 성질
- **상태코드 군**
  - 2xx : 성공
  - 3xx : 리다이렉트(301/302/307/308)
  - 4xx : 클라이언트 오류(400/401/403/404/429)
  - 5xx : 서버 오류(500/502/503)
- **캐시 전략**:
  - `Cache-Control: max-age=600, must-revalidate` (10분 신선도, 만료 뒤 재검증)
  - `ETag: "abc123"` ↔ `If-None-Match: "abc123"` → **304 Not Modified**
  - `Last-Modified` ↔ `If-Modified-Since`(시간 기반 대조)
  - `Vary: Accept-Encoding, Origin`(헤더에 따라 별도 캐시 키)
  - 절대 **민감 데이터는 `no-store`** (디스크 저장 금지)
- **쿠키**: `Set-Cookie: key=val; Secure; HttpOnly; SameSite=Lax; Path=/; Domain=.example.com; Max-Age=86400`
  - `HttpOnly` → JS에서 접근 불가(보안)
  - `SameSite=None`이면 `Secure` **필수** (크롬 정책)
- **CORS**(데이터 호출 주소의 출처가 다를 때):
  - **Simple**: 특정 조건의 GET/POST/HEAD는 프리플라이트 없음
  - **Preflight**: `OPTIONS`로 허가 확인 → `Access-Control-Allow-Origin`, `-Methods`, `-Headers`, `-Credentials`
  - **Creds 포함 시**: `Allow-Origin: *` **불가**, 반드시 특정 Origin

---

### 2.3 브라우저 렌더링(화면 그리는 길)
1) HTML 파싱→**DOM** 생성
2) CSS 파싱→**CSSOM** 생성 (**CSS는 기본 렌더-차단**) , (CSSOM : CSS Object Model)
3) DOM+CSSOM→**Render Tree**
4) **Layout**(좌표/크기 계산) → **Paint**(픽셀 칠하기) → **Composite**(레이어 합성)

- **차단 요소**: 외부 CSS, 파서-차단 스크립트(`<script>` 기본)
- **해결**: `<script defer>`(파싱 병행/DOMContentLoaded 직전 실행), `<script async>`(로드 즉시 실행/순서 불확실), **module 스크립트는 기본 defer**
- **리플로우/리페인트 최소화**: `transform/opacity` 변경은 합성만 발생(빠름), 스타일 변경은 모아서, `requestAnimationFrame` 활용

**지표**
- **DOMContentLoaded**: 초기 DOM 구축 완료
- **load**: 이미지 등 **모든** 리소스 로드 완료
- **FCP/LCP/CLS**: 최초/최대 콘텐츠 페인트, 누적 레이아웃 이동(웹 핵심 지표)

---

## 3. 실습 가이드(오늘 3개)

### 3.1 실습 ① Network Waterfall 읽기
1) 임의의 페이지 열기 → DevTools **Network**
2) **Disable cache** 체크 후 새로고침
3) 각 리소스의 **Queueing, Stalled, DNS, Initial connection, SSL, Request/Response, TTFB** 구간 관찰
4) 상태코드/크기(Transferred vs Resource) 비교, **Hard Reload**(Cmd+Shift+R) vs 일반 Reload 차이 기록

> 기록 위치: `fe-roadmap-progress/00_Guides/study_log/2025-10-30.md` (스크린샷 + 메모)

---

### 3.2 실습 ② `defer` 유무에 따른 렌더 타이밍 차이
- `fe-roadmap-lab/03_playgrounds/dom-render/index.html`에서
  `<script src="./main.js" defer>` 를 **제거** / **복원** 하며
  **DOMContentLoaded vs load** 타이밍 로그 비교

---

### 3.3 실습 ③ 리소스 힌트 체험
- `fe-roadmap-lab/03_playgrounds/resource-hints/index.html` 에서
  `preconnect`, `dns-prefetch`, `preload` 추가/삭제 → Network 탭에서 연결/우선순위/스타트 타이밍 비교

---


## 4. 스스로 설명 점검(구두 시험 체크리스트)
- [ ] “DNS→TCP→TLS→HTTP”을 **초등학생에게 설명하듯** 간단히 말할 수 있다.
- [ ] `Cache-Control`, `ETag`, `Last-Modified`, `Vary`의 **차이/관계**를 예시로 말할 수 있다.
- [ ] `defer`와 `async`의 **차이/선택 기준**을 말할 수 있다.
- [ ] **DOMContentLoaded vs load**를 한 문장씩 설명할 수 있다.

---

## 5. 회고(오늘 느낀 점, 막혔던 점)
- (예시) Waterfall에서 TTFB가 큰 리소스가 있음을 봤다. 서버/네트워크/캐시 가능성으로 가설 세움.
