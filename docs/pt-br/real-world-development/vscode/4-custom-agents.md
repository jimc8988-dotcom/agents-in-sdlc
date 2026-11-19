---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot são assistentes de IA especializados em tarefas ou domínios do desenvolvimento. Arquivos Markdown na pasta `.github/agents` fornecem instruções, práticas recomendadas, padrões de código e conhecimento específico. As equipes podem reutilizar sua experiência em agentes, como um agente de acessibilidade que aplica as diretrizes [WCAG][wcag], um agente de segurança ou um agente de testes.

Os agentes são definidos por arquivos Markdown em `.github/agents` ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento e as instruções.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] são definidos principalmente em arquivos Markdown. Uma forma simples de distingui-los: o **agente personalizado** é quem realiza o trabalho; as **skills** são ferramentas. Agentes têm sua própria janela de contexto e podem orquestrar skills e outros agentes. O agente de acessibilidade deste exercício pode revisar o site e usar skills para executar testes ou fluxos de pull request.

> [!NOTE]
> Não há uma única maneira certa de criar um agente. Teste e itere conforme o ambiente e o cenário.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer tornar sua plataforma acessível a todos. Usuários relataram que o tema escuro é difícil de ler devido ao contraste insuficiente. A equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado. Use um agente personalizado para implementar a funcionalidade.

Neste exercício, você vai revisar o agente de acessibilidade existente e usá-lo no Copilot Chat para implementar o modo de alto contraste.

## Revise o agente personalizado de acessibilidade

Volte ao Codespace e crie um branch separado do `main` para o trabalho de acessibilidade. O PR da filtragem do Exercício 3 permanecerá separado:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md` e observe os campos `name` e `description` no frontmatter YAML.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

2. Examine as responsabilidades, as práticas de acessibilidade, os exemplos de HTML, CSS e JavaScript e os erros comuns.

## Use o agente personalizado no Copilot Chat

O VS Code mostra os agentes personalizados de `.github/agents` no seletor de agentes do Copilot Chat. A seleção de um agente aplica suas instruções e ferramentas à conversa.

> [!TIP]
> **Abra o Copilot Chat**
>
> No Codespace, abra o Copilot Chat e selecione **New Chat**. O modo e o modelo variam por exercício.

1. No seletor de agentes, selecione **Agent**, se ainda não estiver selecionado.

   ![Seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Abra o seletor de agentes na parte inferior da exibição Chat, onde aparece o agente ativo (**default** inicialmente).
3. Selecione **Accessibility agent**.
4. Envie este prompt:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. O Copilot Chat explorará o código, proporá alterações e editará os arquivos do projeto. Cada edição aparece na conversa com o caminho e um diff.
6. Observe o indicador **Files changed** para acompanhar os arquivos alterados.

> [!NOTE]
> Esse processo pode levar alguns minutos. O Copilot pode alterar componentes Astro, CSS e testes relacionados. Você acompanhará e orientará o trabalho no próximo exercício.

## Resumo e próximas etapas

Você explorou [agentes personalizados][custom-agents], assistentes especializados que incorporam práticas e conhecimentos da equipe. Você examinou a definição de agentes e usou um agente personalizado no modo agente do Copilot Chat.

Em seguida, [acompanhe e oriente o trabalho do agente][next-lesson], revise as alterações e peça a inclusão de um controle do modo claro na mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar seu recurso com o MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
