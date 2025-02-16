# 안드로이드 프레임 워크

## Application 
- 선탑재 앱 + 설치 앱

## Application Framework
### 프레임워크 규칙에 따라 생성하면 생명주기 설정 리소스를 찾는 동작은 프레임워크가 진행
- Activity Manager: 앱의 생명주기와 액티비티 스택 관리
- Content Providers: 앱 간 데이터 공유 관리
- Resource Manager: 문자열, 이미지 등 리소스 관리
- View System: UI 구성요소 제공
- Notification Manager: 상태바 알림 관리
### 씬 클라이언트와 서버
- ex) Activity Manager = ActivityManager + ActivityManagerService
  - 앱프로세스는 컴포넌트 실행과 같은 최소한의 작업
  - system_server가 스탠관리, 탐색, ANR 처리를 위임
  - 모든 컴포넌트를 system_server가 관리되며 앱프로세스에 다시 메시지를 보내는 방식
### 시스템 서비스 접근
- system_server가 별도의 프로세스 임으로 Binder IPC로 프로세스 통신
#### 1:N 관계
하나의 서버 프로세스가 여러 클라이언트 프로세스 서비스
예: ActivityManagerService가 여러 앱의 액티비티 관리
#### 양방향 통신
서버 → 클라이언트 콜백 가능
#### 추가
- 하드웨어 제어, 파일 관련 -> JNI <-> C/C++ 사용
## Libraries + Runtime
- 네이티브 라이브러리:
    - SQLite: 데이터베이스
  - OpenGL ES: 3D 그래픽
  - WebKit: 웹 브라우저 엔진
  - Media Framework: 미디어 재생 및 녹화


- 안드로이드 런타임:
  - ART(Android Runtime) 또는 Dalvik VM
  코어 라이브러리
## 리눅스 커널
- 주요 기능:
  - 메모리 관리
  - 프로세스 관리
  - 네트워크 스택
  - 보안 시스템
  - 드라이버 관리


### Binder IPC
<img width="664" alt="스크린샷 2025-02-17 오전 1 51 37" src="https://github.com/user-attachments/assets/a1040cec-a98f-4b48-901d-8c26dbf479f7" />

1. 클라이언트의 요청:
Proxy 객체(대리, service interface)를 통한 메서드 호출 
파라미터 마샬링(Marshalling) - 전송가능한 형태로 변경

2. Binder 드라이버 처리:

데이터 복사 및 메모리 매핑
프로세스 간 권한 체크

3. 서버 프로세스 처리:

Stub 객체(실제 구현)에서 언마샬링(Unmarshalling)
실제 메서드 실행


응답 반환:
결과 마샬링
클라이언트로 전달



- Service, Content Provider
- Binder Thread로 통신
- 트랜잭션 기반 통신
- 공유 메모리 사용 및 매모리 매핑 (성능)
- 데이터 캐싱 + 비동기 (트랜잭션 성능)
- 프로세스 접근제어 (보안)
## 호환성 모드
- 안드로이드 버젼 업에도 기존 동작 유지
### targetSdkVersion 지정
- 가급적 최신 버젼 유지
- 이 버젼까지는 호환성 모드 x

### Compat 클래스
- 버젼 분기에 따른 기존 support-v4 호환 메서드 먼저 사용