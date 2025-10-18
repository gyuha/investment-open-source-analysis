# investment-open-source-analysis

투자 관련 오픈소스 프로젝트들을 체계적으로 분석하여 금융 데이터 처리, AI 기반 투자 전략, 그리고 자동화된 트레이딩 시스템의 베스트 프랙티스를 학습하고 재사용 가능한 컴포넌트를 발굴하는 프로젝트입니다.

## 프로젝트 개요

본 리포지토리는 금융/투자 도메인의 주요 오픈소스 프로젝트들을 git submodule로 관리하며, 각 프로젝트의 아키텍처, 기술 스택, 구현 방식을 심층 분석합니다.

## 분석 대상 프로젝트 (Git Submodules)

### 1. Finance
- **Repository**: [shashankvemuri/Finance](https://github.com/shashankvemuri/Finance)
- **설명**: 오픈소스 금융 데이터 분석 도구
- **분석 초점**:
  - 금융 데이터 수집 및 처리 파이프라인
  - 기술적 지표 계산 알고리즘
  - 데이터 시각화 기법

### 2. ChatGPT Micro Cap Experiment
- **Repository**: [LuckyOne7777/ChatGPT-Micro-Cap-Experiment](https://github.com/LuckyOne7777/ChatGPT-Micro-Cap-Experiment)
- **설명**: ChatGPT를 활용한 주식 분석 및 트레이딩 봇
- **분석 초점**:
  - LLM 기반 시장 분석 방법론
  - 자연어 처리를 통한 투자 의사결정 프로세스
  - 프롬프트 엔지니어링 전략

### 3. Cluefin
- **Repository**: [kgcrom/cluefin](https://github.com/kgcrom/cluefin)
- **설명**: 투자자가 금융 의사결정을 분석, 자동화, 최적화할 수 있도록 돕는 파이썬 툴킷
- **분석 초점**:
  - 모듈화된 투자 도구 아키텍처
  - 백테스팅 및 성과 분석 시스템
  - API 통합 및 데이터 추상화 레이어

### 4. Kronos
- **Repository**: [shiyu-coder/Kronos](https://github.com/shiyu-coder/Kronos)
- **설명**: 전 세계 45개 이상의 거래소 데이터로 학습된 금융 캔들스틱(K-line) 파운데이션 모델
- **분석 초점**:
  - 시계열 데이터 기반 머신러닝 모델 구조
  - 대규모 금융 데이터셋 처리 방법
  - 모델 학습 및 추론 파이프라인

## 분석 방향

### 기술 분석
- **프로그래밍 언어 및 프레임워크**: 각 프로젝트의 기술 스택 비교
- **아키텍처 패턴**: 설계 원칙 및 코드 구조 분석
- **의존성 관리**: 주요 라이브러리 및 외부 서비스 활용 방식

### 기능 분석
- **데이터 소스**: 금융 데이터 API 및 제공자 비교
- **분석 알고리즘**: 투자 전략 및 신호 생성 로직
- **자동화 수준**: 수동/반자동/완전자동 트레이딩 구현 방식

### 비교 평가
- **코드 품질**: 테스트 커버리지, 문서화 수준, 유지보수성
- **확장성**: 새로운 전략 추가, 다중 자산 지원, 커스터마이징 용이성
- **실용성**: 실제 운용 가능성 및 리스크 관리 기능

## 기대 성과

1. **지식 베이스 구축**: 투자 자동화 시스템 개발에 필요한 핵심 개념 정리
2. **컴포넌트 라이브러리**: 재사용 가능한 코드 패턴 및 모듈 추출
3. **베스트 프랙티스**: 각 프로젝트의 장단점 분석을 통한 개선 방향 도출
4. **통합 전략**: 여러 프로젝트의 강점을 결합한 새로운 솔루션 설계

## 시작하기

### Submodule 초기화

이 리포지토리를 클론한 후 submodule을 초기화하려면:

```bash
# 리포지토리 클론
git clone <this-repository-url>
cd investment-open-source-analysis

# Submodule 초기화 및 다운로드
git submodule update --init --recursive
```

### 개별 Submodule 업데이트

```bash
# 모든 submodule 업데이트
git submodule update --remote

# 특정 submodule만 업데이트
cd Finance
git pull origin main
```

## 라이선스

각 submodule 프로젝트는 원본 리포지토리의 라이선스를 따릅니다. 자세한 내용은 각 프로젝트의 LICENSE 파일을 참조하세요.