---
title: "Lição 9 — Comandos slash no GitHub Copilot CLI"
description: "Explore comandos slash para gerenciar contexto, selecionar modelos, compartilhar sessões e delegar tarefas à nuvem, se desejar."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como toda boa ferramenta de CLI, o GitHub Copilot CLI inclui vários comandos slash. Eles dão acesso a recursos avançados, informações internas e opções adicionais de configuração. Você já usou comandos como `/diff`, `/mcp`, `/skills`, `/agent` e `/pr`. Agora, explore outros comandos úteis.

Nesta lição, você vai:

- usar `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa;
- usar `/model` para explorar os modelos disponíveis;
- aprender como `/share` exporta ou compartilha uma sessão;
- explorar comandos opcionais para trabalho paralelo, worktrees e delegação ao agente de nuvem.

## Cenário

Você concluiu o fluxo de trabalho principal da CLI. Agora, conheça alguns recursos adicionais: gerenciamento do contexto, alternância de modelos, compartilhamento de sessões e, opcionalmente, delegação ao [agente de nuvem do Copilot][about-cloud-agent].

## Explore o contexto no Copilot CLI

Em tarefas grandes ou complexas, você poderá atingir o limite da janela de contexto do modelo. O Copilot CLI compacta a conversa automaticamente quando necessário; você também pode inspecionar ou compactar o contexto com comandos slash.

1. Volte ao Codespace e inicie o Copilot CLI na raiz do repositório, se ainda não estiver aberto.
2. Digite:

   ```plaintext
   /context
   ```

3. Observe o modelo, o uso atual de tokens e a divisão do contexto entre instruções do sistema, ferramentas, mensagens e espaço livre.
4. Compacte a conversa:

   ```plaintext
   /compact
   ```

5. Digite `/context` novamente e compare o resultado. Talvez a diferença não seja grande se a conversa já for curta.

> [!NOTE]
> O Copilot CLI compacta o contexto automaticamente à medida que a janela fica cheia. Use `/compact` para escolher quando isso acontece. Use `/clear` ou `/new` ao mudar para uma tarefa sem relação e iniciar uma conversa nova.

## Escolha seu modelo

Modelos diferentes têm pontos fortes diferentes, assim como cada pessoa desenvolvedora tem suas preferências. O Copilot CLI permite listar e selecionar o modelo que você quer usar.

1. Digite:

   ```plaintext
   /model
   ```

2. Explore os modelos disponíveis e as informações de uso.
3. Mantenha o modelo atual, selecione outro ou pressione <kbd>Esc</kbd> para fechar a lista.

## Compartilhe uma sessão

Trabalhar em equipe e compartilhar aprendizados ajuda todas as pessoas a usar melhor as ferramentas de inteligência artificial. O comando `/share` pode exportar uma sessão para um arquivo Markdown ou HTML, criar um link compartilhável ou publicar um gist do GitHub.

1. Digite `/help` e revise as opções de `/share` disponíveis na versão instalada.
2. Se quiser compartilhar a sessão, escolha o destino adequado, como `/share file` para exportá-la localmente em Markdown.
3. Revise o conteúdo exportado antes de enviá-lo a alguém ou publicá-lo. As exportações podem incluir prompts, respostas e detalhes do projeto.

A publicação de um link ou gist é opcional. Não publique conteúdo do repositório ou da conversa que a equipe não pretenda compartilhar.

## Opcional: amplie o trabalho ou delegue tarefas

O workshop principal está concluído. O Copilot CLI também oferece comandos para tarefas maiores:

- `/fleet` pode dividir subtarefas independentes entre subagentes e executá-las em paralelo;
- `/worktree` pode criar uma worktree isolada para outra tarefa;
- `/delegate` pode enviar uma tarefa ao agente de nuvem do Copilot, que trabalha de forma assíncrona e pode abrir um pull request.

Esses comandos são opcionais porque podem criar worktrees adicionais ou iniciar trabalho remoto. Comece uma tarefa nova e bem delimitada antes de experimentá-los e revise o resultado pelo fluxo de trabalho habitual. Para saber mais sobre o trabalho assíncrono com agentes, continue no [workshop do agente de nuvem][cloud-workshop].

## Resumo e próximas etapas

Os comandos slash permitem configurar o Copilot CLI, compartilhar sessões e acompanhar o que acontece nos bastidores. Nesta lição, você:

- usou `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa;
- usou `/model` para explorar os modelos disponíveis;
- aprendeu como `/share` exporta ou compartilha uma sessão;
- explorou comandos opcionais para trabalho paralelo, worktrees e delegação ao agente de nuvem.

Há outros comandos slash e recursos do Copilot CLI para explorar. Para encerrar este percurso, [revise o que aprendeu][next-lesson] e conheça os próximos passos.

## Recursos

- [Referência de comandos do Copilot CLI][cli-reference]
- [Gerenciamento de contexto no Copilot CLI][context-management]
- [Sobre o agente de nuvem do Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
