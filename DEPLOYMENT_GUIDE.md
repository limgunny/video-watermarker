# 🚀 비디오 워터마킹 앱 무료 배포 가이드

## 📋 배포 준비 완료 사항

✅ **백엔드 배포 준비**

- 환경변수 기반 설정으로 변경
- 포트 동적 설정 (배포 플랫폼 호환)
- Procfile 및 runtime.txt 생성

✅ **프론트엔드 배포 준비**

- 하드코딩된 API URL을 환경변수 기반으로 변경
- API 엔드포인트 중앙 관리 (`src/lib/api.ts`)

## 🌐 추천 무료 배포 옵션

### 1. **Railway** (가장 추천)

- **장점**: Python과 Node.js 모두 지원, 자동 배포, 무료 티어
- **무료 한도**: 월 $5 크레딧 (충분한 테스트용)
- **특징**: 데이터베이스 자동 제공, 환경변수 쉬운 설정

### 2. **Render**

- **장점**: 정적 사이트와 웹 서비스 모두 지원
- **무료 한도**: 백엔드는 750시간/월, 프론트엔드는 무제한
- **특징**: 자동 배포, SSL 인증서 자동 제공

### 3. **Vercel (프론트엔드) + Railway (백엔드)**

- **장점**: 각각 최적화된 플랫폼 사용
- **무료 한도**: Vercel은 무제한, Railway는 월 $5 크레딧

## 🛠️ Railway로 전체 배포하기 (추천)

### 1단계: Railway 계정 생성 및 프로젝트 연결

1. [Railway.app](https://railway.app)에서 GitHub 계정으로 로그인
2. "New Project" → "Deploy from GitHub repo" 선택
3. 이 저장소 선택

### 2단계: 백엔드 배포

1. Railway에서 "New Service" → "GitHub Repo" 선택
2. `backend` 폴더 선택
3. 환경변수 설정:
   ```
   SECRET_KEY=your-super-secret-key-here
   JWT_SECRET_KEY=your-super-secret-key-here
   PORT=5000
   ```
4. 배포 완료 후 백엔드 URL 복사 (예: `https://your-app.railway.app`)

### 3단계: 프론트엔드 배포

1. Railway에서 "New Service" → "GitHub Repo" 선택
2. `frontend` 폴더 선택
3. 환경변수 설정:
   ```
   NEXT_PUBLIC_API_URL=https://your-backend-url.railway.app
   ```
4. 배포 완료 후 프론트엔드 URL 확인

## 🛠️ Render로 배포하기

### 백엔드 배포

1. [Render.com](https://render.com)에서 계정 생성
2. "New" → "Web Service" 선택
3. GitHub 저장소 연결
4. 설정:
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `python app.py`
   - **Environment**: `Python 3`
5. 환경변수 설정:
   ```
   SECRET_KEY=your-super-secret-key-here
   JWT_SECRET_KEY=your-super-secret-key-here
   ```

### 프론트엔드 배포

1. "New" → "Static Site" 선택
2. GitHub 저장소 연결
3. 설정:
   - **Build Command**: `npm run build`
   - **Publish Directory**: `out`
4. 환경변수 설정:
   ```
   NEXT_PUBLIC_API_URL=https://your-backend-url.onrender.com
   ```

## 🛠️ Vercel + Railway 조합 배포

### 백엔드 (Railway)

1. Railway에서 백엔드 배포 (위 Railway 가이드 참조)

### 프론트엔드 (Vercel)

1. [Vercel.com](https://vercel.com)에서 계정 생성
2. GitHub 저장소 연결
3. 프로젝트 설정:
   - **Framework Preset**: Next.js
   - **Root Directory**: `frontend`
4. 환경변수 설정:
   ```
   NEXT_PUBLIC_API_URL=https://your-backend-url.railway.app
   ```

## ⚙️ 환경변수 설정 가이드

### 백엔드 필수 환경변수

```bash
SECRET_KEY=your-super-secret-key-here
JWT_SECRET_KEY=your-super-secret-key-here
PORT=5000
```

### 프론트엔드 필수 환경변수

```bash
NEXT_PUBLIC_API_URL=https://your-backend-url.com
```

### 선택적 환경변수 (Cloudinary 사용 시)

```bash
CLOUDINARY_CLOUD_NAME=your-cloud-name
CLOUDINARY_API_KEY=your-api-key
CLOUDINARY_API_SECRET=your-api-secret
```

## 🔧 배포 후 확인사항

1. **백엔드 API 테스트**

   - `https://your-backend-url.com/api/videos` 접속하여 JSON 응답 확인

2. **프론트엔드 연결 테스트**

   - 프론트엔드 사이트 접속하여 로그인/회원가입 테스트

3. **파일 업로드 테스트**
   - 워터마크 삽입 기능 테스트

## 🚨 주의사항

1. **파일 저장소**: 무료 플랫폼은 파일 저장 공간이 제한적입니다
2. **데이터베이스**: SQLite는 로컬 파일이므로 서버 재시작 시 데이터 손실 가능
3. **FFmpeg**: 비디오 처리에 FFmpeg가 필요하므로 배포 플랫폼에서 지원하는지 확인
4. **메모리 제한**: 무료 티어는 메모리 제한이 있어 큰 비디오 파일 처리 시 문제 발생 가능

## 🔄 자동 배포 설정

GitHub에 코드를 푸시하면 자동으로 배포되도록 설정됩니다:

- Railway: 자동 배포 활성화됨
- Render: 자동 배포 활성화됨
- Vercel: 자동 배포 활성화됨

## 📞 문제 해결

### 일반적인 문제들

1. **CORS 오류**: 백엔드 CORS 설정에서 프론트엔드 도메인 추가
2. **환경변수 오류**: 배포 플랫폼에서 환경변수 올바르게 설정했는지 확인
3. **빌드 실패**: 로그를 확인하여 의존성 설치 문제 해결

### 로그 확인 방법

- Railway: Dashboard → Service → Deployments → View Logs
- Render: Dashboard → Service → Logs
- Vercel: Dashboard → Project → Functions → View Function Logs

## 🎉 배포 완료!

배포가 완료되면 전 세계 어디서나 접근 가능한 웹 애플리케이션이 됩니다!











