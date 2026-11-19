---
title: "Lição 7 - Crie e use um agente de QA"
description: "Crie um agente personalizado de QA que combine os requisitos da issue, a skill quality-checks e o MCP do Playwright."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você usou a skill `quality-checks` para executar verificações automatizadas e o MCP do Playwright para observar a filtragem no navegador. Agora, combine esses recursos em um agente personalizado com um processo de QA bem definido.

Nesta lição, você vai:

- explorar como um agente personalizado usa instruções, skills e ferramentas MCP.
- criar e inspecionar um perfil reutilizável de garantia de qualidade.
- selecionar o agente de QA e revisar as descobertas com base na issue de filtragem.

## Cenário

Antes de abrir um pull request (PR), a Tailspin Toys quer revisar de forma consistente os requisitos, a qualidade do código, as verificações automatizadas, a cobertura de testes e o comportamento no navegador. Um agente personalizado pode coordenar esse processo e gerar um relatório reutilizável.

## O que é um agente personalizado?

Um agente personalizado é uma versão especializada do Copilot, definida em um perfil Markdown que descreve sua finalidade, suas instruções e as ferramentas disponíveis. Neste workshop, você definirá a função de QA em `.github/agents/qa.agent.md` e a selecionará no Copilot CLI.

Cada personalização tem uma finalidade: as instruções do repositório descrevem os padrões da equipe; a skill `quality-checks` agrupa verificações repetíveis; o MCP do Playwright fornece ferramentas de navegador; e o perfil de QA orienta o Copilot a usar esses recursos para avaliar os requisitos e relatar as descobertas. O perfil não substitui esses recursos nem exige outra conversa.

## Crie o perfil de QA

Antes de abrir o PR do recurso, peça ao Copilot para criar um perfil de QA reutilizável, com verificações e limites definidos.

1. Volte ao Codespace e confirme se a conversa de filtragem está no modo Interactive.
2. Peça ao agente padrão para criar o agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecione o perfil

Revise o perfil antes de usar o agente para confirmar se ele capturou o fluxo e os limites de QA. Isso evita que um agente incompleto ou com escopo amplo demais altere o recurso quando você quer apenas verificá-lo.

1. Digite `/diff` e abra `.github/agents/qa.agent.md`.
2. Leia o frontmatter. `description` é obrigatório; `name` é opcional, mas fornece um nome claro ao agente.
3. Leia as instruções e confirme se o QA começa pelos requisitos, segue as instruções do repositório, executa a skill `quality-checks` e usa o MCP do Playwright.
4. Confirme se o QA apresenta evidências, pergunta antes de alterar o código de implementação e não confirma alterações nem abre PRs.
5. Se o perfil não incluir alguma responsabilidade ou limite, peça ao agente padrão para revisá-lo antes de continuar.

## Execute o QA com base na issue

O Copilot CLI carrega os agentes do projeto no início de uma conversa. Retome a conversa de filtragem e selecione o QA para usar a issue e as decisões de planejamento já existentes no contexto.

1. Ative o agente com este prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como você acabou de criar o agente, talvez ele não apareça na lista de agentes disponíveis. Mesmo assim, o comando acima o ativa.

2. Peça ao agente para revisar o recurso:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. Aguarde o agente concluir o trabalho.
4. Leia o relatório.

## Resumo e próximas etapas

Você adicionou uma função especialista reutilizável ao fluxo e revisou o trabalho feito por ela. Agora, a implementação, a atualização da skill, o perfil de QA, os testes e o relatório de verificação estão prontos para revisão. Em seguida, [reúna esses elementos em um PR do recurso e use o Agent Merge][next-lesson].

## Recursos

- [Criar agentes personalizados para o Copilot CLI][create-agents]
- [Configuração de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
