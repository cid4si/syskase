# Linux

O Linux é um sistema operacional multiprocessamento simétrico (SMP, do inglês Symmetric Multiprocessing), o que significa que todas as [CPUs](../hardware/CPU.md) (ou núcleos) disponíveis no sistema são tratadas de forma igual e podem executar qualquer processo ou thread do kernel.
Nesse tipo de arquitetura, o kernel do Linux pode agendar um processo para ser executado em qualquer CPU disponível, o que permite uma utilização mais eficiente dos recursos do sistema e melhora o desempenho geral. O kernel Linux implementa um scheduler preemptivo que garante que nenhum processo monopolize indefinidamente uma CPU.
No Linux, o Completely Fair Scheduler (CFS) é o escalonador padrão responsável por decidir qual processo deve ser executado em cada CPU e quando. O CFS utiliza uma abordagem baseada em virtual runtime que garante que todos os processos recebam uma fatia justa do tempo de CPU, levando em conta fatores como:

A prioridade do processo (nice value)
O tempo de execução já consumido
A disponibilidade de recursos do sistema
A CPU affinity (afinidade de CPU)

Com o SMP, o Linux pode:

Executar vários processos simultaneamente em diferentes CPUs
Balancear automaticamente a carga entre os núcleos disponíveis
Melhorar o desempenho de aplicativos multi-threaded
Reduzir a latência do sistema através do load balancing

É importante notar que, além do SMP, o Linux também suporta tecnologias avançadas como NUMA (Non-Uniform Memory Access), CPU hotplug (adição/remoção de CPUs em tempo de execução), e otimizações específicas para arquiteturas como Hyper-Threading, que podem melhorar ainda mais o desempenho e a escalabilidade do sistema.

---

- [Utilitarios](./utilities.md)
- Distro
- Gerenciadores de pacotes