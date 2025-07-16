# PowerShell

## Introdução

O `powershell.exe` é basicamente um interpretador para o assembly .NET `System.Management.Automation`. Este assembly é carregado pelo `powershell.exe`.

## Variáveis de Ambiente (Environment Variables)

O PowerShell acessa os valores das variáveis de ambiente prefixando o nome com `$env:` (tecnicamente: `$env` é um PowerShell drive).

```powershell
PS> echo $env:userprofile
```

Os valores das variáveis de ambiente armazenadas no registro sob `HKEY_CURRENT_USER\Environment` e `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment` podem ser lidos e escritos da seguinte forma:

```powershell
PS C:\> echo [environment]::getEnvironmentVariable("PATH", "user")
PS C:\> echo [environment]::getEnvironmentVariable("PATH", "machine")
PS C:\> [environment]::setEnvironmentVariable("A_VAR", "a value", "user")
PS C:\> [environment]::setEnvironmentVariable("ANOTHER_VAR", "another value", "machine")
```

É importante notar que com esta construção, não é necessário enviar um `WM_SETTINGCHANGE` após alterar ou criar uma variável, porque o PowerShell é inteligente o suficiente para fazer isso sem ser solicitado.

## Tilde (~)

Aparentemente, o PowerShell, no espírito de um shell Unix, expande o tilde (~) para o diretório `%UserProfile%`:

```powershell
PS C:\> dir ~
```

Porém, parece que ele "tropeça" se uma variável contém um tilde...

### Passando um Tilde para um Executável

Quando o tilde é passado como argumento para um executável, o comportamento é diferente no PowerShell 5 e PowerShell 7:

- **PowerShell 5**: O tilde é passado sem modificação para o executável
- **PowerShell 7**: O tilde é expandido para o diretório home do usuário. Isso também é verdadeiro se o tilde for seguido por uma barra normal ou barra invertida

Contudo, em construções como `~otherUser` ou `foo~bar`, nenhuma expansão ocorre.

## Mostrar Versão

A versão da instalação do PowerShell é revelada na variável automática `$psVersionTable`.

## Opções de Linha de Comando (Command Line Options)

| Opção | Abreviação | Descrição |
|-------|------------|-----------|
| `-file` | `-f` | Especifica um script para ser executado pelo interpretador PowerShell. O valor pode ser `-` que lê o "script" do stdin. Possivelmente para ser combinado com `-noExit`. |
| `-command` | `-c` | Especifica um comando para ser executado, depois sai (a menos que `-noExit` seja especificado). O valor pode ser uma string, um script block ou `-`. |
| `-configurationName` | `-config` | Especifica o endpoint de configuração no qual o PowerShell é executado. |
| `-customPipeName` | | Nome para um servidor IPC adicional (named pipe), usado para debugging ou outra comunicação entre processos. |
| `-encodedCommand` | `-e`, `-ec` | Comando é codificado em Base64. |
| `-executionPolicy` | `-ex`, `-ep` | Define a política de execução (que é armazenada na variável de ambiente `$env:psExecutionPolicyPreference`). |
| `-inputFormat` | `-inp`, `-if` | Descrição do formato de dados que é enviado ao PowerShell. Valores possíveis: `text` ou `xml` (que é formato CLIXML serializado). |
| `-interactive` | `-i` | Abre uma sessão interativa do PowerShell. (Veja também `-nonInteractive`) |
| `-login` | `-l` | Inicia o PowerShell como um login shell usando `/bin/sh`. Útil apenas no Linux e macOS. Deve ser o primeiro parâmetro quando usado. |
| `-MTA`, `-STA` | | Inicia o shell usando apartment multi-threaded/single-threaded. Apenas Windows. Padrão é `-STA` se não especificado. |
| `-noExit` | `-noe` | Não sai após executar comandos de inicialização. |
| `-noLogo` | `-nol` | Não mostra o banner de copyright na inicialização. |
| `-nonInteractive` | `-noni` | Prompt não interativo. |
| `-noProfile` | `-nop` | Não carrega o perfil do PowerShell (veja a variável automática `$profile`). |
| `-outputFormat` | `-o`, `-of` | Especifica o formato de saída. Valores possíveis são `text` e `XML` (formato CLIXML serializado). |
| `-settingsFile` | `-settings` | Sobrescreve o arquivo `powershell.config.json` de sistema (que, por padrão, é armazenado no diretório apontado por `$psHome`). |
| `-version` | `-v` | Mostra a versão usada do PowerShell. Veja também a variável automática `$psVersionTable`. |
| `-windowStyle` | `-w` | Estilo da janela para a sessão. Valores possíveis são `normal`, `minimized`, `maximized` e `hidden`. |
| `-workingDirectory` | `-wd` | Define o diretório de trabalho inicial executando `set-location -literalPath ...`. (Este parâmetro parece estar disponível apenas no PowerShell Core) |
| `-help` | `-?`, `/?` | Mostra ajuda |

## Opção Padrão (Default Option)

Se o PowerShell for invocado sem opções (mas valores estão presentes na linha de comando), as opções padrão para PowerShell 5.1 (`powershell.exe`) é `-file` e `-command` para PowerShell 7 (`pwsh.exe`).

Esta mudança foi necessária quando o PowerShell foi portado para ambientes Unix-like para suportar linhas shebang.

## Usando -command (ou -c) para Executar Comandos

`-c statement(s)` cria um novo processo PowerShell no qual uma ou mais declarações PowerShell são executadas.

O valor para `-c` pode ser:
- uma string, ou
- um script block, ou
- `-` (os comandos são lidos do stdin)

```powershell
powershell -c 'write-host going to loop; foreach ($i in 1..10) { $i }; write-host loop is finished'
```

### -noExit

Quando usando `-c`, a nova sessão PowerShell é terminada quando termina de avaliar as declarações. Esta terminação pode ser prevenida usando o parâmetro `-noExit` ao invés do parâmetro `-c`.

## Especificando Valores Booleanos

Opções de switch podem receber um valor booleano com a seguinte sintaxe:

```powershell
pwsh … {-all:$false}
```

## Executável 32-bit vs 64-bit

Em uma instalação Windows 64-bit, o executável 32-bit do PowerShell está localizado em `C:\Windows\SysWOW64\WindowsPowerShell\v1.0\powershell.exe` enquanto o executável 64-bit é encontrado em `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`.

Em uma sessão PowerShell em execução, a arquitetura do host PowerShell pode ser determinada com:

```powershell
PS C:\> $([System.Runtime.InterOpServices.Marshal]::SizeOf([System.IntPtr]::Zero)*8)
```

## Recursos Interessantes

### Criar um Arquivo

A seguinte linha cria um arquivo de texto cuja primeira (e única) linha lê "Hello world.":

```powershell
PS C:\> ${c:\users\Rene\tq84.txt}="Hello World"
```

### Constantes Administrativas

```powershell
1kb
2mb
3gb
4tb
5pb
```

As constantes administrativas não são consistentes com as recomendações IEC (kibibyte etc.), `1kb` avalia para 1024, `1mb` para 1048576 etc.

## Iniciando o PowerShell

O PowerShell pode ser iniciado:
- Via Power User Menu: windows+x -> i
- Do cmd.exe simplesmente digitando `powershell`

## PowerShell Core e PowerShell 7

PowerShell Core parece ser PowerShell 6.x.

PowerShell Core não pretende substituir ou fazer upgrade do PowerShell "regular", ao invés disso, tenta resolver diferentes tarefas.

PowerShell 7.x removeu o "Core" do seu nome (mas ainda é exibido em `psVersionTable.psEdition`).

A classe `System.Management.Automation.ClrFacade` contém todo o código divergente para "FullCLR" e "CoreCLR".

PowerShell 7.x pode ser instalado com `winget.exe`:

```cmd
C:\> winget search PowerShell
C:\> winget install --id Microsoft.PowerShell --source winget
```

## Miscellaneous

O ampersand (&) é o operador de chamada (call operator).

## Continuação de Linha

Um comando pode ser espalhado por múltiplas linhas terminando uma linha com um espaço seguido por um acento grave (backtick):

```powershell
write-host `
foo `
bar `
baz
```

Compare com o caret no cmd.exe.

## Tipos de Comando

Os tipos de comando são:
- alias
- application
- cmdlet
- filter
- function
- script

Estes correspondem aos valores possíveis para o parâmetro `-commandType` do cmdlet `get-command`.

## O Tilde em Nomes de Arquivo Curtos

O Windows tem um conceito de nomes de arquivo curtos (DOS) onde um nome de arquivo arbitrariamente longo é encurtado para oito caracteres (sem sufixo). Ele usa um tilde para isso. Infelizmente, no PowerShell, o tilde tem um significado especial (diretório home). Então, se uma variável de ambiente contém um tilde, ele é erroneamente reconhecido no PowerShell.

## Docker Image

```bash
$ docker pull mcr.microsoft.com/powershell
$ docker run -it --name pwsh mcr.microsoft.com/powershell:latest
```

## TODO Items

- O que é a significância da variável de ambiente `__PSLockdownPolicy` e sua relação com `wldp.dll`
- `%APPDATA%\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt`
- O equivalente de `use strict` parece ser `set-strictMode -version 2`
- `help about_common*`, `-confirm`, `-whatIf`
- Arquivos `.psc` são arquivos de console PowerShell. Eles especificam um ou mais PSSnapIns para carregar na memória na inicialização
- `[object]`, `[psobject]`, `[pscustomobject]`
- As funções `prompt` e `tabCompletion`. BTW: `tabCompletion` é substituído por `psReadLine`?

## Veja Também

- Examples
- Pipelines
- Quando um processo PowerShell é iniciado, ele spawna `conhost.exe` como processo filho (que é o console-host e controla a aparência e funcionalidade do PowerShell)
- "Script cannot be loaded because running scripts is disabled on this system"
- Unix and cmd.exe like default aliases
- PowerShell: language
- Configurando as opções do MS-Word com PowerShell
- A classe .NET `System.Management.Automation.PowerShell`
- Windows 10 S não tem PowerShell
- PowerShell integra com Antimalware Scan Interface (AMSI)
- PowerShell command noun: eventLog
- O namespace `Microsoft.PowerShell`

## Links

- [about powershell @ github](https://github.com/PowerShell/PowerShell)