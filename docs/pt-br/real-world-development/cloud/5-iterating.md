---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Durante este laboratório, você usou o GitHub Copilot em tarefas para melhorar a experiência e adicionar funcionalidades: documentar o código, implementar jogos relacionados para a equipe de design e adicionar recursos de acessibilidade, como alternâncias para os modos de alto contraste e claro. Agora, revise o código e, se necessário, peça ao Copilot para aprimorá-lo.

### Cenário

Os fundamentos de design de software e DevOps continuam os mesmos com a IA generativa. Revise o código gerado e siga o processo DevOps normal. Examine as sugestões do GitHub Copilot para a documentação, os jogos relacionados e a acessibilidade antes de habilitar a revisão da equipe.

## Segurança e o agente de nuvem do GitHub Copilot

Como o agente de nuvem trabalha de forma assíncrona e sem supervisão contínua, existem medidas de segurança:

- O Copilot tem acesso de leitura ao repositório e acesso de escrita **somente** ao branch usado para o código.
- O agente trabalha no GitHub Actions em um ambiente separado e temporário.
- Uma pessoa precisa aprovar os fluxos do GitHub Actions antes da execução.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), inclusive para servidores MCP.

## Revise a documentação gerada

Comece pelo primeiro pull request (PR), que adiciona documentação ao código.

> [!NOTE]
> O PR pode mostrar um aviso de que o Copilot foi bloqueado por um firewall. Isso é esperado: por padrão, o acesso a recursos externos, inclusive servidores MCP externos, é limitado. Se desejar, você pode [personalizar ou desativar o firewall do agente de nuvem][agent-firewall].

1. Acesse seu repositório no github.com e selecione **Pull Requests**.
2. Abra o PR com título semelhante a **Add missing documentation**. Se o Copilot ainda estiver trabalhando, o PR terá a marca **[WIP]**; aguarde a conclusão, que pode levar alguns minutos.
3. Selecione **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

4. Examine os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de arquivo adicionados de acordo com as instruções personalizadas.
5. Volte à guia **Conversation**, role a linha do tempo e procure fluxos aguardando aprovação. Se necessário, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

6. Aguarde os fluxos aparecerem na seção de verificações do PR e as verificações do aplicativo Astro passarem. Isso pode levar alguns minutos.

## Peça alterações ao GitHub Copilot

Você pode mencionar o Copilot em comentários no PR ou em comentários de linha no código. Ele verá o comentário e iniciará outra sessão para atender à solicitação. Por exemplo, peça cabeçalhos nos arquivos de código, comentários TSDoc em arquivos TypeScript e Astro ou um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário ao PR, mencione **@copilot** e solicite uma alteração de documentação.
2. Selecione **View Session** para acompanhar a nova sessão do Copilot.
3. Selecione **Back to pull request** para voltar ao PR.
4. Quando o Copilot concluir, revise o novo commit na guia **Files changed**.

   ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

Você pode iterar até ficar satisfeito. Depois, marque o PR como pronto para revisão e faça o merge.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

1. No github.com, abra a guia **Pull Requests**.
2. Selecione o PR com título semelhante a **Show related games on the game details page** e revise os arquivos alterados.
3. Na guia **Conversation**, role a linha do tempo e procure o indicador de fluxos aguardando aprovação. Se necessário, selecione **Approve and run workflows**; aguarde os fluxos aparecerem na seção de verificações e as verificações do aplicativo Astro passarem.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

4. Opcionalmente, teste manualmente o recurso no Codespace. Substitua `<branch-name>` pelo branch criado pelo Copilot (por exemplo, `copilot/fix-8`):

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Você pode continuar pedindo alterações em comentários da guia **Conversation**, como ajustar quantos jogos relacionados são exibidos ou adicionar cabeçalhos e comentários TSDoc. Observe que a issue foi atribuída antes de atualizar as instruções personalizadas.

## Revise os recursos de acessibilidade

Por fim, revise o PR do agente personalizado de acessibilidade. Ele deve incluir o modo de alto contraste solicitado no Exercício 3 e o modo claro solicitado no Exercício 4.

1. Abra a guia **Pull Requests** e selecione o PR com título semelhante a **Add high contrast mode to website**. Se ainda estiver em andamento, aguarde.
2. Na guia **Files changed**, examine:
   - os controles para alternar entre modos;
   - o uso de armazenamento local para persistir as preferências;
   - as alterações de CSS para os modos de alto contraste e claro;
   - os atributos de acessibilidade, como rótulos ARIA e navegação por teclado;
   - o código JavaScript/TypeScript que alterna os modos.
3. Na guia **Conversation**, aprove os fluxos se necessário e aguarde as verificações.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

4. Opcionalmente, no Codespace, obtenha e selecione o branch criado pelo Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Inicie o aplicativo e teste os modos de alto contraste e claro no navegador. Confirme se as preferências continuam após recarregar a página.

O agente personalizado de acessibilidade orientou a implementação de acordo com práticas recomendadas. Se encontrar algo a melhorar, mencione **@copilot** em um comentário e peça uma atualização.

## Exercício opcional — continue delegando

O agente de nuvem é útil para trabalhar em itens reais do backlog enquanto você se dedica a outras tarefas. Crie issues e atribua-as ao Copilot. Por exemplo:

- Criar um formulário de interesse de apoiadores na página de detalhes de um jogo.
- Implementar paginação na lista de jogos.
- Adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem: examinou as instruções personalizadas; atribuiu issues ao agente e acompanhou sua configuração, planejamento e execução assíncrona; criou e usou um agente de acessibilidade para os modos de alto contraste e claro; monitorou e orientou a sessão na página de agentes; e revisou e aprimorou os PRs do agente, mencionando `@copilot` e aprovando fluxos.

## Revisão e próximas etapas

Se quiser continuar explorando, os outros percursos complementam o que você praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo agente do Copilot Chat e a integração com MCP no IDE.
- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com Plan e Autopilot, skills, um agente de QA, Playwright MCP, Agent Merge e uma ponte opcional `/delegate` para o agente de nuvem.

No seu repositório, experimente atribuir uma issue de refatoração ou cobertura de testes, criar um agente personalizado para outro domínio ou configurar `copilot-setup-steps.yml` para outra pilha.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem][agent-firewall]

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
