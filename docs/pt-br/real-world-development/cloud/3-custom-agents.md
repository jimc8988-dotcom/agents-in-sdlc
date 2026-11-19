---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot são assistentes de IA especializados em tarefas ou domínios do fluxo de desenvolvimento. Arquivos Markdown na pasta `.github/agents` do repositório fornecem instruções, práticas recomendadas, padrões de código e conhecimento específico. As equipes podem transformar sua experiência em agentes reutilizáveis: por exemplo, um agente de acessibilidade que aplica as diretrizes [WCAG][wcag], um agente de segurança ou um agente de testes.

Os agentes podem ser definidos em arquivos Markdown dentro de `.github/agents` ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialização e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] são definidos principalmente em arquivos Markdown e orientam uma IA na execução de tarefas. Uma distinção útil: o **agente personalizado** é quem realiza o trabalho, e as **skills** são ferramentas.

Agentes personalizados têm sua própria janela de contexto e podem orquestrar skills e até outros agentes. Neste exercício, o agente de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade e poderá usar, por exemplo, uma skill de fluxo de pull request ou uma skill que executa e gerencia testes.

> [!NOTE]
> Não há uma única maneira certa de escrever um agente personalizado. Teste e itere para descobrir o que funciona em cada ambiente e cenário.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Neste exercício, você vai:

- entender como agentes personalizados são definidos.
- atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys quer que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas habilidades ou preferências visuais. Usuários relataram que o tema escuro é difícil de ler por causa do contraste insuficiente entre o texto e o fundo. A equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado. Como a acessibilidade é importante, use um agente personalizado para implementar rapidamente a funcionalidade.

## Revise o agente personalizado de acessibilidade

Um agente de acessibilidade já foi criado. Volte ao Codespace e examine `.github/agents/accessibility.md`.

1. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

2. Examine as seções sobre responsabilidades, práticas recomendadas, exemplos de HTML, CSS e JavaScript e erros comuns.

## Crie e atribua uma issue

O painel de agentes do GitHub funciona como centro de controle: nele, você pode atribuir tarefas ao agente de nuvem, acompanhar o trabalho e fornecer orientação adicional. Crie uma issue para adicionar o modo de alto contraste:

1. Acesse seu repositório e selecione **Issues** > **New issue** > **Blank issue**.
2. Defina **Title** como `Add high contrast mode to website`.
3. Defina **Description** como:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

4. Selecione **Create** e, à direita, **Assign to Copilot**.
5. Selecione **Accessibility agent** na lista de agentes personalizados e, em seguida, **Assign**.

   ![Seleção do agente personalizado de acessibilidade na atribuição ao agente de nuvem](../../../_images/ex5-select-custom-agent.png)

O Copilot começará a trabalhar em segundo plano.

## Resumo e próximas etapas

Você explorou [agentes personalizados][custom-agents], assistentes especializados que codificam o conhecimento e os padrões da equipe para orientar o Copilot. Você examinou como agentes personalizados são definidos e atribuiu uma tarefa ao agente.

Enquanto o Copilot implementa o modo de alto contraste, [acompanhe e oriente a sessão do agente][next-lesson] no painel de controle.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar o uso de agentes personalizados na organização][org-custom-agents]
- [Preparar o uso de agentes personalizados na empresa][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
