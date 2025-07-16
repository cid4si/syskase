# Prompt de Comando do Windows (cmd.exe)

## Iniciando e Encerrando o cmd.exe

### Formas de Iniciar o cmd.exe

Uma forma de iniciar o cmd.exe é pressionar **Win+R** e então digitar as três letras `cmd`, seguido da tecla **ENTER**.

No Windows 10, também pode ser iniciado pressionando **Win+X** seguido de **C**. (Porém, isso pode ser alterado em Configurações → Barra de Tarefas: a opção "Substituir o Prompt de Comando pelo Windows PowerShell" precisa estar desativada).

Quando o cmd.exe é iniciado sem a opção `/D`, ele primeiro executa os comandos que estão listados no registro sob `HKEY_CURRENT_USER\Software\Microsoft\Command Processor` e `HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor` (valor do Autorun).

Algumas configurações do cmd.exe, como seu tamanho ou cores, são armazenadas no registro sob `HKEY_CURRENT_USER\Console\%SystemRoot%_system32_cmd.exe`.

### Opções de Linha de Comando

| Opção | Descrição |
|-------|-----------|
| `/c` ou `/k command` | Inicia um novo processo cmd.exe para executar o comando especificado e então sair (`/c`) ou manter (`/k`) o processo |
| `/s` | Altera o tratamento de aspas no cmd após `/c` ou `/k` |
| `/q` | Desativa o echo |
| `/d` | Desabilita comandos autorun (encontrados sob `HKEY_CURRENT_USER\Software\Microsoft\Command Processor`) |
| `/a` - `/u` | Saída é ANSI - Unicode |
| `/t:bf` | Define cor de fundo (b) e primeiro plano (f) (por exemplo `/t:4f` é branco sobre vermelho) |
| `/f:on`, `/f:off` | Habilita ou desabilita o preenchimento automático de nomes de arquivos e diretórios (tab-completion) |
| `/e:on`, `/e:off` | Habilita ou desabilita extensões de comando |
| `/v:on`, `/v:off` | Habilita ou desabilita expansão tardia de variáveis de ambiente |

### %CmdCmdLine%

A nova sessão iniciada do cmd.exe armazena como ela foi iniciada na variável `%CmdCmdLine%`.

### Encerrando o cmd.exe

O interpretador de comandos pode ser terminado com o comando `exit`.

## Obtendo Ajuda Básica

Geralmente, um comando pode ser executado com `/?` para imprimir um resumo curto do propósito e opções de um comando (por exemplo `dir /?`).

Alguma ajuda básica sobre alguns comandos internos (como `assoc`) e alguns executáveis (como `findstr.exe`) pode ser obtida com o utilitário `help.exe`.

```cmd
C:\> help
C:\> help assoc
C:\> help findstr
```

## Executando Programas e Comandos Internos

Os dois usos principais do cmd.exe são iniciar programas (executáveis) ou executar comandos internos (built-in).

A diferença entre executáveis e comandos internos é que os executáveis também podem ser iniciados sem o cmd.exe, enquanto os comandos internos requerem um cmd.exe em execução para interpretá-los e executá-los.

Se o cmd.exe é executado interativamente, ele permite que o usuário digite uma linha de texto. Assim que o usuário pressiona enter, o cmd.exe tenta executar o que o usuário digitou. A primeira palavra da linha digitada é o nome do executável ou comando a ser executado, as palavras restantes são consideradas parâmetros para o comando.

O comando ou executável pode produzir texto (que escreve para stdout). Se este for o caso, o cmd.exe lê stdout e o imprime de volta no console.

Por exemplo, `echo` é um comando interno que simplesmente imprime os parâmetros passados:

```cmd
C:\> echo Hello world
```

`ping.exe`, por outro lado, é um executável. Como comandos internos, executáveis são identificados por sua primeira palavra. Na seguinte execução do ping, a palavra `localhost` é passada como parâmetro para o ping:

```cmd
C:\> ping localhost
Pinging pc3.tq84 [::1] with 32 bytes of data:
Reply from ::1: time<1ms
…
```

**Nota:** o nome do executável é `PING.EXE`, não `ping`. Então, por que o cmd.exe ainda é capaz de encontrar o executável "correto"?

**Resposta:** é possível porque o cmd.exe consulta os valores da variável de ambiente `PATHEXT`. Ela consiste em uma lista de sufixos, separados por ponto e vírgula, que são adicionados ao nome (suposto) do executável se o cmd.exe não conseguir localizar o executável pelo nome dado inicialmente.

Outro sufixo que está presente no `PATHEXT` por padrão é `.msc`. Assim, o gerenciador de dispositivos (cujo nome completo é `devmgmt.msc`) pode ser aberto assim:

```cmd
C:\> devmgmt
```

Neste ponto, deve ser notado que o cmd.exe é capaz de localizar os diretórios onde estão os arquivos `.exe` ou `.msc` através do valor da variável de ambiente `PATH`. Este valor consiste em um conjunto de diretórios, separados por ponto e vírgula. Quando o cmd.exe tenta localizar um executável, ele tenta procurá-los nestes diretórios especificados.

Uma forma conveniente de determinar o que o cmd.exe executaria é o `where.exe` (que é um executável). Por exemplo, se chamado com o argumento `services`, ele (tipicamente) responde com dois arquivos:

```cmd
C:\> where services
C:\Windows\System32\services.exe
C:\Windows\System32\services.msc
```

Isso ocorre porque `PATHEXT` contém ambos os sufixos, `.exe` e `.msc`, e relata ambos. No entanto, se executado na linha de comando, apenas o primeiro que for encontrado será realmente executado.

Se a primeira palavra do texto digitado não for nem um comando interno nem um executável, o cmd.exe responde assim:

```cmd
C:\> doesNotExist foo bar
'doesNotExist' is not recognized as an internal or external command,
operable program or batch file.
```

## Alterando a Aparência do cmd.exe

A cor de primeiro plano e fundo de uma sessão cmd.exe pode ser definida com `color`.

O número de colunas e linhas da janela cmd.exe pode ser ajustado com `mode.com`:

```cmd
C:\> mode con cols=180 lines=41
```

O texto mostrado pode ser limpo com `cls`.

## Configurando o cmd.exe

Configurando o cmd.exe no registro.

Algumas configurações são armazenadas em `%APPDATA%\Microsoft\Windows\Start Menu\Programs\System Tools\Command Prompt.lnk`.

## Variáveis de Ambiente

Algumas variáveis de ambiente controlam o comportamento de alguns comandos ou estão relacionadas ao cmd.exe:

- **CD** - avalia para o diretório de trabalho atual
- **ComSpec** - armazena o caminho completo (diretório mais nome do arquivo, por exemplo `C:\Windows\system32\cmd.exe`)
- **COPYCMD** - para copy
- **DIRCMD** - para dir
- **PROMPT** - especifica como o prompt é formatado

## Histórico

**doskey**: um utilitário básico de histórico.

**F7** abre uma janela que mostra os comandos digitados mais recentemente.

## Windows 10

Com o Windows 10, é possível usar **ctrl-c** e **ctrl-v**.

Texto pode ser pesquisado com **ctrl-f**. Se algo foi encontrado, a região selecionada pode ser estendida pressionando a tecla shift e navegando com as setas.

Note que **ctrl-f** também é o padrão para preenchimento de nomes de arquivos (se habilitado).

A variável de ambiente `%PROMPT%` agora tem um `$e` para inserir um caractere de escape, assim o prompt pode ser colorizado com sequências de escape ANSI.

## Atalhos de Teclado

### Selecionando Texto por Linha

Texto pode ser selecionado linha por linha com **shift + up/down**.

### Modo Mark

Com o atalho de teclado **ctrl+m**, o modo mark pode ser ativado.

No modo mark, é possível navegar pelo texto com as setas e selecionar partes do texto com a tecla shift e então copiar/colar.

No modo mark, infelizmente, **ctrl+shift+right** não pula para o final de uma palavra.

Quando uma seleção é iniciada pressionando **ctrl+shift+alt**, a seleção está em modo de quebra de linha.

Se a seleção é iniciada pressionando **ctrl+shift**, a seleção está em modo de bloco.

## Iniciando Utilitários do Sistema

### Iniciando o Console de Gerenciamento

```cmd
C:\> mmc
```

### Iniciando o Gerenciador de Dispositivos

```cmd
C:\> devmgmt.msc
C:\> mmc devmgmt.msc
```

### Abrindo as Configurações de Controle de Conta de Usuário

```cmd
C:\> UserAccountControlSettings
```

## Arquivos .msc Importantes

| Arquivo | Descrição |
|---------|-----------|
| `azman.msc` | Authorization manager |
| `certlm.msc` | Certificates (local computer) |
| `certmgr.msc` | Certificates (current user) |
| `comexp.msc` | Component services |
| `compmgmt.msc` | Computer management |
| `devmgmt.msc` | Device manager |
| `DevModeRunAsUserConfig.msc` | Start menu and taskbar |
| `diskmgmt.msc` | Disk management (partição, caminhos de drive) |
| `eventvwr.msc` | Event viewer |
| `fsmgmt.msc` | Shared folders |
| `gpedit.msc` | Local group policy editor |
| `lusrmgr.msc` | Local users and groups |
| `perfmon.msc` | Performance monitor |
| `printmanagement.msc` | Gerenciamento de impressão |
| `rsop.msc` | Resultant set of policy |
| `secpol.msc` | Permite configurar o prompt de elevação (Local Security Policy) |
| `services.msc` | Services |
| `taskschd.msc` | Task scheduler, compare com schtasks.exe |
| `tpm.msc` | Trusted platform module (TPM) |
| `WF.msc` | Windows defender firewall |
| `WmiMgmt.msc` | Windows management instrumentation (WMI) |

## Informações Adicionais

O valor padrão para a chave do registro `HKEY_CLASSES_ROOT\Directory\shell\cmd\command` parece ser `cmd.exe /s /k pushd "%V"`.

O programa de console real por trás do cmd.exe é o `conhost.exe`.

O `rundll32` permite executar funções do cmd.exe.

O caminho completo do cmd.exe (tipicamente `C:\Windows\system32\cmd.exe`) é armazenado na variável de ambiente `ComSpec`.

**Nota:** O Windows 10 S não possui cmd.exe.