---
title: "Lição 12 — Publique o site com GitHub Pages"
description: "Use o aplicativo GitHub Copilot para configurar um workflow do Pages, revisar seu pull request e verificar o Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dê ao Space Quiz um endereço público publicando-o com o GitHub Pages. O Copilot pode preparar o workflow e orientar a publicação, mas cabe a você revisar as alterações e confirmar que o site está no ar.

Nesta lição, você vai:

- pedir ao Copilot que configure a publicação no GitHub Pages;
- revisar o workflow e o pull request antes de fazer merge;
- acompanhar a publicação e verificar o site publicado.

## Cenário

O Space Quiz já está em um repositório do GitHub, mas as pessoas ainda precisam de uma maneira simples de experimentá-lo. Publicar o site de arquivo único com o GitHub Pages o disponibiliza em um endereço uniforme de recursos (URL) público sem adicionar um servidor ou uma dependência de tempo de execução.

## Peça ao Copilot para publicar o site

A sessão do aplicativo Copilot referente a `space-quiz` vincula o pedido ao repositório correto.

1. Na sessão de `space-quiz`, peça ao agente que prepare a publicação enviando este prompt:

   ```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
   ```

![Diagrama em três etapas: um pull request de space-quiz mesclado; uma execução bem-sucedida de Deploy website no GitHub Actions, com as etapas Build website, Upload artifact e Deploy to Pages; e o Space Quiz publicado em yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pede ao agente que crie e mescle um pull request e altere as configurações do Pages no repositório. Revise as alterações, as permissões de publicação e todas as solicitações de aprovação do GitHub antes de prosseguir.

## Revise o workflow e verifique a publicação

1. Confirme que o README será adicionado somente se o repositório ainda não tiver um.
2. Revise as alterações do pull request, incluindo as etapas de compilação e publicação do workflow.
3. Faça merge somente depois que as verificações obrigatórias do pull request forem aprovadas.
4. Acompanhe a execução de publicação do GitHub Actions até que ela seja concluída com êxito.
5. Abra a URL informada e confirme que o Space Quiz carrega.
6. Se o agente não conseguir habilitar o Pages ou fazer merge do pull request, siga as instruções específicas que ele fornecer.
7. Confirme que a publicação foi bem-sucedida e que o site está carregando antes de considerar a tarefa concluída.

## Resumo e próximas etapas

Agora o Space Quiz está disponível como um site publicado, e você verificou a publicação em vez de confiar apenas no relato do agente. Continue em [Lição 13: Revisão e próximos passos][next-lesson].

[next-lesson]: ../13-review/
