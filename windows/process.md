# Processos Windows

## Conceito de Processo

Para executar um programa executável (a **base image**), um processo é necessário.

Um processo consiste basicamente em:

- **Private virtual address space** (espaço de endereçamento virtual privado)
- Um executável (**binary**) que é mapeado no virtual address space
- **Handles**: semáforos, objetos de sincronização, arquivos...
- Um **security context** (contexto de segurança)

Um processo também consiste em **modules** que são executáveis e **DLLs**.

- Um processo é identificado por seu **process id** (PID)
- O proprietário do processo, seus privilégios e grupos de segurança são identificados por um **access token**
- O processo possui **virtual memory** para uso privado disponível
- Um processo consiste em uma ou mais **threads** (processos vazios são possíveis, mas não úteis)

Recursos que foram alocados pelo Windows em nome dos processos são identificados por **handles** (e tais handles podem ser mostrados com as ferramentas **Sysinternals** como `handle`).

Duas estruturas importantes para processos são:
- **EPROCESS** (kernel mode)
- **PEB** (user mode)

## Processos em User Mode

Os processos de usuário podem ser divididos em quatro categorias:

1. **User processes** (processos de usuário)
2. **Service processes** (processos de serviço)
3. **System processes** (como `wininit.exe` e `winlogon.exe`)
4. **Environment subsystem server processes** (processos de servidor de subsistema de ambiente)

### Processo em Foreground

Aparentemente, no máximo um processo é o **foreground process**. Parece possível que nenhum processo seja o foreground process.

## Serviços

Processos em **background** que não requerem interação do usuário são referidos como **services**.

Os services são controlados pelo **Service Control Manager** (SCM) (cujo executável é `services.exe`).

Uma **registry key** importante é `HKLM\System\CurrentControlSet\Services`.

O `svchost.exe` é aparentemente usado de alguma forma para **shared processes**.

### explorer.exe

O primeiro processo que é criado quando um usuário faz login é o `explorer.exe`.

Este processo é, portanto, direta ou indiretamente o **parent process** para todos os processos que o usuário então cria.

## Criação de Processos

Um processo é criado pela função **WinAPI** `CreateProcess()`.

### Fluxo do CreateProcess()

1. **Cria um process object**
2. **Aloca memória** para o novo processo
3. **Mapeia `ntdll.dll`** e a **image do .exe** na nova memória alocada
4. **Cria a primeira thread**
5. **Aloca memória** para o **stack** da thread
6. **Transfere controle** para esta thread para a função `LdrpInitialize` em `ntdll.dll`

### Processo de Inicialização

O `LdrpInitialize`:
- Recursivamente percorre as **import tables** do EXE e mapeia os executáveis encontrados na memória
- Chama `LdrpRunInitializeRoutines` que por sua vez chama os **entry points** das DLLs que foram mapeadas na memória (usando o parâmetro `DLL_PROCESS_ATTACH`)

Após inicializar as DLLs, `LdrpInitialize` chama `BaseProcessStart` (em `kernel32.dll`) que eventualmente chama o **entry point** do exe que (provavelmente) eventualmente chama `main` ou `WinMain`.

## Níveis de Integridade

Cada processo em execução recebe um **integrity level** que é identificado por um **SID**:

| Nome | SID | Exemplo ou comentários |
|------|-----|------------------------|
| Untrusted(?) | S-1-16-0 | |
| Low | S-1-16-4096 | `iexplore.exe` (para prevenir propagação de malware) |
| Medium | S-1-16-8192 | `explorer` (o shell process?), `ieuser.exe`, Excel |
| Medium+Mandatory | S-1-16-8448 | 8448 = 8192 + 256 |
| High | S-1-16-12288 | `mmc.exe`, `setup.exe` |
| System | S-1-16-16384 | `svchost.exe`, `winlogon.exe`, Windows kernel |
| Protected-Process(?) | S-1-16-20480 | |
| Secure-Process(?) | S-1-16-28672 | |

## PowerShell

A **variável automática** do PowerShell `$PID` contém o **process identifier** do processo que está hospedando a sessão atual do Windows PowerShell.

O **cmdlet** `get-process` retorna os processos que estão executando localmente ou remotamente.

Processos podem ser iniciados com `start-process`.

## Processos Especiais

Processos especiais são:
- O **System process** (cujo process id é 4)
- O **Idle process** (cujo process id é 0)
- Os processos `csrss.exe`

## Consultando Processos

Processos existentes podem ser consultados com `WMIC.exe`:

**Mostrar process id e nome:**
```cmd
wmic process get processId,name
```

**Mostrar parent process id e process id dos processos cmd.exe:**
```cmd
wmic process where "name='cmd.exe'" get parentProcessId,ProcessId
```

Outra possibilidade para consultar processos em execução (ou parados) é fornecida pelo `tasklist.exe`.

### Obtendo o Parent PID de um Processo

**No PowerShell:**
```powershell
get-cimInstance win32_Process -filter "name = 'oracle.exe'" | select parentProcessId
```

**No PowerShell Core:**
```powershell
get-process | where-object id -eq $pid | select-object { $_.parent.id }
```

A ferramenta **Sysinternals** `pslist` (ou `pslist64`) mostra uma árvore completa quando invocada com a opção `-t`.

## Error Mode

Cada processo tem um **error mode** associado que indica como reagir a erros sérios.

O error mode é definido com `SetErrorMode(…)`. O **MSDN** recomenda chamar `SetErrorMode` com o parâmetro `SEM_FAILCRITICALERRORS`.

## Terminando Processos

- `taskkill.exe` mata processos no `cmd.exe` (e PowerShell)
- O PowerShell tem um cmdlet específico para terminar processos: `stop-process`
- Este cmdlet é usado no script simples do PowerShell `kil.ps1`
- Também existe o `tskill.exe`, o **Remote Desktop Services End Process Utility**
- `kill.exe`

## Número de Processos

No PowerShell, o número de processos pode ser determinado com o cmdlet `get-counter`:

```powershell
"Number of processes: $((get-counter '\Objects\Processes').counterSamples.cookedValue)"
```

## Funcionalidades Avançadas

### Suspensão de Processos

Um processo pode ser colocado em estado **suspended** com a **API nativa** não documentada `NtSuspendProcess`.

### Lista de Processos

Uma lista de processos em execução pode ser obtida com `NtQuerySystemInformation()`.

### Impersonation

**Impersonation** é a capacidade de um processo assumir os atributos de segurança de outro processo.

Veja também o **impersonation access token**.

---
