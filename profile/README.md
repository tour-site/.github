
# 🗺️ 부산 여행 추천 웹 애플리케이션 - TOUR SITE
[👉 배포 사이트 바로가기](http://bjava.iptime.org:8898/) <br>
초보 여행자를 위한 지역 기반 부산 여행지 추천 플랫폼입니다.  <br>
SVG 지도 기반 지역 선택, 테마 필터(여행지/맛집/숙소), PDF 저장, 게시판, 관리자 기능까지 포함된 풀스택 프로젝트입니다.

![image](https://github.com/user-attachments/assets/64f2eede-c01c-4397-93ef-0e16f09a763a)
![image](https://github.com/user-attachments/assets/669e72e9-a5c2-42aa-a007-bb99323a8d0d)
![image](https://github.com/user-attachments/assets/4973a1fa-51d6-4583-9a59-56c98dbc8064)



---

## 📌 프로젝트 개요

- **목표**: SVG 지도 + 지역/테마 선택 → 여행지 추천
- **대상**: 초보 국내 여행자 및 부산 지역 기반 탐색 사용자
- **기능 요약**:
  - 지역 클릭 기반 추천
  - 추천 결과 PDF 생성
  - 일반/카카오 로그인, JWT 인증
  - 게시판 (댓글/좋아요 포함)
  - 관리자 페이지 (회원 관리 + 통계 시각화)

---




## ✅ 주요 기능 & API 명세

### 공통

| 기능 | 설명 | API/경로 |
|------|------|----------|
| 정적 파일 접근 | index.html, 리소스 허용 | `/`, `/assets/**`, `/css/**`, `/img/**` |
| CORS 허용 | 쿠키 포함 요청 허용 | Spring Security 설정 |

### 회원 기능

| 기능 | 설명 | API |
|------|------|-----|
| 회원가입 | 일반 회원 가입 | `POST /api/member/signup` |
| 로그인/로그아웃 | JWT 쿠키 발급/삭제 | `POST /api/member/login` / `logout` |
| 아이디/비번 찾기 | 전화번호 기반 | `POST /api/member/find-id` |
| 마이페이지 | 사용자 정보 확인 | `GET /api/member/mypage` |

### 카카오 로그인

| 기능 | 설명 | API |
|------|------|-----|
| 카카오 인증 | 인가 코드로 토큰 교환 | `GET /api/auth/kakao?code=` |
| 사용자 저장 | 최초 로그인 시 DB 등록 | 내부 처리 |
| 토큰 발급 | ROLE_KAKAO 포함 JWT 발급 | 내부 처리 |

### 여행지 추천

| 기능 | 설명 | API |
|------|------|-----|
| 여행지/맛집/숙소 조회 | 선택된 지역 기반 추천 | `/api/place`, `/api/foods`, `/api/stays` |
| 인프라 차트 | 지역별 시각화 | `/api/infra/districts`, `/infra/chart` |

### 게시판

| 기능 | 설명 | API |
|------|------|-----|
| 글 목록/조회 | 전체/단건 | `GET /api/board`, `/board/{id}` |
| 글 작성/수정/삭제 | JWT 인증 기반 | `POST/PUT/DELETE /api/board` |
| 댓글 기능 | 작성/삭제 | `POST /comments`, `DELETE /comments/{id}` |
| 좋아요 기능 | 토글/조회 | `POST /like`, `GET /likes/count` |

### 관리자

| 기능 | 설명 | API |
|------|------|-----|
| 회원 목록/검색 | 전체/키워드 기반 | `GET /api/admin/members` |
| 수정/삭제 | 전화번호/닉네임/권한 변경 | `PUT /api/admin/members/{id}` |

---

## 📋 요구사항 정의

### 기능 요구

- 지역 선택 + 카테고리 → 추천
- 게시판 작성/댓글/좋아요
- 관리자 기능: 검색/수정/삭제
- 카카오/일반 로그인 통합 관리

### 비기능 요구

- JWT: HttpOnly Cookie + CORS 보안 처리
- Oracle DB 21c XE 기반
- Chart.js 사용 시 controller 등록 필요
- Docker 컨테이너 기반 배포

---

## 📅 프로젝트 일정 (15일 기준)

| 기간 | 내용 |
|------|------|
| 1~2일 | 기획, ERD, 요구사항 정의 |
| 3~5일 | DB 설계, Docker/CORS 설정 |
| 6~7일 | 회원 기능 개발, 게시판 CRUD |
| 8~9일 | 댓글/좋아요 ,SVG 지도, 추천 연동 |
| 10~11일 | 관리자 기능, 차트 시각화 |
| 12~13일 | 배포 환경 구성 |
| 14~15일 | 문서화, 발표 준비 |

---

## 🧩 기술 스택

| 분야 | 기술 |
|------|------|
| Frontend | React, Vite, Axios, React Router, Chart.js |
| Backend | Spring Boot , Spring Security, JPA, JWT |
| DB | Oracle 21c XE |
| 인증 | JWT (HttpOnly 쿠키), Kakao OAuth 2.0 |
| 배포 | Docker |
| 기타 | CORS 설정, Chart.js.register |

---

## 🔄 시스템 구성도

```
사용자 ↔ React (Vite) ↔ Spring Boot API ↔ Oracle DB
                              ↑
                        Kakao API
```

---

## 🔐 인증 흐름 요약

1. 로그인 (일반/카카오)
2. Spring에서 JWT 발급
3. HttpOnly 쿠키 저장
4. 요청 시 자동 포함 → 필터에서 검증

---

## 🧠 주요 이슈 & 해결 경험

<details>
<summary>✅ 배포 후 JWT 인증 실패 (403)</summary>
Spring CORS 설정에서 allowCredentials + 정확한 Origin 설정 필요.  
프론트에서도 withCredentials: true 필수.
</details>

<details>
<summary>📉 Chart.js 차트가 렌더링 안 됨</summary>
ChartJS.register()에 모든 컨트롤러 수동 등록 필요.  
(BarController, LineController 등)
</details>

<details>
<summary>👥 카카오 vs 일반 회원 구분</summary>
DB 테이블 분리 → JWT에 role (`ROLE_USER`, `ROLE_KAKAO`) 부여 → 게시글 작성 시 타입 구분 저장
</details>

---

## 🛠️ 실행 방법

```bash
# frontend
**/tour_react#
npm i 
npm i axios
npm i vite
npm i react-router-dom
npm i react-chartjs-2


// react 실행 명령어 
npm run dev


`application.properties`에 DB 정보 필요

---

## 🐳 Docker 배포 예시

```bash
## 이미지 생성
docker pull gvenzl/oracle-xe:21-slim

## tour란 이름의 컨테이너 생성 
docker run -d --name tour -p 1521:1521 --restart=always  -e ORACLE_PASSWORD=1234 gvenzl/oracle-xe:21-slim


```

---

## 🙌 제작자

- **김기윤**
- **안정민**
- **이경주**
- **이윤범**


## 그외 작성하면서 정리한 노션
https://tabby-roarer-f4c.notion.site/_2-21278e67071480fba94ef6919d606501
