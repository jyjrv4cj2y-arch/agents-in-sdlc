---
title: "Lição 0 — Pré-requisitos"
description: "Crie uma cópia própria da Tailspin Toys e prepare um GitHub Codespace para o workshop do Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Antes de começar as lições do Copilot CLI, prepare tudo o que será necessário. Você criará uma cópia própria do repositório Tailspin Toys e iniciará um [codespace][codespaces], cujo terminal integrado será usado para instalar e executar o Copilot CLI na próxima lição.

Nesta lição, você vai:

- criar uma cópia própria do projeto Tailspin Toys a partir do modelo;
- criar um Codespace e confirmar que o projeto está pronto.

## Configure o repositório do laboratório

Você trabalhará na sua própria cópia do projeto Tailspin Toys. Crie-a agora a partir do [repositório modelo][tailspin-template]. O novo repositório contém todos os arquivos necessários para o laboratório.

1. Em uma nova janela do navegador, acesse o [modelo da Tailspin Toys][tailspin-template].
2. Crie sua própria cópia do repositório selecionando **Use this template** e, em seguida, **Create a new repository**.
3. Se estiver participando de um evento conduzido pela GitHub ou pela Microsoft, siga as instruções das pessoas mentoras. Caso contrário, crie o repositório em uma organização na qual você tenha acesso ao GitHub Copilot.
4. Anote o caminho do repositório criado (`organization-or-user-name/repository-name`), pois você precisará dele mais adiante no workshop.

> [!NOTE]
> Ao criar o repositório com base no modelo, um backlog de issues do GitHub será criado automaticamente. Você trabalhará com essas issues durante o workshop; não é necessário criar nenhuma.

Use uma cópia nova do modelo do workshop. Ela inclui instruções do repositório, código do aplicativo, testes, a skill `quality-checks` e o backlog que será usado. Se estiver usando uma cópia antiga, confirme com a pessoa facilitadora se ela contém os arquivos necessários.

## Crie um Codespace

Em seguida, use um Codespace para concluir o workshop.

[GitHub Codespaces][codespaces] é um ambiente de desenvolvimento na nuvem que permite escrever, executar e depurar código diretamente no navegador. Ele oferece um editor completo, com suporte para várias linguagens de programação, extensões e ferramentas.

1. Acesse o repositório que acabou de criar.
2. Selecione **Code**.
3. Selecione a guia **Codespaces** e, em seguida, **Create codespace on main**.
4. Aguarde a conclusão da configuração do Codespace. O modelo instalará as dependências do projeto, o Chromium do Playwright e o banco de dados local.
5. Se aparecer a pergunta **Do you trust the authors of the files in this folder?**, selecione **Trust Folder & Continue**.
6. Abra um terminal na raiz do repositório e inicie o aplicativo:

   ```bash
   npm run dev
   ```

7. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser** e confirme que o site Tailspin Toys carrega.
8. Volte ao terminal e pare o servidor de desenvolvimento com <kbd>Ctrl</kbd>+<kbd>C</kbd>.

## Resumo e próximas etapas

O ambiente está pronto. Nesta lição, você:

- criou uma cópia própria do projeto Tailspin Toys a partir do modelo;
- criou um Codespace e confirmou que o projeto estava pronto.

Em seguida, [instale o GitHub Copilot CLI][next-lesson] no Codespace e autentique-se com sua conta do GitHub.

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório com base em um modelo][template-repository]
- [Introdução ao Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
