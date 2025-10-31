# Tensortrade 분석 보고서

## 1. 프로젝트 개요

### 1.1. 프로젝트의 목적과 기능

Tensortrade는 강화 학습을 사용하여 강력한 트레이딩 알고리즘을 구축, 훈련, 평가 및 배포하기 위한 오픈 소스 파이썬 프레임워크입니다. 이 프레임워크는 구성 가능성과 확장성에 중점을 두어 단일 CPU에서의 간단한 트레이딩 전략부터 HPC 기계 분산 환경에서 실행되는 복잡한 투자 전략까지 확장할 수 있도록 설계되었습니다.

### 1.2. 문제 정의 및 해결 방법

금융 시장에서 수익성 있는 트레이딩 전략을 개발하고 테스트하는 것은 복잡하고 시간이 많이 소요되는 과정입니다. 전통적인 방법에만 의존할 경우, 변화하는 시장 상황에 동적으로 적응하는 전략을 만들기가 어렵습니다.

Tensortrade는 강화 학습을 트레이딩 문제에 적용하여 이 문제를 해결합니다. 에이전트가 시뮬레이션된 트레이딩 환경과 상호작용하며 시행착오를 통해 학습함으로써, 시장의 패턴을 발견하고 수익을 극대화하는 동적인 전략을 개발할 수 있습니다.

### 1.3. 핵심 기능

- **모듈성**: 환경의 모든 부분이 재사용 가능한 구성 요소로 분리되어 있어, 커뮤니티에서 구축한 일반적인 구성 요소를 활용하면서 독점적인 기능은 비공개로 유지할 수 있습니다. (예: `ActionScheme`, `RewardScheme`, `Observer`)
- **확장성**: 새로운 모듈을 클래스와 함수로 쉽게 추가할 수 있어 고급 연구 및 프로덕션 사용에 적합합니다.
- **사용자 친화성**: 일관되고 간단한 API를 제공하여 일반적인 사용 사례에 필요한 사용자 작업을 최소화합니다.
- **다양한 구성 요소**: 거래소, 피처 파이프라인, 액션 및 보상 체계, 트레이딩 에이전트, 성과 보고서 등 다양한 모듈을 조합하여 새로운 트레이딩 환경을 만들 수 있습니다.
- **기존 생태계 활용**: `numpy`, `pandas`, `gym`, `keras`, `tensorflow`와 같은 기존 라이브러리를 활용하여 고품질 데이터 파이프라인과 학습 모델을 유지합니다.

### 1.4. 대상 사용자 및 사용 사례

- **개발자**: 강화 학습을 이용한 트레이딩 봇을 개발하려는 소프트웨어 엔지니어.
- **퀀트 분석가**: 복잡한 금융 시장에서 알파를 찾기 위해 새로운 트레이딩 전략을 빠르고 효율적으로 테스트하려는 연구원.
- **학생 및 교육자**: 강화 학습 및 알고리즘 트레이딩의 개념을 실제 데이터에 적용하여 배우고 가르치는 데 사용.

## 2. 기술 아키텍처

### 2.1. 고수준 시스템 아키텍처

```mermaid
graph TD
    subgraph User
        A[RL Agent]
    end

    subgraph Trading Environment (TensorTrade)
        B[ActionScheme]
        C[TradingEnv]
        D[Observer]
        E[RewardScheme]
        F[DataFeed]
        G[Portfolio]
    end

    subgraph Data Sources
        H[Historical Data]
        I[Live Data]
    end

    A -- Action --> B
    B -- Processed Action --> C
    C -- Observation Request --> D
    D -- Observation --> A
    C -- Reward Request --> E
    E -- Reward --> A
    F -- Market Data --> D
    F -- Market Data --> G
    H --> F
    I --> F
    C -- Manages --> G
```

### 2.2. 기술 스택 및 종속성

- **언어**: Python (>= 3.11.9)
- **핵심 라이브러리**:
    - **TensorFlow / Keras**: 강화 학습 모델의 신경망을 구축하고 훈련하는 데 사용됩니다.
    - **Pandas**: 금융 시계열 데이터를 효율적으로 처리하고 조작하는 데 사용됩니다.
    - **NumPy**: 수치 연산을 위해 사용됩니다.
    - **Gymnasium**: OpenAI Gym의 후속으로, 강화 학습 에이전트를 위한 표준 환경 API를 제공합니다.
    - **Plotly / Matplotlib**: 트레이딩 성과 및 데이터를 시각화하는 데 사용됩니다.
- **주요 종속성**: `numpy`, `pandas`, `gymnasium`, `pyyaml`, `stochastic`, `tensorflow`, `plotly`, `deprecated`.

### 2.3. 디자인 패턴 및 아키텍처 결정사항

- **전략 패턴 (Strategy Pattern)**: `ActionScheme`, `RewardScheme`, `Observer` 등의 구성 요소를 `TradingEnv`에서 분리하여 사용자가 다양한 전략을 쉽게 교체하고 조합할 수 있도록 설계되었습니다. 예를 들어, `SimpleReward` 대신 `RiskAdjustedReward`를 사용하여 보상 계산 방식을 변경할 수 있습니다.
- **컴포넌트 기반 아키텍처**: 프레임워크의 각 부분(Exchange, Wallet, Feed 등)이 독립적인 컴포넌트로 작동하여 재사용성과 유지보수성을 높입니다.
- **이벤트 기반 시스템**: `Clock` 컴포넌트를 중심으로 시간이 흐르면서 각 단계(step)마다 이벤트가 발생하고, 이에 따라 각 컴포넌트가 업데이트되는 구조입니다.

### 2.4. 구성 요소 상호작용 및 데이터 흐름

1.  **데이터 피드 (DataFeed)**: Pandas DataFrame이나 다른 소스로부터 시계열 데이터를 받아 `Stream`으로 변환하고, 이를 환경에 공급합니다.
2.  **관찰자 (Observer)**: `DataFeed`로부터 데이터를 받아 에이전트가 사용할 관찰(observation) 공간을 생성합니다. 여기에는 가격, 거래량, 기술적 분석 지표 등이 포함될 수 있습니다.
3.  **에이전트 (Agent)**: `Observer`가 제공한 관찰을 바탕으로 `ActionScheme`에 따라 행동(action)을 결정합니다.
4.  **액션 স্কিম (ActionScheme)**: 에이전트의 행동을 해석하여 `Portfolio` 내에서 실제 주문(order)을 생성하거나 포지션을 변경합니다.
5.  **포트폴리오 (Portfolio)**: 여러 `Wallet`과 `Exchange`를 관리하며, 자산, 주문, 거래 내역을 추적합니다. `ActionScheme`의 주문을 받아 `Exchange`를 통해 실행합니다.
6.  **보상 체계 (RewardScheme)**: 행동이 실행된 후, `Portfolio`의 상태 변화(예: 순자산 가치 변화)를 기반으로 보상(reward)을 계산합니다.
7.  **환경 (TradingEnv)**: 이 모든 구성 요소를 통합하고 OpenAI Gym 호환 인터페이스(`step`, `reset` 메서드)를 제공하여 에이전트와의 상호작용을 관리합니다.

## 3. 프로젝트 구조

### 3.1. 디렉토리별 설명

- **`/source/tensortrade/`**: 핵심 소스 코드가 위치합니다.
    - `agents/`: A2C, DQN 등 사전 구축된 강화 학습 에이jent가 포함됩니다.
    - `core/`: `Clock`, `Component` 등 프레임워크의 기본 구성 요소가 포함됩니다.
    - `data/`: `CryptoDataDownload`와 같이 외부에서 데이터를 가져오는 기능이 포함됩니다.
    - `env/`: `TradingEnv`와 `ActionScheme`, `RewardScheme` 등 환경 관련 구성 요소가 포함됩니다.
    - `feed/`: 시계열 데이터를 환경에 공급하는 `DataFeed` 및 `Stream` 관련 코드가 포함됩니다.
    - `oms/`: 주문 관리 시스템(Order Management System)으로, `wallets`, `exchanges`, `orders`, `instruments` 등을 관리합니다.
    - `stochastic/`: 확률적 과정을 생성하여 시뮬레이션 데이터를 만드는 데 사용됩니다.
- **`/source/docs/`**: Sphinx를 사용한 프로젝트 문서가 포함됩니다.
- **`/source/examples/`**: Jupyter Notebook 형태의 다양한 사용 예제가 포함되어 있습니다.
- **`/source/tests/`**: `pytest`를 사용한 단위 테스트 및 통합 테스트 코드가 위치합니다.

### 3.2. 프로젝트 계층 구조 (Mermaid)

```mermaid
graph TD
    root[/source/tensortrade]
    root --> core
    root --> data
    root --> env
    root --> feed
    root --> oms
    root --> agents
    root --> stochastic

    env --> generic
    env --> default

    oms --> wallets
    oms --> exchanges
    oms
 --- orders
    oms --> instruments
    oms --> services
```

## 4. 설치 및 설정

### 4.1. 전제 조건 및 시스템 요구사항

- **운영체제**: OS 무관 (Linux, macOS, Windows)
- **Python**: 버전 3.11.9 이상

### 4.2. 단계별 설치 가이드

1.  **PyPI를 통한 설치 (권장)**:
    ```bash
    pip install tensortrade
    ```

2.  **소스 코드에서 직접 설치 (최신 기능)**:
    ```bash
    pip install git+https://github.com/tensortrade-org/tensortrade.git
    ```

3.  **로컬 클론 후 설치**:
    ```bash
    git clone https://github.com/tensortrade-org/tensortrade.git
    cd tensortrade
    pip install -r requirements.txt
    ```

### 4.3. 구성 지침

Tensortrade는 구성 파일 대신 코드 내에서 직접 구성 요소를 조합하여 환경을 설정합니다. 주요 구성 단계는 다음과 같습니다.

1.  **데이터 로드**: Pandas DataFrame으로 과거 데이터를 로드합니다.
2.  **피처 엔지니어링**: `ta` 라이브러리 등을 사용하여 기술적 분석 지표를 추가합니다.
3.  **`Stream` 생성**: 가격, 지표 등 각 데이터를 `Stream` 객체로 만듭니다.
4.  **`DataFeed` 생성**: 생성된 `Stream`들을 모아 `DataFeed`를 구성합니다.
5.  **`Exchange` 및 `Portfolio` 설정**: 시뮬레이션할 거래소와 초기 자산을 포함하는 `Portfolio`를 정의합니다.
6.  **`ActionScheme` 및 `RewardScheme` 선택**: تريد링 전략과 보상 계산 방식을 선택합니다.
7.  **`TradingEnv` 생성**: 위의 모든 구성 요소를 조합하여 최종 트레이딩 환경을 생성합니다.

### 4.4. 일반적인 문제 해결

- **의존성 충돌**: 가상 환경(`venv` 또는 `conda`)을 사용하여 프로젝트별로 의존성을 격리하는 것이 좋습니다.
- **TensorFlow GPU 설정**: GPU 버전 TensorFlow를 사용하려면 NVIDIA 드라이버, CUDA, cuDNN을 시스템에 맞게 설치해야 합니다.
- **데이터 형식 오류**: `DataFeed`에 전달되는 데이터는 `numpy` 배열이나 리스트 형태여야 하며, `dtype`이 올바르게 지정되었는지 확인해야 합니다.

## 5. 사용 가이드

### 5.1. 기본 사용 예제

다음은 기본적인 `TradingEnv`를 설정하고 실행하는 예제입니다.

```python
import pandas as pd
import tensortrade.env.default as default
from tensortrade.data.cdd import CryptoDataDownload
from tensortrade.feed.core import Stream, DataFeed
from tensortrade.oms.instruments import USD, BTC
from tensortrade.oms.wallets import Wallet, Portfolio
from tensortrade.oms.exchanges import Exchange
from tensortrade.oms.services.execution.simulated import execute_order

# 1. 데이터 다운로드 및 피드 생성
cdd = CryptoDataDownload()
data = cdd.fetch("Bitfinex", "USD", "BTC", "1h")
feed = DataFeed([
    Stream.source(list(data['close']), dtype="float").rename("USD-BTC")
])

# 2. 거래소 및 포트폴리오 설정
bitfinex = Exchange("bitfinex", service=execute_order)(
    feed.stream("USD-BTC")
)

portfolio = Portfolio(USD, [
    Wallet(bitfinex, 10000 * USD),
    Wallet(bitfinex, 1 * BTC)
])

# 3. 액션 및 보상 체계 정의
action_scheme = "managed-risk" # 0-1 사이의 값으로 투자 비율을 정함
reward_scheme = "simple" # 순자산의 변화율로 보상 계산

# 4. 환경 생성
env = default.create(
    portfolio=portfolio,
    action_scheme=action_scheme,
    reward_scheme=reward_scheme,
    feed=feed,
    window_size=15
)

# 5. 환경 사용
obs, info = env.reset()
done = False
while not done:
    action = env.action_space.sample() # 랜덤 액션
    obs, reward, terminated, truncated, info = env.step(action)
    done = terminated or truncated
```

### 5.2. API 문서

- **`tensortrade.env.TradingEnv`**: 핵심 환경 클래스.
    - `.step(action)`: 환경을 한 단계 진행합니다.
    - `.reset()`: 환경을 초기화합니다.
    - `.render()`: 환경 상태를 시각화합니다.
- **`tensortrade.feed.core.DataFeed`**: 데이터 스트림을 관리하는 클래스.
- **`tensortrade.oms.portfolio.Portfolio`**: 자산 포트폴리오를 관리하는 클래스.
- **`tensortrade.env.default.create()`**: 사전 정의된 구성 요소로 환경을 쉽게 생성하는 팩토리 함수.

자세한 API 문서는 공식 문서 사이트 또는 소스 코드 내 docstring을 통해 확인할 수 있습니다.

## 6. 개발 지침

### 6.1. 개발 환경 설정

1.  프로젝트를 클론합니다.
2.  테스트 및 문서 빌드에 필요한 추가 의존성을 설치합니다.
    ```bash
    pip install -e .[tests,docs]
    ```

### 6.2. 코드 스타일 및 규칙

- **PEP8**: PEP8 스타일 가이드를 따릅니다. `autopep8`와 `pytest-pep8`를 사용하여 코드 스타일을 검사하고 수정할 수 있습니다.
- **Docstrings**: 모든 새로운 함수와 클래스에는 MarkDown 형식의 `docstring`을 작성해야 합니다. (`Arguments`, `Returns`, `Raises` 섹션 포함)

### 6.3. 테스트 절차 및 커버리지

- **테스트 실행**: `pytest`를 사용하여 테스트를 실행합니다.
    ```bash
    pytest tests/
    ```
- **커버리지**: `coverage` 모듈을 사용하여 코드 커버리지를 확인합니다. Pull Request는 코드 커버리지를 감소시키지 않아야 합니다.

### 6.4. 기여 가이드라인

- **Issue**: 버그 리포트나 기능 제안은 GitHub Issue를 통해 제출합니다.
- **Pull Request (PR)**:
    1.  기능 변경 시, 먼저 GitHub Issue를 통해 디자인을 논의합니다.
    2.  코드를 작성하고, `docstring`과 문서를 업데이트합니다.
    3.  단위 테스트를 작성하고 모든 테스트가 통과하는지 확인합니다.
    4.  적절하고 설명적인 커밋 메시지를 사용합니다.
    5.  PR을 제출합니다.

## 7. 추가 정보

### 7.1. 성능 고려사항

- **데이터 처리**: 대용량 시계열 데이터 처리를 위해 `pandas`와 `numpy`의 벡터화 연산을 최대한 활용합니다.
- **분산 학습**: 복잡한 전략이나 대규모 하이퍼파라미터 튜닝을 위해 `Ray`와 같은 분산 컴퓨팅 프레임워크와 통합할 수 있습니다.

### 7.2. 보안 고려사항

- **API 키 관리**: 실제 거래소와 연동할 경우, API 키와 비밀 키를 환경 변수나 보안 저장소를 통해 안전하게 관리해야 합니다. 코드에 직접 하드코딩하지 않도록 주의해야 합니다.
- **백테스팅의 한계**: 시뮬레이션된 환경(백테스팅)의 결과는 실제 시장에서의 성과를 보장하지 않습니다. 슬리피지, 거래 수수료, 유동성 등의 현실적인 제약을 `SlippageModel` 등을 통해 최대한 반영해야 합니다.

### 7.3. 프로젝트 로드맵 및 향후 계획

- 명시적인 로드맵은 없으나, GitHub 프로젝트 보드에서 현재 진행 중인 이슈와 추가될 기능을 확인할 수 있습니다.
- 커뮤니ティ의 제안을 통해 지속적으로 새로운 기능(예: 추가 거래소 지원, 새로운 RL 에이전트)이 추가될 예정입니다.

### 7.4. 라이선스 및 저작권

- **라이선스**: Apache License 2.0
- **저작권**: Copyright 2021 The TensorTrade Authors.
