---
title: "Lição 0 - Pré-requisitos"
description: "Crie sua própria cópia da Tailspin Toys e prepare um GitHub Codespace para o workshop do Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Antes de começar as lições do Copilot CLI, prepare tudo. Você criará sua própria cópia do repositório Tailspin Toys e iniciará um [codespace][codespaces], cujo terminal integrado será usado para instalar e executar o Copilot CLI na próxima lição.

Nesta lição, você vai:

- criar sua própria cópia do projeto Tailspin Toys a partir do modelo;
- criar um Codespace e confirmar se o projeto está pronto.

## Configurar o repositório do laboratório

Você trabalhará na sua própria cópia do projeto Tailspin Toys. Crie-a agora a partir do [repositório de modelo][tailspin-template]. O novo repositório contém todos os arquivos necessários para o laboratório.

1. Em uma nova janela do navegador, acesse o [modelo da Tailspin Toys][tailspin-template].
2. Crie sua própria cópia do repositório selecionando **Use this template** e, em seguida, **Create a new repository**.
3. Se você estiver fazendo o workshop como parte de um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções dos mentores. Caso contrário, crie o novo repositório em uma organização na qual tenha acesso ao GitHub Copilot.
4. Anote o caminho do repositório que você criou (`organization-or-user-name/repository-name`), pois ele será usado mais adiante no workshop.

> [!NOTE]
> **Seu backlog está pronto**
>
> Quando você cria o repositório a partir do modelo, um backlog de issues do GitHub é criado automaticamente. Você trabalhará com essas issues durante o workshop e não precisará criar nenhuma.

Use uma cópia nova do modelo do workshop. Ela inclui instruções do repositório, código do aplicativo, testes, uma skill `quality-checks` e o backlog que você usará. Se usar uma cópia mais antiga, confirme com a pessoa que conduz o workshop se ela contém os arquivos necessários.

## Criar um Codespace

Em seguida, você usará um Codespace para concluir o workshop.

O [GitHub Codespaces][codespaces] é um ambiente de desenvolvimento na nuvem que permite escrever, executar e depurar código diretamente no navegador. Ele oferece um editor completo com suporte a várias linguagens de programação, extensões e ferramentas.

1. Acesse o repositório que acabou de criar.
2. Selecione **Code**.
3. Selecione a guia **Codespaces** e, em seguida, **Create codespace on main**.
4. Aguarde a conclusão da configuração do Codespace. O modelo instala as dependências do projeto, o Chromium do Playwright e o banco de dados local.
5. Se aparecer a solicitação **Do you trust the authors of the files in this folder?**, selecione **Trust Folder & Continue**.
6. Abra um terminal na raiz do repositório e inicie o aplicativo:

   ```bash
   npm run dev
   ```

7. Quando o Codespaces indicar que a porta `4321` está disponível, selecione **Open in Browser** e confirme se o site Tailspin Toys é carregado.
8. Volte ao terminal e interrompa o servidor de desenvolvimento com <kbd>Ctrl</kbd>+<kbd>C</kbd>.

## Resumo e próximos passos

Tudo pronto! Nesta lição, você:

- criou sua própria cópia do projeto Tailspin Toys a partir do modelo;
- criou um Codespace e confirmou que o projeto estava pronto.

Em seguida, você [instalará o GitHub Copilot CLI][next-lesson] no Codespace e fará a autenticação com sua conta do GitHub.

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um modelo][template-repository]
- [Introdução ao Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
