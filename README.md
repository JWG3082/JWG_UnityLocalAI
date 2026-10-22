# Unity Local AI

개인용 **Unity Editor + Ollama** 로컬 AI 개발 도우미 (초기 개발 버전, v0.1).

## 현재 구현된 기능

- Unity Editor에서 Ollama 로컬 모델과 채팅
- 모델 응답 대기 표시, 응답 스크롤 및 복사
- 프로젝트의 C# 스크립트를 읽고 AI 수정안 생성
- 원본/수정안/변경 비교 및 수동 승인 후 적용
- 적용 전 원본 백업 (`Library/UnityLocalAIBackups/`)
- 현재 Unity 프로젝트의 Assets 폴더·씬·스크립트·패키지 구조 스캔 및 AI 설명 요청

> 주의: AI가 생성한 코드는 항상 맞는 것이 아닙니다. 코드 수정은 작은 테스트 파일부터 진행하고 Unity Console에서 컴파일 오류를 확인하세요. 자동 컴파일 검증 및 스크립트 간 호출·참조 분석은 아직 구현하지 않았습니다.

## 요구 사항

- Windows에서 실행 중인 Ollama (기본 API: `http://localhost:11434`)
- Ollama 모델 `granite3.3:8b`
- Unity Editor (Unity 6에서 테스트 권장)

## 설치 방법

1. [Ollama](https://ollama.com/download)를 설치하고 실행합니다.
2. CMD 또는 PowerShell에서 모델을 다운로드합니다.

   ```powershell
   ollama pull granite3.3:8b
   ollama list
   ```

3. 이 저장소의 `Assets/Editor/UnityLocalAI.cs`를 **사용할 Unity 프로젝트**의 `Assets/Editor/`로 복사합니다. 동일한 이름의 기존 스크립트가 있다면 먼저 백업하고, 중복 클래스가 생기지 않도록 주의하세요.
4. Unity 컴파일이 끝나면 상단의 **Tools → Unity Local AI**를 엽니다.
5. `일반 채팅`, `C# 수정`, `프로젝트 구조` 기능을 사용합니다.

## 모델 변경

현재 소스에는 `granite3.3:8b`가 지정되어 있습니다. 다른 모델로 시험할 때는 `UnityLocalAI.cs`의 `ModelName` 값을 해당 Ollama 모델 태그로 바꿉니다. 바꾼 모델은 Ollama에서 먼저 다운로드해야 합니다.

## 저장소 구성

```text
UnityLLM/
├── Assets/
│   └── Editor/
│       └── UnityLocalAI.cs
├── .gitignore
└── README.md
```

AI 모델 파일은 저장소에 넣지 않습니다. 각 PC의 Ollama에서 별도로 다운로드합니다.

## 보안 및 제한 사항

- 기본 연결 주소는 로컬 PC의 `localhost`입니다. Ollama Cloud나 외부 API가 활성화되어 있지 않은지 따로 확인하세요.
- 회사 소유 프로젝트 코드·비밀번호·API 키를 GitHub에 등록하지 마세요. Private 저장소라도 보안 수칙은 필요합니다.
- 프로젝트 구조 스캔은 경로·파일 종류·패키지를 중심으로 보고서를 작성합니다. 전체 코드의 의미나 씬 오브젝트 간 연결을 자동으로 이해하는 기능은 아직 없습니다.
- 백업은 Unity 프로젝트의 `Library` 아래 생성되므로, `Library` 폴더 삭제 또는 프로젝트 이동 전에 별도 보관이 필요할 수 있습니다.

## 향후 개발 계획

- C# 스크립트 간 의존성·참조 분석
- 코드 수정 결과의 컴파일 및 테스트 검증
- 사용자 선택형 자동/수동 수정 모드
- 복수 LLM 모델 성능 비교 및 선택 UI
