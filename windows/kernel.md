# Kernel do Windows

O primeiro processo iniciado pelo kernel é o Windows Session Manager (smss.exe).

## Componentes Executados em Modo Kernel

Os componentes que executam em modo kernel (em oposição ao modo usuário) incluem:

- **Executive** - implementado em ntoskrnl.exe
- **Windows kernel** - também implementado em ntoskrnl.exe
- **Device drivers** - implementados em arquivos em %SystemRoot%\System32\drivers*.sys
- **Hardware Abstraction Layer (HAL)** - implementado em hal.dll
- **Sistema de janelas e gráficos** - implementado em win32k.sys
- **Camada de Hypervisor** - implementada em hvax.exe (AMD) ou hvix.exe (Intel)

## Prefixos de Função

| Prefixo | Descrição |
|---------|-----------|
| Alpc | Advanced Local Inter-Process Communication |
| Cc | Cache manager (Common Cache) |
| Ci | Code integrity |
| Cm | Configuration Manager (implementação do Registry, compare com Hyp) |
| Csr | Client Server support functions (LPC; relacionado: CSRSS.EXE) |
| Dbg | Debugger support |
| Dbgk | Debugging Framework for User-Mode |
| Em | Errata manager |
| Etw | Extended tracing |
| Ex | Executive |
| Fs | File system support |
| FsRtl | File System driver Run-Time Library |
| Hal | Hardware abstraction layer |
| Hv | Hive library |
| Hvl | Hypervisor library |
| Inbv | Funções Initial Boot Video |
| Io | I/O. Funcionalidade fornecida através de device drivers |
| Kd | Kernel debugger |
| Ke | Exported functions |
| Ki | Kernel interrupt support functions |
| Kse | Kernel shim engine |
| Ldr | PE image loader support |
| Lpc | LPC support |
| Lsa | Local security authority |
| Mm | Memory management |
| Nls | Native language support |
| Nt | Native API - implementações de syscall (Compare com Zw) |
| Ob | Object manager |
| Pf | Name prefix support functions (Prefetcher) |
| Po | Power management |
| Ppm | Processor Power Manager |
| Ps | Process management |
| Rtl | Runtime library (também funcionaria em modo usuário) |
| Rtlp | Private runtime library |
| Se | Security reference monitor (SRM). Implementação dos mecanismos de segurança que restringem quais usuários podem acessar quais recursos |
| Sm | Store Manager |
| Tm | Transaction manager |
| Ttm | Terminal timeout manager |
| Vf | Driver Verification |
| Whea | Windows Hardware Error Architecture |
| Wdi | Windows Diagnostic Infrastructure |
| Wmi | Windows management instrumentation |
| Zw | Similar ao NT, mas define o modo de acesso para Kernel, eliminando qualquer validação de parâmetros |

**Nota**: No modo usuário NTDLL, Nt e Zw são sinônimos. No entanto, se o código executando no kernel chamar funções Zw, o acesso de segurança é alterado.

## Object Manager

O Object Manager gerencia a alocação de memória dos objetos (mas veja também Mm), tempos de vida, etc.

Os objetos gerenciados pelo Object Manager incluem arquivos, processos, threads, etc.

### Tipos de Objeto

Os tipos de objeto podem ser consultados com o cmdlet do PowerShell `get-ntType` (requer o módulo NtObjectManager):

```powershell
PS:> get-ntType | sort
```

**Lista de Tipos de Objeto:**
- ActivationObject
- ActivityReference
- Adapter
- ALPC Port
- Callback
- Composition
- Controller
- CoreMessaging
- CoverageSampler
- CrossVmEvent
- CrossVmMutant
- DebugObject
- Desktop
- Device
- Directory
- DmaAdapter
- Driver
- DxgkCompositionObject
- DxgkCurrentDxgThreadObject
- DxgkDisplayManagerObject
- DxgkSharedBundleObject
- DxgkSharedKeyedMutexObject
- DxgkSharedProtectedSessionObject
- DxgkSharedResource
- DxgkSharedSwapChainObject
- DxgkSharedSyncObject
- EnergyTracker
- EtwConsumer
- EtwRegistration
- EtwSessionDemuxEntry
- Event
- File
- FilterCommunicationPort
- FilterConnectionPort
- IoCompletion
- IoCompletionReserve
- IRTimer
- Job
- Key
- KeyedEvent
- Mutant
- NdisCmState
- Partition
- PcwObject
- PowerRequest
- Process
- Profile
- PsSiloContextNonPaged
- PsSiloContextPaged
- RawInputManager
- RegistryTransaction
- Section
- Semaphore
- Session
- SymbolicLink
- Thread
- Timer
- TmEn
- TmRm
- TmTm
- TmTx
- Token
- TpWorkerFactory
- Type
- UserApcReserve
- VRegConfigurationContext
- WaitCompletionPacket
- WindowStation
- WmiGuid

## Object Manager Namespace (OMNS)

Exemplos de comandos PowerShell para explorar o namespace:

```powershell
PS:\> get-childItem ntObject:\
PS:\> get-childItem ntObject:\KernelObjects
PS:\> get-childItem ntObject:\KernelObjects | where-object typename -eq Session
PS:\> get-childItem ntObject:\KernelObjects | where-object typename -eq Event
PS:\> get-childItem ntObject:\Sessions
PS:\> get-childItem ntObject:\REGISTRY\USER\.DEFAULT\
PS:\> get-childItem ntObject:\REGISTRY\MACHINE\SAM\SAM
PS:\> get-childItem NtObject:\KnownDlls\
PS:\> get-childItem NtObject:\PowerPort
```

### BaseNamedObjects (BNO)

Por convenção, `\BaseNamedObjects` é o diretório no qual os usuários podem criar objetos kernel nomeados e assim compartilhar recursos com outros usuários no sistema. (Note que os usuários também podem escolher outros diretórios).

```powershell
PS:\> get-childItem ntObject:\BaseNamedObjects
```

### GLOBAL??

`\GLOBAL??` é o diretório global para links simbólicos, incluindo mapeamentos de drive.

```powershell
PS:\> get-childItem NtObject:\Global??\* | sort
```

### Windows

Objetos relacionados ao Window Manager:

```powershell
PS:\> get-childItem NtObject:\Windows
```

### Links Simbólicos e seus Destinos

```powershell
PS: get-childItem ntObject:\DriverData | select-object symbolicLinkTarget
```

**Exemplo de saída:**
```
SymbolicLinkTarget
------------------
\SystemRoot\System32\Drivers\DriverData
```

## LSA (Local Security Authority)

Entre as tarefas da LSA está a conversão entre SIDs e nomes.

## SRM (Security Reference Monitor)

O SRM gerencia os access tokens dos quais um é atribuído a cada processo.

Em uma verificação de acesso, o SRM compara o security descriptor de um recurso com o access token e então concede ou nega acesso ao recurso.

Se habilitado, o SRM também gera eventos de auditoria.

## Drivers

Os device drivers executam no mesmo nível de privilégio que o próprio kernel.