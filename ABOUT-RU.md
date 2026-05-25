
В ASPICE важно иметь цепочку:
```
Stakeholder Requirement
  → System Requirement SYS.2
    → System Architecture SYS.3
      → Software Requirement SWE.1
        → Software Architecture SWE.2
          → Software Detailed Design SWE.3
            → Code
              → Test Case
                → Test Result
```
Это типичная **ALM-трассировка**.

Она показывает, как требования заинтересованных сторон (Stakeholder Requirement) связаны с системными требованиями (System Requirement), архитектурой системы (System Architecture), программными требованиями (Software Requirement), архитектурой программного обеспечения (Software Architecture), детальным дизайном программного обеспечения (Software Detailed Design), кодом, тестовыми случаями и результатами тестов.

В Jira это можно смоделировать через issue types и links:
```
Epic → Story → Task → Bug → Test
```

Но это будет скорее **управление work items**, а не полноценный engineering lifecycle, если нет строгой модели требований, версий, baseline, review, approval, verification и traceability matrix.

В ALM-трассировке важно поддерживать связь между всеми уровнями требований и артефактами разработки, чтобы обеспечить прозрачность и контроль над процессом разработки. Это позволяет легко отслеживать изменения, выявлять проблемы и обеспечивать соответствие конечного продукта требованиям заинтересованных сторон.

### Где Jira подходит хорошо
```
Backlog
Sprint planning
Tasks
Bugs
User stories
Workflow statuses
Assignees
Priorities
Agile boards
Release tracking
```

### Где Jira слабее как ALM-инструмент
```
Requirements engineering
Formal traceability
Baseline management
Impact analysis
Verification evidence
Compliance with ASPICE / ISO 26262
Requirements review and approval flow
Test coverage matrix
```

**Jira** — это инструмент для управления задачами и agile-разработкой.

**ALM** — это более широкая система управления всем жизненным циклом продукта, включая требования, архитектуру, код, тесты, релизы, изменения и трассировку.

Для твоего кейса с ASPICE requirements generation правильнее думать не просто в терминах Jira, а в терминах **ALM work item model**:
```
Requirement
System Requirement
Software Requirement
Architecture Element
Test Case
Test Run
Risk
Change Request
Bug
Release
```

Каждый из этих типов work item может иметь свои атрибуты, связи, статусы и workflow, которые поддерживают процесс разработки в соответствии с ASPICE.
```
Stakeholder / Customer Requirements
  ↓ input / source
System Requirements (SYS.2)
  ↓
System Architecture (SYS.3)
  ↓
Software Requirements (SWE.1)
  ↓
Software Architecture (SWE.2)
  ↓
Software Detailed Design (SWE.3)
  ↓
Code
  ↓
Test Cases / Verification Evidence
```

То есть:

**Stakeholder Requirement** — это то, что хочет customer/user/business/stakeholder.

**System Requirement SYS.2** — это уже инженерно оформленное системное требование, полученное из stakeholder/customer needs.

Это более детализированная ALM-трассировка, которая показывает, как требования заинтересованных сторон (Stakeholder / Customer Requirements) влияют на системные требования (System Requirements), которые затем определяют архитектуру системы (System Architecture). Архитектура системы влияет на программные требования (Software Requirements), которые в свою очередь определяют архитектуру программного обеспечения (Software Architecture) и детальный дизайн программного обеспечения (Software Detailed Design). Все эти артефакты в конечном итоге приводят к написанию кода и созданию тестовых случаев / доказательств верификации.

Для твоей канонической модели я бы хранил **Stakeholder Requirement** отдельно, потому что от него строится traceability:
```
stakeholder_requirement
  → system_requirement
    → system_architecture_element
      → software_requirement
      → test_case
```
Но в финальном ASPICE SYS.2 output не надо смешивать stakeholder requirement и system requirement в один тип. Лучше явно указывать связь:
```json
{
  "id": "SYS-REQ-001",
  "type": "system_requirement",
  "derived_from": ["STK-REQ-001"],
  "statement": "The system shall display a low-battery telltale when the battery state of charge is below the defined threshold."
}
```
Итого: да, отдельно, но как source / upstream requirement, а не как прямой заменитель SYS.2.


## Codebeamer и сценарности

Что обычно есть в Codebeamer
```
Stakeholder Requirements
System Requirements
Software Requirements
Architecture-related items
Risks
Test Cases
Test Runs
Bugs / Defects
Change Requests
Releases
Baselines
Traceability Matrix
```

Да, Codebeamer более “заточен под сценарии”, но не в смысле “рисовать бизнес-сценарии как BPMN”. Он заточен под инженерные lifecycle-сценарии:
```
requirement → review → approval → implementation → test case → test run → defect → change request → release
```

Пример сценария в Codebeamer

Допустим, есть требование:
```
The system shall display a low-battery warning.
```

В Codebeamer вокруг него можно построить цепочку:
```
Requirement created
  → reviewed by system engineer
    → approved
      → linked to software requirement
        → linked to test case
          → test executed
            → test failed
              → defect created
                → fix implemented
                  → test passed
                    → requirement verified
```

Вот это и есть “сценарность” Codebeamer: он хорошо поддерживает переходы состояний, связи между артефактами, проверки, роли, approval, traceability matrix.

Но важная оговорка

Codebeamer — это не совсем инструмент для описания use-case сценариев типа:
```
User opens menu
User selects setting
System displays confirmation
```
Такое можно хранить как требования, test cases, user requirements или structured work items, но главный фокус Codebeamer — не “сценарное моделирование поведения”, а управление жизненным циклом инженерных артефактов.

# #########################################################################################


# какая связь процессной модели и сценариев?

Процессная модель описывает “как организация работает”, а сценарий описывает “как конкретный случай проходит через этот процесс”.

То есть:
```
Process model = общая схема
Scenario = конкретный путь по этой схеме
```

Сценарий — это конкретная “история прохождения” одного объекта через процесс.

#### 1. Процессная модель

Процессная модель отвечает на вопросы:
```
Какие есть этапы?
Кто участвует?
Какие входы и выходы?
Какие артефакты создаются?
Какие проверки нужны?
Какие переходы между состояниями?
```

Например для requirements engineering:
```
Capture stakeholder need

  → Analyze need
  → Derive system requirement
  → Review requirement
  → Approve requirement
  → Link to architecture
  → Link to test case
  → Verify
  ```

Это **общий процесс**.


#### 2. Сценарии
Сценарии поддерживает сценарности, потому что он позволяет моделировать конкретные случаи прохождения артефактов через процесс.

**Сценарий** — это конкретная “история прохождения” одного объекта через процесс.

Например:
```
Stakeholder asks for low-battery warning
  → system engineer creates SYS.2 requirement
  → reviewer rejects it because threshold is unclear
  → engineer updates requirement
  → requirement is approved
  → software requirement is derived
  → test case is created
  → test fails
  → defect is created
  → defect is fixed
  → test passes
  → requirement becomes verified
```
Это уже сценарий.

##### Простая аналогия

**Процессная модель** — это карта дорог.

**Сценарий** — это конкретный маршрут по этой карте.

Например карта говорит:
```
Draft → Review → Approved → Implemented → Verified
```

А сценарий говорит:
```
REQ-001 was created by Anna,
reviewed by Mark,
rejected once,
updated,
approved,
linked to TC-014,
tested in release 1.2,
and verified.
```

#### В ALM это связано напрямую
В ALM-трассировке процессная модель определяет, какие артефакты и связи нужны для управления жизненным циклом продукта. Сценарии же показывают, как эти артефакты и связи используются в реальных случаях.

В ALM-системах вроде Codebeamer процессная модель обычно задаёт:
```
work item types
statuses
allowed transitions
roles
review rules
approval rules
traceability rules
required fields
```

А сценарии — это реальные проходы артефактов через эту модель:
```
Requirement scenario
Change request scenario
Defect scenario
Test execution scenario
Release scenario
Risk mitigation scenario
```

### Для твоего AI-пайплайна

Если ты делаешь генерацию требований из Confluence + code, то процессная модель нужна как каркас, чтобы AI не просто генерировал текст, а понимал:

```
какой тип артефакта создать
из какого источника
на каком уровне абстракции
какие связи добавить
какие проверки выполнить
какой следующий артефакт должен появиться
```

Например:

```
Input evidence from Confluence/code
  → candidate stakeholder need
  → derived SYS.2 requirement
  → linked software evidence
  → verification idea
  → review status = Draft
```

А сценарий будет конкретным примером:

```
From menu description in Confluence and display code,
AI derives a SYS.2 requirement for telltale visibility,
links it to related UI code,
suggests a test case,
and marks safety relevance as TBD.
```

#### Главное

**Процессная модель** задаёт правила и структуру.

**Сценарии** показывают, как эти правила применяются в конкретных случаях.

Для ASPICE/ALM лучше иметь оба слоя:

**Process model:**
```
What must happen according to the engineering process.
```

**Scenario:**
```
How one requirement, defect, risk, or test actually moves through that process.
```


## ASPICE scope

Процессная модель ASPICE описывает, какие инженерные процессы должны существовать и какие результаты они должны давать, а сценарии — это конкретные случаи применения этих процессов к требованиям, изменениям, дефектам, тестам или релизам.

### 1. ASPICE задаёт процессы, например:
```
SYS.1  Requirements Elicitation
SYS.2  System Requirements Analysis
SYS.3  System Architectural Design
SYS.4  System Integration and Integration Test
SYS.5  System Qualification Test

SWE.1  Software Requirements Analysis
SWE.2  Software Architectural Design
SWE.3  Software Detailed Design and Unit Construction
SWE.4  Software Unit Verification
SWE.5  Software Integration and Integration Test
SWE.6  Software Qualification Test
```

Это **процессная модель**.

Она отвечает:
```
Какие действия должны выполняться?
Какие артефакты должны появляться?
Какие связи между артефактами должны быть?
Как обеспечивается traceability?
Как выполняется verification?
Как управляются изменения?
```

### 2. Сценарий в ASPICE = конкретный проход через процессы
Например сценарий:
```
Customer wants low-battery warning
  → stakeholder need is captured
  → SYS.2 requirement is derived
  → SYS.3 architecture element is linked
  → SWE.1 software requirement is derived
  → SWE.2 software component is designed
  → SWE.3 code/unit is implemented
  → SWE.4 unit test is executed
  → SWE.5 integration test is executed
  → SYS.5 system qualification test verifies the behavior
  ```
  Это уже ***конкретный engineering scenario** внутри ASPICE.

### 3. ALM-трассировка = поддержка процесса и сценариев
#### Главное: ASPICE оценивает не сценарии, а процесс + evidence
ASPICE assessor обычно смотрит не “красивый сценарий”, а доказательства, что процесс работает:

```
есть требования
есть архитектура
есть тесты
есть трассировка
есть review
есть change management
есть consistency checks
есть verification evidence
```

Например для одного требования должна быть видна цепочка:
```
Stakeholder Requirement
  → SYS.2 System Requirement
    → SYS.3 System Architecture Element
      → SWE.1 Software Requirement
        → SWE.2 Software Architecture Element
          → SWE.3 Detailed Design / Unit
            → SWE.4 Unit Test
            → SWE.5 Software Integration Test
            → SYS.5 System Qualification Test
```
И для каждого элемента этой цепочки должны быть доказательства, что он существует, что он связан с предыдущим элементом, что он прошёл review/approval, что он был протестирован и что тесты были успешными.

```
Stakeholder Requirement
  → SYS.2 System Requirement
    → SYS.3 System Architecture Element
      → SWE.1 Software Requirement
        → SWE.2 Software Architecture Element
          → SWE.3 Detailed Design / Unit
            → SWE.4 Unit Test
            → SWE.5 Software Integration Test
            → SYS.5 System Qualification Test
```
Это и есть ALM-трассировка, которая поддерживает ASPICE процессы и позволяет демонстрировать соответствие требованиям и качество разработки.

Вот это и есть ASPICE-смысл сценария: не storytelling, а **демонстрация управляемого прохождения артефакта через V-model**.


### 4. Как это связано с Codebeamer / ALM

В Codebeamer или другой ALM-системе процессная модель может быть настроена так:
```
Work item types:
- Stakeholder Requirement
- System Requirement
- Software Requirement
- Architecture Item
- Test Case
- Test Run
- Defect
- Change Request
- Risk
```

Workflow statuses:
```
Draft → In Review → Approved → Implemented → Verified
```

Traceability rules:
```
Stakeholder Requirement must be linked to SYS.2
SYS.2 must be linked to SYS.3 and/or SWE.1
Requirement must be linked to Test Case
Test Case must have Test Run result
Defect must be linked to failed Test Run
```

Сценарии в Codebeamer будут конкретными примерами того, как один артефакт (например, SYS.2 requirement) проходит через эти статусы, получает связи, создаёт тесты, выполняет тесты, и как это всё документируется.

А сценарий — это конкретный пример:
```
REQ-001 created
REQ-001 reviewed
REQ-001 approved
REQ-001 linked to TC-001
TC-001 executed
TC-001 passed
REQ-001 marked verified
```


### 5. Для твоего AI-пайплайна

Если ты строишь AI-систему генерации требований, то в ASPICE-смысле тебе нужна не просто генерация текста, а генерация процессно-согласованных артефактов.

То есть не просто:
```json
{
  "requirement": "The system shall display a low-battery warning."
}
```

А лучше
```json
{
  "type": "system_requirement",
  "aspice_process": "SYS.2",
  "id": "SYS-REQ-001",
  "statement": "The system shall display a low-battery warning when the battery state of charge is below the configured threshold.",
  "derived_from": ["STK-REQ-001"],
  "linked_architecture": ["SYS-ARCH-001"],
  "verified_by": ["SYS-TC-001"],
  "status": "Draft",
  "review_state": "Not reviewed",
  "safety_relevance": "TBD",
  "evidence_sources": [
    "confluence: battery warning feature description",
    "code: display warning handler"
  ]
}
```

Такой формат уже ближе к ASPICE, потому что он содержит:
```
тип артефакта
процесс ASPICE
уровень V-model
источник
traceability
verification link
review state
evidence
```

#### Коротко:
В ASPICE:
```
Process model = правила инженерного процесса
Scenario = конкретный проход требования/дефекта/изменения/теста через этот процесс
Evidence = доказательство, что этот проход реально управляемый и трассируемый
```

Для твоего кейса лучше думать так:
```
AI не просто генерирует requirements.
AI помогает создать ALM/ASPICE-compatible chain of artifacts.
```

То есть:
```
Confluence + Code
  → Candidate Stakeholder Needs
  → SYS.2 Requirements
  → SYS.3 / SWE.1 links
  → Test Case ideas
  → Traceability evidence
  → Review-ready ALM items
```


## Конструктор сценариев для моделирования ASPICE
Можно сделать конструктор сценариев для моделирования ASPICE, который позволяет пользователю создавать и визуализировать сценарии прохождения требований, дефектов, изменений и тестов через процессы ASPICE.

Не как “рисовалку диаграмм”, а как конструктор ALM/ASPICE-сценариев, который может потом питать AI-пайплайн генерации требований.

Да, можно сделать **конструктор сценариев для моделирования ASPICE**. Более того, это может быть очень полезный инструмент: не просто рисовать процесс, а моделировать **как конкретный артефакт проходит через ASPICE V-model**.

ASPICE 4.0 официально описывает себя как **Process Reference Model / Process Assessment Model** и использует процессы, outcomes, base practices и information items как assessment indicators. То есть основа ASPICE — не “документы ради документов”, а управляемые процессы и доказуемые результаты процесса.


### Что именно моделировать
scenario-based ASPICE simulator.

Например пользователь собирает сценарий:
```
Customer Need
  → Stakeholder Requirement
  → SYS.2 System Requirement
  → SYS.3 System Architecture Element
  → SWE.1 Software Requirement
  → SWE.2 Software Architecture Element
  → SWE.3 Detailed Design / Unit
  → SWE.4 Unit Verification
  → SWE.5 Software Integration Test
  → SYS.5 System Qualification Test
```

И система проверяет:
```
Есть ли traceability?
Есть ли verification link?
Есть ли source evidence?
Есть ли review / approval?
Есть ли consistency check?
Есть ли test evidence?
Есть ли gap между SYS и SWE?
```

**Traceability** — один из ключевых принципов ASPICE: нужно уметь показать связи между work products, например от stakeholder request к system/software requirements, design, implementation и tests.

**Verification** — это доказательство, что требование было проверено через тесты или анализ.


### Как это может выглядеть в продукте

Можно сделать 3 слоя.

#### 1. Process Model Layer

Это шаблон ASPICE-процесса:
```
{
  "process": "SYS.2",
  "name": "System Requirements Analysis",
  "input_items": [
    "stakeholder_requirements",
    "system_context",
    "constraints"
  ],
  "output_items": [
    "system_requirements",
    "system_requirements_traceability",
    "verification_criteria"
  ],
  "required_checks": [
    "consistency_with_stakeholder_requirements",
    "verifiability",
    "traceability",
    "review_status"
  ]
}
```
Для SYS.2 цель — создать структурированный и проанализированный набор system requirements, согласованный со stakeholder requirements.


#### 2. Scenario Layer
Это конкретный сценарий прохождения:
```
{
  "scenario_id": "SCN-LOW-BATTERY-001",
  "title": "Low battery warning requirement lifecycle",
  "steps": [
    {
      "step": 1,
      "artifact_type": "stakeholder_requirement",
      "action": "capture_need",
      "output": "STK-REQ-001"
    },
    {
      "step": 2,
      "artifact_type": "system_requirement",
      "aspice_process": "SYS.2",
      "action": "derive_system_requirement",
      "input": ["STK-REQ-001"],
      "output": "SYS-REQ-001"
    },
    {
      "step": 3,
      "artifact_type": "test_case",
      "aspice_process": "SYS.5",
      "action": "define_qualification_test",
      "input": ["SYS-REQ-001"],
      "output": "SYS-TC-001"
    }
  ]
}
```

```
{
  "scenario_id": "SCN-001",
  "description": "Derive SYS.2 requirement from stakeholder need for low-battery warning",
  "steps": [
    {
      "step_id": "STEP-001",
      "action": "capture_stakeholder_need",
      "input": "Customer wants low-battery warning",
      "output": "STK-REQ-001"
    },
    {
      "step_id": "STEP-002",
      "action": "derive_sys2_requirement",
      "input": "STK-REQ-001",
      "output": "SYS-REQ-001"
    },
    {
      "step_id": "STEP-003",
      "action": "link_to_architecture",
      "input": "SYS-REQ-001",
      "output": ["SYS-ARCH-001"]
    },
    {
      "step_id": "STEP-004",
      "action": "create_test_case",
      "input": ["SYS-REQ-001", "SYS-ARCH-001"],
      "output": ["SYS-TC-001"]
    }
  ]
}
```

#### 3. Compliance / Gap Check Layer
Этот слой проверяет сценарий:
```
{
  "scenario_id": "SCN-LOW-BATTERY-001",
  "checks": [
    {
      "rule": "Every SYS.2 requirement must be derived from stakeholder input",
      "status": "passed"
    },
    {
      "rule": "Every SYS.2 requirement must have verification criteria",
      "status": "failed",
      "gap": "SYS-REQ-001 has no verification criteria"
    },
    {
      "rule": "Every system requirement must be linked to a system qualification test",
      "status": "warning",
      "gap": "SYS-REQ-001 has no linked SYS.5 test case"
    }
  ]
}
```


```
{
  "scenario_id": "SCN-001",
  "compliance_checks": [
    {
      "check_id": "CHK-001",
      "description": "Check traceability from SYS.2 to stakeholder requirement",
      "result": "PASS"
    },
    {
      "check_id": "CHK-002",
      "description": "Check if SYS.2 requirement is verifiable",
      "result": "PASS"
    },
    {
      "check_id": "CHK-003",
      "description": "Check if SYS.2 requirement is linked to architecture",
      "result": "PASS"
    },
    {
      "check_id": "CHK-004",
      "description": "Check if test case is defined for SYS.2 requirement",
      "result": "PASS"
    }
  ]
}
```

Такой конструктор сценариев позволит пользователю не просто рисовать процесс, а моделировать **конкретные случаи прохождения требований через ASPICE**, и видеть, где есть compliance gaps, что может быть очень полезно для обучения, планирования и подготовки к ASPICE assessment.


#### Почему это полезно именно для AI-пайплайна
Такой конструктор сценариев будет полезен для AI-пайплайна, потому что он задаёт структуру и правила, по которым AI должен генерировать требования и связанные артефакты. AI не будет просто генерировать текст, а будет создавать **ALM/ASPICE-compatible work items**, которые уже будут соответствовать процессной модели и сценариям ASPICE.

Например, если AI генерирует SYS.2 requirement, он будет знать, что ему нужно указать:
```
- derived_from stakeholder requirement
- linked architecture element
- verification criteria
- review status
```

И если AI генерирует тестовый случай, он будет знать, что ему нужно связать его с SYS.2 requirement и указать, что это SYS.5 qualification test.

**Твой AI тогда будет не просто генерировать requirement text, а работать по сценарию:**
```
1. Найти evidence в Confluence/code
2. Извлечь candidate stakeholder need
3. Сгенерировать SYS.2 requirement
4. Проверить уровень абстракции
5. Связать с source evidence
6. Предложить verification criteria
7. Предложить test case
8. Найти gaps
9. Подготовить item для ALM
```

То есть сценарий становится **управляющим шаблоном для AI-агента**.

### Пример типов сценариев
Я бы заложил такие шаблоны:
```
Requirement derivation scenario
Change request impact scenario
Defect-to-requirement scenario
Test failure scenario
Safety-relevant feature scenario
Architecture allocation scenario
Release readiness scenario
Traceability gap scenario
```

Например:
```
Change Request
  → impacted stakeholder requirements
  → impacted SYS.2 requirements
  → impacted SWE.1 requirements
  → impacted tests
  → impacted release
  → required re-review
```
Это уже очень ALM/ASPICE-like логика.


### Главное отличие от обычного workflow-конструктора
Обычный workflow-конструктор говорит:
```
Draft → Review → Approved → Implemented → Verified
```

ASPICE scenario constructor должен говорить:
```
This SYS.2 requirement is valid only if:
- it is derived from stakeholder input
- it has rationale or evidence
- it is verifiable
- it is reviewed
- it is traceable downstream
- it is covered by verification
- changes are controlled
```

То есть он проверяет не только статус, а **инженерную полноту артефакта**.


## Минимальная MVP-модель

Для MVP я бы сделал такие сущности:
```
Scenario
Process Step
Artifact Type
ASPICE Process
Input Artifact
Output Artifact
Trace Link
Evidence Source
Validation Rule
Gap
Recommendation
```

И такие основные артефакты:
```
Stakeholder Requirement
System Requirement
System Architecture Element
Software Requirement
Software Architecture Element
Detailed Design Item
Code Evidence
Test Case
Test Run
Defect
Change Request
Risk
Release
```

### Коротко

Конструктор должен быть как: 
`Scenario-based ASPICE lifecycle modeling and gap analysis tool`.

Или по-русски:
`Конструктор сценариев жизненного цикла ASPICE-артефактов с проверкой traceability, evidence, verification и process gaps`.

Для проекта должно стать центральной частью: **ProcessGrid** / **Codebeamer-like модель** + **AI generation pipeline** + **ASPICE gap checker**.

