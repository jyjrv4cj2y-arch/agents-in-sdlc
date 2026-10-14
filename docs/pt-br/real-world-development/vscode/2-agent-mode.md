---
title: "Exercício 2 — Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo as atualizações mais simples de um aplicativo costumam exigir alterações em vários arquivos e a execução de tarefas, como testes. Em geral, a pessoa desenvolvedora precisa localizar todos os arquivos necessários, fazer as alterações, executar testes, depurar, descobrir qual arquivo foi esquecido e fazer outra atualização. E assim por diante.

É aí que entra o modo de agente do Copilot.

O modo de agente do Copilot trabalha com mais autonomia no IDE. Ele se comporta de maneira semelhante a uma pessoa desenvolvedora: começa explorando a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Veja como usá-lo para adicionar funcionalidades ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente executa tarefas no IDE, permitindo ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona como uma pessoa da equipe e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá a:

- usar o modo de agente do Copilot para explorar o projeto, identificar arquivos relevantes e coordenar alterações;
- implementar recursos nas camadas de interface e de dados;
- revisar as alterações e os testes gerados pelo Copilot antes de fazer merge na base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem por categoria e editora. No exercício anterior, você criou um auxiliar de editoras; agora, concluirá o trabalho restante na camada de dados, na interface e nos testes usando o modo de agente do Copilot.

## Execute o site Tailspin Toys

Antes de fazer alterações, explore o site Tailspin Toys para conhecer seu comportamento atual.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. As pessoas podem listar jogos e exibir seus detalhes. O site é um único aplicativo Astro que renderiza suas páginas como linguagem de marcação de hipertexto (HTML) estática durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, por meio do Drizzle, um mapeador objeto-relacional (ORM); não há uma interface de programação de aplicativos (API) de back-end separada nem uma estrutura de interface no cliente. Os auxiliares de acesso a dados reutilizáveis ficam em `src/lib/`, e a interatividade é adicionada com um pequeno `<script>` Astro com escopo definido, usando APIs padrão do modelo de objeto de documento (DOM).

### Inicie o site

Uma tarefa `npm run dev` inicia o único servidor de desenvolvimento Astro. Execute-a no GitHub Codespace:

1. Volte ao codespace. Depois do exercício anterior, você deverá estar na ramificação `main`.
2. Abra um novo terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e selecione uma nova ramificação para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, você deverá ver um aviso semelhante a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo solicitar que você abra uma janela do navegador para `http://localhost:4321`, feche-a selecionando **x**.

5. Para abrir o site, mantenha pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecione o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, selecionar o link localhost no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o codespace, que agora hospeda o servidor Web.

### Explore o site

Com o site em execução, explore suas principais funcionalidades:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você acessa uma página com mais informações, incluindo título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. A seleção do modo e do modelo varia por exercício; cada etapa indica as opções relevantes.

A implementação inicial do site funciona, mas queremos adicionar recursos. Comece examinando o backlog. A criação do projeto com base no modelo já adicionou issues ao backlog. Peça ao GitHub Copilot que as mostre.

1. Na lista suspensa de agentes da visualização Chat, selecione **Agent**. O agente planeja e implementa alterações autonomamente em vários arquivos, executa comandos do terminal e invoca ferramentas.

   ![Captura de tela do seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Na lista de modelos disponíveis, selecione **Claude Sonnet 4.5**.

> [!CAUTION]
> A escolha desse modelo não indica preferência dos autores do workshop por um modelo específico. Ele foi usado ao criar o workshop para manter as sugestões relativamente consistentes. Como os modelos de linguagem de grande porte (LLMs) são probabilísticos, suas sugestões podem ser diferentes dos exemplos; isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos modelos de linguagem de grande porte (LLMs), o Copilot pode usar um comando do protocolo Model Context Protocol (MCP) diferente e ainda assim concluir a tarefa.

3. Envie este prompt ao Copilot para consultar o backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista gerada.

Observe como o Copilot também priorizou os itens de acordo com aqueles que considera mais úteis para as pessoas usuárias.

## Revise os arquivos de instruções

Antes de pedir ao agente que gere código, revise o arquivo de instruções que fornecerá contexto ao Copilot. Use o arquivo da [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como abordar a adição de funcionalidades ao site.

1. No codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura;
   - princípios para design de componentes, testabilidade e acessibilidade;
   - links para instruções específicas de tipos de arquivo, incluindo Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem referenciar outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao local do arquivo de instruções. Isso permite reutilizar orientações, dividir instruções complexas em partes menores e fornecer exemplos e modelos.

## Implemente a funcionalidade de filtragem

Para concluir a filtragem, será necessário fazer pelo menos três alterações distintas no aplicativo:

- adicionar ou aperfeiçoar a lógica de filtragem restante na camada de dados (`src/lib/`);
- adicionar ou atualizar testes do comportamento de filtragem;
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Os testes também precisam ser executados e aprovados antes do merge. O modo de agente do Copilot pode executar essas tarefas.

1. Continue na conversa atual do Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Captura de tela de exemplo da seleção das instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado na lista suspensa de agentes da visualização Chat.

   ![Captura de tela do seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base na issue correspondente do backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Acompanhe o Copilot enquanto ele explora o projeto e localiza os arquivos relacionados ao recurso. Ele deverá encontrar os auxiliares da camada de dados, a interface e os testes; em seguida, modificará os arquivos e executará os testes.

   ![Captura de tela do Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, os controles **Keep** e **Undo** poderão aparecer no editor. Quando o Copilot terminar, haverá um controle **Keep** ou **Undo** para cada alteração; não é necessário selecioná-los enquanto o trabalho ainda está em andamento.

7. Quando o Copilot solicitar, selecione **Continue** para executar os testes.

   ![Captura de tela de uma caixa de diálogo no painel Copilot Chat pedindo confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. É possível que haja pausas e até falhas em alguns testes durante o processo. Isso é esperado: o Copilot alterna entre gerar código e executar testes até concluir a tarefa sem detectar erros.

   ![Captura de tela de uma sessão completa do modo de agente do Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado e procure possíveis problemas.

> [!CAUTION]
> Sempre revise o código gerado pelo Copilot ou por qualquer outra ferramenta de inteligência artificial.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela Copilot Chat.

## Resumo e próximas etapas

Você explorou como usar o modo de agente do GitHub Copilot para adicionar funcionalidades ao site da Tailspin Toys. Aprendeu a:

- usar o modo de agente para implementar recursos nas camadas de interface e de dados;
- explorar o projeto, identificar arquivos relevantes e coordenar alterações;
- revisar as alterações e os testes gerados antes do merge.

Agora, [teste o recurso com o servidor MCP do Playwright][next-lesson] e abra um pull request.

### Exercício extra — implementar paginação

À medida que a lista de jogos crescer, será necessário adicionar paginação. Use o que aprendeu neste exercício e peça ao Copilot que atualize o site. Considere:

- seguir as práticas existentes, incluindo o uso dos arquivos de instruções;
- decidir como implementar a paginação e se as pessoas usuárias poderão selecionar o tamanho da página ou se ele será fixo;
- fornecer ao Copilot as orientações necessárias no prompt;
- iterar com o GitHub Copilot, solicitando alterações e fornecendo contexto — esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usá-los][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
