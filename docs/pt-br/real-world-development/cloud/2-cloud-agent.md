---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Quase todas as organizações enfrentam dívida técnica, como problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos que ficaram no backlog por falta de tempo. O agente de nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidades de forma autônoma. Ao concluir o trabalho, ele cria um pull request de rascunho para revisão por um desenvolvedor. Assim, você pode delegar tarefas repetitivas e se concentrar em trabalho de maior impacto.

Neste exercício, você vai explorar como:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- criar issues com escopo claro.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys quer resolver parte da dívida técnica. Os prestadores de serviço contratados para criar a primeira versão do site deixaram a documentação em um estado inadequado: ela está completamente ausente. Como primeiro passo, a equipe quer comentários TSDoc em todas as funções exportadas do aplicativo.

A equipe de design também quer facilitar a descoberta de jogos. Ela deseja mostrar, na página de detalhes de cada jogo, outros jogos da mesma categoria. Não é preciso criar uma implementação refinada: a equipe precisa apenas de algo que possa usar para testes de aceitação da experiência. Essa tarefa está bloqueada por outras issues de prioridade mais alta no momento.

Essas tarefas podem ser adiadas facilmente. Atribua-as ao agente de nuvem para que ele trabalhe de forma assíncrona enquanto você cuida de outras atividades e, depois, revise o resultado.

## Conheça o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de maneira semelhante a um desenvolvedor. Você pode, por exemplo, [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão durante a execução ou depois da conclusão. Quando a solução estiver pronta para revisão, o agente marcará você no pull request.

## A importância de instruções com escopo claro

O GitHub Copilot não é mágico: ainda é preciso pensar. Mesmo tarefas aparentemente simples podem ter complexidades. [Defina com cuidado o escopo do trabalho para o agente de nuvem][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software continuam os mesmos.

## Instruções personalizadas neste repositório

Antes de atribuir trabalho, examine os arquivos de instruções incluídos no repositório:

- `.github/copilot-instructions.md`: examine **Code standards**, especialmente as convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que limita essas instruções a arquivos `**/*.test.ts`.

Ao atribuir a issue **Code lacks documentation** na próxima seção, procure no pull request comentários TSDoc, convenções de TypeScript e cabeçalhos de arquivo. Essas orientações vêm dos arquivos de instruções.

## Configure o ambiente de desenvolvimento do agente de nuvem

O agente de nuvem usa [GitHub Actions][github-actions] como ambiente. Você pode personalizá-lo com um fluxo de configuração em `.github/workflows/copilot-setup-steps.yml`, executado antes do início do trabalho, para disponibilizar ferramentas e dependências. O projeto já inclui uma configuração para instalar Node.js e dependências, baixar navegadores para testes de ponta a ponta e preparar o banco de dados SQLite da aplicação Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório durante a configuração, por exemplo, para instalar dependências, você precisa da permissão `contents: read`. Se não fizer isso, o Copilot clonará o repositório automaticamente depois da conclusão das etapas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo - Node.js (o aplicativo agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e preenche o banco de dados SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo segue o formato dos fluxos do GitHub Actions. O job `copilot-setup-steps` é executado antes do Copilot começar a trabalhar. O gatilho `workflow_dispatch` permite executar o fluxo manualmente na guia **Actions** para testá-lo.

## Adicione documentação

A documentação desatualizada ou inexistente é uma dívida técnica que reduz a produtividade e dificulta a manutenção e a integração de novos desenvolvedores. O Copilot pode criar essa documentação em segundo plano.

1. Em uma nova guia, acesse seu repositório no github.com.
2. Selecione **Issues** e depois **New issue**.
3. Selecione **Blank issue**.
4. Defina **Title** como `Code lacks documentation`.
5. Defina **Description** como:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

6. Selecione **Create**. À direita, selecione **Assign to Copilot** e depois **Assign**.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

7. Abra a guia **Pull Requests** e selecione o novo PR, com título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer, aguarde alguns minutos e atualize o navegador.
8. Aguarde o Copilot criar uma lista de tarefas. Isso pode levar alguns minutos enquanto o agente executa o fluxo de configuração mencionado anteriormente, analisa o projeto e determina a melhor abordagem.
9. Revise a lista e as tarefas que o Copilot planeja concluir. Na linha do tempo do PR, procure a atualização que indica que o Copilot começou a trabalhar e selecione **View session**.

   ![Detalhes da atribuição do Copilot](../../../_images/ex4-assign-copilot-details.png)

   ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador de status atualizado.

Talvez seja necessário atualizar a janela para ver o indicador de status atualizado. Na sessão ao vivo, role para acompanhar como o Copilot explora o código, entende o estado do projeto, pausa para planejar e decidir a abordagem e cria código. Isso pode levar vários minutos. A execução assíncrona permite que você cuide de outras tarefas; enquanto o Copilot trabalha, atribua outra issue e avance para outro trabalho.

## Adicione uma seção de jogos relacionados

O trabalho assíncrono permite que você e o agente cuidem de tarefas diferentes ao mesmo tempo. Atribua ao Copilot a solicitação da equipe de design:

1. No github.com, abra o repositório e selecione **Issues** > **New issue** > **Blank issue**.
2. Defina **Title** como `Show related games on the game details page`.
3. Defina **Description** como:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

4. Selecione **Create**, depois **Assign to Copilot** e **Assign**.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

O emoji 👀 no primeiro comentário indica que o Copilot começou a trabalhar. O agente de nuvem funciona de forma semelhante a um desenvolvedor: você não precisa acompanhá-lo continuamente, mas deve revisar o resultado depois da conclusão. Enquanto isso, explore os agentes personalizados.

![Emoji de olhos indicando que o Copilot está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

## Resumo e próximas etapas

Você explorou o [agente de nuvem do GitHub Copilot][copilot-agents], que pode trabalhar de forma assíncrona em dívida técnica, novos recursos ou migrações entre estruturas. Você personalizou o ambiente, considerou a segurança, definiu issues com escopo claro e atribuiu trabalho ao Copilot.

Com o agente trabalhando em segundo plano, passe a criar e usar agentes personalizados. O agente de nuvem também pode usar [servidores MCP][cloud-agent-mcp] e as instruções personalizadas vistas nos módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
