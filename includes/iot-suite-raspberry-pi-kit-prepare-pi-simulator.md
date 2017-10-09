## <a name="prepare-your-raspberry-pi"></a>Příprava vašeho Malinová platformy

### <a name="install-raspbian"></a>Nainstalujte Raspbian

Pokud je to hello poprvé použijete vaše malin platformy, je nutné tooinstall hello Raspbian operačního systému pomocí NOOBS na kartě SD hello součástí hello kit. Hello [malin pí softwaru průvodce] [ lnk-install-raspbian] popisuje, jak tooinstall operačního systému na vaše malin pí. Tento kurz předpokládá, že jste nainstalovali hello Raspbian operačního systému na vaše malin pí.

> [!NOTE]
> Hello SD karty součástí hello [Microsoft Azure IoT Starter Kit malin pí 3] [ lnk-starter-kits] již NOOBS nainstalována. Můžete spustit hello malin pí z této karty a zvolte tooinstall hello Raspbian operačního systému.

toocomplete hello hardwaru Instalační program, potřebujete:

- Připojte vaše malin pí toohello zdroj napájení součástí hello kit.
- Propojení vaší sítě tooyour malin pí pomocí kabelu Ethernet hello součástí vaší sady. Alternativně můžete nastavit [bezdrátové připojení] [ lnk-pi-wireless] vaše malin pí.

Teď jste dokončili nastavení hardwaru hello vaší malin pí.

### <a name="sign-in-and-access-hello-terminal"></a>Přihlaste se a přístup k Terminálové hello

Dvě možnosti tooaccess máte na vaše platformy malin terminálu prostředí:

- Pokud máte klávesnici a sledování připojených tooyour malin platformy, můžete použít grafické uživatelské rozhraní Raspbian tooaccess hello okno terminálu.

- Přístup hello příkazového řádku na vaší malin pí pomocí protokolu SSH ze stolního počítače.

#### <a name="use-a-terminal-window-in-hello-gui"></a>Použijte okno terminálu v hello grafického uživatelského rozhraní

uživatelské jméno jsou Hello výchozí pověření pro Raspbian **pí** a heslo **malin**. Hello hlavním panelu v hello grafického uživatelského rozhraní, můžete spustit hello **Terminálové** nástroj pomocí hello ikonu, která vypadá jako monitorování.

#### <a name="sign-in-with-ssh"></a>Přihlaste se pomocí protokolu SSH

SSH můžete použít pro přístup přes příkazový řádek tooyour malin pí. článek Hello [SSH (Secure Shell)] [ lnk-pi-ssh] popisuje, jak tooconfigure SSH na vaše malin platformy a jak tooconnect z [Windows] [ lnk-ssh-windows] nebo [Operačního systému Linux & Mac][lnk-ssh-linux].

Přihlaste se pomocí uživatelského jména **pí** a heslo **malin**.

#### <a name="optional-share-a-folder-on-your-raspberry-pi"></a>Volitelné: Sdílené složky na vaše malin platformy

Volitelně můžete tooshare do složky na vaše malin pí s prostředí plochy. Sdílení složky umožňuje vám toouse upřednostňované plochy textový editor (například [Visual Studio Code](https://code.visualstudio.com/) nebo [Sublime Text](http://www.sublimetext.com/)) tooedit soubory na vaše malin platformy místo použití `nano` nebo `vi`.

tooshare složka s Windows, konfigurace serveru Samba hello malin pí. Můžete taky použít integrované hello [SFTP](https://www.raspberrypi.org/documentation/remote-access/) serveru s klientem SFTP na ploše.

[lnk-install-raspbian]: https://www.raspberrypi.org/learning/software-guide/quickstart/
[lnk-pi-wireless]: https://www.raspberrypi.org/documentation/configuration/wireless/README.md
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/