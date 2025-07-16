# Painel de Controle do Windows

O painel de controle pode ser usado para configurar recursos opcionais do Windows (ao invés de aplicativos) como configuração de hardware e software, configurações relacionadas à segurança, manutenção geral, gerenciamento de usuários, etc.

No Windows 10, os itens do painel de controle estão sendo gradualmente movidos para o aplicativo Settings (Configurações).

### Abrindo o Painel de Controle pelo Console

O painel de controle pode ser aberto no cmd.exe com um simples:
```cmd
c:\> control.exe
```

### Itens do Painel de Controle

Os painéis individuais que podem ser abertos a partir do painel de controle são chamados de **control panel items** (itens do painel de controle).

Se um item do painel de controle pode ser acessado diretamente do próprio painel de controle, o item é chamado de **top-level control panel item**.

Um item do painel de controle pode ser aberto no cmd.exe com um GUID ou um nome canônico:
```cmd
C:\> explorer shell:::{GUID}
C:\> control /name nome.do.painel.controle
```

Os GUIDs são na verdade CLSIDs que pertencem aos itens do painel de controle. Esses CLSIDs também são encontrados no registro sob a chave `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\ControlPanel\NameSpace`.

O nome canônico é encontrado sob essa chave de registro no valor de `System.ApplicationName`.

### Nomes Canônicos

Cada item do painel de controle recebe um nome canônico (não localizado) que permite abri-lo pela linha de comando (cmd.exe ou PowerShell):
```cmd
c:\> %systemroot%\system32\control.exe /name nome.canonico
```

A convenção de nomenclatura para esses nomes canônicos é `CorporateName.ControlPanelItemName`.

### Principais Itens do Painel de Controle

| Nome | Nome Canônico | Descrição |
|------|---------------|-----------|
| **Action Center** | `Microsoft.ActionCenter` | Central de ações e notificações |
| **Administrative Tools** | `Microsoft.AdministrativeTools` | Ferramentas administrativas |
| **AutoPlay** | `Microsoft.AutoPlay` | Configurações de reprodução automática |
| **Biometric Devices** | `Microsoft.BiometricDevices` | Dispositivos biométricos |
| **BitLocker Drive Encryption** | `Microsoft.BitLockerDriveEncryption` | Criptografia de unidade BitLocker |
| **Color Management** | `Microsoft.ColorManagement` | Gerenciamento de cores |
| **Credential Manager** | `Microsoft.CredentialManager` | Gerenciador de credenciais |
| **Date and Time** | `Microsoft.DateAndTime` | Data e hora |
| **Default Programs** | `Microsoft.DefaultPrograms` | Programas padrão |
| **Device Manager** | `Microsoft.DeviceManager` | Gerenciador de dispositivos |
| **Devices and Printers** | `Microsoft.DevicesAndPrinters` | Dispositivos e impressoras |
| **Display** | `Microsoft.Display` | Configurações de exibição |
| **Ease of Access Center** | `Microsoft.EaseOfAccessCenter` | Central de acessibilidade |
| **Family Safety** | `Microsoft.ParentalControls` | Controle parental |
| **File History** | `Microsoft.FileHistory` | Histórico de arquivos |
| **Folder Options** | `Microsoft.FolderOptions` | Opções de pasta |
| **Fonts** | `Microsoft.Fonts` | Fontes |
| **HomeGroup** | `Microsoft.HomeGroup` | Grupo doméstico |
| **Indexing Options** | `Microsoft.IndexingOptions` | Opções de indexação |
| **Internet Options** | `Microsoft.InternetOptions` | Opções da Internet |
| **Keyboard** | `Microsoft.Keyboard` | Teclado |
| **Mouse** | `Microsoft.Mouse` | Mouse |
| **Network and Sharing Center** | `Microsoft.NetworkAndSharingCenter` | Central de rede e compartilhamento |
| **Notification Area Icons** | `Microsoft.NotificationAreaIcons` | Ícones da área de notificação |
| **Pen and Touch** | `Microsoft.PenAndTouch` | Caneta e toque |
| **Personalization** | `Microsoft.Personalization` | Personalização |
| **Phone and Modem** | `Microsoft.PhoneAndModem` | Telefone e modem |
| **Power Options** | `Microsoft.PowerOptions` | Opções de energia |
| **Programs and Features** | `Microsoft.ProgramsAndFeatures` | Programas e recursos |
| **Recovery** | `Microsoft.Recovery` | Recuperação |
| **Region** | `Microsoft.RegionAndLanguage` | Região e idioma |
| **Sound** | `Microsoft.Sound` | Som |
| **Speech Recognition** | `Microsoft.SpeechRecognition` | Reconhecimento de fala |
| **Storage Spaces** | `Microsoft.StorageSpaces` | Espaços de armazenamento |
| **Sync Center** | `Microsoft.SyncCenter` | Central de sincronização |
| **System** | `Microsoft.System` | Sistema |
| **Taskbar and Navigation** | `Microsoft.Taskbar` | Barra de tarefas e navegação |
| **Troubleshooting** | `Microsoft.Troubleshooting` | Solução de problemas |
| **User Accounts** | `Microsoft.UserAccounts` | Contas de usuário |
| **Windows Defender** | `Microsoft.WindowsDefender` | Windows Defender |
| **Windows Firewall** | `Microsoft.WindowsFirewall` | Firewall do Windows |
| **Windows Mobility Center** | `Microsoft.MobilityCenter` | Central de mobilidade |
| **Windows Update** | `Microsoft.WindowsUpdate` | Windows Update |

### Exemplos de Uso do Painel de Controle

#### Abrir itens específicos:
```cmd
# Abrir configurações de sistema
c:\> control /name Microsoft.System

# Abrir gerenciador de dispositivos
c:\> control /name Microsoft.DeviceManager

# Abrir opções de energia
c:\> control /name Microsoft.PowerOptions

# Abrir programas e recursos
c:\> control /name Microsoft.ProgramsAndFeatures

# Abrir central de rede e compartilhamento
c:\> control /name Microsoft.NetworkAndSharingCenter
```

#### Usando GUIDs:
```cmd
# Abrir configurações do mouse
c:\> explorer shell:::{6C8EEC18-8D75-41B2-A177-8831D59D2D50}

# Abrir configurações de som
c:\> explorer shell:::{F2DDFC82-8F12-4CDD-B7DC-D4FE1425AA4D}

# Abrir programas e recursos
c:\> explorer shell:::{7B81BE6A-CE2B-4676-A29E-EB907A5126C5}
```

### Aplicativos do Painel de Controle

Os aplicativos do painel de controle são armazenados em uma DLL com o sufixo especial `.cpl`.

Essas DLLs precisam de um ponto de entrada chamado `CPlApplet` com a seguinte assinatura:
```c
LONG CPlApplet(
  HWND hwndCPl,
  UINT msg,
  LPARAM lParam1,
  LPARAM lParam2
);
```

### Visualizando Itens do Painel de Controle no PowerShell

No PowerShell, os itens disponíveis do painel de controle podem ser exibidos com:
```powershell
get-controlPanelItem | select-object -property name,description,category
```

### Notas Importantes

- O **Ease of Access Center** também pode ser aberto com `control access.cpl`
- No Windows 10, muitos itens do painel de controle estão sendo migrados para o aplicativo Settings
- Os GUIDs são CLSIDs encontrados no registro sob `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\ControlPanel\NameSpace`
- A chave `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Control Panel\don't load` lista alguns arquivos `*.cpl` que estão ocultos no painel de controle

---

## Categorias de Configurações

As configurações do Windows são organizadas nas seguintes categorias principais:

| Categoria | Descrição | URI |
|-----------|-----------|-----|
| **System** | Display, som, notificações, energia, armazenamento, etc. | `system` |
| **Devices** | Bluetooth, impressoras, mouse | `devices` |
| **Phone** | Vincular dispositivos Android, iPhone | `mobile-devices` |
| **Network & Internet** | Wi-Fi, modo avião, VPN | `network` |
| **Personalization** | Plano de fundo, tela de bloqueio, cores | `personalization` |
| **Apps** | Desinstalar, padrões, recursos opcionais | `appsfeatures-app` |
| **Accounts** | Sua conta, email, sincronização, trabalho, família | - |
| **Time & Language** | Fala, região, data | - |
| **Gaming** | Xbox Game Bar, capturas, Game Mode | - |
| **Ease of Access** | Narrator, lupa, alto contraste | - |
| **Cortana** | Configurações da Cortana | - |
| **Search** | Encontrar arquivos, permissões | - |
| **Privacy** | Localização, câmera, microfone | - |
| **Update & Security** | Windows Update, backup, recuperação | - |

## Abrindo o Painel de Configurações

### Atalho de Teclado
As configurações podem ser abertas pressionando o atalho de teclado **Windows+I**.

### Menu Iniciar
O Menu Iniciar possui um botão proeminente para abrir as Configurações.

### cmd.exe
No console cmd.exe, as configurações podem ser abertas assim:
```cmd
c:\> start ms-settings:
```

**Nota:** No Windows, `ms-settings:` é um URI scheme (assim como `ms-store:`, `ms-tonepicker:` ou outros mais conhecidos como `http:`).

### PowerShell
No PowerShell, utilize:
```powershell
PS C:\Users\Rene> start-process ms-settings:
```

### Power User Menu
O Power User Menu (que é aberto com o atalho **Windows+X**) também possui uma entrada para configurações.

### Mostrar Todas as Configurações
No explorer, abra o endereço especial `shell:::{ED7BA470-8E54-465E-825C-99712043E01C}` ou digite no cmd.exe:
```cmd
C:\> explorer shell:::{ED7BA470-8E54-465E-825C-99712043E01C}
```

## Iniciando Diálogos de Configuração pela Linha de Comando

Tanto no cmd.exe quanto no PowerShell, diálogos de configuração podem ser abertos com `start ms-...`. (No PowerShell, `start` é um alias para `start-process`, no cmd.exe `start` é um comando built-in).

**Importante:** Diferente de muitos outros comandos do Windows, o texto após `ms-settings:` é case sensitive.

## Configurações Específicas

### Sistema e Display
```cmd
ms-settings:regionlanguage          # País/região e idioma
ms-settings:developers             # Configurações do Windows para developer mode
ms-settings:display                # Gerenciar displays múltiplos, resolução e rotação de tela, night light, brilho
ms-settings:screenrotation         # Alias para ms-settings:display
ms-settings:dateandtime            # Definir fuso horário e formatos de data
ms-settings:powersleep             # Definir tempo de inatividade para desligar tela e suspender
```

### Acessibilidade
```cmd
ms-settings:easeofaccess-magnifier # Ativar lupa (usar com Win + e Win -)
ms-settings:easeofaccess-mouse     # Mover ponteiro do mouse com teclado numérico
ms-settings:magnifier              # Configurações da lupa (versões anteriores)
ms-settings:easeofaccess-narrator  # Configurações do Narrator
```

### Aplicativos e Recursos
```cmd
ms-settings:typing                 # Digitação (autocorreção e destaque de palavras com erro)
ms-settings:defaultapps            # Definir aplicativo padrão para email, mapas, player de música, etc.
ms-settings:startupapps            # Configurar apps que iniciam automaticamente no login
ms-settings:personalization-start # Configuração limitada do menu Iniciar
ms-settings:storagesense           # Storage Sense: liberar espaço removendo arquivos desnecessários
ms-settings:optionalfeatures       # Adicionar recursos opcionais (Internet Explorer 11, OpenSSH Client, Windows Media Player)
```

### Personalização
```cmd
ms-settings:personalization                # Personalizar o PC (Windows precisa estar ativado)
ms-settings:personalization-background     # Configurar plano de fundo
ms-settings:personalization-colors         # Configurar cores
ms-settings:colors                         # Configurar cores (alias)
ms-settings:fonts                          # Configurar fontes
ms-settings:lockscreen                     # Configurar tela de bloqueio
ms-settings:taskbar                        # Configurar barra de tarefas
ms-settings:themes                         # Configurar temas
```

### Rede e Internet
```cmd
ms-settings:network-airplanemode     # Habilitar/desabilitar modo avião
ms-settings:proximity                # Configurações de proximidade
ms-settings:network-ethernet         # Configurações Ethernet
ms-settings:network-wifi             # Configurações Wi-Fi
ms-settings:network-wifisettings     # Gerenciar redes Wi-Fi conhecidas e novas
ms-settings:network-mobilehotspot    # Configurar hotspot móvel
ms-settings:network-proxy            # Configurações de proxy
```

### Dispositivos
```cmd
ms-settings:connecteddevices         # Mostrar dispositivos conectados (Bluetooth, mouse, teclado, etc.)
ms-settings:printers                 # Configurações de impressoras
ms-settings:mousetouchpad            # Configurações de mouse e touchpad
ms-settings:devices-touchpad         # Configurações específicas do touchpad
ms-settings:pen                      # Configurações de caneta
ms-settings:autoplay                 # Configurações de reprodução automática
ms-settings:usb                      # Configurações USB
```

### Notificações e Som
```cmd
ms-settings:notifications            # Desativar notificações como "dicas e truques"
ms-settings:sound                    # Configurações de som
ms-settings:sound-devices            # Dispositivos de som
ms-settings:apps-volume              # Volume de aplicativos
ms-settings:quiethours               # Configurar horas silenciosas
```

### Energia e Bateria
```cmd
ms-settings:batterysaver             # Configurações de economia de bateria
ms-settings:batterysaver-usagedetails # Detalhes de uso da bateria
ms-settings:batterysaver-settings    # Configurações do economizador de bateria
```

### Telefone
```cmd
ms-settings:mobile-devices           # Dispositivos móveis
ms-settings:mobile-devices-addphone  # Adicionar telefone
ms-settings:mobile-devices-addphone-direct # Adicionar telefone diretamente
```

### Contas
```cmd
ms-settings:yourinfo                 # Suas informações
ms-settings:emailandaccounts         # Email e contas
ms-settings:signinoptions            # Opções de entrada
ms-settings:signinoptions-launchfaceenrollment # Cadastro de reconhecimento facial
ms-settings:signinoptions-launchfingerprintenrollment # Cadastro de impressão digital
ms-settings:signinoptions-launchsecuritykeyenrollment # Cadastro de chave de segurança
ms-settings:workplace                # Configurações do local de trabalho
ms-settings:otherusers               # Outros usuários
ms-settings:sync                     # Sincronização
```

### Hora e Idioma
```cmd
ms-settings:dateandtime              # Data e hora
ms-settings:regionformatting         # Formatação regional
ms-settings:regionlanguage-languageoptions # Opções de idioma
ms-settings:regionlanguage-setdisplaylanguage # Definir idioma de exibição
ms-settings:regionlanguage-adddisplaylanguage # Adicionar idioma de exibição
ms-settings:keyboard                 # Configurações do teclado
ms-settings:speech                   # Configurações de fala
```

### Gaming
```cmd
ms-settings:gaming-gamebar           # Xbox Game Bar
ms-settings:gaming-gamedvr           # Gravação de jogos
ms-settings:gaming-broadcasting      # Transmissão
ms-settings:gaming-gamemode          # Game Mode
ms-settings:gaming-trueplay          # TruePlay
ms-settings:gaming-xboxnetworking    # Rede Xbox
```

### Busca e Cortana
```cmd
ms-settings:search-permissions       # Permissões de busca
ms-settings:cortana-windowssearch    # Busca do Windows
ms-settings:search-moredetails       # Mais detalhes da busca
ms-settings:cortana                  # Configurações da Cortana
ms-settings:cortana-talktocortana    # Falar com a Cortana
ms-settings:cortana-permissions      # Permissões da Cortana
```

### Privacidade
```cmd
ms-settings:privacy                  # Configurações de privacidade
ms-settings:privacy-speech           # Privacidade de fala
ms-settings:privacy-speechtyping     # Privacidade de digitação por voz
ms-settings:privacy-location         # Privacidade de localização
ms-settings:privacy-webcam           # Privacidade da webcam
ms-settings:privacy-microphone       # Privacidade do microfone
ms-settings:privacy-voiceactivation  # Ativação por voz
ms-settings:privacy-notifications    # Privacidade das notificações
ms-settings:privacy-accountinfo      # Informações da conta
ms-settings:privacy-contacts         # Contatos
ms-settings:privacy-calendar         # Calendário
ms-settings:privacy-email            # Email
ms-settings:privacy-backgroundapps   # Aplicativos em segundo plano
ms-settings:privacy-appdiagnostics   # Diagnósticos de aplicativos
ms-settings:privacy-documents        # Documentos
ms-settings:privacy-pictures         # Imagens
```

### Atualização e Segurança
```cmd
ms-settings:windowsupdate            # Windows Update
ms-settings:windowsupdate-action     # Ações do Windows Update
ms-settings:windowsupdate-history    # Histórico de atualizações
ms-settings:windowsupdate-restartoptions # Opções de reinicialização
ms-settings:windowsupdate-options    # Opções do Windows Update
ms-settings:windowsupdate-activehours # Horas ativas
ms-settings:delivery-optimization    # Otimização de entrega
ms-settings:windowsdefender          # Windows Defender
ms-settings:backup                   # Backup
ms-settings:troubleshoot             # Solução de problemas
ms-settings:recovery                 # Recuperação
ms-settings:activation               # Ativação
ms-settings:findmydevice             # Encontrar meu dispositivo
ms-settings:windowsinsider-optin     # Windows Insider
```

### Mixed Reality
```cmd
ms-settings:holographic              # Configurações holográficas
ms-settings:holographic-audio        # Áudio holográfico
ms-settings:privacy-holographic-environment # Privacidade do ambiente holográfico
ms-settings:holographic-headset      # Headset holográfico
ms-settings:holographic-management   # Gerenciamento holográfico
```

### Surface Hub
```cmd
ms-settings:surfacehub-accounts      # Contas do Surface Hub
ms-settings:surfacehub-calling       # Chamadas do Surface Hub
ms-settings:surfacehub-devicemanagenent # Gerenciamento de dispositivos
ms-settings:surfacehub-sessioncleanup # Limpeza de sessão
ms-settings:surfacehub-welcome       # Boas-vindas do Surface Hub
```

## Configurações Adicionais

### Multitarefas e Recursos Avançados
```cmd
ms-settings:about                    # Sobre o sistema
ms-settings:multitasking             # Configurações de multitarefas
ms-settings:nightlight               # Luz noturna
ms-settings:display-advanced         # Configurações avançadas de display
ms-settings:tabletmode               # Modo tablet
ms-settings:project                  # Projetar para este PC
ms-settings:crossdevice              # Experiências entre dispositivos
ms-settings:clipboard                # Área de transferência
ms-settings:remotedesktop            # Área de trabalho remota
ms-settings:deviceencryption         # Criptografia de dispositivo
```

### Armazenamento
```cmd
ms-settings:storagepolicies          # Políticas de armazenamento
ms-settings:savelocations            # Locais de salvamento
```

**Nota:** No Windows 11 (diferente do Windows 10), o comando `ms-settings:easeofaccess` abre a janela principal de configurações de acessibilidade.

## Exemplos de Uso

Para personalizar o PC (requer Windows ativado):
```cmd
c:\> start ms-settings:personalization
c:\> start ms-settings:personalization-background
c:\> start ms-settings:personalization-colors
```

Para configurar rede:
```cmd
c:\> start ms-settings:network-airplanemode
c:\> start ms-settings:network-ethernet
c:\> start ms-settings:network-wifi
```

Para gerenciar aplicativos:
```cmd
c:\> start ms-settings:appsfeatures-app
c:\> start ms-settings:defaultapps
c:\> start ms-settings:startupapps
```