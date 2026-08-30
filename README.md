# GPT Work Director

제품의 목표와 방향을 책임지고, 필요한 시니어 역할을 구성한 뒤 세부 실행을 역할별 ChatGPT 대화에 위임하는 제품 총괄 스킬입니다.

핵심은 간단합니다.

- 현재 스킬을 사용하는 대화는 **제품 총괄**입니다.
- 제품 하나당 **ChatGPT 프로젝트 하나**를 기본 협업 공간으로 사용합니다.
- 총괄을 제외한 시니어와 하위 작업자는 각각 별도의 **ChatGPT Chat**이 담당합니다.
- 총괄은 방향·우선순위·역할·의사결정을 관리하고, 조사·디자인·구현·테스트 같은 세부 작업은 직접 하지 않습니다.

## 이런 상황에 사용하세요

- 새 제품이나 복합적인 기능을 처음부터 조직적으로 추진할 때
- 기획, 디자인, 개발, 품질 등 여러 전문 역할이 함께 필요한 때
- 역할 중복을 줄이고 각 역할의 책임과 보고 체계를 분명히 하고 싶을 때
- 여러 ChatGPT 대화를 하나의 제품 조직처럼 운영하고 싶을 때

다음 상황에는 적합하지 않습니다.

- 한 명의 전문가만 필요한 단일 작업
- 코드, 디자인, 문서 등 세부 산출물을 현재 대화에서 바로 만들어야 하는 작업
- 여러 역할이나 역할별 Chat을 운영할 필요가 없는 간단한 요청

## 운영 구조

```text
사용자
└── 제품 총괄: gpt-work-director를 사용하는 현재 대화
    ├── 시니어 역할 A: 별도의 ChatGPT Chat
    │   ├── 하위 작업자 A1: 별도의 ChatGPT Chat
    │   └── 하위 작업자 A2: 별도의 ChatGPT Chat
    └── 시니어 역할 B: 별도의 ChatGPT Chat
        └── 하위 작업자 B1: 별도의 ChatGPT Chat
```

총괄은 시니어를 관리하고, 각 시니어가 자신의 하위 작업자를 구성하고 관리합니다. 총괄이 하위 작업자에게 일상 업무를 직접 지시하지 않습니다.

## 설치

### npx skills 사용

Node.js와 npm이 준비되어 있다면 다음 명령으로 Codex의 사용자 전역 스킬에 추가할 수 있습니다.

```bash
npx skills add Ronaldony/gpt-work-director --global --agent codex
```

현재 프로젝트에서만 사용하려면 `--global`을 빼고 실행합니다.

```bash
npx skills add Ronaldony/gpt-work-director --agent codex
```

전체 GitHub 주소를 사용해도 됩니다.

```bash
npx skills add https://github.com/Ronaldony/gpt-work-director --global --agent codex
```

저장소가 비공개이므로 명령을 실행하는 환경에 이 저장소를 읽을 수 있는 GitHub 인증이 설정되어 있어야 합니다. 설치 후 목록을 확인하거나 나중에 업데이트하려면 다음 명령을 사용하세요.

```bash
npx skills list --global --agent codex
npx skills check
npx skills update
```

`npx skills`의 옵션과 동작에 관한 자세한 내용은 [Skills CLI 문서](https://www.skills.sh/docs/cli)를 참고하세요.

### Skill Installer 사용

Codex 안에서 자연어로 설치하고 싶다면 다음과 같이 요청할 수 있습니다.

```text
$skill-installer 다음 GitHub 저장소에서 gpt-work-director 스킬을 설치해줘:
https://github.com/Ronaldony/gpt-work-director
```

저장소가 비공개라면 설치하는 환경에서 해당 GitHub 저장소에 접근할 수 있어야 합니다.

### 직접 설치

Windows PowerShell:

```powershell
New-Item -ItemType Directory -Force -Path "$HOME\.agents\skills"
git clone https://github.com/Ronaldony/gpt-work-director.git "$HOME\.agents\skills\gpt-work-director"
```

macOS 또는 Linux:

```bash
mkdir -p "$HOME/.agents/skills"
git clone https://github.com/Ronaldony/gpt-work-director.git "$HOME/.agents/skills/gpt-work-director"
```

Codex는 설치된 스킬을 자동으로 감지합니다. 목록에 바로 나타나지 않으면 Codex를 다시 시작하세요.

## 빠른 시작

### 1. 제품용 ChatGPT 프로젝트 준비

제품 하나를 관리할 ChatGPT 프로젝트를 만들거나 기존 프로젝트를 선택합니다. 제품 공통 지침, 확정된 방향, 역할별 Chat과 중요한 의사결정 기록을 이 프로젝트 안에 모읍니다.

### 2. 스킬 호출

Codex에서 `$gpt-work-director`를 명시하고 제품 정보를 전달합니다.

```text
$gpt-work-director

제품: 소규모 팀을 위한 고객 피드백 관리 서비스
목표: 여러 채널의 피드백을 모아 우선순위를 정할 수 있는 MVP 출시
대상 사용자: 5~20명 규모의 SaaS 제품팀
성공 기준: 6주 안에 핵심 사용자 5개 팀이 주 1회 이상 사용
제약: 개발자 2명, 디자이너 1명, 외부 유료 도구 최소화

먼저 제품 헌장을 정리하고, 꼭 필요한 시니어 역할만 근거와 함께 제안해줘.
```

정보가 부족하면 억지로 채우지 않아도 됩니다. 스킬은 결과를 바꾸는 핵심 정보만 질문하고, 아직 정해지지 않은 값은 `미정`으로 관리합니다.

### 3. 제안된 시니어 역할 검토

총괄은 역할마다 다음을 설명합니다.

- 책임질 제품 수준의 결과
- 지금 필요한 이유와 역할이 없을 때의 위험
- 다른 역할과 겹치지 않는 의사결정 권한
- 필요한 입력과 제공할 출력
- 역할을 축소하거나 종료할 조건

역할 수를 늘리는 것이 목표가 아닙니다. 필요성이 약하거나 중복되는 역할은 만들지 않습니다.

### 4. 역할별 Chat 구성

승인된 시니어마다 같은 ChatGPT 프로젝트 안에 별도 Chat을 만듭니다. 총괄이 제공한 `시니어 역할 헌장`을 해당 Chat의 첫 메시지로 전달합니다.

Chat 또는 프로젝트를 자동으로 만들 수 있는 접근 수단이 없다면, 스킬은 대신 다음을 제공합니다.

- 생성할 프로젝트와 역할별 Chat 목록
- 권장 Chat 이름
- 각 Chat에 그대로 붙여넣을 역할 지침

### 5. 보고와 의사결정

작업은 `총괄 → 시니어 → 하위 작업자` 순서로 내려가고, 결과와 위험은 반대로 올라옵니다. 총괄은 시니어의 통합 보고를 바탕으로 승인, 수정, 보류, 역할 보강 또는 종료를 결정합니다.

## 스킬이 만드는 관리 산출물

- 제품 헌장
- 시니어 역할 필요성 기록
- 시니어 역할 헌장
- 하위 작업자 작업 브리프
- 시니어 상태 보고
- 총괄 수용 판단

각 산출물의 바로 쓸 수 있는 형식은 [운영 템플릿](references/operating-templates.md)에 있습니다.

## 사용 원칙 한눈에 보기

| 주체 | 책임 | 하지 않는 일 |
| --- | --- | --- |
| 제품 총괄 | 제품 방향, 우선순위, 역할 구성, 자원 배분, 의존성, 승인 | 조사·설계·구현·테스트 등 세부 실행 |
| 시니어 | 담당 영역의 작업 분해, 하위 역할 배정, 결과 검토와 통합 | 제품 전체 방향의 임의 변경 |
| 하위 작업자 | 배정된 전문 작업 수행과 근거 보고 | 다른 역할의 관리 또는 제품 수준 결정 |

범위, 비용, 일정 또는 외부 영향이 달라지는 중요한 결정은 사용자의 승인 없이 확정하지 않습니다.

## 저장소 구성

```text
gpt-work-director/
├── README.md
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    └── operating-templates.md
```

- [`SKILL.md`](SKILL.md): 스킬의 핵심 지침과 호출 조건
- [`agents/openai.yaml`](agents/openai.yaml): 표시 이름, 설명, 기본 호출 프롬프트
- [`references/operating-templates.md`](references/operating-templates.md): 역할 임명, 위임, 보고, 승인 템플릿

## 문제가 있을 때

- 스킬이 보이지 않으면 설치 경로가 `.agents/skills/gpt-work-director`인지 확인한 뒤 Codex를 다시 시작하세요.
- 자동으로 선택되지 않으면 프롬프트 첫 줄에 `$gpt-work-director`를 명시하세요.
- 역할별 Chat이 자동 생성되지 않으면 총괄에게 `각 Chat에 붙여넣을 역할 지침을 만들어줘`라고 요청하세요.
- 총괄이 세부 작업을 직접 하려 하면 `세부 실행은 담당 시니어에게 재위임해줘`라고 요청하세요.

스킬의 구조와 설치 위치에 관한 자세한 내용은 [OpenAI 공식 Build skills 문서](https://developers.openai.com/codex/skills)를 참고하세요.

