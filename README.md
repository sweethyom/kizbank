프로젝트 준비 기록
프로젝트 시작 배경
목표: 사용자 친화적 금융 서비스를 제공하는 웹 애플리케이션 개발.
대상: 주로 미성년자 및 금융 초보자를 타겟으로.
기능: 실시간 금융 상품 비교, 위치 기반 지도 서비스, 금융 데이터를 활용한 대시보드 제공.
사용 기술 스택
프론트엔드

Vue.js: SPA(Single Page Application) 구현.
Pinia: 상태 관리.
Axios: API 통신.
Kakao Maps API: 지도 및 위치 기반 서비스.
백엔드

Django: 서버와 데이터 관리.
Django Rest Framework (DRF): RESTful API 설계.
dj-rest-auth: 인증 및 회원가입 기능 구현.
데이터 관리

SQLite: 초기 로컬 데이터베이스.
AWS QuickSight: 데이터 시각화 대시보드.
금융감독원 상품 데이터 API: 금융 상품 정보.
배포 및 환경 관리

AWS: IAM, QuickSight, S3, API Gateway 활용.
Node.js: 서버-대시보드 연결 및 CORS 문제 해결.
.env: 민감한 키 및 설정 값 관리.
주요 구현 내용
1. 회원가입 및 인증
dj-rest-auth 사용으로 비밀번호 해싱 문제 해결.
커스터마이징:
RegisterSerializer 상속하여 추가 필드 및 유효성 검사 구현.
get_cleaned_data()와 save() 함수 활용.
2. 금융 상품 비교
금융감독원 API: 예금/적금 데이터를 통합 관리.
Django ORM:
annotate()로 금리(intr_rate2) 계산.
Vue.js에서 정렬(sort) 및 데이터 표시.
프론트엔드:
Props를 활용하여 API 데이터를 컴포넌트에 전달.
localeCompare()를 통해 이름 순 정렬 구현.
3. 지도 서비스 (Kakao Maps API)
농협은행 본점을 지도 중심으로 설정.
검색 및 마커 표시:
검색 결과에 따라 마커 생성.
마커 클릭 시 상세 정보 표시.
4. 환율 조회
한국수출입은행 API 사용.
프론트-백 통신:
프론트에서 통화 선택 시 백엔드에 GET 요청.
백엔드에서 API 데이터를 받아 다시 프론트로 응답.
5. 데이터 시각화 (AWS QuickSight)
데이터 흐름:
Vue 요청 → Node.js → AWS QuickSight → Node.js → Vue.js.
Vue에서 iframe으로 대시보드 표시.
CORS 문제 해결: Node.js 서버에서 프록시 역할 수행.
정책 설정:
AWS IAM 사용자에 대한 정책을 작성하여 QuickSight 사용 권한 부여.
예제 정책은 공식 문서 참고.
도전과 해결
API_KEY 설정 문제:

.env 파일에서 = 뒤 공백 문제로 API 인증 실패.
공백 제거로 해결.
회원가입 커스터마이징:

RegisterSerializer를 활용해 확장 가능성을 확보.
기존 dj-rest-auth 문서 분석 및 소스 코드 참조.
CORS 문제:

Node.js 프록시 서버로 Vue.js와 AWS 간의 통신 문제 해결.
데이터 업데이트 문제:

CRUD 후 리스트와 디테일 화면 데이터 비동기 업데이트.
디테일 화면에서 백엔드 재요청하도록 로직 수정.
폴더 구조
plaintext
코드 복사
project/
│
├── .env                       # 환경 변수
├── src/
│   ├── stores/                # Pinia 상태 관리
│   ├── views/                 # Vue.js 화면
│   ├── components/            # Vue.js 컴포넌트
│   └── assets/                # 정적 파일
│
├── server/                    # Node.js 서버
│   ├── server.js              # Express 서버
│   ├── .env                   # 서버용 환경 변수
│   └── package.json           # 서버 패키지 설정
│
└── README.md                  # 프로젝트 설명
앞으로의 계획
배포: AWS EC2와 S3를 활용한 정적 및 동적 배포.
UI/UX 개선: 사용자 피드백 기반 디자인 개선.
추가 기능:
커뮤니티 기능 완성.
알림 및 예약 서비스 도입.