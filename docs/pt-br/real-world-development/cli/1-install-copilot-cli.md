---
title: "Lição 1 — Instalar o GitHub Copilot CLI"
description: "Instale e autentique o Copilot CLI no Codespace, conheça o ambiente e localize a issue de filtragem existente."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O [GitHub Copilot CLI][about-copilot-cli] é um assistente de programação agêntico avançado executado no terminal, permitindo explorar bases de código, gerar código, executar comandos e interagir com ferramentas externas — tudo pela linha de comando. Ele permite delegar tarefas, solicitar alterações e manter o foco. Como você deve imaginar, a primeira etapa é instalar a ferramenta. Felizmente, isso pode ser feito com ferramentas que você já conhece.

Nesta lição, você vai:

- instalar o GitHub Copilot CLI com npm;
- autenticar-se com sua conta do GitHub;
- confiar no repositório do workshop e iniciar uma conversa;
- localizar a issue de filtragem pelo servidor Model Context Protocol (MCP) integrado do GitHub.

## Cenário

Sua equipe está começando a usar agentes de inteligência artificial para trabalhar em um backlog crescente. O Copilot CLI leva esse recurso ao terminal, onde muitas pessoas desenvolvedoras já trabalham. Nesta lição, você instalará e autenticará a ferramenta e se preparará para usá-la no restante do workshop.

## Instale o Copilot CLI

Você pode instalar o Copilot CLI com [npm][install-cli], WinGet ou Homebrew. Como o GitHub Codespaces já inclui o Node.js, você usará o npm.

1. Volte ao Codespace e abra um terminal.
2. Confirme que o Node.js está instalado e atende ao requisito de versão:

   ```bash
   node --version
   ```

   A versão deve ser 24 ou posterior.

3. Instale o Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Confirme a instalação:

   ```bash
   copilot --version
   ```

   O comando deverá exibir o número da versão.

## Autentique-se com o GitHub

Na primeira inicialização, o Copilot CLI solicitará a autenticação com sua conta do GitHub.

1. Inicie o Copilot CLI:

   ```bash
   copilot
   ```

2. Se solicitado, siga as instruções de código do dispositivo para autenticar e autorizar o Copilot CLI.
3. O Copilot CLI exibirá este prompt:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Confirme que o caminho corresponde ao repositório Tailspin Toys e responda afirmativamente selecionando **Yes, and remember this folder for future sessions**.

> [!NOTE]
> Em um Codespace, você talvez já esteja autenticado pela sessão do GitHub. Se o Copilot CLI iniciar sem solicitar autenticação, está tudo certo.

## Conheça o ambiente

Os comandos digitados no prompt normal do shell são executados diretamente no Codespace. Depois que o Copilot CLI é iniciado, a linguagem natural é enviada ao agente e os comandos slash controlam a conversa.

1. Digite `/model`, use as teclas de seta para selecionar **Auto**, pressione <kbd>Enter</kbd> e pressione <kbd>Enter</kbd> novamente para confirmar.
2. Digite `/help` para ver os comandos disponíveis na versão instalada e pressione <kbd>Esc</kbd> para fechar a tela de ajuda.
3. Faça uma pergunta simples ao Copilot para confirmar que tudo funciona:

   ```plaintext
   What are the key files in this project?
   ```

4. Leia a resposta e observe que o Copilot explora o repositório antes de responder.
5. Digite `/mcp list` e confirme que o servidor MCP integrado do GitHub está disponível.
6. Peça ao Copilot que localize a issue de filtragem:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abra a URL e leia a issue. Você a usará depois de concluir uma pequena alteração inicial.

> [!TIP]
> Uma sessão normal do Copilot CLI usa a ramificação selecionada no terminal; ela não cria uma worktree automaticamente. Você criará uma ramificação de recurso antes de cada alteração.

## Use o atalho do workshop

Normalmente, o Copilot CLI pede permissão antes de usar ferramentas fora das permissões já estabelecidas. Neste workshop, você o reiniciará com `--yolo`, um atalho aprovado para este ambiente que remove essas solicitações no Codespace e permite que você se concentre nos exercícios.

> [!CAUTION]
> `--yolo` habilita permissões automáticas completas (`--allow-all-tools`, `--allow-all-paths` e `--allow-all-urls`). Use-o somente em um ambiente isolado, como um Codespace ou uma máquina virtual (VM), e nunca o defina como alias padrão no desenvolvimento diário. Consulte [Permitir e negar o uso de ferramentas][allow-all-warning] para saber mais.

Neste workshop, `--enable-all-github-mcp-tools` habilita as ferramentas MCP de leitura e gravação do GitHub usadas em lições posteriores para trabalhar com issues e pull requests. O Codespace limita o acesso ao computador local, mas os recursos do GitHub autenticados continuam sendo reais. Revise as alterações antes de publicá-las ou fazer merge.

1. Saia do Copilot CLI com `/exit`.
2. Reinicie-o na raiz do repositório:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Faça outra pergunta breve sobre o projeto para confirmar que a conversa funciona e saia com `/exit`.

O Copilot salva as conversas automaticamente. Mais adiante, após alterar instruções ou adicionar um agente, você usará `copilot --resume` para voltar à mesma conversa e ramificação do recurso.

## Resumo e próximas etapas

Nesta lição, você:

- instalou o GitHub Copilot CLI com npm;
- autenticou-se com sua conta do GitHub;
- confiou no repositório do workshop e iniciou uma conversa;
- localizou a issue de filtragem pelo servidor MCP integrado do GitHub.

Em seguida, [faça sua primeira alteração focada][next-lesson] e use o Copilot CLI para exibir uma avaliação por estrelas nos cartões dos jogos.

## Recursos

- [Instalar o GitHub Copilot CLI][install-cli]
- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
