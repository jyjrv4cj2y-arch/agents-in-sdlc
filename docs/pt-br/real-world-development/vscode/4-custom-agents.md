---
title: "Exercício 4 — Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de inteligência artificial especializados para tarefas ou domínios específicos do fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você fornece instruções, práticas recomendadas, padrões de codificação e conhecimento específico do domínio para orientar o Copilot a executar determinados tipos de trabalho com mais eficiência. As equipes podem registrar sua experiência em agentes reutilizáveis — por exemplo, um agente de acessibilidade que aplica as Diretrizes de Acessibilidade para Conteúdo da Web (WCAG), um agente de segurança que segue práticas de codificação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter em YAML (YAML Ain't Markup Language) com, no mínimo, os campos `name` e `description`, seguidos por um prompt Markdown que define o comportamento, a especialização e as instruções do agente.

### Agentes personalizados e skills de agente

Há alguma sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e orientam uma inteligência artificial sobre como executar operações. A distinção mais simples é: um **agente personalizado** é quem executa o trabalho; as **skills** são as ferramentas.

Os agentes personalizados têm sua própria janela de contexto e foram criados para orquestrar skills (e até outros agentes) durante o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisa e atualiza o site de acordo com diretrizes de acessibilidade. Durante esse trabalho, ele pode chamar skills, como uma skill de fluxo de trabalho para pull requests ou outra que executa e gerencia testes.

> [!NOTE]
> Não existe uma única maneira correta de criar um agente personalizado. Como em qualquer trabalho com inteligência artificial, teste e itere para encontrar o que funciona melhor nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys está comprometida em tornar sua plataforma de financiamento coletivo acessível a todas as pessoas, independentemente das capacidades ou preferências visuais. Comentários recentes de usuários destacaram que algumas pessoas têm dificuldade para ler o tema escuro atual devido ao contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver essa questão de acessibilidade, a equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente;
- usar esse agente no Copilot Chat para implementar um modo de alto contraste.

## Revise o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado. A revisão da configuração mostra como ele orientará o Copilot. Uma ramificação separada mantém o PR de filtragem do Exercício 3 isolado do trabalho de acessibilidade.

1. Volte ao codespace, abra um terminal e crie uma nova ramificação baseada em `main`:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. Abra `.github/agents/accessibility.md`.
3. Observe o frontmatter em YAML (YAML Ain't Markup Language) com os campos `name` e `description`.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

4. Leia as seções seguintes, que destacam:
   - as principais responsabilidades ao gerar código para um site acessível;
   - práticas recomendadas de acessibilidade;
   - exemplos de código em linguagem de marcação de hipertexto (HTML), folhas de estilo em cascata (CSS) e JavaScript;
   - armadilhas e erros comuns.

## Use o agente personalizado no Copilot Chat

O VS Code mostra todos os agentes personalizados definidos em `.github/agents` na lista suspensa de agentes, na parte inferior da visualização Copilot Chat. Você pode selecionar um agente personalizado para limitar a sessão de chat às instruções e ferramentas dele.

> [!TIP]
> **Abra o Copilot Chat**
>
> A seleção do modo e do modelo varia por exercício; cada etapa indica as opções relevantes.

1. Volte ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa.
2. Na lista suspensa de agentes da visualização Chat, selecione **Agent**, caso ainda não esteja selecionado.

   ![Captura de tela do seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

3. Selecione a lista suspensa de agentes na parte inferior da visualização Chat. Ela mostra o agente ativo, que por padrão é **default**.
4. Selecione **Accessibility agent** na lista de agentes disponíveis.
5. Envie este prompt ao agente de acessibilidade:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

6. O Copilot Chat explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá na conversa com o caminho do arquivo e um diff para revisão.
7. À medida que as edições forem aplicadas, o indicador **Files changed** será atualizado para mostrar o conjunto de arquivos alterados pelo agente.

> [!NOTE]
> O processo provavelmente levará alguns minutos. O Copilot está fazendo alterações reais no repositório — editará arquivos existentes, como componentes Astro, CSS e testes relacionados.

Você revisará e orientará esse trabalho em andamento no próximo exercício.

## Resumo e próximas etapas

Você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes especializados para tarefas e domínios específicos. Com agentes personalizados, você pode registrar a experiência e os padrões da equipe em agentes reutilizáveis que orientam o Copilot a executar certos tipos de trabalho com mais eficiência.

Você explorou estes conceitos:

- como agentes personalizados são definidos;
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, [acompanhe e oriente o trabalho do agente][next-lesson], revise as alterações à medida que forem feitas e adicione uma opção de modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no ambiente de desenvolvimento integrado (IDE)][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar o recurso com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
