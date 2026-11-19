---
title: "Lição 10 - Encerramento e próximos passos"
description: "Recapitule o fluxo de trabalho do Copilot CLI, os dois pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI em um fluxo contínuo de trabalho na Tailspin Toys. Você preparou um Codespace, instalou o Copilot CLI, explorou o projeto e encontrou a issue de filtragem; adicionou avaliações por estrelas e integrou manualmente seu primeiro pull request (PR); planejou a filtragem no modo Plan, desenvolveu no Autopilot e revisou no Interactive; orientou o agente com instruções personalizadas; personalizou a skill `quality-checks`; usou o servidor Model Context Protocol (MCP) do Playwright para explorar a filtragem no navegador; criou um agente personalizado de garantia de qualidade (QA); revisou a alteração e autorizou o Agent Merge; e explorou comandos de barra para contexto, modelos, compartilhamento e delegação opcional à nuvem.

## O que você entregou

O workshop inclui dois marcos de PR, cada um em seu próprio branch criado a partir de `main` atualizado:

1. **Avaliações por estrelas:** exibir nos cards dos jogos o valor existente de `starRating` e um estado explícito para jogos sem avaliação.
2. **Filtragem e fluxo de qualidade:** implementar a filtragem, atualizar e aplicar as instruções ao recurso, personalizar o relatório de `quality-checks`, criar um perfil de QA e incluir os testes relacionados.

Do planejamento da filtragem até a abertura do PR, você usou a mesma conversa e o mesmo branch. Esse trabalho foi reunido em um único PR para agilizar o workshop.

## Diferentes tipos de verificação

Você verificou o código com testes automatizados, uma inspeção própria no navegador e a exploração do navegador pelo Copilot com MCP. A skill `quality-checks` executou testes unitários, lint e verificações de tipos e apresentou os resultados no novo formato. Antes do PR, o QA reuniu esses resultados e revisou os requisitos e a cobertura de testes.

Os testes adicionados devem resolver lacunas reais; uma execução de QA que não exija novos testes também pode estar correta. Revise o código e as evidências antes de autorizar o merge e atualize as evidências afetadas depois de qualquer alteração.

## Práticas recomendadas

O contexto e as ferramentas fornecidos ao Copilot influenciam o trabalho. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações em outras conversas e ajuste-as conforme as necessidades da equipe. Instruções definem padrões, skills descrevem tarefas repetíveis, agentes personalizados definem funções especialistas e servidores MCP conectam ferramentas externas. Revise as alterações e os resultados das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** adequados à tarefa. Use **Plan** para avaliar uma abordagem antes de começar, **Interactive** para acompanhar alterações específicas e **Autopilot** para tarefas bem delimitadas. Escolha um modelo mais rápido para edições rotineiras e outro mais capaz para trabalhos complexos.

O contexto continua sendo tão importante quanto a infraestrutura. Descrever claramente *o que* você quer criar, *por que* e *como* influencia significativamente o resultado.

## Explore mais

Você concluiu o fluxo de trabalho principal. Confira outros recursos da CLI:

- `/review` para pedir ao agente de revisão de código que analise as alterações.
- `/rubber-duck` para discutir um problema e obter outra perspectiva.
- `/fleet` para coordenar subtarefas independentes em paralelo.
- `/worktree` para isolar outra tarefa.
- `/delegate` para enviar uma tarefa ao agente de nuvem do Copilot.

## Próximas etapas

A melhor maneira de melhorar com qualquer ferramenta é continuar usando-a. Use-a em código de produção, projetos pessoais e naquele pequeno aplicativo que você planeja criar há anos. Compartilhe o que aprendeu com a equipe, aprenda com os outros e explore a documentação.

Se quiser ampliar a Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para conhecer outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do agente de nuvem do Copilot][cloud].

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
