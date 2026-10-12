# PhotoCraft Live Controls for OpenDeck

[Português do Brasil](#português-do-brasil) · [Contributing](CONTRIBUTING.md) · [PhotoCraft](https://github.com/storytold/photocraft) · [OpenDeck](https://github.com/nekename/OpenDeck)

An independent Linux-only [OpenDeck](https://github.com/nekename/OpenDeck) plugin that gives a Stream Deck live control over the open [PhotoCraft](https://github.com/storytold/photocraft) document. Build masks and adjustment layers, switch to Crop or Brush, save, navigate selections, and use encoders for brush size, hardness, zoom, selected-layer opacity, and non-destructive develop adjustments.

**PhotoCraft Edit Control** talks to the desktop app through its authenticated loopback control channel. The retained **Run PhotoCraft CLI** action is for advanced or version-specific automation: it stores an executable path and JSON argument array, then launches the executable directly without `sh -c` or another shell.

## Features

- A dedicated **PhotoCraft Edit Control** action for the document open in the desktop app, using PhotoCraft's authenticated loopback control channel.
- Key actions for undo/redo/save, Crop and Brush tools, reveal-all layer masks, Exposure and Vibrance adjustment layers, selection commands, and useful view presets.
- Encoder modes for brush size, brush hardness, canvas zoom, selected-layer opacity, and selected Exposure, Brightness/Contrast, or Vibrance adjustment layers; encoder press is configured separately for fast fit, undo, crop, or mask workflows.
- The action stores only a local token-file path, never the authentication secret itself.
- One configurable **Run PhotoCraft CLI** action for keypad keys and encoders.
- Supports every PhotoCraft CLI subcommand, option, and future addition: the action forwards the exact JSON argument array you configure.
- Loads the installed CLI's command or tool catalog in the Property Inspector and creates an app-specific, editable action template.
- Configurable executable path, working directory, timeout (1–300 seconds), label, and optional output summary on the key.
- Test a configuration in the Property Inspector before assigning it to a key.
- Linux x86_64 release ZIP created on every CI run; tagged versions are published as GitHub Releases.

## Requirements

- Linux x86_64
- OpenDeck with OpenAction-compatible plugin support
- PhotoCraft and `photocraft-cli` installed and available in `PATH`, or its absolute path entered in the Property Inspector

For **PhotoCraft Edit Control**, start PhotoCraft with a loopback control port and a private token file, then enter that file's path in the action inspector:

```bash
photocraft --control 7878 --control-token-file /private/path/photocraft-control.token
```

PhotoCraft creates the token file when it does not exist. The OpenDeck action accepts only `localhost`, `127.0.0.1`, or `::1` and reads the token only when sending a control request.

## Install

1. Download `getcraft-photocraft-opendeck-linux-x86_64.zip` from the [latest release](../../releases/latest).
2. In OpenDeck, open **Plugins** → **Install from file** and select the ZIP. Do not extract or rename the `.sdPlugin` directory.
3. Start PhotoCraft with the control command above, then drag **PhotoCraft Editing Controls → PhotoCraft Edit Control** to a key or encoder.
4. Enter the token-file path and choose key, encoder-turn, and encoder-press workflows in the Property Inspector.
5. Keep **PhotoCraft CLI → Run PhotoCraft CLI** for an arbitrary CLI operation; enter its path and JSON argument array, then use **Test command**.

## Configuration and examples

The **Arguments** field must contain a JSON array of strings. It is not a shell command.

| Goal | Arguments |
| --- | --- |
| List the installed command catalog | `["commands", "--json"]` |

Use the exact argument syntax shown by `photocraft-cli --help` for project, file, export, and live-app control operations. The generic array accepts every documented subcommand and option.

The Property Inspector's **Load command catalog** button asks the installed CLI for the catalog supported by this application. Selecting an ID fills an app-specific, editable argument template; use **Test command** after adding the parameters and options required by your installed CLI version. The generic JSON array remains the canonical path to every CLI capability.

## CI/CD and release

Pull requests and pushes to `main` run formatting checks, unit tests, and `scripts/package.sh`. The workflow uploads the Linux installer ZIP as an artifact. Pushing a semantic tag such as `v0.1.0` publishes the same ZIP as a GitHub Release.

To package locally:

```bash
rustup target add x86_64-unknown-linux-gnu
bash scripts/package.sh
```

The output is `dist/getcraft-photocraft-opendeck-linux-x86_64.zip`, containing exactly `wt.luizgustavo.getcraft.photocraft.sdPlugin/` at its root.

## Local test installation

On Linux, build the local installation package with:

```bash
bash scripts/install-local.sh
```

By default, the installer only builds the ZIP and does not change OpenDeck's active plugin directory. This prevents development plugins from disrupting an active setup. To deliberately test one plugin in the active OpenDeck installation, opt in explicitly:

```bash
OPENDECK_ALLOW_LIVE_INSTALL=1 bash scripts/install-local.sh
```

Install one development plugin at a time, then use OpenDeck's plugin reload control or restart it manually. The installer itself never restarts or reloads OpenDeck. To target a separate configuration, set `OPENDECK_PLUGINS_DIR` before running the command.

## Platform roadmap

| Platform | Status |
| --- | --- |
| Linux x86_64 | Supported now |
| Windows | Planned; not implemented or packaged yet |
| macOS | Planned; not implemented or packaged yet |

## Security and governance

The plugin never evaluates a shell command. It executes the chosen binary with the strings in the JSON array, and it limits a process to five minutes.

This is an open-contribution repository. Direct write/merge access is reserved for `LuizGustavoWT`; outside contributors use forks and pull requests. GitHub does not have a separate permission that only controls closing issues: users without repository write/triage access cannot close other users' issues, so retaining those permissions only for the maintainer satisfies the governance model. See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT. PhotoCraft and OpenDeck are independent upstream projects with their own licenses and release policies.

---

# Português do Brasil

Plugin independente, somente para Linux, do [OpenDeck](https://github.com/nekename/OpenDeck) que dá ao Stream Deck controle ao vivo sobre o documento aberto no [PhotoCraft](https://github.com/storytold/photocraft). Crie máscaras e camadas de ajuste, selecione Crop ou Brush, salve, navegue pelas seleções e use encoders para tamanho e dureza do pincel, zoom, opacidade da camada selecionada e ajustes não destrutivos de revelação.

A **PhotoCraft Edit Control** conversa com o aplicativo desktop pelo canal local autenticado. A ação **Run PhotoCraft CLI**, mantida para automações avançadas ou específicas de versão, guarda o executável e um vetor JSON de argumentos e o chama diretamente, sem `sh -c` nem outro shell.

## Recursos

- Ação dedicada **PhotoCraft Edit Control** para o documento que está aberto no aplicativo, usando o canal local autenticado do PhotoCraft.
- Ações de tecla para desfazer/refazer/salvar, ferramentas Crop e Brush, máscaras de camada revelar tudo, camadas de ajuste Exposure e Vibrance, comandos de seleção e atalhos úteis de visualização.
- Modos de encoder para tamanho e dureza do pincel, zoom da tela, opacidade da camada selecionada e os valores das camadas de ajuste Exposure, Brightness/Contrast e Vibrance selecionadas.
- O encoder pode ter uma ação de pressão independente para ajustar rapidamente a tela, desfazer, usar Crop ou criar uma máscara; o segredo de autenticação fica no arquivo de token local, nunca no perfil do Stream Deck.
- Ação configurável **Run PhotoCraft CLI** para teclas e encoders.
- Todos os subcomandos, opções e futuras adições da CLI: a ação repassa exatamente o vetor JSON configurado.
- Catálogo dinâmico de comandos ou ferramentas da CLI no Property Inspector, com modelo de ação editável e específico do aplicativo.
- Caminho do executável, diretório de trabalho, timeout (1–300 s), rótulo e resumo de saída configuráveis.
- Botão de teste no Property Inspector antes de associar a ação a uma tecla.
- ZIP Linux x86_64 gerado em toda execução da CI; tags são publicadas como Releases.

## Requisitos e instalação

Você precisa de Linux x86_64, OpenDeck compatível com OpenAction e PhotoCraft com `photocraft-cli` no `PATH` (ou o caminho absoluto no Property Inspector).

Para usar a **PhotoCraft Edit Control**, inicie o aplicativo com uma porta local e um arquivo de token privado; informe o caminho do arquivo no Property Inspector:

```bash
photocraft --control 7878 --control-token-file /private/path/photocraft-control.token
```

O PhotoCraft cria o arquivo de token se ele não existir. A ação aceita somente `localhost`, `127.0.0.1` ou `::1` e lê o token apenas ao enviar a solicitação de controle.

1. Baixe `getcraft-photocraft-opendeck-linux-x86_64.zip` na [release mais recente](../../releases/latest).
2. No OpenDeck, abra **Plugins** → **Install from file** e escolha o ZIP. Não extraia nem renomeie a pasta `.sdPlugin`.
3. Arraste **PhotoCraft Editing Controls → PhotoCraft Edit Control** para uma tecla ou encoder.
4. Informe o caminho do arquivo de token e escolha os fluxos da tecla, do giro e da pressão do encoder no Property Inspector.
5. Use **PhotoCraft CLI → Run PhotoCraft CLI** para uma operação CLI arbitrária; configure o caminho e o vetor JSON, depois use **Test command**.

## Configuração

O campo **Arguments** precisa ser um vetor JSON de strings; ele não é um comando de shell.

| Objetivo | Arguments |
| --- | --- |
| Listar o catálogo de comandos instalado | `["commands", "--json"]` |

Use a sintaxe exata de `photocraft-cli --help` para operações de projeto, arquivo, exportação e controle do aplicativo em execução. O vetor genérico aceita todos os subcomandos e opções documentados.

**Load command catalog** consulta a CLI instalada pelo catálogo compatível com este aplicativo. Ao escolher um ID, o inspetor preenche um modelo de argumentos específico e editável; use **Test command** depois de adicionar os parâmetros e opções exigidos pela versão instalada da CLI. O vetor JSON genérico continua sendo o caminho para toda a CLI.

## CI/CD, roadmap e contribuição

PRs e pushes em `main` executam formatação, testes e `scripts/package.sh`. O ZIP vira artefato; uma tag como `v0.1.0` cria uma GitHub Release.

Para gerar o pacote local no Linux, execute `bash scripts/install-local.sh`. Por padrão, o instalador gera o ZIP e não altera o diretório de plugins ativo do OpenDeck, protegendo a configuração em uso. Para testar deliberadamente um único plugin na instalação ativa, confirme explicitamente:

```bash
OPENDECK_ALLOW_LIVE_INSTALL=1 bash scripts/install-local.sh
```

Instale um plugin de desenvolvimento por vez e então recarregue-o pelo OpenDeck ou reinicie manualmente. O instalador jamais reinicia ou recarrega o OpenDeck. Para usar outra configuração, defina `OPENDECK_PLUGINS_DIR` antes do comando.

| Plataforma | Situação |
| --- | --- |
| Linux x86_64 | Suportado agora |
| Windows | Planejado; ainda não implementado nem empacotado |
| macOS | Planejado; ainda não implementado nem empacotado |

Contribuições são bem-vindas via fork e pull request. Apenas `LuizGustavoWT` possui acesso direto para merge e para fechar issues de terceiros. O GitHub não oferece uma permissão separada somente para fechar issues; sem acesso de escrita/triagem, colaboradores públicos não conseguem fechar issues de outras pessoas. Veja [CONTRIBUTING.md](CONTRIBUTING.md).

## Licença

MIT. PhotoCraft e OpenDeck são projetos upstream independentes, cada um com sua própria licença.
