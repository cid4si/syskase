# Desenvolvimento do Windows

## Formato PE (Portable Executable)

Os binários do Windows seguem o formato PE (enquanto os binários do Linux seguem o formato ELF).

Sufixos comuns de arquivos PE são:
- `.exe`
- `.dll`
- `.sys`

**Veja também**: [Executavel portátil]()

## Console Application vs Windows Application

Um executável do Windows é vinculado para um subsistema específico. Na maioria dos casos, é uma console application ou uma Windows application (aparentemente também conhecida como SFU ou native application).

No caso de uma console application:
- A função `CreateProcess` da WinAPI criará uma janela de console
- Anexará os streams `STDIN`, `STDOUT` e `STDERR` ao console
- Esses streams serão fechados para Windows applications

## Threads

Cada thread tem sua própria fila de entrada (input queue).

A janela do thread atual que recebe entrada pode ser obtida com `GetActiveWindow` (ou seria `GetFocus()`???).

**Compare com**: `GetForegroundWindow` que retorna a janela que está recebendo entrada no momento.

## Diversos / TODO

### NtAPI - Syscalls

Diferentemente do Unix, as aplicações Windows não chamam syscalls diretamente. Em vez disso:

1. Chamam a **WinAPI**
2. A WinAPI chama funções da **Native API (NtAPI)**
3. A NtAPI executa as syscalls

A NtAPI é implementada em `ntdll.dll` e não é documentada.

No entanto, existem syscalls no Windows. A Windows X86 System Call Table (para NT/2000/XP/2003/Vista/2008/7/8/10) mostra como os números de syscall são instáveis entre as versões do Windows.

**Recursos relacionados**:
- SyscallTables: https://github.com/hfiref0x/SyscallTables
- Adventures in Windows debugging and reverse engineering: http://www.nynaeve.net/?p=48

### No MSVCRT

Alguns módulos que não requerem msvcrt:
- console
- args
- bstr
- regexp

**Recursos relacionados**:
- Projeto no-msvcrt: https://github.com/henkman/no-msvcrt
- Stefan Kanthak's Minimalist Runtime Library for Microsoft® C Compiler

## Subsistemas

Existem dois subsistemas principais para programar:
- **CONSOLE**
- **WINDOWS**

## Checked Builds

Checked builds estavam disponíveis em versões antigas do Windows antes do Windows 10, versão 1803.



