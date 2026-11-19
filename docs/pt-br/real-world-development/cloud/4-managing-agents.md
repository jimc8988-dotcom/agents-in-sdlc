---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos exercícios anteriores, você atribuiu três tarefas ao agente de nuvem do Copilot para melhorar a experiência e adicionar funcionalidades. Embora o agente trabalhe de forma assíncrona e autônoma, é importante acompanhar essas tarefas.

Você pode gerenciar o trabalho atribuído ao agente de nuvem usando várias ferramentas, inclusive a [página de agentes][agents-page] no GitHub.com. Esse centro de controle mostra as tarefas com pull requests (PRs abertos), as operações realizadas e permite orientar sessões em andamento.

Neste exercício, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente de nuvem.
- orientar uma sessão em andamento e solicitar uma funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a criação do modo de alto contraste, a equipe decidiu adicionar também um modo claro. Como o agente já está atualizando o estilo do site e adicionando controles de alternância, peça que inclua os dois modos.

## Revise as tarefas do agente de nuvem

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Examine as tarefas nas áreas principal e lateral. Você deve encontrar a documentação do código, APIs para modificar jogos e o modo de alto contraste.
3. Selecione uma tarefa em andamento. Observe operações que o Copilot pode realizar, como obter o código, configurar o ambiente, preparar servidores MCP e executar as etapas da tarefa.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado do trabalho e a abordagem do Copilot.

4. Examine o painel de pull request à direita, que mostra o PR e os arquivos alterados.

## Oriente o agente de nuvem

Peça ao Copilot para adicionar o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão do modo de alto contraste. O título exato depende do nome escolhido pelo Copilot e do andamento do trabalho.

   ![Sessão de acessibilidade no centro de controle](../../../_images/ex6-accessibility-session.png)

2. Aguarde alguns minutos, até o agente concluir a configuração e iniciar o trabalho.
3. Na caixa de diálogo **Steer active session while Copilot is working**, envie este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Caixa de diálogo para orientar uma tarefa do agente de nuvem](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd>. Observe que o Copilot reconhece a solicitação e a incorpora ao trabalho.

## Deixe o Copilot trabalhar

O Copilot incorporará a nova solicitação depois de concluir a etapa em andamento. Isso pode levar alguns minutos; aproveite o tempo para revisar o que aprendeu.

## Resumo e próximas etapas

Você explorou a página de agentes do Copilot, o centro para acompanhar e orientar tarefas do agente de nuvem. Você monitorou as tarefas e redirecionou uma sessão em andamento para adicionar uma funcionalidade. Em seguida, [itere sobre os pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
