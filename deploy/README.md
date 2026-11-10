# 이루다Bank 배포 가이드 (Firebase + GitHub + Vercel)

## 1. Firebase 콘솔 (프로젝트: cashcash-3cb24)
1. **Authentication → 시작하기 → 로그인 방법 → 이메일/비밀번호** 사용 설정
2. **Firestore Database → 데이터베이스 만들기** (프로덕션 모드, 지역 asia-northeast3 서울 권장)
3. **Firestore → 규칙** 탭에 `firestore.rules` 내용을 붙여넣고 **게시**
4. **관리자 계정 만들기**
   - Authentication → 사용자 추가: 이메일 `admin@iruda.bank` / 원하는 비밀번호(6자 이상)
   - 생성된 사용자의 **UID 복사**
   - Firestore → 컬렉션 시작 → 컬렉션 ID `admins` → 문서 ID에 **그 UID** 붙여넣기 → 필드는 아무거나(예: `role` = `admin`)
   - 관리자 페이지 로그인 아이디는 `admin` (이메일 앞부분)
5. **서비스 계정 키**: 프로젝트 설정 → 서비스 계정 → *새 비공개 키 생성* → JSON 파일 다운로드 (절대 GitHub에 올리지 마세요)

## 2. GitHub
이 폴더(압축 해제한 내용 전체)가 저장소 루트가 됩니다. 이미 올린 파일은 삭제하고 이 구성으로 다시 커밋하세요.
```
Main.dc.html  Admin.dc.html  Payslip.dc.html  PayslipLaw.dc.html   ← 화면 4개
support.js  shared.js  firebase-config.js  firebase-backend.js  ← 화면 실행/데이터 코드
assets/  vendor/      ← 폰트·이미지·React (외부 의존 없이 동작)
api/users.js  package.json  vercel.json  ← 서버 함수 + 배포 설정
firestore.rules       ← 참고용(규칙은 콘솔에도 게시)
```

## 3. Vercel
1. GitHub 저장소를 Vercel에 연결(Import)
2. **Settings → Environment Variables** 추가
   - `FIREBASE_SERVICE_ACCOUNT` = 1-5 에서 받은 JSON 파일 **전체 내용**
   - (선택) `IRUDA_EMAIL_DOMAIN` = `iruda.bank` (바꿀 때는 `firebase-config.js` 의 `emailDomain` 도 같이)
3. Deploy → 접속: `/` = 직원 앱, `/admin` = 관리자 페이지

## 4. 첫 실행
1. 관리자 페이지 접속 → `admin` / (4번에서 정한 비밀번호) 로그인
2. 법률 근거 탭에 기본 카드가 채워져 있고 "저장하지 않은 변경 사항" 표시가 떠 있습니다 → **저장하기**
3. 사원 관리에서 계정 생성(또는 엑셀 업로드) → **저장하기**
4. 업로드 탭에서 월 급여 엑셀 업로드 → **저장하기**

## 참고
- Claude 미리보기에서는 자동으로 로컬(브라우저) 모드로 동작합니다. 배포 사이트 주소에서 `?local=1` 을 붙이면 강제로 로컬 모드입니다.
- 로그인 검증은 Firebase Auth 가 하고, 관리자 표에 보여주기 위한 비밀번호 사본은 관리자만 읽을 수 있는 `credentials` 컬렉션에 평문으로 저장됩니다. (직원 본인은 읽을 수 없음) 비밀번호를 바꾸려면 표에서 수정 후 저장하면 됩니다.
- 관리자 아이디/비밀번호 변경 후에는 다시 로그인해 주세요.
