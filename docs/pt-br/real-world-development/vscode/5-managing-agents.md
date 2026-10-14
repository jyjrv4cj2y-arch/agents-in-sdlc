---
title: "Exercício 5 — Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo de agente, o GitHub Copilot trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real no computador, o Copilot Chat no VS Code mostra cada chamada de ferramenta e cada edição de arquivo. Você pode revisar cada diff no próprio chat, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts adicionais para refinar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- acompanhar em tempo real o trabalho do agente de acessibilidade;
- revisar e aceitar as edições de arquivo propostas;
- orientar o agente durante a sessão para adicionar uma opção de modo claro junto ao modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Durante o trabalho, a equipe de design solicitou também uma opção de modo claro para quem considera o tema padrão escuro demais. Em vez de iniciar uma nova conversa, oriente a sessão existente para ampliar o trabalho.

## Acompanhe o progresso do Copilot

A visualização Copilot Chat mostra cada etapa do agente: os arquivos que explora, os comandos que executa e as edições propostas. Cada alteração de arquivo aparece no chat com um diff e os controles **Keep** e **Undo**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra o chat para acompanhar cada chamada de ferramenta, leitura e edição de arquivo e os comandos de terminal que ele solicitar para executar.
3. Quando o agente propuser uma edição, o diff aparecerá na conversa:
   - Selecione **Keep** para aceitar a alteração.
   - Selecione **Undo** para revertê-la e permitir que o agente tente novamente.
4. Abra o indicador **Files changed** na parte superior do chat para ver o conjunto completo de arquivos alterados pelo agente durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta solicitar permissão — por exemplo, para executar um comando do terminal —, o agente fará uma pausa e exibirá uma solicitação **Allow**. Aprove somente os comandos que considerar seguros.

5. Aguarde o agente concluir a implementação do modo de alto contraste. Ele deverá exibir uma mensagem final resumindo as alterações.

## Oriente o agente para adicionar o modo claro

Com o modo de alto contraste implementado, amplie a mesma conversa para adicionar uma opção de modo claro. Como você continuará na mesma sessão, o agente manterá o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilo e o padrão de armazenamento local.

1. No campo de entrada do Copilot Chat, envie este prompt complementar:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente continuará de onde parou, explorará as alterações existentes e proporá adições para o modo claro.
3. Revise cada nova edição à medida que for proposta, como fez com o modo de alto contraste. Use **Keep** ou **Undo** em cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar a interface dos controles de uma maneira que você não prefere —, interrompa-o com o botão **Stop** ao lado do campo de chat e envie uma mensagem complementar para esclarecer o pedido.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts complementares para pedir ajustes (“simplifique as folhas de estilo em cascata (CSS) — use variáveis CSS em vez de duplicar seletores”), corrigir a abordagem (“não adicione um terceiro controle; inclua o modo claro no painel de configurações existente”) ou ampliar o escopo (“adicione também atalhos de teclado para as duas opções”).

## Revise o conjunto de alterações

Revisar o conjunto completo de alterações antes do commit ajuda a confirmar que o resultado corresponde ao que você solicitou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações em componentes Astro, estilos e eventuais testes relacionados.
3. Abra alguns arquivos alterados e examine os diffs. Confirme que a implementação segue as práticas de acessibilidade do agente personalizado: atributos de Accessible Rich Internet Applications (ARIA), navegação por teclado, linguagem de marcação de hipertexto (HTML) semântica e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximas etapas

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento: revisou cada edição de arquivo, aceitou ou rejeitou alterações e enviou prompts complementares para ampliar o trabalho sem perder o contexto.

Nesta lição, você:

- acompanhou o trabalho de um agente em tempo real;
- aceitou e rejeitou edições individuais de arquivos;
- orientou uma sessão durante a execução com prompts complementares.

Na [próxima lição][next-lesson], publique o trabalho de acessibilidade em um pull request e revise as alterações produzidas pelo agente personalizado local durante este laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
