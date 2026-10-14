---
title: "Lição 10 — Recapitulação e próximos passos"
description: "Recapitule o fluxo de trabalho do Copilot CLI, os dois pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI em um fluxo contínuo de trabalho na Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e localizou a issue de filtragem já criada;
- adicionou avaliações por estrelas, revisou o resultado em um navegador com porta encaminhada e fez merge manual do primeiro pull request (PR);
- começou pela issue de filtragem, definiu a abordagem no modo Plan, desenvolveu no modo Autopilot e revisou no modo Interactive;
- orientou o agente com instruções personalizadas, personalizou a skill existente `quality-checks` e usou-a para executar testes de unidade, lint e verificações de tipos;
- adicionou o servidor Model Context Protocol (MCP) do Playwright e usou-o para explorar a filtragem em um navegador real;
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar requisitos, cobertura, resultados da skill e evidências do navegador;
- revisou a alteração completa de filtragem e autorizou o Agent Merge para o PR de filtragem;
- explorou comandos slash para contexto, modelos, compartilhamento e delegação opcional à nuvem.

## O que você entregou

O workshop tem dois marcos de PR, cada um em sua própria ramificação baseada na versão atualizada de `main`:

1. **Avaliações por estrelas:** exibir `starRating` e um estado explícito para jogos sem avaliação nos cartões.
2. **Filtragem e fluxo de qualidade:** implementar a filtragem, atualizar as instruções e aplicá-las ao recurso, personalizar o relatório de `quality-checks`, criar um perfil de QA e incluir os testes correspondentes.

Do planejamento da filtragem até a abertura do PR, você usou a mesma conversa e ramificação. Esse trabalho foi reunido em um único PR para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias maneiras: com testes automatizados, sua própria inspeção no navegador e a exploração do navegador pelo Copilot usando MCP. A skill `quality-checks` executou testes de unidade, lint e verificações de tipos e apresentou os resultados no novo formato. O agente de QA reuniu esses resultados com uma revisão dos requisitos e da cobertura de testes antes do PR.

Os testes adicionados devem preencher lacunas reais; uma execução de garantia de qualidade (QA) que conclui que não é necessário adicionar testes também pode estar correta. Revise o código e as evidências antes de autorizar o merge e atualize as evidências afetadas após qualquer alteração.

## Práticas recomendadas

O contexto e as ferramentas fornecidos ao Copilot influenciam o trabalho dele. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações em outras conversas e ajuste-as conforme as necessidades da equipe. Instruções definem padrões, skills descrevem tarefas repetíveis, agentes personalizados definem funções especializadas e servidores MCP conectam ferramentas externas. Revise as alterações reais e os resultados das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** adequados à tarefa. Use **Plan** para definir uma abordagem antes de desenvolver, **Interactive** para acompanhar alterações específicas e **Autopilot** para tarefas bem delimitadas. Escolha um modelo mais rápido para edições rotineiras e um modelo mais capaz para trabalhos complexos.

O contexto continua sendo tão importante quanto a infraestrutura. Descrever claramente *o que* você quer desenvolver, *por que* e *como* altera significativamente o resultado.

## Explore mais

Você conheceu o fluxo principal. Veja alguns outros recursos da CLI:

- `/review` para pedir ao agente de revisão de código que analise as alterações;
- `/rubber-duck` para discutir um problema e obter outra perspectiva;
- `/fleet` para coordenar subtarefas independentes em paralelo;
- `/worktree` para isolar outra tarefa;
- `/delegate` para enviar uma tarefa ao agente de nuvem do Copilot.

## Próximas etapas

A melhor maneira de aprimorar o uso de qualquer ferramenta é continuar usando-a. Use-a em código de produção, em projetos pessoais ou naquele pequeno aplicativo que você planeja desenvolver há anos. Compartilhe seus aprendizados com a equipe e aprenda com os dela. E, como sempre, explore a documentação.

Para ampliar a Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do agente de nuvem do Copilot][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
