# Utilitários Linux

O `util-linux` é uma coleção diversificada de utilitários fundamentais para sistemas Linux. Esta documentação apresenta os principais comandos organizados por categoria, mantendo os termos técnicos essenciais em inglês para facilitar o trabalho prático com o sistema.

## Utilitários de Sistema e Controle

### /bin/dmesg
O `dmesg` é utilizado para examinar ou controlar o **kernel ring buffer** (buffer circular do kernel). Essencial para diagnóstico de problemas de hardware e drivers.

### /bin/su
O `su` permite executar comandos com um **user ID** e **group ID** substitutos. Fundamental para elevação de privilégios no sistema.

### /sbin/agetty
O `agetty` abre uma porta **tty**, solicita um nome de login e invoca o comando `/bin/login`. Normalmente é invocado pelo `init(8)`.

### /sbin/chcpu
O `chcpu` pode modificar o estado das CPUs. Pode habilitar ou desabilitar CPUs, procurar por novas CPUs, alterar o modo de **dispatching** de CPU do **hypervisor** subjacente, e solicitar CPUs do hypervisor (configure) ou retornar CPUs para o hypervisor (deconfigure).

### /sbin/ctrlaltdel
Baseado no exame do código `linux/kernel/reboot.c`, é claro que existem duas funções suportadas que a sequência Ctrl-Alt-Del pode executar.

### /sbin/sulogin
O `sulogin` é invocado pelo `init` quando o sistema entra em **single-user mode** (modo monousuário).

### /sbin/switch_root
O `switch_root` move os diretórios já montados `/proc`, `/dev`, `/sys` e `/run` para o **newroot** e torna o newroot o novo **root filesystem**, iniciando o processo `init`.

## Gerenciamento de Arquivos e Sistema de Arquivos

### /bin/findmnt
O `findmnt` lista todos os **filesystems** montados ou procura por um filesystem específico. O comando é capaz de pesquisar em `/etc/fstab`, `/etc/mtab` ou `/proc/self/mountinfo`. Se o dispositivo ou **mountpoint** não for fornecido, todos os filesystems são exibidos.

### /bin/mountpoint
O `mountpoint` verifica se o diretório ou arquivo fornecido é mencionado no arquivo `/proc/self/mountinfo`.

### /bin/more
O `more` é um filtro para paginar texto uma tela por vez. Esta versão é especialmente primitiva. Os usuários devem perceber que o `less(1)` fornece emulação do `more(1)` além de extensas melhorias.

### /sbin/fsck
O `fsck` é usado para verificar e opcionalmente reparar um ou mais **filesystems** Linux. O **filesys** pode ser um nome de dispositivo (ex: `/dev/hdc1`, `/dev/sdb2`), um **mount point** (ex: `/`, `/usr`, `/home`) ou um especificador de **label** ou **UUID** do filesystem.

### /sbin/fsfreeze
O `fsfreeze` suspende ou resume o acesso a um filesystem.

### /sbin/fstrim
O `fstrim` é usado em um filesystem montado para descartar (ou "trim") blocos que não estão em uso pelo filesystem. Isso é útil para **solid-state drives** (SSDs) e armazenamento com **thin provisioning**.

### /sbin/wipefs
O `wipefs` pode apagar assinaturas de filesystem, **raid** ou **partition table** (**magic strings**) do dispositivo especificado para tornar as assinaturas invisíveis para `libblkid`. O `wipefs` não apaga o filesystem em si nem quaisquer outros dados do dispositivo.

## Dispositivos de Bloco e Armazenamento

### /bin/lsblk
O `lsblk` lista informações sobre todos os **block devices** disponíveis ou especificados. O comando lê o **sysfs filesystem** e o **udev db** para coletar informações. Se o udev db não estiver disponível ou o lsblk for compilado sem suporte ao udev, ele tenta ler **LABELs**, **UUIDs** e tipos de filesystem.

### /sbin/blkdiscard
O `blkdiscard` é usado para descartar **device sectors**. Isso é útil para **solid-state drivers** (SSDs) e armazenamento com **thin provisioning**. Diferentemente do `fstrim(8)`, este comando é usado diretamente no **block device**.

### /sbin/blkid
O programa `blkid` é a interface de linha de comando para trabalhar com a biblioteca `libblkid(3)`. Pode determinar o tipo de conteúdo (ex: filesystem ou swap) que um block device contém, e também os atributos (**tokens**, pares **NAME=value**) dos **content metadata** (ex: campos **LABEL** ou **UUID**).

### /sbin/blkzone
O `blkzone` é usado para executar comandos de zona em dispositivos que suportam **Zoned Block Commands** (ZBC) ou **Zoned-device ATA Commands** (ZAC). As zonas para operar podem ser especificadas usando as opções **offset**, **count** e **length**.

### /sbin/blockdev
Chama **ioctls** de block device.

### /sbin/mkswap
O `mkswap` configura uma **Linux swap area** em um dispositivo ou arquivo.

### /sbin/swaplabel
O `swaplabel` exibe ou altera a **label** ou **UUID** de uma **swap partition** localizada no dispositivo (ou arquivo regular).

### /sbin/zramctl
O `zramctl` é usado para configurar rapidamente parâmetros de dispositivo **zram**, para resetar dispositivos zram e para consultar o status dos dispositivos zram usados.

## Criação de Sistemas de Arquivos

### /sbin/mkfs
Este **frontend** do `mkfs` está obsoleto em favor dos utilitários específicos de filesystem `mkfs.<type>`.

### /sbin/mkfs.bfs
O `mkfs.bfs` cria um filesystem **SCO bfs** em um block device (geralmente uma **disk partition** ou um arquivo acessado via **loop device**).

### /sbin/mkfs.cramfs
Arquivos em **cramfs file systems** são comprimidos com **zlib** uma página por vez para permitir acesso de leitura aleatória. Os **metadata** não são comprimidos, mas são expressos em uma representação concisa que é mais eficiente em espaço que filesystems convencionais.

### /sbin/mkfs.minix
O `mkfs.minix` cria um **Linux MINIX filesystem** em um dispositivo (geralmente uma disk partition).

## Verificação de Sistemas de Arquivos

### /sbin/fsck.cramfs
O `fsck.cramfs` é usado para verificar o **cramfs file system**.

### /sbin/fsck.minix
O `fsck.minix` executa uma verificação de consistência para o **Linux MINIX filesystem**.

## Gerenciamento de Partições

### /usr/bin/addpart
O `addpart` informa ao kernel Linux sobre a existência da **partition** especificada. O comando é um wrapper simples ao redor do **ioctl** "add partition".

### /usr/bin/delpart
O `delpart` pede ao kernel Linux para esquecer a partition especificada (um número) no dispositivo especificado. O comando é um wrapper simples ao redor do **ioctl** "del partition".

### /usr/bin/partx
Dado um dispositivo ou **disk-image**, o `partx` tenta analisar a **partition table** e listar seu conteúdo. Também pode informar ao kernel para adicionar ou remover partitions de seu controle.

### /usr/bin/resizepart
O `resizepart` informa ao kernel Linux sobre o novo tamanho da partition especificada. O comando é um wrapper simples ao redor do **ioctl** "resize partition".

### /sbin/findfs
O `findfs` procura nos **block devices** do sistema por um filesystem ou partition com **tag** especificada.

## Informações do Sistema

### /usr/bin/lscpu
O `lscpu` coleta informações de arquitetura de CPU do **sysfs**, `/proc/cpuinfo` e quaisquer bibliotecas específicas de arquitetura aplicáveis (ex: librtas no Powerpc). A saída do comando pode ser otimizada para **parsing** ou para fácil legibilidade por humanos. As informações incluem, por exemplo, o número de CPUs, **threads**.

### /usr/bin/lsmem
O comando `lsmem` lista os intervalos de memória disponível com seu status online. Os **memory blocks** listados correspondem à representação de memory block no **sysfs**. O comando também mostra o tamanho do memory block e a quantidade de memória em estado online e offline.

### /usr/bin/lsns
O `lsns` lista informações sobre todos os **namespaces** atualmente acessíveis ou sobre o namespace fornecido. O identificador de namespace é um número **inode**.

### /usr/bin/lsipc
O `lsipc` mostra informações sobre as facilidades de **System V inter-process communication** para as quais o processo chamador tem acesso de leitura.

### /usr/bin/lslocks
O `lslocks` lista informações sobre todos os **file locks** atualmente mantidos em um sistema Linux.

### /usr/bin/lslogins
Examina os logs **wtmp** e **btmp**, `/etc/shadow` (se necessário) e `/etc/passwd` e produz os dados desejados.

## Controle de Processos e Recursos

### /usr/bin/choom
O comando `choom` exibe e ajusta a configuração de pontuação do **Out-Of-Memory killer**.

### /usr/bin/chrt
O `chrt` define ou recupera os atributos de **real-time scheduling** de um **pid** existente, ou executa um comando com os atributos fornecidos.

### /usr/bin/ionice
Este programa define ou obtém a classe e prioridade de **I/O scheduling** para um programa. Se nenhum argumento ou apenas `-p` for fornecido, o `ionice` consultará a classe e prioridade atual de I/O scheduling para esse processo.

### /usr/bin/prlimit
Dado um **process ID** e um ou mais recursos, o `prlimit` tenta recuperar e/ou modificar os limites.

### /usr/bin/setpriv
Define ou consulta várias configurações de privilégios Linux que são herdadas através de `execve(2)`.

### /usr/bin/taskset
O `taskset` é usado para definir ou recuperar a **CPU affinity** de um processo em execução dado seu **pid**, ou para iniciar um novo comando com uma dada CPU affinity. A CPU affinity é uma propriedade do **scheduler** que "vincula" um processo a um conjunto dado de CPUs no sistema. O scheduler Linux respeitará a CPU affinity fornecida.

### /usr/bin/nsenter
O comando `nsenter` executa um programa no(s) **namespace(s)** especificados nas opções de linha de comando. Se o programa não for fornecido, então `${SHELL}` é executado (padrão: `/bin/sh`).

### /usr/bin/unshare
O comando `unshare` cria novos namespaces (conforme especificado pelas opções de linha de comando) e então executa o programa especificado. Se o programa não for fornecido, então `${SHELL}` é executado (padrão: `/bin/sh`).

### /usr/bin/setsid
O `setsid` executa um programa em uma nova **session**. O comando chama `fork(2)` se já for um **process group leader**. Caso contrário, executa um programa no processo atual. Este comportamento padrão pode ser sobrescrito pela opção `--fork`.

## Gerenciamento de Tempo e Hardware

### /sbin/hwclock
O `hwclock` é uma ferramenta administrativa para os relógios de tempo. Pode: exibir a hora do **Hardware Clock**; definir o Hardware Clock para uma hora especificada; definir o Hardware Clock a partir do **System Clock**; definir o System Clock a partir do Hardware Clock; compensar o **drift** do Hardware Clock; corrigir o System Clock.

### /usr/sbin/rtcwake
Este programa é usado para entrar em um estado de **sleep** do sistema e para despertar automaticamente dele em um horário especificado.

### /bin/wdctl
Mostra o status do **hardware watchdog**. O dispositivo padrão é `dev/watchdog`. Se mais de um dispositivo for especificado, a saída é separada por uma linha em branco.

## Comunicação Inter-Processos

### /usr/bin/ipcmk
O `ipcmk` permite criar objetos de **System V inter-process communication** (IPC): **shared memory segments**, **message queues** e **semaphore arrays**.

### /usr/bin/ipcrm
O `ipcrm` remove objetos de **System V inter-process communication** (IPC) e estruturas de dados associadas do sistema. Para deletar tais objetos, você deve ser **superuser**, ou o criador ou proprietário do objeto.

### /usr/bin/ipcs
O `ipcs` mostra informações sobre facilidades de **System V inter-process communication**. Por padrão, mostra informações sobre todos os três recursos: shared memory segments, message queues e semaphore arrays.

## Utilitários de Sistema e Texto

### /usr/bin/fallocate
O `fallocate` é usado para manipular o espaço em disco alocado para um arquivo, seja para desalocá-lo ou pré-alocá-lo. Para filesystems que suportam a **system call** `fallocate`, a pré-alocação é feita rapidamente alocando blocos e marcando-os como não inicializados, não requerendo **IO** para os **data blocks**.

### /usr/bin/fincore
O `fincore` conta páginas de conteúdo de arquivo sendo residentes na memória (**in core**), e reporta os números. Se ocorrer um erro durante a contagem, uma mensagem de erro é impressa no **stderr** e o fincore continua processando o resto dos arquivos listados na linha de comando.

### /usr/bin/flock
Este utilitário gerencia **locks** `flock(2)` de dentro de **shell scripts** ou da linha de comando.

### /usr/bin/getopt
O `getopt` é usado para quebrar (**parse**) opções em linhas de comando para fácil análise por **shell procedures**, e para verificar opções válidas. Usa as rotinas **GNU getopt(3)** para fazer isso.

### /usr/bin/mcookie
O `mcookie` gera um número hexadecimal aleatório de 128 bits para uso com o **X authority system**.

### /usr/bin/mesg
O utilitário `mesg` é invocado por um usuário para controlar o acesso de escrita que outros têm ao dispositivo **terminal** associado com a **standard error output**. Se acesso de escrita for permitido, então programas como `talk(1)` e `write(1)` podem exibir mensagens no terminal.

### /usr/bin/namei
O `namei` interpreta seus argumentos como **pathnames** para qualquer tipo de arquivo Unix (**symlinks**, arquivos, diretórios, etc.). O `namei` então segue cada pathname até que um endpoint seja encontrado (um arquivo, um diretório, um **device node**, etc). Se encontrar um **symbolic link**, mostra o link e começa a segui-lo.

### /usr/bin/rev
O utilitário `rev` copia os arquivos especificados para a **standard output**, invertendo a ordem dos caracteres em cada linha. Se nenhum arquivo for especificado, a **standard input** é lida.

### /usr/bin/setarch
O `setarch` modifica **execution domains** e **process personality flags**.

### /usr/bin/setterm
O `setterm` escreve na standard output uma string de caracteres que invocará as **terminal capabilities** especificadas. Onde possível, o **terminfo** é consultado para encontrar a string a usar. Algumas opções, entretanto (marcadas como "apenas consoles virtuais" abaixo) não correspondem a uma **capability** do `terminfo(5)`.

### /usr/bin/utmpdump
O `utmpdump` é um programa simples para despejar arquivos **UTMP** e **WTMP** em formato bruto, para que possam ser examinados. O `utmpdump` lê da **stdin** a menos que um nome de arquivo seja passado.

### /usr/bin/whereis
O `whereis` localiza os arquivos **binary**, **source** e **manual** para os nomes de comando especificados. Os nomes fornecidos são primeiro despojados de componentes de **pathname** iniciais e qualquer extensão final única da forma `.ext` (por exemplo: `.c`). Prefixos de `s.` resultantes do uso de controle de **source code** também são tratados.

## Logs e Auditoria

### /usr/bin/last
O `last` procura no arquivo `/var/log/wtmp` (ou o arquivo designado pela opção `-f`) e exibe uma lista de todos os usuários que fizeram login (e logout) desde que esse arquivo foi criado. Um ou mais **usernames** e/ou **ttys** podem ser fornecidos, caso em que o last mostrará apenas as entradas que correspondem a esses argumentos.

### /usr/bin/lastb
O `lastb` procura no arquivo `/var/log/wtmp` (ou o arquivo designado pela opção `-f`) e exibe uma lista de todos os usuários que fizeram login (e logout) desde que esse arquivo foi criado. Um ou mais usernames e/ou ttys podem ser fornecidos, caso em que o last mostrará apenas as entradas que correspondem a esses argumentos.

## Utilitários de Execução

### /sbin/runuser
O `runuser` pode ser usado para executar comandos com um **user ID** e **group ID** substitutos. Se a opção `-u` não for fornecida, o runuser volta à semântica compatível com `su` e um **shell** é executado. A diferença entre os comandos `runuser` e `su` é que o runuser não pede uma senha (porque pode ser executado).

### /usr/bin/setarch
O `setarch` modifica execution domains e process personality flags.

### /usr/bin/i386, /usr/bin/linux32, /usr/bin/linux64, /usr/bin/x86_64
**Symlinks** para `setarch`.

## Utilitários de Discos e Mídia

### /sbin/isosize
Este comando produz o comprimento de um **iso9660 filesystem** que está contido no arquivo especificado. Este arquivo pode ser um arquivo normal ou um block device (ex: `/dev/hdd` ou `/dev/sr0`). Na ausência de quaisquer opções (e erros), produzirá o tamanho do iso9660 filesystem em bytes.

### /usr/sbin/fdformat
O `fdformat` faz uma formatação de baixo nível em um **floppy disk**. O dispositivo é geralmente um dos seguintes (para **floppy devices** o **major** = 2, e o **minor** é mostrado apenas para fins informativos).

### /usr/sbin/ldattach
O **daemon** `ldattach` abre o arquivo de dispositivo especificado (que deve se referir a um **serial device**) e anexa a **line discipline** `ldisc` a ele para processamento dos dados enviados e/ou recebidos. Em seguida, vai para o **background** mantendo o dispositivo aberto para que a line discipline permaneça carregada.

### /sbin/pivot_root
O `pivot_root` move o **root file system** do processo atual para o diretório `put_old` e torna `new_root` o novo root file system. Como `pivot_root(8)` simplesmente chama `pivot_root(2)`, referimos à página de manual do último para mais detalhes.

### /sbin/raw
O `raw` é usado para vincular um **Linux raw character device** a um block device. Qualquer block device pode ser usado: no momento da vinculação, o **device driver** nem precisa estar acessível (pode ser carregado sob demanda como um **kernel module** posteriormente).

## Gerenciamento de Memória

### /usr/sbin/chmem
O comando `chmem` define um tamanho ou intervalo específico de memória online ou offline.

### /usr/sbin/readprofile
O comando `readprofile` usa as informações de `/proc/profile` para imprimir dados **ascii** na **standard output**.

