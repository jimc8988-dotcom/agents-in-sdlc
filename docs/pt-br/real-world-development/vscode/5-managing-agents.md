---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo agente, o GitHub Copilot trabalha de forma autônoma: explora o código, propõe alterações, edita arquivos e executa comandos. Como o trabalho ocorre na sua máquina, o Copilot Chat no VS Code mostra as chamadas de ferramentas e as edições em tempo real. Você pode revisar diffs, aceitar ou rejeitar alterações e orientar a conversa sem sair do editor.

Neste exercício, você vai acompanhar o agente de acessibilidade, revisar e aceitar as edições e orientá-lo durante a sessão para adicionar um controle do modo claro.

## Cenário

O agente personalizado de acessibilidade do [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho continua, a equipe de design também solicita um modo claro para quem considera o tema padrão escuro demais. Oriente a sessão existente para ampliar o trabalho sem perder o contexto.

## Acompanhe o progresso do Copilot

1. Volte à exibição Copilot Chat do Exercício 4.
2. Acompanhe as chamadas de ferramentas, leituras, edições e comandos de terminal.
3. Para cada diff exibido na conversa, selecione **Keep** para aceitar ou **Undo** para reverter.
4. Abra **Files changed** na parte superior do Chat para ver todos os arquivos alterados.

> [!NOTE]
> Se uma ferramenta solicitar permissão, como para executar um comando de terminal, o agente aguardará sua aprovação. Autorize apenas comandos que você considerar seguros.

5. Aguarde a conclusão do modo de alto contraste e leia o resumo do agente.

## Oriente o agente para adicionar o modo claro

Como a conversa continua, o agente mantém o contexto dos componentes, dos estilos e do padrão de armazenamento local que já usou.

1. Envie este prompt complementar no Copilot Chat:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. O agente examinará as alterações existentes e proporá as adições do modo claro.
3. Revise cada diff e use **Keep** ou **Undo**.
4. Se a abordagem não for adequada, interrompa o agente com **Stop** ao lado da entrada do Chat e envie uma instrução complementar mais clara.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar outra. Peça ajustes, como simplificar o CSS usando variáveis, integrar o modo claro ao painel de configurações existente ou adicionar atalhos de teclado aos controles.

## Revise o conjunto de alterações

Antes de confirmar o trabalho:

1. Abra **Source Control** no VS Code e revise todos os arquivos alterados.
2. Examine alguns diffs e confirme se incluem atributos ARIA, navegação por teclado, HTML semântico e persistência com armazenamento local.
3. Quando estiver satisfeito, prepare e confirme as alterações no painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximas etapas

Você acompanhou e orientou uma sessão em andamento no modo agente, revisou cada edição, aceitou ou rejeitou alterações e usou prompts complementares para ampliar o trabalho sem perder o contexto.

Na [próxima lição][next-lesson], publique seu trabalho de acessibilidade em um pull request e revise as alterações produzidas pelo agente local.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
