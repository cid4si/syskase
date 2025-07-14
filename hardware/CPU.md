# CPU (Central Processing Unit)

A sigla CPU significa **Central Processing Unit**, ou **Unidade Central de Processamento**. É o principal componente de hardware de um computador, responsável por executar as instruções de um programa e realizar cálculos aritméticos e lógicos.

A CPU é frequentemente chamada de "cérebro" do computador, pois coordena e controla todas as operações do sistema, interpretando e executando instruções de software.

## Identificação e Características da CPU

É possível identificar as características da CPU de um sistema através de comandos específicos no terminal de cada sistema operacional.

### Em Sistemas Linux

No [Linux](), as informações detalhadas da CPU podem ser obtidas com os seguintes comandos:

```bash
# Informações completas da CPU
cat /proc/cpuinfo

# Informações resumidas da topologia da CPU
lscpu

# Número de processadores disponíveis
nproc

# Número total de CPUs (incluindo threads)
nproc --all
```

O comando `lscpu` fornece informações organizadas sobre:
- Arquitetura da CPU
- Modo de operação
- Número de CPUs e cores
- Threads por core
- Velocidade (MHz)
- Cache L1, L2, L3
- Flags e recursos suportados

### Em Sistemas Windows

No **prompt de comando do Windows (cmd.exe)** ou no **PowerShell**, comandos semelhantes podem ser usados:

**Usando o wmic:**
```cmd
wmic cpu get deviceId,numberOfCores,numberOfLogicalProcessors
wmic cpu get name,manufacturer,maxClockSpeed
```

**Usando o PowerShell:**
```powershell
# Informações básicas da CPU
Get-ComputerInfo | select-object csProcessors

# Informações detalhadas
Get-WmiObject -Class Win32_Processor | Select-Object Name, NumberOfCores, NumberOfLogicalProcessors, MaxClockSpeed

# Informações sobre cache
Get-WmiObject -Class Win32_CacheMemory
```

## Topologia da CPU

A organização física e lógica dos processadores segue uma hierarquia bem definida:

### Hierarquia de Componentes

1. **Thread/Logical Processor**: Menor unidade de processamento que pode executar instruções independentemente
2. **Core (Núcleo)**: Contém uma ou mais threads (processadores lógicos). Com tecnologia como Hyper-Threading, um core pode executar múltiplas threads simultaneamente
3. **Socket/Package**: Contém um ou mais cores. Fisicamente, é o local na placa-mãe onde o processador é encaixado
4. **Book**: Contém um ou mais sockets (mais comum em sistemas mainframe)
5. **Node (NUMA)**: Contém um ou mais books e representa um domínio de memória local

### Exemplo de Topologia
```
Node 0
├── Socket 0
│   ├── Core 0 (2 threads: 0, 1)
│   ├── Core 1 (2 threads: 2, 3)
│   └── Core 2 (2 threads: 4, 5)
└── Socket 1
    ├── Core 0 (2 threads: 6, 7)
    └── Core 1 (2 threads: 8, 9)
```

## SMP (Symmetric Multiprocessing)

**SMP** significa **Symmetric Multiprocessing**, ou **Multiprocessamento Simétrico**. Refere-se a uma arquitetura de computador onde:

- Dois ou mais processadores idênticos estão conectados a uma única memória principal compartilhada
- Todos os processadores têm acesso igual à memória e aos recursos de I/O
- São controlados por uma única instância do sistema operacional
- Cada processador pode executar qualquer tarefa do sistema operacional

### Características do SMP:
- **Uniformidade**: Todos os processadores têm capacidades idênticas
- **Acesso compartilhado**: Memória e recursos são compartilhados igualmente
- **Escalabilidade**: Melhoria de desempenho com adição de processadores
- **Balanceamento de carga**: O SO distribui tarefas entre os processadores

## Arquiteturas de CPU

### ARM (Advanced RISC Machines)

**ARM** significa **Advanced RISC Machines** (anteriormente **Acorn RISC Machine**). 

**Características principais:**
- Família de arquiteturas **RISC** (Reduced Instruction Set Computer)
- Dominância em dispositivos móveis (em 2013, 60% dos dispositivos móveis continham chips ARM)
- Atualmente tem praticamente monopólio em dispositivos portáteis
- Foco em eficiência energética e desempenho otimizado

**ARM Holdings Inc.** é uma empresa **fabless** (sem fabricação própria):
- Projeta chips e licencia sua tecnologia
- Principais licenciados: ATMEL, NXP, Samsung, Apple, Qualcomm
- Modelo de negócio baseado em royalties e licenciamento

### Intel CPUs

**Características das CPUs Intel:**
- Arquitetura **x86** e **x86-64**
- **Little endian** (byte menos significativo primeiro)
- Dominância em computadores desktop e servidores
- Tecnologias como Hyper-Threading, Turbo Boost

**Links úteis:**
- Processor numbers (sistema de numeração Intel)
- Especificações técnicas no site oficial Intel

## Endianness (Ordem de Bytes)

**Endianness** refere-se à ordem em que os bytes são armazenados na memória:

### Little Endian
- Byte **menos significativo** vem primeiro
- Exemplo: `0x12345678` é armazenado como: `0x78 | 0x56 | 0x34 | 0x12`
- Usado por: Intel x86, AMD x86-64, ARM (configurável)

### Big Endian
- Byte **mais significativo** vem primeiro
- Exemplo: `0x12345678` é armazenado como: `0x12 | 0x34 | 0x56 | 0x78`
- Também conhecido como **network byte order** (RFC 1700)
- Usado por: PowerPC, SPARC, protocolos de rede

### Verificação de Endianness

**Em Oracle Database:**
```sql
SELECT d.name, tp.endian_format 
FROM v$database d 
JOIN v$transportable_platform tp ON tp.platform_name = 'Your Platform';
```

**Em C:**
```c
#include <stdio.h>

int main() {
    unsigned int x = 0x12345678;
    unsigned char *c = (unsigned char*)&x;
    
    if (*c == 0x78) {
        printf("Little Endian\n");
    } else {
        printf("Big Endian\n");
    }
    return 0;
}
```

## Monitoramento e Utilização da CPU

### Linux
```bash
# Utilização da CPU por processo
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu

# Monitoramento em tempo real
top
htop

# Estatísticas detalhadas
iostat -c 1 10
sar -u 1 10

# Informações sobre utilização por core
mpstat -P ALL 1 10
```

### Windows
```powershell
# Utilização da CPU
Get-Counter "\Processor(_Total)\% Processor Time"

# Processos com maior uso de CPU
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10

# Monitoramento contínuo
Get-Counter "\Processor(_Total)\% Processor Time" -SampleInterval 1 -MaxSamples 60
```

## Componentes Relacionados

### ALU (Arithmetic Logic Unit)
- **Unidade Lógica Aritmética**
- Componente da CPU que realiza operações aritméticas (+, -, ×, ÷)
- Executa operações lógicas (AND, OR, NOT, XOR)
- Operações de comparação e deslocamento de bits

### Cache da CPU
- **L1 Cache**: Mais rápido, menor capacidade (32-64 KB por core)
- **L2 Cache**: Intermediário (256 KB - 1 MB por core)
- **L3 Cache**: Maior capacidade, compartilhado entre cores (8-64 MB)

### Barramento PCI
A CPU se conecta aos periféricos através do **barramento PCI** (Peripheral Component Interconnect):
- **PCI Express (PCIe)**: Versão moderna, mais rápida
- **Lanes**: Canais de comunicação (x1, x4, x8, x16)
- **Bandwidth**: Largura de banda varia por versão (PCIe 3.0, 4.0, 5.0)

## Programação e Desenvolvimento

### Contagem de CPUs em C (Linux)
```c
#include <stdio.h>
#include <dirent.h>
#include <string.h>

int count_cpus() {
    DIR *dir;
    struct dirent *entry;
    int count = 0;
    
    dir = opendir("/sys/devices/system/cpu");
    if (dir == NULL) return -1;
    
    while ((entry = readdir(dir)) != NULL) {
        if (strncmp(entry->d_name, "cpu", 3) == 0 && 
            isdigit(entry->d_name[3])) {
            count++;
        }
    }
    
    closedir(dir);
    return count;
}
```

### Afinidade de CPU
```bash
# Linux - definir afinidade de processo
taskset -c 0,1 ./meu_programa

# Verificar afinidade atual
taskset -p PID
```

```powershell
# Windows - definir afinidade
$process = Get-Process -Name "notepad"
$process.ProcessorAffinity = 3  # Usar cores 0 e 1
```

## Tecnologias Modernas

### Hyper-Threading (Intel)
- Tecnologia que permite que um core físico execute duas threads simultaneamente
- Melhora a utilização de recursos do pipeline da CPU
- Aumento de desempenho de 15-30% em aplicações multi-threaded

### Turbo Boost / Turbo Core
- Aumento automático da frequência da CPU quando há demanda
- Funciona quando temperatura e consumo estão dentro dos limites
- Pode aumentar significativamente o desempenho em cargas de trabalho single-threaded

### Tecnologias de Virtualização
- **Intel VT-x / AMD-V**: Virtualização assistida por hardware
- **IOMMU**: Mapeamento de memória para dispositivos virtualizados
- **Nested Virtualization**: Virtualização dentro de ambientes virtualizados

## Considerações de Desempenho

### Fatores que Afetam o Desempenho:
1. **Frequência (Clock Speed)**: Medida em GHz
2. **Número de cores**: Paralelismo de tarefas
3. **Arquitetura**: Instruções por ciclo (IPC)
4. **Cache**: Redução de latência de memória
5. **Largura de banda de memória**: Throughput de dados
6. **Thermal Design Power (TDP)**: Limitações térmicas

### Otimizações:
- **Localidade de dados**: Manter dados próximos no cache
- **Paralelização**: Dividir tarefas entre múltiplos cores
- **Vectorização**: Usar instruções SIMD (SSE, AVX)
- **Branch prediction**: Otimizar estruturas condicionais

