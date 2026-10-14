---
title: "Exercício 3 — Testar o recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem com o modo de agente. Antes de abrir um pull request, confirme se ele funciona no navegador. Em vez de testar o aplicativo manualmente, conecte o **servidor do protocolo Model Context Protocol (MCP) do Playwright** e peça ao Copilot que controle um navegador real para testar o recurso. Depois, publique a ramificação e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como servidores MCP adicionam ferramentas ao Copilot;
- adicionar o servidor MCP do Playwright ao espaço de trabalho;
- pedir ao Copilot que teste manualmente o recurso de filtragem no navegador;
- publicar a ramificação e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica muito mais poderoso quando pode acessar recursos fora do editor. O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite essa conexão: é um padrão para o agente se comunicar com ferramentas e serviços externos.

![Diagrama que mostra o funcionamento interno do modo de agente e como ele interage com contexto, modelos de linguagem de grande porte (LLMs) e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O MCP permite que agentes de inteligência artificial se comuniquem com ferramentas e serviços externos em tempo real. Com recursos, eles podem acessar informações atualizadas; com ferramentas, podem executar ações em seu nome.

Essas ferramentas e esses recursos são acessados por meio de um servidor MCP, que faz a ponte entre o agente e os serviços externos, como interfaces de programação de aplicativos (APIs) existentes ou ferramentas locais, como pacotes npm. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente pode acessar.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: fornece acesso a APIs para gerenciar repositórios do GitHub. O agente pode, por exemplo, criar repositórios, atualizar os existentes e gerenciar issues e pull requests.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: oferece automação de navegador com Playwright. O agente pode navegar por páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP disponíveis. O GitHub hospeda um [registro MCP](https://github.com/mcp) para facilitar a descoberta e as contribuições ao ecossistema.

> [!CAUTION]
> Do ponto de vista de segurança, trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um, revise cuidadosamente o código-fonte, verifique o publicador e considere as implicações de segurança. Use somente servidores em que confia e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revise a configuração de MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste espaço de trabalho do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Observe que já há um servidor `github` configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

Essa entrada `github` vem do modelo do projeto e permitiu que o Copilot lesse o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], que não precisa ser instalado localmente. O VS Code se conecta a ele pelo protocolo de transferência de hipertexto (HTTP) e você se autentica no GitHub na primeira vez que o Copilot usa uma das ferramentas.

## Adicione o servidor MCP do Playwright

Agora, adicione um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador que ele pode controlar, permitindo testar o recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github` para que o arquivo fique assim:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, `playwright` é um servidor **local**: o VS Code o inicia no computador executando `npx @playwright/mcp@latest`. O sinalizador `--headless` faz o Playwright executar o navegador sem uma janela visível, o que é necessário em um codespace, onde não há área de trabalho para exibi-la.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright nos testes de ponta a ponta, então o navegador necessário geralmente já está instalado. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Inicie e confie no servidor

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisa das ferramentas de um servidor, o VS Code o inicia e solicita confirmação de confiança.

1. Em `.vscode/mcp.json`, aparecerá uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor agora e confirmar que a configuração está correta.
2. Se o VS Code solicitar que você confirme se confia no servidor, revise a configuração e escolha confiar para que ele possa iniciar.

Iniciar o servidor manualmente é opcional. Se você pular essa etapa, o VS Code iniciará o servidor e solicitará confirmação de confiança na primeira vez que o Copilot precisar das ferramentas, na etapa seguinte.

## Teste o recurso de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador real. Em vez de navegar pelo aplicativo para conferir o trabalho, o agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumir o que observou. Assim, você pode confirmar rapidamente o comportamento do recurso sem sair da conversa.

Internamente, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, e não capturas de tela. Isso permite que o agente trabalhe com elementos estruturados e rotulados (botões, links e itens de lista), da mesma maneira que uma tecnologia assistiva — uma verificação funcional rápida também oferece uma checagem básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, o Copilot poderá exercitar a filtragem implementada.

1. Envie este prompt ao Copilot:

   ```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
   ```

O Copilot iniciará um navegador pelo servidor MCP do Playwright, seguirá as etapas e informará o que encontrou. Compare o resumo com os critérios de aceitação da issue. Se algo parecer incorreto, faça perguntas adicionais ou peça que o Copilot corrija o código antes de abrir o pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` durante o teste. Se você tiver parado o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor MCP do Playwright, talvez seja necessário baixar um navegador. Se ele informar que o navegador está ausente, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publique a ramificação e crie um pull request

Depois de confirmar que o recurso funciona, abra um pull request (PR) para que a equipe o revise. Primeiro, publique a ramificação `filtering-vscode`.

1. No codespace, abra o painel **Source Control** e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Captura de tela do painel Source Control mostrando as alterações feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a ramificação ao repositório.

## Crie o pull request

Há várias maneiras de criar um pull request, inclusive pelo github.com e pela interface de linha de comando do GitHub (CLI). Como você já está trabalhando com o GitHub Copilot, peça que ele crie o PR, localize a issue correspondente e associe-a ao PR.

1. Abra o painel Copilot Chat e selecione **New Chat** para iniciar uma nova sessão.
2. Confirme que **Agent** está selecionado na lista suspensa de agentes para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie o PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Quando necessário, selecione **Continue** para permitir que o Copilot reúna informações e execute operações. Na primeira vez que usar uma ferramenta do GitHub, talvez seja necessário autenticar-se; siga as instruções para permitir o acesso.
5. Observe o Copilot pesquisar as issues, encontrar a correta e criar o PR.
6. Selecione o link fornecido pelo Copilot para revisar o pull request, mas **não faça merge ainda**.

## Resumo e próximas etapas

Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como servidores MCP adicionam ferramentas ao Copilot;
- adicionou o servidor MCP do Playwright ao espaço de trabalho;
- usou o servidor para testar manualmente o recurso de filtragem no navegador antes de entregá-lo;
- publicou a ramificação e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* um recurso, mas MCP é apenas uma das maneiras de ampliar o Copilot. Em seguida, [crie um agente personalizado][next-lesson] para agilizar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre ele?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
