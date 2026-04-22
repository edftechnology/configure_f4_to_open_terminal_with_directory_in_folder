# Como configurar/instalar/usar o `F4` para abrir o `Terminal Emulator` com o diretório na pasta no `Linux Ubuntu`

## Resumo

Neste documento estão contidos os principais comandos e configurações para configurar/instalar/usar o `F4` para abrir o terminal com o diretório na pasta no `Linux Ubuntu`

## _Abstract_

_This document contains the main commands and settings to configure/install/use `F4` to open the terminal with the directory in the folder in `Linux Ubuntu`._


## Descrição [2]

### `shell`

Um `shell` é uma interface de linha de comando que permite aos usuários interagir com um sistema operacional por meio de comandos de texto. Ele atua como uma camada intermediária entre o usuário e o núcleo do sistema, facilitando a execução de programas, manipulação de arquivos e configuração do sistema. Os _shells_ podem variar em complexidade e funcionalidade, desde simples _shells_ de linha de comando até ambientes de desenvolvimento avançados com recursos de automação e personalização. Os exemplos incluem o `Bash`, `Zsh` e `PowerShell`.

### `thunar`

`Thunar` é um gerenciador de arquivos leve e eficiente projetado para ambientes de desktop baseados no `Xfce`. Ele oferece uma interface intuitiva e fácil de usar, com funcionalidades básicas como navegação por diretórios, visualização de arquivos e operações de gerenciamento, como copiar, mover e excluir. O `Thunar` se destaca pela sua velocidade e simplicidade, tornando-o ideal para usuários que buscam uma experiência de gerenciamento de arquivos sem sobrecarga. Além disso, ele suporta extensões e personalizações, permitindo aos usuários adicionar funcionalidades extras conforme necessário.


## 1. Como configurar/instalar/usar o `F4` para abrir o `Terminal Emulator` com o diretório na pasta no `Linux Ubuntu` [1]

Para configurar/instalar/usar o `F4` para abrir o `Terminal Emulator` com o diretório na pasta no `Linux Ubuntu`, você pode seguir estes passos:

1. Abrir o `Terminal Emulator`. Você pode fazer isso pressionando:

    ```bash
    Ctrl + Alt + T
    ```

2. Certifique-se de que seu sistema esteja limpo e atualizado.

    2.1 Limpar o `cache` do gerenciador de pacotes `apt`. Especificamente, ele remove todos os arquivos de pacotes (`.deb`) baixados pelo `apt` e armazenados em `/var/cache/apt/archives/`. Digite o seguinte comando:
    ```bash
    sudo apt clean
    ```

    2.2 Remover pacotes `.deb` antigos ou duplicados do `cache` local. É útil para liberar espaço, pois remove apenas os pacotes que não podem mais ser baixados (ou seja, versões antigas de pacotes que foram atualizados). Digite o seguinte comando:
    ```bash
    sudo apt autoclean
    ```

    2.3 Remover pacotes que foram automaticamente instalados para satisfazer as dependências de outros pacotes e que não são mais necessários. Digite o seguinte comando:
    ```bash
    sudo apt autoremove -y
    ```

    2.4 Buscar as atualizações disponíveis para os pacotes que estão instalados em seu sistema. Digite o seguinte comando e pressione `Enter`:
    ```bash
    sudo apt update
    ```

    2.5 **Corrigir pacotes quebrados**: Isso atualizará a lista de pacotes disponíveis e tentará corrigir pacotes quebrados ou com dependências ausentes:
    ```bash
    sudo apt --fix-broken install
    ```

    2.6 Limpar o `cache` do gerenciador de pacotes `apt` novamente:
    ```bash
    sudo apt clean
    ```

    2.7 Para ver a lista de pacotes a serem atualizados, digite o seguinte comando e pressione `Enter`:
    ```bash
    sudo apt list --upgradable
    ```

    2.8 Realmente atualizar os pacotes instalados para as suas versões mais recentes, com base na última vez que você executou `sudo apt update`. Digite o seguinte comando e pressione `Enter`:
    ```bash
    sudo apt full-upgrade -y
    ```

### 1.1 Configurar/Instalar/Usar o `F4` para abrir o `Terminal Emulator` com o diretório na pasta no `Linux Ubuntu`

Para conseguir o comportamento desejado, o mais seguro é alinhar a configuração local do `Thunar` com o padrão do `Xubuntu`. Em muitos sistemas, esse comportamento já existe em `/etc/xdg/xdg-xubuntu/Thunar/uca.xml` e `/etc/xdg/xdg-xubuntu/Thunar/accels.scm`; quando necessário, a personalização do usuário fica em `~/.config/Thunar/`.

1. Abra o `Thunar` e acesse `"Edit" > "Configure custom actions..."`.

2. Verifique se já existe uma ação `Open Terminal Here`. Se não existir, crie uma nova.

3. Para cobrir os dois cenários comuns, configure **duas ações**:

    3.1 **Ação para diretórios selecionados**

    **Nome:** `Open Terminal Here`

    **Comando:** `exo-open --working-directory %f --launch TerminalEmulator`

    **Condições de Aparência:** marque `Diretórios`

    3.2 **Ação para arquivos selecionados**

    **Nome:** `Open Terminal Here`

    **Comando:** `exo-open --working-directory %d --launch TerminalEmulator`

    **Condições de Aparência:** marque `Outros arquivos`, `Arquivos de texto`, `Arquivos de imagem`, `Arquivos de áudio` e `Arquivos de vídeo`, conforme necessário

4. Atribua `F4` à ação nas versões do `Thunar` que permitem configurar o atalho diretamente na janela de Ações Personalizadas.

5. Só verifique `Settings` > `Keyboard` > `Application Shortcuts` se houver indício real de conflito com um atalho global do `XFCE`. O foco principal do ajuste deve estar nas ações do `Thunar`, e não no atalho global.

6. Salve as ações personalizadas.

7. Feche a janela de configuração e **teste a nova ação personalizada** no `Thunar`:

    - Se uma pasta estiver selecionada, o `F4` deve usar a ação com `%f`.
    - Se um arquivo estiver selecionado, o `F4` deve usar a ação com `%d`.

Se o atalho `F4` ainda não estiver funcionando como esperado, revise os arquivos `~/.config/Thunar/uca.xml` e `~/.config/Thunar/accels.scm`, reinicie o `Thunar` com `thunar -q` e teste novamente. Em último caso, verifique se existe conflito com algum atalho global do `XFCE`.

## 2. Código completo para configurar/instalar/usar

Para configurar/instalar/usar o `F4` para abrir o terminal com o diretório na pasta no `Linux Ubuntu`, você pode seguir estas etapas. O comando abaixo restaura a configuração estável do `Thunar`, remove uma ação antiga/frágil chamada `terminal-f4-unified`, mapeia o `F4` para as duas ações corretas e define o `xfce4-terminal` como `TerminalEmulator` padrão.

1. Abrir o `Terminal Emulator`. Você pode fazer isso pressionando:

    ```bash
    Ctrl + Alt + T
    ```

2. Digite o seguinte comando e pressione `Enter`:

    ```bash
    mkdir -p "$HOME/.config/Thunar" "$HOME/.config/xfce4"

    thunar -q 2>/dev/null || true

    cat > "$HOME/.config/Thunar/uca.xml" <<'EOF'
    <?xml version="1.0" encoding="UTF-8"?>
    <actions>
    <action>
        <icon>Terminal</icon>
        <name>Open Terminal Here</name>
        <unique-id>1-1</unique-id>
        <command>exo-open --working-directory %f --launch TerminalEmulator</command>
        <description>Open terminal here</description>
        <patterns>*</patterns>
        <startup-notify/>
        <directories/>
    </action>
    <action>
        <icon>Terminal</icon>
        <name>Open Terminal Here</name>
        <unique-id>2-2</unique-id>
        <command>exo-open --working-directory %d --launch TerminalEmulator</command>
        <description>Open terminal in containing directory</description>
        <patterns>*</patterns>
        <audio-files/>
        <image-files/>
        <other-files/>
        <text-files/>
        <video-files/>
    </action>
    </actions>
    EOF

    touch "$HOME/.config/Thunar/accels.scm"
    sed -i '/terminal-f4-unified/d;/uca-action-1-1/d;/uca-action-2-2/d' "$HOME/.config/Thunar/accels.scm"
    {
        printf '%s\n' '(gtk_accel_path "<Actions>/ThunarActions/uca-action-1-1" "F4")'
        printf '%s\n' '(gtk_accel_path "<Actions>/ThunarActions/uca-action-2-2" "F4")'
    } >> "$HOME/.config/Thunar/accels.scm"

    touch "$HOME/.config/xfce4/helpers.rc"
    sed -i '/^TerminalEmulator=/d' "$HOME/.config/xfce4/helpers.rc"
    printf '%s\n' 'TerminalEmulator=xfce4-terminal' >> "$HOME/.config/xfce4/helpers.rc"

    thunar -q 2>/dev/null || true
    ```


## Referências

[3] OPENAI. ***Definindo atalho f4 terminal***. Disponível em: <https://chatgpt.com/c/69b06d38-7344-8326-ae4b-4523dc1b0afe> (texto adaptado). Acessado em: 21/02/2024 13:47.

[2] OPENAI. ***Vs code: editor popular***. Disponível em: <https://chat.openai.com/c/b640a25d-f8e3-4922-8a3b-ed74a2657e42> (texto adaptado). Acessado em: 21/02/2024 13:48.
