# 변경 이력

## v1.2.1 (2026-03-09)

### 신규 기능
- **다중 모니터 지원**: 가장 큰 모니터에서 프랙탈 렌더링, 나머지 모니터는 종횡비 보존 중앙 크롭으로 복사
  - EnumDisplayMonitors로 모니터 열거, 면적 기준 렌더 모니터 자동 선택
  - 보조 모니터: 경량 미러 창 (입력 처리, 커서 숨김, grace period 2초)
  - 오버레이/시계는 프라이머리 모니터에만 표시
  - 단일 모니터 환경에서는 기존과 동일 동작
- **다중 모니터 에뮬레이션**: `/emu` (듀얼) 또는 `/emu:N` (N=2~4)
  - 서로 다른 종횡비 프리셋 (16:9, 4:3, 21:9, 16:10)으로 가상 모니터 생성
  - 단일 모니터 환경에서 다중 모니터 동작 테스트 가능
- **통합 exe**: 인스톨러 + 스크린세이버를 단일 바이너리로 통합
  - AV 오탐 방지: RCDATA 임베딩(드로퍼 패턴) 제거, CopyFileW(자기 자신) 방식
  - 확장자 기반 모드 감지 (.exe=설치, .scr=스크린세이버)

### 개선
- **프레임워크/콘텐츠 분리**: ScreenSaverEngine + IScreenSaverContent 아키텍처로 리팩토링
  - 프레임워크를 wsse.lib 정적 라이브러리로 분리
  - 새 스크린세이버 개발 시 IScreenSaverContent만 구현하면 됨
- **프레임워크 기능 플래그**: AppDescriptor에 hasOverlay, hasClock, hasAutoUpdate 추가
- **컬러 채널 셔플 순환**: 매 타겟마다 6개 컬러 채널(R/G/B/C/M/Gold) 셔플 순환
- **스크린샷 PNG 전환**: BMP에서 PNG 형식으로 변경 (GDI+ 인코더)

### 버그 수정
- desk.cpl 미리보기 창 검은 화면 시작 수정 (ZoomAnimator SkipFadeIn)

### 변경
- 주석/문서의 불필요한 non-ASCII 문자를 ASCII로 대체
- 수학 기호(π, θ, λ), 트리 문자, 고유명사는 유지

## v1.2.0 (2026-03-04)

### 신규 기능
- **I18N**: 7개 언어 지원 (영어, 한국어, 일본어, 중국어 간체/번체, 독일어, 프랑스어, 스페인어)
  - OS 로케일 자동 감지, `/lang:xx` 오버라이드
  - 인스톨러, 설정 다이얼로그, 오버레이, 시스템 정보 모두 현지화
- **7-세그먼트 디지털 시계**: GDI Polygon 기반 LED 스타일 시계 오버레이
  - 당구공 바운스 물리 (벽 반사, 매 실행 랜덤 궤적)
  - 꺼진 세그먼트 dim 표시 (2-pass 렌더링)
- **시스템 정보 오버레이**: GPU/CPU 모델, RAM, 해상도, FPS, 줌 배율, 반복 횟수
  - 좌우 cos-wave 바운스, fadeAlpha 연동 밝기
- **Triangle Inequality Average (TIA)**: 4번째 컬러링 스타일 추가
  - 삼각 부등식 기반 궤도 활성도 측정, 방사형 패턴 생성
  - CPU (AVX2) + GPU (HLSL) 동기화
- **주기적 스크린샷**: `/screenshot:간격[:폴더]` 명령행 옵션
  - 스타일 순회 완료 시 자동 종료
- **자동 업데이트 확인**: GitHub Releases API (WinHTTP, 백그라운드 스레드)
  - 설정 다이얼로그에서 수동 확인 + 다운로드 링크
- **Lite 버전 빌드 시스템**: `LITE_VERSION` 컴파일 타임 분기
  - 절반 해상도 렌더링 + StretchBlt 업스케일, 30fps 캡

### 개선
- **컬러 스타일 셔플**: 순차 순환에서 랜덤 셔플로 변경
- **설정 대화상자 재설계**: 실제 작동하는 항목만 유지
  - Zoom Speed 슬라이더 (1~10), Color Style 콤보박스, Show Overlay/Clock 체크박스
- **오버레이 텍스트 밝기**: 시계와 동일한 ~15% 불투명도로 통일
- **CPU 최적화**:
  - Partial SIMD early rejection (일부 interior 레인 즉시 스킵)
  - FMA3 intrinsics 적용
  - One-pass 행 렌더링 (132MB 힙 -> ~30KB 스택 버퍼)
  - Cardioid/bulb SIMD 조기 거절 활성화
  - Stripe/TIA 조건부 계산 (style 0/1에서 orbit trap 스킵)
  - ApplyFadeToSurface AVX2 (8픽셀 동시 처리)
- **GPU 최적화**: Cardioid/Period-2 early rejection, orbit trap 조건부 계산
- **내부 영역 컬러 대비 감소**: 팔레트 주파수 2.5->0.8, 진폭 0.35->0.20
- **CRT 정적 링크**: /MD -> /MT (vcruntime 불필요)
- **코드 리팩토링**: ScreenSaverApp 모듈 분리 (SystemInfo, TextOverlay, ClockOverlay)
- **매직 넘버 상수화**: 애니메이션/디스플레이/팔레트/계산 상수 90+개 constexpr 추출
- **Strategy 패턴**: GPU/CPU 렌더링 분기를 GPURenderStrategy/CPURenderStrategy로 분리
- **Cross-edition 정책**: Full <-> Lite 상호 배타적 설치/제거
- **TaskDialog 런처**: 설치/미리보기/제거 커맨드 링크, 버전 비교 (재설치/업그레이드/다운그레이드)

### 버그 수정
- 내부/외부 컬러 경계 불연속 수정 (별도 darken 팩터 분리)
- 인스톨러: STRINGTABLE + VERSIONINFO 추가 (desk.cpl 드롭다운에 표시되지 않던 문제)
- 줌 방향 전환 시 순간 이동(텔레포트) 현상 수정
- PowerShell/start가 `/S` 인자를 소문자로 변환하는 문제 우회 (`GetCommandLineW()` 사용)
- 패닝 속도 clamp (줌아웃처럼 보이는 현상 수정)
- 설정 다이얼로그 열린 동안 마우스 커서 표시되지 않던 문제 수정
- 줌 다양성 개선: RNG 시드를 `random_device`로 변경, 타겟 오프셋 1000배 확대

## v1.1.0 (2026-03-01)

### 신규 기능
- **단일 채널 모노크롬 컬러링**: RGB 코사인 팔레트 -> 6개 컬러 채널 (Red, Green, Blue, Cyan, Magenta, Gold)
  - 내부/외부 색상 구분: 같은 색 계열의 틴트 변형
  - CPU + GPU 동기화

### 개선
- 줌 탐색 좌표 25 -> 100개 확장 (15개 카테고리)
- PCA 곡률 필터링: 시각적으로 단조로운 경계 자동 건너뜀
- 자체 포함 인스톨러 (FractalSaver.scr RCDATA 임베드)
- BoundaryPoint.found 플래그: 빈 공간 수렴 방지

## v1.0.0 (2026-02-28)

### 최초 릴리스
- **만델브로 프랙탈 스크린세이버**: 실시간 줌 애니메이션
- **GPU 렌더링**: D3D11 Compute Shader, double-float 에뮬레이션 (~48비트 정밀도)
- **CPU 렌더링**: AVX2 SIMD 4픽셀 동시 계산, 멀티스레드
- **2x2 슈퍼샘플링 안티앨리어싱**
- **지수적 줌**: 적응형 패닝 + 경계 추적
- **설정 다이얼로그**: Force CPU, Zoom Speed, Color Style, Overlay/Clock
- **Windows 스크린세이버 표준 인터페이스**: /s, /c, /p 지원
