---
title: "모듈 3 - 에이전트를 웹사이트에 연결하기"
description: "로컬 Azure Functions 프록시와 접근성을 갖춘 채팅 위젯을 통해 호스트된 Backer Concierge를 Tailspin Toys에 연결합니다."
authors:
  - juliamuiruri4
lastUpdated: 2026-09-16
next: false
---

[모듈 2][previous-lesson]에서는 Backer Concierge를 배포하고 테스트했습니다. [선택 사항 컨시어지 시리즈][overview]의 마지막 모듈에서는 로컬 Tailspin Toys 웹사이트에서 해당 에이전트를 사용할 수 있도록 합니다.

이 모듈에서는 다음을 수행합니다.

- Foundry 자격 증명을 서버에만 유지하는 로컬 Azure Functions 프록시(Proxy)를 빌드합니다.
- 사이트에 접근성을 갖춘 채팅 위젯을 추가합니다.
- 전체 대화 흐름을 검증하고 리소스를 정리합니다.

## 시나리오

후원자는 개발자 터미널이나 Azure Portal이 아니라 Tailspin Toys 웹사이트에서 게임을 찾습니다. 팀은 카탈로그와 함께 컨시어지(Concierge)를 제공하면서 후속 질문을 지원하고 서비스 자격 증명을 보호하는 채팅 경험을 구현하려고 합니다.

## 호스트된 에이전트에서 계속하기

웹사이트 통합에는 모듈 2에서 배포한 에이전트가 필요합니다. 프록시와 웹사이트는 로컬에서 실행하고, 에이전트는 Foundry에서 계속 실행 상태로 유지합니다.

1. Codespace로 돌아간 다음 `foundry-agent-cli` 브랜치의 Tailspin Toys 리포지토리와 기존 Copilot CLI 세션을 엽니다.
2. Backer Concierge가 배포되어 있고 [에이전트 빌드 및 배포하기][previous-lesson]의 원격 호출이 성공했는지 확인합니다. Azure 리소스를 이미 제거했다면 계속 진행하기 전에 이전 모듈을 따라 다시 만듭니다.

> [!IMPORTANT]
> 이 모듈의 프록시와 웹사이트는 로컬에서 실행합니다. 프로덕션 웹사이트 배포가 아닙니다. 모델과 호스트된 에이전트(Hosted agent)는 [정리][cleanup]를 완료할 때까지 요금이 부과되는 Azure 리소스로 남아 있습니다.

## 서버 측 프록시 빌드하기

Tailspin Toys는 전체를 사전 렌더링합니다. 브라우저 코드는 호스트된 에이전트를 직접 호출하거나 Foundry 자격 증명을 받아서는 안 됩니다. Foundry에 인증하고 에이전트 응답만 브라우저에 반환하는 로컬 Azure Functions **서버 측 자격 증명 경계**를 추가합니다.

`microsoft-foundry` 스킬은 호스트된 에이전트 작업 흐름을 담당하며, 같은 플러그인에 포함된 더 폭넓은 Azure 스킬은 로컬 Function 프로젝트를 준비할 수 있습니다. 이러한 스킬로 프록시를 빌드한 뒤 자격 증명을 노출하지 않고 에이전트에 연결하는지 확인합니다.

1. Copilot CLI에 다음을 입력합니다.

    ```text
    Azure skills를 사용하여 api에 Azure Functions v4, Node.js, TypeScript 프로젝트를 추가하고, 배포된 Backer Concierge 호스팅 에이전트를 호출하는 POST /api/concierge 엔드포인트 하나를 만듭니다. 이 Function은 로컬에서만 실행합니다. azure.yaml에 추가하거나 Azure 배포 인프라를 만들지 않습니다. 로컬 Azure 로그인과 함께 DefaultAzureCredential을 사용합니다. HTTP 트리거는 얇게 유지하고, Foundry 클라이언트를 단위 테스트가 가능한 모듈로 분리하며, 요청 본문을 검증하고 크기를 제한하고, 명시적 시간 제한을 설정하고, 민감 정보를 제거한 오류를 반환합니다. Foundry 프로젝트 엔드포인트와 에이전트 이름은 버전 관리에서 제외되는 로컬 서버 측 설정에 저장합니다. 자격 증명이나 액세스 토큰을 브라우저에 절대 반환하지 않습니다. Astro 사이트는 `output: 'static'`이고 개발 프록시가 없으므로, astro.config.mjs에서 /api 요청이 `astro dev` 중 Function 포트로 전달되도록 로컬 전용 Vite 개발 서버 프록시도 추가합니다.

    대화 상태를 위해 서버에서 충분한 엔트로피를 가진 핸들을 생성하고, 만료 기간과 함께 서버 측에서 Foundry 대화에 매핑합니다. 원본 Foundry 대화 또는 스레드 식별자를 절대 노출하지 않습니다. 형식이 잘못되었거나 만료되었거나 알 수 없는 핸들은 거부합니다. 관련 단위 테스트를 추가합니다.
    ```

    ![Azure Functions 로컬 프록시 설정](../../../../_images/cli-8-azure-functions-proxy.png)

2. 다른 터미널을 열고 Copilot이 제공한 명령으로 로컬 Function을 시작합니다. Function을 실행 상태로 둡니다.
3. Copilot CLI로 돌아가 로컬 프록시 테스트를 요청합니다.

    ```text
    로컬 /api/concierge 엔드포인트에 "30달러 미만인 게임은 무엇인가요?"라고 요청하고 민감 정보를 제거한 JSON 응답을 보여 줍니다. DefaultAzureCredential을 통해 요청이 배포된 Backer Concierge에 도달하는지 확인합니다.
    ```

4. 응답을 살펴봅니다. 카탈로그에 가격 정보가 없다고 설명해야 합니다. Foundry 토큰, 자격 증명, 프로젝트 엔드포인트, 원본 Foundry 대화 식별자, 스택 추적이 포함되어서는 안 됩니다.

    ![로컬 컨시어지 엔드포인트에서 받은 민감 정보를 제거한 JSON 응답](../../../../_images/cli-8-sanitized-json-response.png)

## 채팅 위젯 빌드하기

프록시를 통해 브라우저가 안전하게 컨시어지에 연결할 수 있게 되었습니다. 이제 사이트에 채팅 위젯을 추가하고 Playwright로 전체 대화 흐름을 확인합니다.

1. Copilot에 사이트 통합을 요청합니다.

    ```text
    Astro 컴포넌트로 접근성 있는 Backer Concierge 채팅 위젯을 추가하고 Layout.astro에서 사이트 전체에 렌더링합니다. 위젯은 /api/concierge에 POST 요청을 보내고, 반환된 불투명 대화 핸들로 대화 맥락을 이어가며, style.instructions.md의 어두운 테마를 따르고, Escape 키로 닫을 수 있으며, data-testid 속성을 포함해야 합니다.
    ```

2. 로컬 Function을 실행 상태로 유지한 채 다른 터미널에서 Copilot이 제공한 명령으로 Astro 사이트를 시작합니다.
3. Copilot CLI로 돌아갑니다. [레슨 6][playwright-lesson]에서 살펴보거나 추가한 Playwright MCP 서버를 이미 사용할 수 있습니다. Copilot에 위젯 테스트를 요청합니다.

    ```text
    Playwright MCP 서버를 사용하여 실행 중인 Tailspin Toys 사이트에서 Backer Concierge 위젯을 엔드투엔드로 테스트합니다. 기본 채팅 흐름, 대화 연속성, 접근성, 오류 처리, 그라운딩 범위, 로컬 프록시의 안전한 사용을 검증합니다. 결과를 보고하고 실패 근거를 포함합니다.
    ```

    ![Tailspin Toys 사이트의 Backer Concierge 위젯 화면](../../../../_images/cli-8-backer-concierge-widget.png)

4. 보고된 증거와 결과를 대조해 검토합니다. 실패한 검사가 있다면 마무리하기 전에 Copilot에 해당 프록시나 위젯 동작을 수정하고 실패한 검사를 다시 실행하도록 요청합니다.

## 리소스 정리하기

로컬 웹사이트에서 작동하는 컨시어지라는 최종 결과물을 완성했습니다. 공통 정리 지침은 시리즈 전반에서 만든 로컬 서비스와 Azure 리소스를 모두 다룹니다.

1. [리소스 정리하기][cleanup]를 완료합니다. 로컬 서비스를 중지하고 Azure 리소스 삭제가 완료되었는지 확인하는 작업도 포함됩니다.

## 요약 및 다음 단계

로컬 서버 측 프록시와 접근성을 갖춘 채팅 위젯을 통해 호스트된 Backer Concierge를 Tailspin Toys에 연결했습니다. 시리즈 전반에서 GitHub Copilot CLI와 Foundry를 사용해 모델을 준비하고, 에이전트를 빌드 및 배포한 뒤 완전한 웹사이트 통합을 검증했습니다.

CLI 워크숍을 마무리하려면 [마무리 및 다음 단계][review]로 이동합니다.

[overview]: ../
[previous-lesson]: ../2-build-and-deploy/
[review]: ../../10-review/
[playwright-lesson]: ../../6-mcp-playwright/
[cleanup]: ../#리소스-정리하기
