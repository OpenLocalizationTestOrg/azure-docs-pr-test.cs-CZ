---
title: "aaaUse serveru Azure Backup tooback až tooAzure úlohy | Microsoft Docs"
description: "Použít Azure Backup Server tooprotect nebo zálohovat úlohy toohello portálu Azure."
services: backup
documentationcenter: 
author: PVRK
manager: shivamg
editor: 
keywords: "Azure backup server; chránit úlohy; zálohování úloh"
ms.assetid: e7fb1907-9dc1-4ca1-8c61-50423d86540c
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/20/2017
ms.author: masaran;trinadhk;pullabhk;markgal
ms.openlocfilehash: a99b2919ffd44c6133960e3a935038a2bb1281c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="preparing-tooback-up-workloads-using-azure-backup-server"></a>Příprava tooback až úlohy pomocí serveru Azure Backup
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
>
>

Tento článek vysvětluje, jak tooprepare vaše prostředí tooback až úlohy pomocí serveru Azure Backup. Pomocí serveru Azure Backup Server může chránit úlohy aplikace například virtuální počítače Hyper-V, Microsoft SQL Server, SharePoint Server, Microsoft Exchange a klienty systému Windows z jediné konzoly.

> [!NOTE]
> Azure Backup Server nyní chrání virtuální počítače VMware a poskytuje vyšší úroveň zabezpečení možnosti. Nainstalujte produkt hello, jak je popsáno v následující části obsahují; hello Použijte Update 1 a hello nejnovější Azure Backup Agent. toolearn Další informace o zálohování serverů VMware s Azure Backup Server, najdete v článku hello, [tooback pomocí serveru Azure Backup server VMware](backup-azure-backup-server-vmware.md). toolearn o funkcích zabezpečení, najdete v příliš[zabezpečení Azure backup funkce dokumentaci](backup-azure-security-feature.md).
>
>

Můžete taky chránit infrastruktury jako služby (IaaS) zatížení jako virtuální počítače v Azure.

> [!NOTE]
> Azure má dva modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../azure-resource-manager/resource-manager-deployment-model.md). Tento článek obsahuje hello informace a postupy pro obnovení virtuální počítače nasazené pomocí modelu Resource Manager hello.
>
>

Azure Backup Server většinu funkcí zálohování zatížení hello dědí z Data Protection Manager (DPM). V tomto článku odkazy tooDPM dokumentace tooexplain některé hello sdílené funkci. Když Azure Backup Server sdílí velkou část hello stejné funkce jako aplikace DPM. Azure Backup Server není zálohovat tootape ani umožňuje integraci s nástrojem System Center.

## <a name="1-choose-an-installation-platform"></a>1. Zvolte instalační platformy
první krok Hello k hello serveru Azure Backup zprovoznění je tooset systém Windows Server. Váš server může být v Azure nebo místně.

### <a name="using-a-server-in-azure"></a>Pomocí serveru v Azure
Při výběru serveru ke spuštění serveru Azure Backup, doporučuje se, že začínáte s Galerie bitové kopie systému Windows Server 2012 R2 Datacenter. článek Hello [vytvořit svůj první virtuální počítač Windows v hello portál Azure](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), obsahuje návod pro Začínáme s hello doporučuje virtuálního počítače v Azure, i v případě, že jste Azure před nepoužívali. Hello doporučené minimální požadavky by měla být hello serveru virtuálního počítače (VM): A2 standardní dvě jádra a 3.5 GB paměti RAM.

Ochrana zatížení pomocí serveru Azure Backup má mnoho drobné odlišnosti. článek Hello [instalaci aplikace DPM jako virtuální počítač Azure](https://technet.microsoft.com/library/jj852163.aspx), pomáhá popisují tyto drobné odlišnosti. Před nasazením hello počítače, přečtěte si tento článek úplně.

### <a name="using-an-on-premises-server"></a>Pomocí místního serveru
Pokud nechcete, aby toorun hello základní server v Azure, můžete spustit hello server na virtuální počítač Hyper-V, virtuální počítače VMware nebo fyzické hostitele. Hello doporučené minimální požadavky na hardware serveru hello jsou dvě jádra a 4 GB paměti RAM. Hello podporované operační systémy jsou uvedené v následující tabulce hello:

| Operační systém | Platforma | Skladová jednotka (SKU) |
|:--- | --- |:--- |
| Windows Server 2012 R2 a nejnovější aktualizace Service Packu |64bitová verze |Standard, Datacenter, Foundation |
| Windows Server 2012 a nejnovější aktualizace Service Packu |64bitová verze |Datacenter, Foundation, Standard |
| Windows Storage Server 2012 R2 a nejnovější aktualizace Service Packu |64bitová verze |Standard, Workgroup |
| Windows Storage Server 2012 a nejnovější aktualizace Service Packu |64bitová verze |Standard, Workgroup |

Úložiště DPM hello použitím odstranění duplicit Windows serveru můžete duplicity odstranit. Další informace o [aplikace DPM a odstranění duplicit](https://technet.microsoft.com/library/dn891438.aspx) spolupracují při nasazení ve virtuálních počítačích Hyper-V.

> [!NOTE]
> Azure Backup Server je navrženou toorun na vyhrazené samostatném jednoúčelovém serveru. Azure Backup Server nejde nainstalovat na:
> - Počítač se systémem jako řadič domény
> - Počítač, na které hello aplikační Server je nainstalována role
> - Počítač, který je serveru pro správu System Center Operations Manager
> - Počítač, na kterém běží systém Exchange Server
> - Počítač, který je uzlem clusteru

Azure Backup Server tooa doméně počítač připojte. Pokud máte v plánu toomove hello serveru tooa jiné domény, doporučuje se před instalací serveru Azure Backup Server připojení k nové doméně toohello hello serveru. Přesunutí existující Server Azure Backup počítač tooa nové domény po nasazení *nepodporuje*.

## <a name="2-recovery-services-vault"></a>2. Trezor služby Recovery Services
Ať už odesílat data záloh tooAzure nebo udržovat místně, musí hello softwaru tooAzure toobe připojení. toobe konkrétnější, počítač serveru Azure Backup hello vyžaduje toobe zaregistrována trezoru služeb zotavení.

toocreate obnovení služby úložiště:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. V nabídce centra hello, klikněte na tlačítko **Procházet** a v hello seznamu prostředků zadejte **služeb zotavení**. Během zadávání hello seznamu filtrů podle vašeho zadání. Klikněte na **Trezor Recovery Services**.

    ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-azure-microsoft-azure-backup/open-recovery-services-vault.png) <br/>

    Zobrazí se Hello seznamu trezorů služeb zotavení.
3. Na hello **trezory služeb zotavení** nabídky, klikněte na tlačítko **přidat**.

    ![Vytvoření trezoru Recovery Services – krok 2](./media/backup-azure-microsoft-azure-backup/rs-vault-menu.png)

    Služby zotavení Hello otevře se okno trezoru výzvou tooprovide **název**, **předplatné**, **skupiny prostředků**, a **umístění**.

    ![Vytvoření trezoru Recovery Services – krok 5](./media/backup-azure-microsoft-azure-backup/rs-vault-attributes.png)
4. Pro **název**, zadejte popisný název tooidentify hello trezoru. Název Hello musí toobe jedinečný pro hello předplatného Azure. Zadejte název v rozsahu 2 až 50 znaků. Musí začínat písmenem a může obsahovat pouze písmena, číslice a pomlčky.
5. Klikněte na tlačítko **předplatné** toosee hello seznam dostupných předplatných. Pokud si nejste jisti, jaké předplatné toouse, použít výchozí hello (nebo navrhované) předplatné. Více možností je dostupných, jen pokud je váš účet organizace přidružený k více předplatným Azure.
6. Klikněte na tlačítko **skupiny prostředků** toosee hello seznam dostupných skupin prostředků, nebo klikněte na **nový** toocreate novou skupinu prostředků. Úplnější informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md)
7. Klikněte na tlačítko **umístění** tooselect hello zeměpisnou oblast trezoru hello.
8. Klikněte na možnost **Vytvořit**. Může trvat nějakou dobu hello toobe vytvořit trezor služeb zotavení. Sledujte oznámení stavu hello v horním pravém oblasti hello hello portálu.
   Po vytvoření svůj trezor otevře na portálu hello.

### <a name="set-storage-replication"></a>Nastavení replikace úložiště
možnost replikace úložiště Hello umožňuje toochoose mezi geograficky redundantním úložištěm a místně redundantního úložiště. Ve výchozím nastavení má váš trezor nastavené geograficky redundantní úložiště. Pokud se tento trezor je primární trezoru, nechte hello úložiště možnost set toogeo redundantní úložiště. Zvolte místně redundantní úložiště, pokud chcete levnější možnost, která není tak trvanlivá. Další informace o [geograficky redundantní](../storage/common/storage-redundancy.md#geo-redundant-storage) a [místně redundantní](../storage/common/storage-redundancy.md#locally-redundant-storage) možnosti úložiště v hello [Přehled replikace Azure Storage](../storage/common/storage-redundancy.md).

nastavení replikace úložiště tooedit hello:

1. Vyberte řídicí panel trezoru tooopen hello trezoru a okna nastavení hello. Pokud hello **nastavení** neotevře, klikněte na tlačítko **všechna nastavení** na řídicím panelu trezoru hello.
2. Na hello **nastavení** okně klikněte na tlačítko **infrastruktura zálohování** > **konfigurace zálohování** tooopen hello **konfigurace zálohování** okno. Na hello **konfigurace zálohování** okno, zvolte možnost replikace úložiště hello pro svůj trezor.

    ![Seznam trezorů záloh](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

    Po výběru možnosti hello úložiště pro svůj trezor, jsou připravené tooassociate hello virtuálních počítačů s úložištěm hello. přidružení hello toobegin, by měl zjistit a zaregistrujte hello virtuální počítače Azure.

## <a name="3-software-package"></a>3. Balíček softwaru
### <a name="downloading-hello-software-package"></a>Stažení balíčku softwaru hello
1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Pokud již máte otevřete trezoru služeb zotavení, pokračujte toostep 3. Pokud nemáte otevřený trezor služeb zotavení, ale jsou v hello portál Azure, v nabídce centra hello, klikněte na tlačítko **Procházet**.

   * V seznamu hello prostředků, zadejte **služeb zotavení**.
   * Během zadávání hello seznam bude filtrovat podle vašeho zadání. Až uvidíte **Trezory Recovery Services**, klikněte na ně.

     ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-azure-microsoft-azure-backup/open-recovery-services-vault.png)

     Zobrazí se seznam trezorů služeb zotavení Hello.
   * Hello seznamu trezorů služeb zotavení vyberte trezor.

     Otevře se řídicí panel vybraného trezoru Hello.

     ![Otevřené okno trezoru](./media/backup-azure-microsoft-azure-backup/vault-dashboard.png)
3. Hello **nastavení** okno otevře ve výchozím nastavení. Pokud je zavřený, klikněte na **nastavení** okno nastavení tooopen hello.

    ![Otevřené okno trezoru](./media/backup-azure-microsoft-azure-backup/vault-setting.png)
4. Klikněte na tlačítko **zálohování** Průvodce Začínáme tooopen hello.

    ![Zálohování Začínáme](./media/backup-azure-microsoft-azure-backup/getting-started-backup.png)

    V hello **Začínáme se zálohováním** okno, které se otevře, **zálohování cíle** bude automaticky vybrána.

    ![Zálohování cíle – výchozí – otevřít](./media/backup-azure-microsoft-azure-backup/getting-started.png)

5. V hello **cíl zálohování** okno z hello **kde běží vaše úlohy** nabídce vyberte možnost **místní**.

    ![místní a úloh jako cíle](./media/backup-azure-microsoft-azure-backup/backup-goals-azure-backup-server.png)

    Z hello **co chcete toobackup?** rozevírací nabídky, vyberte hello úlohy tooprotect pomocí serveru Azure Backup a pak klikněte na **OK**.

    Hello **Začínáme se zálohováním** Průvodce přepínače hello **připravit infrastrukturu** možnost tooback až tooAzure úlohy.

   > [!NOTE]
   > Pokud chcete pouze tooback soubory a složky, doporučujeme pomocí agenta Azure Backup hello a hello pokynů v článku hello [první pohled: zálohování souborů a složek](backup-try-azure-backup-in-10-mins.md). Pokud chcete tooprotect více než soubory a složky nebo plánování ochrany hello tooexpand musí v budoucnosti hello, vyberte tyto úlohy.
   >
   >

    ![Získávání změnu Průvodce Začínáme](./media/backup-azure-microsoft-azure-backup/getting-started-prep-infra.png)

6. V hello **připravit infrastrukturu** okno otevře, klikněte na tlačítko hello **Stáhnout** odkazy pro instalaci serveru Azure Backup a přihlašovací údaje trezoru stáhnout. Během registrace trezoru služeb zotavení Azure Backup Server toohello používat přihlašovací údaje trezoru hello. Hello odkazů můžete toohello Download Center, kde hello balíček softwaru si můžete stáhnout.

    ![Příprava infrastruktury pro Azure Backup Server](./media/backup-azure-microsoft-azure-backup/azure-backup-server-prep-infra.png)

7. Vyberte všechny soubory hello a klikněte na **Další**. Stažení všech hello soubory brzo ze stránky pro stažení hello Microsoft Azure Backup a místě, které se všechny soubory v hello hello stejné složce.

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
2. Na úvodní obrazovce hello klikněte na tlačítko hello **Další** tlačítko. Tím přejdete toohello *požadovaných součástí kontroluje* části. Na této obrazovce klikněte na tlačítko **zkontrolujte** toodetermine, pokud byly splněny hello hardwarové a softwarové požadavky pro Azure Backup Server. Pokud jsou úspěšně splněny všechny požadavky, zobrazí se zpráva oznamující, že tento počítač hello splňuje požadavky hello. Klikněte na hello **Další** tlačítko.

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

    dalším krokem Hello je tooconfigure hello agenta služeb zotavení Microsoft Azure. Jako součást konfigurace hello, budete mít tooprovide váš trezor pověření tooregister hello toohello obnovení služby počítače trezoru. Bude také zadejte přístupové heslo tooencrypt/dešifrování hello data odesílaná mezi Azure a vaše místní. Může automaticky generovat přístupové heslo nebo zadejte minimální heslo 16 znaků. V hello Průvodce pokračujte, dokud byl hello agent nakonfigurován.

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
Azure Backup Server vyžaduje připojení služby Azure Backup toohello pro hello produktu toowork úspěšně. toovalidate, zda má počítač hello hello tooAzure připojení, používat hello ```Get-DPMCloudConnection``` rutiny v konzole prostředí PowerShell pro Azure Backup Server hello. Pokud hello výstup hello rutiny hodnotu TRUE, pak připojení existuje, jinak je k dispozici žádné připojení.

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
