---
title: "Lição 6 — Validar funcionalidades com Playwright MCP"
description: "Inspecione ou configure o Playwright MCP no Copilot CLI e use-o para explorar o recurso de filtragem no navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já destacamos, escrever código envolve mais do que apenas escrever código. Precisamos trabalhar com dados e serviços externos e disponibilizar mais automações ao Copilot. É aí que entram os servidores do protocolo Model Context Protocol (MCP). Eles permitem que o Copilot vá além dos recursos integrados à CLI, oferecendo mais ferramentas e serviços.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza;
- adicionar o servidor MCP do Playwright, caso ainda não esteja disponível;
- pedir ao agente que controle um navegador e explore o recurso de filtragem.

## Cenário

Testes de unidade e de ponta a ponta são importantes, mas validar alterações na interface do usuário exige interagir com ela. Você quer permitir que o Copilot use o site como uma pessoa usuária faria para automatizar ainda mais as alterações e aumentar a confiança de que elas funcionam como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de inteligência artificial se comuniquem com ferramentas e serviços externos. Com MCP, eles podem acessar informações atualizadas e executar ações em seu nome.

As ferramentas e os recursos são acessados por meio de um servidor MCP, que funciona como uma ponte entre o agente e as ferramentas e os serviços externos. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente pode acessar.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub][github-mcp]**: fornece acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Servidor MCP do Playwright][playwright-mcp-server]**: oferece recursos de automação de navegador com Playwright.

Há muitos outros servidores MCP disponíveis. O GitHub hospeda um [registro de MCP][mcp-registry] para facilitar a descoberta e as contribuições ao ecossistema.

> [!CAUTION]
> Trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um, revise o código-fonte, verifique o publicador e considere as implicações de segurança.

## Adicione o servidor MCP do Playwright

Se o servidor MCP do Playwright ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como uma pessoa usuária.

> [!NOTE]
> Se o Playwright MCP já estiver configurado no Copilot CLI, ignore estas etapas de configuração e continue na próxima seção.

1. Volte ao Codespace.
2. No Copilot CLI, abra a caixa de diálogo para adicionar um servidor MCP com o comando:

   ```plaintext
   /mcp add
   ```

3. Digite `playwright` como nome e pressione <kbd>Tab</kbd>.
4. Confirme o tipo de servidor **STDIO** pressionando <kbd>Enter</kbd> e, em seguida, pressione <kbd>Tab</kbd>.
5. Cole o seguinte na caixa de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo de MCP.

## Peça ao Copilot que explore o recurso com o Playwright

Você já confirmou manualmente que a funcionalidade se comporta como esperado. Agora, use o servidor MCP do Playwright disponível no Copilot para fazer o mesmo.

1. Use este prompt para pedir ao Copilot que valide a funcionalidade com o servidor MCP do Playwright:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Em geral, não é necessário dizer ao Copilot para usar o servidor MCP, pois ele costuma identificá-lo sozinho. Ainda assim, indicar a ferramenta pode tornar os resultados mais consistentes e economizar tokens.

2. Observe as etapas executadas pelo Copilot no navegador para confirmar que a funcionalidade funciona.
3. Leia o relatório e confirme que tudo se comporta como esperado.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, parará o servidor e fornecerá um relatório.

## Resumo e próximas etapas

Você usou o servidor MCP do Playwright para explorar o recurso em um navegador real pelo Copilot CLI. Em resumo, você:

- aprendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza;
- adicionou o servidor MCP do Playwright, caso ainda não estivesse disponível;
- pediu ao agente que controlasse um navegador e explorasse o recurso de filtragem.

Em seguida, [crie um agente personalizado de QA][next-lesson] que reúna a skill e as ferramentas de navegador em uma função especializada.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre ele?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
