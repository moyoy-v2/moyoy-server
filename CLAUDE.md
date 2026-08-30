# moyoy-server

Spring Boot 기반 moyoy 백엔드 서버.

## 스택

| 항목 | 버전 |
|---|---|
| Kotlin | 2.3.21 |
| Spring Boot | 4.1.1 |
| Java toolchain | 25 |
| 빌드 | Gradle (Kotlin DSL) |

주요 의존성: `spring-boot-starter-webmvc`, `kotlin-reflect`, `jackson-module-kotlin`

컴파일러 옵션으로 `-Xjsr305=strict`가 켜져 있어 Java 상호운용 시 플랫폼 타입이
non-null로 강제된다.

## 빌드 · 실행

```bash
./gradlew build          # 컴파일 + 테스트
./gradlew test           # 테스트만
./gradlew bootRun        # 로컬 실행
```

## 커밋 규칙

### 절대 넣지 말 것

커밋 메시지에 아래 트레일러를 **절대 붙이지 않는다.** 도구 정보는 변경 이력의
노이즈이며, 커밋 로그는 변경 내용 자체만 담는다.

```
Co-Authored-By: Claude ...
Claude-Session: https://claude.ai/code/...
```

메시지는 실제 내용(제목, 본문, 이슈 참조 등)의 마지막 줄에서 끝낸다.

### 메시지 형식

[Conventional Commits](https://www.conventionalcommits.org)를 따르되 본문은 한국어로 쓴다.

```
<type>: <제목>

<본문 — 무엇을 왜 바꿨는지>
```

| type | 용도 |
|---|---|
| `feat` | 사용자가 체감하는 기능 추가 |
| `fix` | 버그 수정 |
| `chore` | 설정 · 빌드 · 의존성 등 동작 변화 없는 작업 |
| `refactor` | 동작 변경 없는 구조 개선 |
| `test` | 테스트 추가 · 수정 |
| `docs` | 문서 |

판별 기준: **릴리즈 노트에 쓸 수 있으면 `feat`, 아니면 `chore`.**

## 브랜치 전략

Git Flow를 따른다.

```
master   ← 배포된 실제 서비스
develop  ← 개발 통합 (기본 브랜치, PR의 base)
feature/ ← 새 작업. develop에서 파고 develop으로 머지
hotfix/  ← 운영 긴급 수정
release/ ← 배포 준비
```

브랜치 prefix는 커밋 type과 맞춘다. 예를 들어 설정 작업이면 브랜치도
`chore/...`, 커밋도 `chore: ...`로 통일한다.

PR의 base는 항상 `develop`이다. 작업 중인 PR은 draft로 연다.

## .gitignore 관련 주의

- **`gradle-wrapper.jar`는 반드시 커밋한다.** `*.jar`를 무시하되 이 파일만
  부정 패턴(`!gradle/wrapper/gradle-wrapper.jar`)으로 예외 처리해 두었다.
  이게 빠지면 클론한 사람이 `./gradlew`를 실행할 수 없다.
- 로컬 설정과 시크릿(`application-local.*`, `application-secret.*`, `.env`,
  `*.key`, `*.pem`, `*.jks`)은 사전 차단돼 있다. 환경별 설정이 필요하면
  이 이름 규칙을 쓰고, 팀 공유가 필요한 값은 `.example` 파일로 따로 둔다.
- `.gradle/`, `build/`, `.idea/`, `.DS_Store`, `.omc/`는 커밋 대상이 아니다.
