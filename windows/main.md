# Windows

O Windows é um *sistema operacional multiprocessamento simétrico* (SMP, do inglês Symmetric Multiprocessing), o que significa que todas as [CPUs](./hardware/CPU.md) (ou núcleos) disponíveis no sistema são tratadas de forma igual e podem executar qualquer processo.

Nesse tipo de arquitetura, o kernel do Windows pode agendar um processo para ser executado em qualquer CPU disponível, o que permite uma utilização mais eficiente dos recursos do sistema e melhora o desempenho geral.

No Windows, o escalonador de processos (ou scheduler) é responsável por decidir qual processo deve ser executado em cada CPU e quando. O escalonador leva em conta vários fatores, como a prioridade do processo, o tempo de execução necessário e a disponibilidade de recursos do sistema.

Com o SMP, o Windows pode:

- Executar vários processos simultaneamente em diferentes CPUs;
- Melhorar o desempenho de aplicativos que utilizam múltiplos núcleos;
- Reduzir o tempo de resposta do sistema.
É importante notar que, além do SMP, o Windows também suporta outras tecnologias, como o Hyper-Threading (HT) e o multithreading, que podem melhorar ainda mais o desempenho do sistema.

# Configurando o Windows

O Windows pode ser configurado de diversas formas:

- Configurações: uma interface de usuário para ajustar as preferências do sistema.
- Painel de Controle: um conjunto de ferramentas para gerenciar e personalizar o sistema, geralmente destinado a usuários gerais.
- MMC (Microsoft Management Console): uma ferramenta avançada para gerenciar e configurar o sistema, direcionada a profissionais de TI e administradores de sistemas.