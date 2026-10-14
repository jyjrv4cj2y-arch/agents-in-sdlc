---
title: "Exercício 5 — Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design explorar e implementasse melhorias de acessibilidade, incluindo opções de alto contraste e modo claro. A revisão confirma se as alterações correspondem ao escopo solicitado e permite pedir ajustes antes da aprovação.

### Cenário

Como já destacamos várias vezes, os fundamentos de design de software e das práticas de desenvolvimento e operações (DevOps) não mudam com a chegada da inteligência artificial generativa. Sempre é necessário revisar o código gerado e seguir o processo habitual de DevOps. As sugestões do GitHub Copilot para a documentação, os jogos relacionados e os recursos de acessibilidade precisam ser revisadas antes que o restante da equipe seja convidado a revisar.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, há restrições de segurança para manter o trabalho protegido:

- O Copilot tem acesso somente de leitura ao repositório e acesso de gravação **somente** à ramificação que usará para o código.
- O agente de nuvem é executado no GitHub Actions, em um ambiente separado e temporário.
- Todos os workflows do GitHub Actions precisam de aprovação humana antes de serem executados.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), inclusive o acesso a servidores do protocolo Model Context Protocol (MCP).

## Revise a documentação gerada

Comece pelo primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot, que adiciona documentação ao código. A interface padrão de PRs no GitHub.com permite revisar os arquivos alterados e aprovar os workflows necessários.

> [!NOTE]
> Ao explorar o PR, talvez você veja um aviso de que o firewall bloqueou o GitHub Copilot. Isso é **esperado**, pois o acesso do Copilot a recursos externos é limitado por padrão, inclusive chamadas a servidores MCP externos. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem do Copilot][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com um título semelhante a **Add missing documentation**. Se o Copilot ainda estiver trabalhando, o pull request exibirá a indicação **[WIP]**; nesse caso, aguarde até que o trabalho seja concluído.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui os novos comentários de documentação TSDoc e outros elementos de documentação. As alterações exatas variam.

    Ao revisar as alterações, procure comentários TypeScript Documentation (TSDoc), convenções TypeScript e cabeçalhos de comentário. Eles vêm dos arquivos de instruções personalizadas examinados no início do Exercício 2.

6. Depois de revisar as alterações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns workflows aguardam aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão aparecer na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Peça alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você também pode marcar o Copilot em comentários no PR ou em comentários associados a linhas de código, assim como faria com outras pessoas da equipe. O Copilot verá esses comentários e iniciará outra sessão para tratá-los. Como os resultados são não determinísticos, não há um texto único que você precise usar. Algumas ideias de alterações que pode solicitar:

- adicionar um cabeçalho de comentário no início de cada arquivo de código com uma breve descrição da finalidade;
- adicionar comentários de documentação TSDoc aos arquivos TypeScript e Astro;
- criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e marque **@copilot**, como faria com qualquer outra pessoa. Use uma das ideias acima ou sugira outra alteração de documentação que gostaria de ver na base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao pull request.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, deverá aparecer um novo commit no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

6. Continue iterando até que as alterações atendam às suas expectativas. Quando estiver tudo pronto, marque o PR como pronto para revisão e faça merge na ramificação principal.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

O PR referente à issue sobre a exibição de jogos relacionados na página de detalhes contém as alterações do segundo recurso.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as alterações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
6. Você deverá ver um indicador de que alguns workflows aguardam aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows deverão aparecer na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** Você também pode mudar para essa ramificação no Codespace e testar manualmente o recurso de jogos relacionados. Acesse o Codespace, abra o terminal e execute os comandos abaixo (substitua `<branch-name>` pelo nome da ramificação criada pelo Copilot, por exemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

10. Se quiser iterar com o agente de nuvem do Copilot, adicione outro comentário na guia **Conversation** para solicitar alterações, como mudar a quantidade de jogos relacionados ou adicionar cabeçalhos de comentário e comentários de documentação TSDoc. Lembre-se de que esta issue foi atribuída **antes** da atualização das instruções personalizadas.

## Revise os recursos de acessibilidade

O último PR desta revisão reúne os recursos de acessibilidade implementados pelo agente personalizado: o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no painel de controle no Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website**. Se o Copilot ainda estiver trabalhando, o pull request exibirá a indicação **[WIP]**; nesse caso, aguarde a conclusão do trabalho.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - componentes de interface para alternar entre os modos;
   - uso do armazenamento local para manter as preferências das pessoas usuárias;
   - alterações de folhas de estilo em cascata (CSS) ou estilo dos modos de alto contraste e claro;
   - atributos de Accessible Rich Internet Applications (ARIA), como rótulos e navegação por teclado;
   - código JavaScript/TypeScript que gerencia a alternância de modos.

6. Depois de revisar as alterações e confirmar que estão corretas, volte à guia **Conversation** e role a página para baixo.
7. Você deverá ver um indicador de que alguns workflows aguardam aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão aparecer na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** Você pode mudar para essa ramificação no Codespace e testar manualmente os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute os comandos abaixo (substitua `<branch-name>` pelo nome da ramificação criada pelo Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste as opções de alto contraste e modo claro no navegador. Confirme que funcionam e que as preferências persistem após recarregar a página.

11. Revise como o agente personalizado de acessibilidade orientou o Copilot a implementar os recursos seguindo práticas recomendadas de acessibilidade. Se encontrar problemas ou oportunidades de melhoria, marque **@copilot** em um comentário para solicitar alterações, como fez nos PRs anteriores.

## Exercício opcional — continue delegando

O agente de nuvem é mais útil quando você pode delegar itens reais do backlog e se concentrar em outro trabalho. Para desenvolver esse hábito, registre uma issue e atribua-a ao Copilot:

1. Escolha uma destas ideias:
   - criar um formulário de interesse de apoiadores na página de detalhes dos jogos;
   - implementar paginação na lista de jogos;
   - adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Ao longo das lições, você:

- **inspecionou os arquivos de instruções personalizadas incluídos no repositório** para observar posteriormente seu efeito no resultado do agente de nuvem;
- **atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona;
- **criou e usou um agente personalizado** de acessibilidade para adicionar opções de alto contraste e modo claro;
- **usou a página de agentes do Copilot como painel de controle** para acompanhar e orientar a sessão de acessibilidade em andamento;
- **revisou e iterou sobre os pull requests do agente de nuvem**, marcando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Se quiser continuar explorando, os outros percursos complementam o que praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas no terminal usando os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, Playwright MCP, Agent Merge e a ponte opcional `/delegate` para o agente de nuvem.

No seu próprio repositório, experimente estas ideias:

- atribuir ao agente de nuvem uma issue de refatoração ou cobertura de testes;
- criar um agente personalizado para outro domínio;
- configurar `copilot-setup-steps.yml` para outra pilha de tecnologias.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
