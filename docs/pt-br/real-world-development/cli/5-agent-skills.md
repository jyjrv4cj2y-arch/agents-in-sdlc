---
title: "Lição 5 — Personalizar e usar a skill quality-checks"
description: "Explore a skill quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escrever código envolve mais do que apenas programar. Você já verificou manualmente se o código funciona e usou arquivos de instruções para garantir que ele siga os padrões. E os testes, o lint e as outras partes da integração contínua (CI)?

Para esse tipo de tarefa, as **skills de agente** são ideais. Elas ajudam o Copilot a entender como executar operações desse tipo.

Nesta lição, você vai:

- explorar a skill existente `quality-checks`;
- personalizar o formato dos resultados;
- recarregar e executar a skill.

## Cenário

A Tailspin Toys usa a skill `quality-checks` para executar testes de unidade, lint e verificações de tipos. A equipe quer melhorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

As skills de agente reúnem instruções reutilizáveis para tarefas, scripts executáveis e recursos de apoio que o agente carrega quando necessário. Em sua forma básica, uma skill é uma pasta com o nome correspondente e um arquivo Markdown chamado `SKILL.md`. O Markdown contém frontmatter com o nome e a descrição da skill, uma visão geral da função e orientações sobre quando ela deve ser usada. A pasta também pode conter subpastas com scripts e outros recursos.

> [!NOTE]
> Uma skill não precisa de pastas ou arquivos adicionais. A skill `quality-checks` da Tailspin Toys contém apenas `SKILL.md`, pois usa os comandos já existentes no projeto.

As skills podem ficar na pasta `.github/skills` do projeto, como um recurso compartilhado e reutilizável pela equipe, ou na pasta de skills do usuário em `~/.copilot/skills`.

## Explore a skill

Explore a skill `quality-checks`, criada pela equipe Tailspin Toys para executar testes de unidade, lint e verificações de tipos.

1. Volte ao Codespace. No editor do Codespaces, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia os campos `name` e `description` no início. A descrição ajuda o Copilot a decidir quando usar a skill.
3. Leia as instruções e observe como orientam o Copilot durante os testes e o lint.
4. Observe que a skill ainda não tem uma seção **Results output formatting**.

## Execute a skill antes de alterá-la

As skills podem ser chamadas diretamente pelo Copilot CLI ou por linguagem natural. Peça ao Copilot que execute as três verificações da skill.

1. Volte à conversa de filtragem no modo Interactive.
2. Envie este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório apresentado no final.

## Personalize o relatório

Melhore o relatório para mostrar o que foi executado, se cada operação teve êxito e o que as ferramentas informaram. Atualize a skill para produzir esse relatório.

1. Volte a `.github/skills/quality-checks/SKILL.md`.
2. Adicione esta seção ao final do arquivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. O arquivo será salvo automaticamente.

## Execute a skill atualizada

Agora, confira o resultado. O Copilot CLI pode recarregar skills editadas sem reiniciar a conversa.

1. Digite:

   ```plaintext
   /skills reload
   ```

2. Envie exatamente o mesmo prompt usado anteriormente:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório final e compare-o com o primeiro.

## Resumo e próximas etapas

Você personalizou e usou uma skill de agente existente. Nesta lição, você:

- explorou a skill `quality-checks` para testes de unidade, lint e verificações de tipos;
- personalizou o formato dos resultados;
- recarregou e executou a skill.

Essa alteração acompanhará a filtragem no PR do recurso. Em seguida, permita que o Copilot interaja diretamente com o site [por meio do servidor MCP do Playwright][next-lesson].

## Mais exemplos de skills

Estes exemplos da comunidade são referências, não tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e script de exportação integrado: `drawio`][drawio-example]
- [Testes no navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
