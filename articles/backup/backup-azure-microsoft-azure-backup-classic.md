---
title: "aaaUse serveru Azure Backup tooback až úlohy tooAzure klasického portálu | Microsoft Docs"
description: "Ujistěte se, že vaše prostředí je řádně připraveno tooback až úlohy pomocí serveru Azure Backup"
services: backup
documentationcenter: 
author: pvrk
manager: shivamg
editor: 
keywords: "Azure backup server; úložiště záloh"
ms.assetid: d86450e8-da63-4283-8fd7-2ee629fa14ab
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: masaran;trinadhk;pullabhk;markgal
ms.openlocfilehash: 7b574824c448096e0c0ba74a872ab8f2a434f6a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="preparing-tooback-up-workloads-using-azure-backup-server"></a>Příprava tooback až úlohy pomocí serveru Azure Backup
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
> * [Server Azure Backup (klasické)](backup-azure-microsoft-azure-backup-classic.md)
> * [SCDPM (klasické)](backup-azure-dpm-introduction-classic.md)
>
>

Tento článek je o přípravě vašeho prostředí tooback až úlohy pomocí serveru Azure Backup. Pomocí serveru Azure Backup Server může chránit úlohy aplikace jako virtuální počítače Hyper-V, Microsoft SQL Server, SharePoint Server, Microsoft Exchange a klienti systému Windows z jediné konzoly.

> [!WARNING]
> Azure Backup Server dědí hello funkce Data Protection Manager (DPM) pro úlohu zálohování. Zjistíte ukazatele tooDPM dokumentace pro některé z těchto funkcí. Azure zálohování serveru ale není zajistit ochranu na pásce nebo integrovat s nástrojem System Center.
>
>

## <a name="1-windows-server-machine"></a>1. Počítače Windows serveru
![step1](./media/backup-azure-microsoft-azure-backup/step1.png)

první krok Hello k hello serveru Azure Backup zprovoznění je toohave počítače s Windows serverem.

| Umístění | Minimální požadavky | Další pokyny |
| --- | --- | --- |
| Azure |Virtuální počítač Azure IaaS<br><br>A2 Standardní: 2 jádra, 3.5GB paměti RAM |Můžete začít s jednoduché Galerie bitové kopie systému Windows Server 2012 R2 Datacenter. [Ochrana zatížení IaaS v Azure Backup Server (DPM)](https://technet.microsoft.com/library/jj852163.aspx) má mnoho drobné odlišnosti. Přečtěte si článek hello úplně před nasazením hello počítače. |
| Lokálně |Virtuální počítač technologie Hyper-V,<br> Virtuální počítač VMWare,<br> nebo adresu fyzického hostitele.<br><br>2 jádra a 4GB paměti RAM |Úložiště DPM hello použitím odstranění duplicit Windows serveru můžete duplicity odstranit. Další informace o [aplikace DPM a odstranění duplicit](https://technet.microsoft.com/library/dn891438.aspx) spolupracují při nasazení ve virtuálních počítačích Hyper-V. |

> [!NOTE]
> Doporučuje se nainstalovaný Azure Backup Server na počítači s Windows Server 2012 R2 Datacenter. Mnoho hello požadavky jsou popsané automaticky hello nejnovější verzi operačního systému Windows hello.
>
>

Pokud máte v plánu toojoin serveru Azure Backup tooa domény, doporučuje se připojit hello fyzický server nebo doménu toohello virtuální počítač před instalací softwaru hello serveru Azure Backup. Přesunutí nové doméně tooa serveru Azure Backup po nasazení je *nepodporuje*.

## <a name="2-backup-vault"></a>2. Úložiště záloh
![step2](./media/backup-azure-microsoft-azure-backup/step2.png)

Ať už odesílat data záloh tooAzure nebo udržovat místně, hello Azure Backup Server musí být registrovaný tooa trezoru. Pokud jsou nového uživatele Azure Backup a chcete toouse serveru Azure Backup, přečtěte si téma hello Azure portálu verzi tohoto článku - [Příprava tooback až úlohy pomocí serveru Azure Backup](backup-azure-microsoft-azure-backup.md).

> [!IMPORTANT]
> Od března 2017 se už můžete trezory Backup portálu classic toocreate hello.
> Teď můžete upgradovat vaše trezory služeb tooRecovery trezory Backup. Podrobnosti najdete v tématu hello článku [upgradu tooa trezoru zálohování trezor služeb zotavení](backup-azure-upgrade-backup-to-recovery-services.md). Společnost Microsoft doporučuje tooupgrade zálohování trezory tooRecovery trezory služeb.<br/> Po 15 říjen 2017 nemůžete použít trezory Backup toocreate prostředí PowerShell. **Do 1. listopadu 2017:**
>- Všechny zbývající trezory Backup bude automaticky upgradovaný tooRecovery trezory služeb.
>- Můžete nebudou moct tooaccess zálohovaných dat na portálu classic hello. Místo toho použijte hello Azure portálu tooaccess zálohovaných dat v trezory služeb zotavení.
>



## <a name="3-software-package"></a>3. Balíček softwaru
![ke kroku 3](./media/backup-azure-microsoft-azure-backup/step3.png)

### <a name="downloading-hello-software-package"></a>Stažení balíčku softwaru hello
Podobně jako přihlašovací údaje toovault, si můžete stáhnout Microsoft Azure Backup pro zatížení aplikace z hello **stránky rychlý Start** hello úložiště záloh.

1. Klikněte na tlačítko **pro zatížení aplikace (disku tooDisk tooCloud)**. Zobrazí se stránka Stažení softwaru toohello ze kterých si můžete stáhnout balíček softwaru hello.

    ![Microsoft Azure Backup úvodní obrazovce](./media/backup-azure-microsoft-azure-backup/dpm-venus1.png)
2. Klikněte na **Stáhnout**.

    ![Stažení softwaru 1](./media/backup-azure-microsoft-azure-backup/downloadcenter1.png)
3. Vyberte všechny soubory hello a klikněte na **Další**. Stažení všech hello soubory brzo ze stránky pro stažení hello Microsoft Azure Backup a místě, které se všechny soubory v hello hello stejné složce.
   ![Stažení softwaru 1](./media/backup-azure-microsoft-azure-backup/downloadcenter.png)

    Vzhledem k tomu, že hello stažení velikost všech souborů hello je dohromady > 3G, na odkaz ke stažení 10 MB/s, který může trvat až too60 minut hello stahoval toocomplete.

### <a name="extracting-hello-software-package"></a>Extrahování hello softwarového balíčku
Po stažení všechny soubory hello, klikněte na tlačítko **MicrosoftAzureBackupInstaller.exe**. Tato akce spustí hello **Průvodce instalací aplikace Microsoft Azure Backup** tooextract hello instalační soubory tooa umístění, které jste zadali. Pomocí Průvodce hello nadále pokračovat a klikněte na hello **extrahovat** tlačítko proces extrakce toobegin hello.

> [!WARNING]
> Minimálně 4GB volného místa je požadovaná tooextract hello instalační soubory.
>
>

![Průvodce instalací zálohování Microsoft Azure](./media/backup-azure-microsoft-azure-backup/extract/03.png)

Po dokončení procesu extrakce hello, zkontrolujte hello pole toolaunch hello čerstvě extrahovat *setup.exe* toobegin instalaci serveru Microsoft Azure Backup a klikněte na hello **Dokončit** tlačítko.

### <a name="installing-hello-software-package"></a>Instalace balíčku softwaru hello
1. Klikněte na tlačítko **Microsoft Azure Backup** Průvodce instalací toolaunch hello.

    ![Průvodce instalací zálohování Microsoft Azure](./media/backup-azure-microsoft-azure-backup/launch-screen2.png)
2. Na úvodní obrazovce hello klikněte na tlačítko hello **Další** tlačítko. Tím přejdete toohello *požadovaných součástí kontroluje* části. Na této obrazovce klikněte na hello **zkontrolujte** tlačítko toodetermine, pokud byly splněny hello hardwarové a softwarové požadavky pro Azure Backup Server. Pokud všechny hello, které jsou požadované součásti byly úspěšně splněny, zobrazí se zpráva oznamující, že tento počítač hello splňuje požadavky hello. Klikněte na hello **Další** tlačítko.

    ![Zkontrolujte Server Azure Backup - uvítací a požadavky](./media/backup-azure-microsoft-azure-backup/prereq/prereq-screen2.png)
3. Microsoft Azure Backup Server vyžaduje SQL Server Standard a hello serveru Azure Backup instalační balíček obsahuje připojené hello odpovídající SQL binárních souborů serveru potřeba. Při spouštění s novou instalací serveru Azure Backup, měli byste vybrat možnost hello **nainstalovat nové Instance systému SQL Server s tímto nastavením** a klikněte na tlačítko hello **zkontrolovat a nainstalovat** tlačítko. Jakmile hello požadavky jsou úspěšně nainstalováni, klikněte na možnost **Další**.

    ![Server Azure Backup - kontrola SQL](./media/backup-azure-microsoft-azure-backup/sql/01.png)

    Pokud dojde k selhání s počítačem hello toorestart doporučení, je a klikněte na tlačítko **zkontrolujte znovu**.

   > [!NOTE]
   > Azure Backup Server nebude fungovat se vzdálenou instanci systému SQL Server. instance Hello používá Azure Backup Server musí toobe místní.
   >
   >

4. Zadejte umístění pro instalaci hello souborů serveru Microsoft Azure Backup a klikněte na **Další**.

    ![PreReq2 zálohování Microsoft Azure](./media/backup-azure-microsoft-azure-backup/space-screen.png)

    pomocné umístění Hello je požadavek na zálohování tooAzure. Zkontrolujte, zda pomocné místo hello nejméně 5 % dat hello plánované toobe zálohovat toohello cloudu. U ochrany disku musí samostatné disky toobe nakonfigurované po dokončení instalace hello. Další informace týkající se fondů úložiště najdete v tématu [nakonfigurujte fondy úložiště a disk úložiště](https://technet.microsoft.com/library/hh758075.aspx).
5. Zadejte silné heslo pro omezené místní uživatelské účty a klikněte na **Další**.

    ![PreReq2 zálohování Microsoft Azure](./media/backup-azure-microsoft-azure-backup/security-screen.png)
6. Vyberte, zda má toouse *Microsoft Update* toocheck aktualizace a klikněte na tlačítko **Další**.

   > [!NOTE]
   > Doporučujeme mít přesměrování tooMicrosoft aktualizace, který nabízí zabezpečení a důležité aktualizace pro Windows a další produkty, jako je Microsoft Azure Backup Server služby Windows Update.
   >
   >

    ![PreReq2 zálohování Microsoft Azure](./media/backup-azure-microsoft-azure-backup/update-opt-screen2.png)
7. Zkontrolujte hello *souhrn nastavení* a klikněte na tlačítko **nainstalovat**.

    ![PreReq2 zálohování Microsoft Azure](./media/backup-azure-microsoft-azure-backup/summary-screen.png)
8. instalace Hello se stane ve fázích. V první fázi hello hello agenta služeb zotavení Microsoft Azure je nainstalován na serveru hello. Průvodce Hello také zkontroluje připojení k Internetu. Pokud je k dispozici připojení k Internetu může pokračovat v instalaci, pokud ne, je nutné tooprovide proxy podrobnosti tooconnect toohello Internetu.

    dalším krokem Hello je tooconfigure hello agenta služeb zotavení Microsoft Azure. Jako součást konfigurace hello bude mít tooprovide hello trezoru pověření tooregister hello počítač toohello zálohování trezoru. Bude také zadejte přístupové heslo tooencrypt/dešifrování hello data odesílaná mezi Azure a vaše místní. Může automaticky generovat přístupové heslo nebo zadejte minimální heslo 16 znaků. V hello Průvodce pokračujte, dokud byl hello agent nakonfigurován.

    ![Azure Backup Serer PreReq2](./media/backup-azure-microsoft-azure-backup/mars/04.png)
9. Po úspěšném dokončení registrace Microsoft Azure Backup server hello hello celkové Průvodce instalací pokračuje toohello instalace a konfigurace systému SQL Server a součásti serveru Azure Backup hello. Po dokončení instalace součásti systému SQL Server hello hello serveru Azure Backup součásti jsou nainstalovány.

    ![Server Azure Backup](./media/backup-azure-microsoft-azure-backup/final-install/venus-installation-screen.png)

Po dokončení kroku instalace hello hello produkt ikony na ploše se vytvořily také. Právě dvakrát klikněte na hello ikonu toolaunch hello produktu.

### <a name="add-backup-storage"></a>Přidání úložiště záloh
první záložní kopii Hello se ukládají na úložišti připojeném toohello počítače Azure Backup Server. Další informace o přidávání disků, najdete v tématu [nakonfigurujte fondy úložiště a disk úložiště](https://technet.microsoft.com/library/hh758075.aspx).

> [!NOTE]
> Úložiště zálohování tooadd musíte i v případě, že máte v plánu toosend data tooAzure. V hello aktuální architektura Azure Backup Server, obsahuje úložiště záloh Azure hello hello *druhý* kopii hello data během hello místní úložiště obsahuje hello první (a povinná) záložní kopii.  
>
>

## <a name="4-network-connectivity"></a>4. Připojení k síti
![step4](./media/backup-azure-microsoft-azure-backup/step4.png)

Azure Backup Server vyžaduje připojení služby Azure Backup toohello pro hello produktu toowork úspěšně. toovalidate, zda má počítač hello hello tooAzure připojení, používat hello ```Get-DPMCloudConnection``` příkaz v konzole prostředí PowerShell pro Azure Backup Server hello. Pokud hello výstup hello příkaz hodnotu TRUE, pak připojení existuje, jinak je k dispozici žádné připojení.

V hello musí současně hello předplatného Azure toobe v dobrém stavu. toofind se stav hello vaše předplatné a toomanage ho přihlášení toohello [předplatné portál](https://account.windowsazure.com/Subscriptions).

Jakmile znáte hello stavu hello Azure připojení a hello předplatné Azure, můžete na funkce zálohování a obnovení hello nabízené hello tabulce toofind out hello dopad.

| Stav připojení | předplatné Azure | Zálohování tooAzure | Zálohování toodisk | Obnovení z Azure | Obnovení z disku |
| --- | --- | --- | --- | --- | --- |
| připojení |Aktivní |Povoleno |Povoleno |Povoleno |Povoleno |
| připojení |Platnost |Zastaveno |Zastaveno |Povoleno |Povoleno |
| připojení |Zrušit |Zastaveno |Zastaveno |Body obnovení zastaven a Azure odstranit |Zastaveno |
| Ke ztrátě připojení > 15 dnů |Aktivní |Zastaveno |Zastaveno |Povoleno |Povoleno |
| Ke ztrátě připojení > 15 dnů |Platnost |Zastaveno |Zastaveno |Povoleno |Povoleno |
| Ke ztrátě připojení > 15 dnů |Zrušit |Zastaveno |Zastaveno |Body obnovení zastaven a Azure odstranit |Zastaveno |

### <a name="recovering-from-loss-of-connectivity"></a>Obnovení z ztráty připojení
Pokud máte bránu firewall nebo proxy server, který brání tooAzure přístup, je třeba toowhitelist hello následující domény adresy v profilu brány firewall a proxy hello:

* www.msftncsi.com
* \*.Microsoft.com
* \*.WindowsAzure.com
* \*.microsoftonline.com
* \*.windows.net

Po připojení tooAzure byl počítač serveru Azure Backup obnovené toohello, hello operace, které lze provést určuje hello stav předplatného Azure. výše uvedené tabulce Hello obsahuje podrobnosti o operacích hello povolené po hello je "připojený počítač".

### <a name="handling-subscription-states"></a>Zpracování – stavy předplatného
Je možné tootake z předplatného Azure *platnost vypršela* nebo *Deprovisioned* stavu toohello *Active* stavu. To má ale některé dopad na hello produktu chování při hello stav není *Active*:

* A *Deprovisioned* předplatné ztratí funkce hello dobu, která je zrušit. Na měnící *Active*, je obnovená hello produktu funkce zálohování a obnovení. také možné načíst Hello zálohování dat na místní disk hello, pokud byla u s dobou uchování dostatečně velké. Hello zálohování dat v Azure je však nenahraditelně ztraceny, jakmile hello předplatné zadá hello *Deprovisioned* stavu.
* *Platnost vypršela* předplatné pouze ztratí funkce pro, dokud byly provedeny *Active* znovu. Všechny záloh naplánovaných pro hello období, které hello odběr byl *platnost vypršela* se nespustí.

## <a name="troubleshooting"></a>Řešení potíží
Pokud Microsoft Azure Backup server selže s chybami během fáze instalace hello (nebo zálohování nebo obnovení), přečtěte si téma toothis [dokumentu kódy chyb](https://support.microsoft.com/kb/3041338) Další informace.
Můžete se také podívat příliš[nejčastější dotazy týkající se Azure Backup](backup-azure-backup-faq.md)

## <a name="next-steps"></a>Další kroky
Můžete získat podrobné informace [Příprava prostředí pro aplikaci DPM](https://technet.microsoft.com/library/hh758176.aspx) na webu Microsoft TechNet hello. Také obsahuje informace o podporovaných konfiguracích, na kterých serveru Azure Backup lze nasadit a použít.

Tyto články toogain lépe pochopili, ochrana pracovního vytížení pomocí Microsoft Azure Backup server můžete použít.

* [Zálohování serveru SQL Server](backup-azure-backup-sql.md)
* [Zálohování serveru SharePoint](backup-azure-backup-sharepoint.md)
* [Alternativní server zálohování](backup-azure-alternate-dpm-server.md)
