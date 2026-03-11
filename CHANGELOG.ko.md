# 변경 이력

[English](CHANGELOG.md)

## v1.2.2 (2026-03-11)

### 신규 기능
- **애니메이션 표시 토글**: 프랙탈 렌더링, 오버레이, 시계를 각각 독립적으로 켜고 끌 수 있는 옵션
  - 설정 다이얼로그 Display 그룹에 체크박스 추가 (7개 언어)
  - F1 설정창에서 실시간 전환 지원

### 개선
- **오버레이 텍스트 가시성 향상**: 두 패스 그림자+텍스트 AlphaBlend 합성
  - Pass 1: 검은 그림자로 밝은 배경에서 대비 확보
  - Pass 2: 흰색 반투명 텍스트로 어두운 배경에서 가독성 확보
  - 32비트 DIB + premultiplied alpha (시계와 동일 파이프라인)
- **시계 바운스 방향**: 항상 사선(45/135/225/315도)으로만 이동
- **시계 콜론 깜빡임**: 정확히 1초 주기 (0.5초 켜짐/0.5초 꺼짐)
- **오버레이 텍스트 순서**: GPU-CPU-RAM -> CPU-RAM-GPU
- **F1 안내 문구 간략화**: "F1: 설정창" (7개 언어)
- **GPU TDR 방지**: Compute Shader 디스패치를 64행 청크 단위로 분할 + Flush()
- **GPU Device Lost 자동 복구**: GPU 장치 손실 감지 시 CPU 렌더링으로 자동 전환
- **장시간 실행 안정성**: ZoomAnimator 비동기 data race 제거, _malloca null 체크, RenderSurface 재생성 시 리소스 누수 방지

### 다운로드
| 에디션 | 설명 | 다운로드 |
|--------|------|----------|
| **Full** | 네이티브 해상도, FPS 제한 없음 | [**FractalSaver_v1.2.2.exe**](https://github.com/jogakdal/fractal-screensaver/releases/download/v1.2.2/FractalSaver_v1.2.2.exe) |
| **Lite** | 절반 해상도, 30fps 제한 | [**FractalSaverLite_v1.2.2.exe**](https://github.com/jogakdal/fractal-screensaver/releases/download/v1.2.2/FractalSaverLite_v1.2.2.exe) |

## v1.2.1 (2026-03-09)

### 신규 기능
- **다중 모니터 지원**: 가장 큰 모니터에서 프랙탈 렌더링, 나머지 모니터는 종횡비 보존 중앙 크롭으로 미러링
  - 오버레이/시계는 프라이머리 모니터에만 표시
  - 단일 모니터 환경에서는 기존과 동일 동작
- **다중 모니터 에뮬레이션**: `/emu` (듀얼) 또는 `/emu:N` (N=2~4)으로 단일 모니터에서 다중 모니터 레이아웃 테스트

### 개선
- **오버레이 가독성** - 이중 외곽선 텍스트 렌더링으로 어떤 배경에서도 가독성 확보
- **업데이트 링크 로케일 대응** - 한국어 로케일 시 CHANGELOG.ko.md로 연결
- **설치 시 기존 프로세스 자동 종료** - 화면 보호기 설정 창이 열려 있어도 설치 가능

### 변경
- 주석/문서의 불필요한 non-ASCII 문자를 ASCII로 대체

## v1.2.0 (2026-03-04)

### 신규 기능
- **7개 언어 지원**: 영어, 한국어, 일본어, 중국어 간체/번체, 독일어, 프랑스어, 스페인어
  - OS 로케일 자동 감지, `/lang:xx` 오버라이드
  - 인스톨러, 설정 다이얼로그, 오버레이, 시스템 정보 모두 현지화
- **7-세그먼트 디지털 시계**: 바운스 물리 + LED 스타일 반투명 렌더링
- **시스템 정보 오버레이**: GPU/CPU 모델, RAM, 해상도, FPS, 줌 배율, 반복 횟수
- **Triangle Inequality Average (TIA)**: 4번째 컬러링 스타일 추가
- **주기적 스크린샷**: `/screenshot:간격[:폴더]` 명령행 옵션
- **자동 업데이트 확인**: GitHub Releases API, 설정 다이얼로그에서 수동 확인 가능
- **Lite 버전**: 절반 해상도 + 30 fps 제한 - 저사양 PC용
- **통합 바이너리**: 인스톨러 + 스크린세이버를 단일 실행 파일로 통합
  - 확장자 기반 모드 감지 (.exe=설치, .scr=스크린세이버)
  - AV 오탐 방지: RCDATA 임베딩(드로퍼 패턴) 제거

### 개선
- **프레임워크/콘텐츠 분리**: ScreenSaverEngine + IScreenSaverContent 아키텍처, 프레임워크를 wsse.lib으로 분리
- **프레임워크 기능 플래그**: AppDescriptor에 hasOverlay, hasClock, hasAutoUpdate 추가
- **컬러 채널 셔플 순환**: 매 타겟마다 6개 컬러 채널 (R/G/B/C/M/Gold) 셔플
- **스크린샷 형식**: BMP에서 PNG로 변경 (GDI+ 인코더)
- **컬러 스타일 셔플**: 순차 순환에서 랜덤 셔플로 변경
- **설정 대화상자 재설계**: Zoom Speed 슬라이더, Color Style 콤보박스, Overlay/Clock 체크박스
- **성능**: CPU/GPU 최적화 (SIMD 조기 거절, FMA3, one-pass 렌더링, 조건부 orbit trap)
- **부드러운 interior 컬러링**: 팔레트 주파수 및 진폭 감소로 대비 완화
- **스마트 인스톨러**: 설치/미리보기/제거 옵션, 버전 비교
- **Cross-edition 정책**: Full/Lite 상호 배타적 설치 (다른 에디션 자동 제거)
- **정적 CRT**: Visual C++ 런타임 의존성 제거

### 버그 수정
- desk.cpl 미리보기 창 검은 화면 시작 수정 (ZoomAnimator SkipFadeIn)
- 내부/외부 컬러 경계 불연속 수정
- 줌 방향 전환 시 순간 이동 현상 수정
- 설정 다이얼로그 열린 동안 마우스 커서 표시되지 않던 문제 수정
- 패닝 속도로 인한 줌아웃 현상 수정
- 줌 다양성 개선 (RNG 시드 및 타겟 오프셋 확대)

## v1.1.0 (2026-03-01)

### 신규 기능
- **6가지 모노크롬 컬러 채널**: Red, Green, Blue, Cyan, Magenta, Gold

### 개선
- 줌 탐색 좌표 25개에서 100개로 확장 (15개 카테고리)
- PCA 곡률 필터링: 시각적으로 단조로운 경계 자동 건너뜀
- 자체 포함 인스톨러

## v1.0.0 (2026-02-28)

### 최초 릴리스
- **만델브로 프랙탈 스크린세이버**: 실시간 줌 애니메이션
- **GPU 렌더링**: D3D11 Compute Shader, double-float 에뮬레이션 (~48비트 정밀도)
- **CPU 렌더링**: AVX2 SIMD 4픽셀 동시 계산, 멀티스레드
- **2x2 슈퍼샘플링 안티앨리어싱**
- **지수적 줌**: 적응형 패닝 + 경계 추적
- **설정 다이얼로그**: Force CPU, Zoom Speed, Color Style, Overlay/Clock
- **Windows 스크린세이버 표준 인터페이스**: /s, /c, /p 지원
