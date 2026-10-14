---
title: "Lição 8 — Criar e fazer merge do PR do recurso"
description: "Revise a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; em seguida, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações de instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única ramificação. Agora, revise-os em conjunto e abra um pull request. Você fez merge manualmente do pull request (PR) de avaliação por estrelas; desta vez, deixe o **Agent Merge** gerenciar o processo.

> [!NOTE]
> Em geral, dividiríamos o recurso, as atualizações das instruções, a atualização da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, todo o fluxo de filtragem e qualidade ficou em uma conversa e ramificação, e será incluído neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de merge;
- inspecionar a alteração completa do recurso e as evidências de verificação;
- criar o PR de filtragem;
- habilitar o Agent Merge somente após a revisão e confirmar que o PR foi mesclado.

## Cenário

Durante o fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização de merge sob o controle da pessoa desenvolvedora.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho restante para incorporar um pull request. Quando habilitado, o Copilot resolve o que estiver impedindo o merge — corrigindo verificações de integração contínua (CI) com falha, respondendo a comentários de revisão e fazendo rebase quando necessário — e, em seguida, habilita o auto-merge do GitHub quando o repositório permite.

Até agora, você selecionou **Merge pull request**. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que está pronto antes de habilitá-lo.

## Use o Agent Merge para gerenciar o PR

Com todo o código pronto, revise as alterações, crie o PR e deixe o Agent Merge gerenciar o restante do processo.

1. Volte ao Codespace.
2. Abra a caixa de diálogo do agente digitando `/agent`.
3. Selecione **Default** na lista de opções e pressione <kbd>Enter</kbd>.
4. Crie um novo PR com o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua no PR. Quando todas as verificações forem concluídas com êxito, ele fará o merge.
7. Confirme que o Copilot exibe uma mensagem semelhante a `PR #14 was squash-merged successfully.`

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de ramificações, filas de merge, configurações do repositório nem permissões ausentes. Se o processo for bloqueado, leia o motivo informado e faça manualmente o merge revisado quando o repositório permitir.

## Resumo e próximas etapas

Você automatizou várias partes do processo de desenvolvimento, incluindo a geração, os testes e a validação do código, além do fluxo do pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de merge;
- inspecionou a alteração completa do recurso e as evidências de verificação;
- criou o PR de filtragem;
- habilitou o Agent Merge somente após a revisão e confirmou que o PR foi mesclado.

Em seguida, [explore outros comandos slash úteis do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional à nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
