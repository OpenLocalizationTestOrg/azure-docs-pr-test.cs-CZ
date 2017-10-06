---
title: "Zálohování Azure: Příprava tooback až virtuální počítače | Microsoft Docs"
description: "Ujistěte se, že vaše prostředí je připravený pro zálohování virtuálních počítačů v Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "zálohování; zálohování;"
ms.assetid: e87e8db2-b4d9-40e1-a481-1aa560c03395
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: 5c3a41b5d3bd56e62ca5f207442867913aa99816
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-your-environment-tooback-up-resource-manager-deployed-virtual-machines"></a>Příprava vašeho prostředí tooback až virtuálních počítačů nasazených Resource Managerem
> [!div class="op_single_selector"]
> * [Model Resource Manager](backup-azure-arm-vms-prepare.md)
> * [Klasického modelu](backup-azure-vms-prepare.md)
>
>

Tento článek obsahuje hello kroků při přípravě vašeho prostředí tooback nasazených Resource Managerem virtuální počítač (VM). Hello kroky uvedené v hello postupy používají hello portálu Azure.  

Hello služby zálohování Azure má dva typy trezorů (zálohovat trezory služeb zotavení a trezory) pro ochranu virtuálních počítačů. Trezor záloh chrání virtuální počítače nasazené pomocí modelu nasazení Classic hello. Trezor služeb zotavení chrání **nasazení Classic i Resource Manager nasazené virtuální počítače**. Je nutné použít tooprotect trezoru služeb zotavení virtuálních počítačů nasazených Resource Managerem.

> [!NOTE]
> Azure obsahuje dva modely nasazení pro vytváření a práci s prostředky: [Resource Manager a Classic](../azure-resource-manager/resource-manager-deployment-model.md). V tématu [připravit vaše prostředí tooback zálohu virtuálních počítačích Azure](backup-azure-vms-prepare.md) podrobnosti o práci s klasického nasazení modelu virtuálních počítačů.
>
>

Chcete-li chránit nebo zálohovat nasazených Resource Managerem virtuálního počítače (VM), zkontrolujte, zda že existují tyto požadavky:

* Vytvoření trezoru služeb zotavení (nebo identifikovat existující trezor služeb zotavení) *v hello stejné umístění jako virtuální počítač*.
* Vyberte scénář, definovat zásady zálohování hello a definovat tooprotect položky.
* Zkontrolujte hello instalace agenta virtuálního počítače na virtuálním počítači.
* Zkontrolujte připojení k síti
* Pro virtuální počítače s Linuxem, v případě, že chcete toocustomize zálohování prostředí pro zálohování konzistentní s aplikací postupujte hello [kroky tooconfigure předem snímku a po pořízení snímku skripty](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent)

Pokud znáte tyto podmínky již neexistuje ve vašem prostředí poté pokračovat toohello [zálohování váš článek virtuálních počítačů](backup-azure-vms.md). Potřebujete-li tooset nahoru, nebo zkontrolovat, žádný z těchto požadavků, tento článek vás provede kroky tooprepare hello splnění tohoto požadavku.

##<a name="supported-operating-system-for-backup"></a>Podporovaný operační systém pro zálohování
 * **Linux**: Azure Backup podporuje [seznam distribucí schválených pro Azure](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), kromě základního OS Linux. _Další přineste-vaše – vlastní-Linuxových distribucích také může fungovat, dokud je k dispozici na virtuálním počítači hello hello agent virtuálního počítače a podpora pro Python existuje. Jsme však není neschvaluje těchto distribuce pro zálohování._
 * **Windows Server**: Verze starší než Windows Server 2008 R2 nejsou podporovány.

## <a name="limitations-when-backing-up-and-restoring-a-vm"></a>Omezení při zálohování a obnovení virtuálních počítačů
Než se připravíte prostředí, Vezměte prosím na vědomí hello omezení.

* Zálohování virtuálních počítačů s více než 16 datových disků není podporována.
* Nepodporuje zálohování virtuálních počítačů s daty velikosti disku je větší než 1023GB.
* Zálohování virtuálních počítačů s vyhrazenou IP adresu a žádný definovaný koncový bod není podporována.
* Zálohování virtuálních počítačů, které jsou šifrované pomocí právě BEK není podporováno. Zálohování virtuálních počítačů Linux zašifrovaná pomocí šifrování na LUKS není podporováno.
* Zálohování virtuálních počítačů na škálování se konfigurace souborového serveru se nedoporučuje.
* Zálohovaná data neobsahuje tooVM jednotky připojené síti připojit.
* Nahrazení existujícího virtuálního počítače během obnovení se nepodporuje. Když zkusíte toorestore hello virtuálních počítačů při hello virtuálního počítače existuje, hello operace obnovení se nezdaří.
* Mezi oblastmi zálohování a obnovení nejsou podporovány.
* Můžete zálohovat virtuální počítače ve všech oblastech veřejný Azure (viz hello [kontrolní seznam](https://azure.microsoft.com/regions/#services) z podporovaných oblastí). Pokud hello oblast, kterou hledáte, není podporován dnes, nezobrazí se v rozevíracím seznamu hello při vytváření trezoru.
* Obnovení řadiče domény (DC) virtuálního počítače, který je součástí konfigurace více – řadič domény je možné pouze pomocí prostředí PowerShell. Další informace o [obnovení řadiče domény, řadiče domény služby více](backup-azure-restore-vms.md#restoring-domain-controller-vms).
* Obnovení virtuálních počítačů, které mají hello následující zvláštní síťové konfigurace je podporována pouze pomocí prostředí PowerShell. Virtuální počítače vytvořené pomocí pracovního postupu obnovení hello v hello uživatelského rozhraní nebude mít tyto konfigurace sítě, po dokončení operace obnovení hello. Další, najdete v části toolearn [obnovení virtuálních počítačů s konfigurací speciální síťových](backup-azure-restore-vms.md#restoring-vms-with-special-network-configurations).
  * Virtuální počítače v části Konfigurace služby Vyrovnávání zatížení (interních a externích)
  * Virtuální počítače s více vyhrazené IP adresy
  * Virtuální počítače s více síťových adaptérů

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Vytvoření trezoru Recovery Services pro virtuální počítač
Trezor služeb zotavení je entita, která ukládá hello zálohy a body obnovení, které byly vytvořeny v čase. Hello trezoru služeb zotavení obsahuje také zásady zálohování hello přidružené hello chráněné virtuální počítače.

toocreate obnovení služby úložiště:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. V nabídce centra hello, klikněte na tlačítko **Procházet** a v hello seznamu prostředků zadejte **služeb zotavení**. Během zadávání hello seznam bude filtrovat podle vašeho zadání. Klikněte na **Trezor Recovery Services**.

    ![Klikněte na tlačítko Procházet hello a zadejte služeb zotavení. Až uvidíte služeb zotavení hello trezoru možnost, klikněte na něj tooopen hello trezor služeb zotavení okno.](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

    Zobrazí se Hello seznamu trezorů služeb zotavení.
3. Na hello **trezory služeb zotavení** nabídky, klikněte na tlačítko **přidat**.

    ![Vytvoření trezoru Recovery Services – krok 2](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

    Služby zotavení Hello otevře se okno trezoru výzvou tooprovide **název**, **předplatné**, **skupiny prostředků**, a **umístění**.

    ![Vytvoření trezoru Recovery Services – krok 5](./media/backup-azure-arm-vms-prepare/rs-vault-attributes.png)
4. Pro **název**, zadejte popisný název tooidentify hello trezoru. Název Hello musí toobe jedinečný pro hello předplatného Azure. Zadejte název v rozsahu 2 až 50 znaků. Musí začínat písmenem a může obsahovat pouze písmena, číslice a pomlčky.
5. Klikněte na tlačítko **předplatné** toosee hello seznam dostupných předplatných. Pokud si nejste jisti, jaké předplatné toouse, použít výchozí hello (nebo navrhované) předplatné. Více možností bude dostupných pouze pokud je váš účet organizace přidružený k více předplatným Azure.
6. Klikněte na tlačítko **skupiny prostředků** toosee hello seznam dostupných skupin prostředků, nebo klikněte na **nový** toocreate novou skupinu prostředků. Úplnější informace o skupinách prostředků najdete v článku [Přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md)
7. Klikněte na tlačítko **umístění** tooselect hello zeměpisnou oblast trezoru hello. Hello trezoru **musí** v hello stejné oblasti jako hello virtuálních počítačů, které chcete tooprotect.

   > [!IMPORTANT]
   > Pokud si nejste jistí hello umístění, ve kterém je váš virtuální počítač, přejděte toohello seznam virtuálních počítačů hello portálu a zavřete dialogové okno Vytvoření trezoru hello. Pokud máte virtuální počítače v několika oblastech, budete potřebovat toocreate trezoru služeb zotavení v každé oblasti. Vytvořte trezor hello v první oblasti hello před přechodem toohello další umístění. Neexistuje žádné účty úložiště toospecify nutné toostore hello zálohovaných dat – trezor služeb zotavení hello a hello služba Azure Backup se o to postarají automaticky.
   >
   >

8. Klikněte na možnost **Vytvořit**. Může trvat nějakou dobu hello toobe vytvořit trezor služeb zotavení. Sledujte oznámení stavu hello v horním pravém oblasti hello hello portálu. Po vytvoření svůj trezor se zobrazí v seznamu hello trezorů služeb zotavení. Pokud svůj trezor nevidíte, klikněte na tlačítko **aktualizovat** na

    ![Seznam trezorů záloh](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

    Teď, když jste vytvořili trezor, zjistěte, jak tooset hello replikace úložiště.

## <a name="set-storage-replication"></a>Nastavení replikace úložiště
možnost replikace úložiště Hello umožňuje toochoose mezi geograficky redundantním úložištěm a místně redundantního úložiště. Ve výchozím nastavení má váš trezor nastavené geograficky redundantní úložiště. Ponechte hello možnost set toogeo redundantní úložiště, pokud se jedná o vaši primární zálohu. Zvolte místně redundantní úložiště, pokud chcete levnější možnost, která není tak trvanlivá.

nastavení replikace úložiště tooedit hello:

1. Na hello **trezory služeb zotavení** okně vyberte svůj trezor.
    Po kliknutí na tlačítko trezoru, hello okno nastavení (*jehož hello název trezoru hello v horní části hello*) a otevře se okno Podrobnosti hello trezoru.

    ![Vyberte svůj trezor hello seznamu trezorů záloh](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)

2. Na hello **nastavení** okně použití hello svislé posuvníku tooscroll dolů toohello **spravovat** části. Klikněte na tlačítko **infrastruktura zálohování** tooopen její okno. V hello **Obecné** klikněte na část **konfigurace zálohování** tooopen její okno. Na hello **konfigurace zálohování** okno, zvolte možnost replikace úložiště hello pro svůj trezor. Ve výchozím nastavení má váš trezor nastavené geograficky redundantní úložiště. Pokud změníte typ replikace úložiště hello, klikněte na tlačítko **Uložit**.

    ![Seznam trezorů záloh](./media/backup-azure-arm-vms-prepare/full-blade.png)

     Pokud používáte Azure jako primární koncový bod úložiště záloh, pokračujte v používání geograficky redundantního úložiště. Pokud používáte Azure jako koncový bod úložiště záloh není primární, zvolte místně redundantní úložiště. Další informace o [geograficky redundantní](../storage/common/storage-redundancy.md#geo-redundant-storage) a [místně redundantní](../storage/common/storage-redundancy.md#locally-redundant-storage) možnosti úložiště v hello [Přehled replikace Azure Storage](../storage/common/storage-redundancy.md).
    Po výběru možnosti hello úložiště pro svůj trezor, jsou připravené tooassociate hello virtuálních počítačů s úložištěm hello. přidružení hello toobegin, by měl zjistit a zaregistrujte hello virtuální počítače Azure.

## <a name="select-a-backup-goal-set-policy-and-define-items-tooprotect"></a>Výběr cíle zálohování, nastavení zásad a definovat tooprotect položky
Před registrací virtuálních počítačů k trezoru, spusťte tooensure proces zjišťování hello, jsou identifikované všechny nové virtuální počítače, které byly přidány toohello předplatné. Hello proces se dotáže Azure hello seznam virtuálních počítačů v rámci předplatného hello, společně s dalšími informacemi, jako třeba název hello cloudové služby a oblast hello. V hello portálu Azure výraz scénář vyjadřuje toowhat budete tooput do trezoru služeb zotavení hello. Zásada je plán hello jak často a kdy jsou pořizovány body obnovení. Zásada také obsahuje hello rozsah uchování pro body obnovení hello.

1. Pokud již máte otevřete trezoru služeb zotavení, pokračujte toostep 2. Pokud nemáte trezoru služeb zotavení otevřít, otevřete hello [portál Azure](https://portal.azure.com/) a v nabídce centra hello, klikněte na tlačítko **další služby**.

   * V seznamu hello prostředků, zadejte **služeb zotavení**.
   * Během zadávání hello seznam bude filtrovat podle vašeho zadání. Až uvidíte **Trezory Recovery Services**, klikněte na ně.

     ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

     Zobrazí se seznam trezorů služeb zotavení Hello. Pokud ve vašem předplatném nejsou žádné trezory, bude tento seznam prázdný.

    ![Zobrazení hello služeb zotavení trezory seznamu](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

   * Hello seznamu trezorů služeb zotavení vyberte trezor tooopen jeho řídicího panelu.

     okno nastavení Hello a hello řídícím panelu trezoru hello vybrali, otevře se okno trezoru.

     ![Otevřené okno trezoru](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)
2. V nabídce řídícího panelu trezoru hello klikněte na tlačítko **zálohování** okno zálohování tooopen hello.

    ![Otevřené okno Zálohování](./media/backup-azure-arm-vms-prepare/backup-button.png)

    otevření okna zálohování a cíl zálohování Hello.

    ![Otevřené okno Scénář](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

3. V okně cíl zálohování hello nastavte **kde běží vaše úlohy** tooAzure a **co chcete toobackup** tooVirtual počítač a potom klikněte na **OK**.

    Takovém postupu zaregistruje hello rozšíření virtuálního počítače s úložištěm hello. Hello zavře se okno cíl zálohování a hello **zálohování zásad** otevře se okno.

    ![Otevřené okno Scénář](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)
4. V okně zásady zálohování hello vyberte zásady zálohování hello chcete tooapply toohello trezoru.

    ![Výběr zásady zálohování](./media/backup-azure-arm-vms-prepare/setting-rs-backup-policy-new.png)

    v rozevírací nabídce hello jsou uvedeny podrobnosti Hello hello výchozích zásad. Pokud chcete, aby toocreate novou zásadu, vyberte **vytvořit nový** z rozevírací nabídky hello. Pokyny k definování zásad zálohování naleznete v tématu [Definování zásad zálohování](backup-azure-vms-first-look-arm.md#defining-a-backup-policy).
    Klikněte na tlačítko **OK** tooassociate zásady zálohování hello hello trezoru.

    Dobrý den, zavře okno zásady zálohování a hello **vybrat virtuální počítače** otevře se okno.
5. V hello **vybrat virtuální počítače** okně zvolte hello tooassociate virtuální počítače s hello zadané zásady a klikněte na tlačítko **OK**.

    ![Výběr úlohy](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

    Hello vybraný virtuální počítač byl ověřen. Pokud nevidíte měla toosee hello virtuální počítače, zkontrolujte, že existují ve stejné oblasti Azure jako hello trezoru služeb zotavení a ještě nejsou chráněné v jiné trezoru hello. umístění Hello hello trezor služeb zotavení se zobrazí na panelu trezoru hello.

6. Teď, když jste definovali všechna nastavení trezoru hello, v hello okno zálohování klikněte na **povolit zálohování**. To nasadí hello zásad toohello trezor a virtuální počítače hello. To nevytváří hello počátečního bodu obnovení pro virtuální počítač hello.

    ![Povolení zálohování](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Po povolení úspěšně hello zálohování, zásady zálohování spustí podle plánu. Pokud teď chcete toogenerate tooback úlohu zálohování na vyžádání až hello virtuální počítače, přečtěte si téma [úlohy zálohování hello Triggering](./backup-azure-arm-vms.md#triggering-the-backup-job).

Pokud máte potíže s registrací hello virtuálního počítače, přečtěte si téma hello následující informace o instalaci hello agenta virtuálního počítače a na připojení k síti. Pravděpodobně ani nepotřebujete hello následující informace, pokud chráníte virtuální počítače vytvořené v Azure. Ale pokud jste migrovali virtuální počítače do Azure, pak Ujistěte se, že jste správně nainstalovali hello agenta virtuálního počítače a virtuální počítač může komunikovat se službou hello virtuální sítě.

## <a name="install-hello-vm-agent-on-hello-virtual-machine"></a>Nainstalujte agenta virtuálního počítače hello hello virtuálního počítače
Hello agenta virtuálního počítače Azure musí být nainstalován na hello virtuálního počítače, které jsou pro toowork rozšíření hello zálohování Azure. Pokud virtuální počítač byl vytvořen z Galerie Azure hello, pak hello agenta virtuálního počítače již existuje v hello virtuálního počítače. Tyto informace jsou poskytovány hello situacích, kde se *není* pomocí virtuální počítač vytvořen z Galerie Azure hello – třeba migrovat virtuální počítač z překážek místní datacentra. V takovém případě musí hello agenta virtuálního počítače toobe nainstalovaný pořadí tooprotect hello virtuálního počítače. Další informace o hello [agenta virtuálního počítače](../virtual-machines/windows/classic/agents-and-extensions.md#azure-vm-agents-for-windows-and-linux).

Pokud máte problémy se zálohováním hello virtuálního počítače Azure, zkontrolujte, zda text hello agenta virtuálního počítače Azure je správně nainstalovaná hello virtuálního počítače (viz následující tabulka hello). Hello následující tabulka obsahuje další informace o hello virtuální počítač agenta pro Windows a virtuální počítače s Linuxem.

| **Operace** | **Windows** | **Linux** |
| --- | --- | --- |
| Instalace hello agenta virtuálního počítače |Stáhněte a nainstalujte hello [MSI agenta](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Budete potřebovat instalaci hello toocomplete oprávnění správce. |<li> Nainstalujte nejnovější hello [agenta systému Linux](../virtual-machines/linux/agent-user-guide.md). Budete potřebovat instalaci hello toocomplete oprávnění správce. Doporučujeme nainstalovat agenta z distribuční úložiště. Jsme **nedoporučujeme** instalaci agenta virtuálního počítače s Linuxem přímo z githubu.  |
| Aktualizace hello agenta virtuálního počítače |Hello aktualizace agenta virtuálního počítače je stejně jednoduché jako přeinstalování hello [binárních souborů agenta virtuálního počítače](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). <br>Ujistěte se, že během aktualizace agenta virtuálního počítače hello neběží žádná operace zálohování. |Postupujte podle pokynů hello na [aktualizace agenta virtuálního počítače Linux hello](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Doporučujeme aktualizovat agenta z distribuční úložiště. Jsme **nedoporučujeme** aktualizace agenta virtuálního počítače s Linuxem přímo z githubu.<br>Ujistěte se, že při hello Probíhá aktualizace agenta virtuálního počítače neběží žádná operace zálohování. |
| Ověření instalace agenta virtuálního počítače hello |<li>Přejděte toohello *C:\WindowsAzure\Packages* složky v hello virtuálního počítače Azure. <li>Byste měli najít přítomný soubor WaAppAgent.exe hello.<li> Klikněte pravým tlačítkem na soubor hello, přejděte příliš**vlastnosti**a potom vyberte hello **podrobnosti** kartě hello verze produktu pole by mělo být 2.6.1198.718 nebo vyšší. |Není k dispozici |

### <a name="backup-extension"></a>Rozšíření zálohování
Jednou hello agenta virtuálního počítače je nainstalována na virtuálním počítači hello, hello služba Azure Backup nainstaluje hello rozšíření zálohování toohello agenta virtuálního počítače. Hello služba Azure Backup bezproblémově upgraduje a opravy Rozšíření zálohování hello.

Rozšíření zálohování Hello je nainstalována služba zálohování hello zda hello virtuálních počítačů se systémem. Spuštění virtuálního počítače poskytuje hello největší šanci získání bod obnovení konzistentních s aplikací. Hello služby Azure Backup však pokračuje tooback až hello virtuálních počítačů, i když je vypnutý a rozšíření hello nebylo možné nainstalovat. To se označuje jako Virtuální počítač v režimu offline. V takovém případě bude bod obnovení hello *zhroutí konzistentní*.

## <a name="network-connectivity"></a>Připojení k síti
Snímky virtuálních počítačů hello pořadí toomanage, musí rozšíření zálohování hello toohello připojení k Azure veřejné IP adresy. Bez hello správné připojení k Internetu, hello virtuálního počítače HTTP žádosti o vypršení časového limitu a hello selže celá operace zálohování. Pokud vaše nasazení má omezení přístupu na místě (prostřednictvím skupinu zabezpečení sítě (NSG), např.), zvolte jednu z těchto možností pro poskytování zrušte cestu pro provoz zálohování:

* [Povolit rozšíření rozsahy IP adres Azure datacenter hello](http://www.microsoft.com/en-us/download/details.aspx?id=41653) -najdete v článku hello pokyny na tom, jak toowhitelist hello IP adres.
* Nasazení proxy server HTTP pro směrování provozu.

Při rozhodování, kterou možnost toouse, jsou hello kompromis mezi spravovatelnosti, přesná kontrola a náklady.

| Možnost | Výhody | Nevýhody |
| --- | --- | --- |
| Rozsahy povolených IP adres |Žádné dodatečné poplatky.<br><br>Pro přístup k otevření v skupinu NSG, použijte hello <i>Set-AzureNetworkSecurityRule</i> rutiny. |Komplexní toomanage jako hello dopad IP rozsahy změn v průběhu času.<br><br>Poskytuje přístup k celé toohello Azure a ne jenom úložiště. |
| Server proxy protokolu HTTP |Přesná kontrola proxy serveru hello nad úložištěm hello povolené adresy URL.<br>Jeden bod Internet tooVMs přístup.<br>Není subjektu tooAzure změny IP adresy. |Další náklady pro spuštění virtuálního počítače se hello proxy softwaru. |

### <a name="whitelist-hello-azure-datacenter-ip-ranges"></a>Povolit hello datové centrum Azure, rozsahy IP adres
rozsazích IP adres toowhitelist hello datové centrum Azure, najdete v tématu hello [webu Azure](http://www.microsoft.com/en-us/download/details.aspx?id=41653) podrobnosti o hello rozsahy IP adres a pokyny.

### <a name="using-an-http-proxy-for-vm-backups"></a>Pomocí proxy serveru HTTP pro zálohování virtuálních počítačů
Při zálohování virtuálních počítačů, rozšíření zálohování hello na hello virtuální počítač odešle hello snímku správu příkazy tooAzure Storage pomocí rozhraní API HTTPS. Směrování provozu rozšíření zálohování hello přes proxy server hello HTTP vzhledem k tomu, že je nakonfigurovaná pro přístup k toohello jedinou komponentou hello veřejného Internetu.

> [!NOTE]
> Neexistuje žádná doporučení pro hello proxy software, který se má použít. Ujistěte se, že vyberete proxy server, který je kompatibilní s hello provedením následujících kroků konfigurace.
>
>

Následující obrázek příkladu Hello ukazuje hello tři konfigurační kroky potřebné toouse HTTP proxy:

* Virtuální počítač aplikace tras, které veškerý provoz protokolu HTTP vázaný hello veřejného Internetu prostřednictvím proxy serveru virtuálního počítače.
* Proxy virtuálních počítačů umožňuje příchozí provoz z virtuálních počítačů ve virtuální síti hello.
* Hello skupina zabezpečení sítě (NSG) s názvem NFP uzamčení musí zabezpečení pravidlo povolení odchozí internetové přenosy z virtuálního počítače proxy serveru.

![Skupina NSG s diagram nasazení proxy serveru HTTP](./media/backup-azure-vms-prepare/nsg-with-http-proxy.png)

toouse HTTP proxy toocommunicating toohello veřejný Internet, postupujte takto:

#### <a name="step-1-configure-outgoing-network-connections"></a>Krok 1. Konfigurace odchozích síťových připojení
###### <a name="for-windows-machines"></a>Pro počítače s Windows
To se nastavit konfiguraci proxy serveru pro místní systémový účet.

1. Stáhněte si [nástroje PsExec](https://technet.microsoft.com/sysinternals/bb897553)
2. Z řádku se zvýšenými oprávněními spusťte následující příkaz

     ```
     psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"
     ```
     Otevře se okno internet Exploreru.
3. Přejděte tooTools -> Možnosti Internetu -> připojení -> Nastavení místní sítě.
4. Ověřte nastavení proxy serveru pro účet System. Nastavení proxy serveru IP adresy a portu.
5. Zavřete Internet Explorer.

To se nastavit konfiguraci proxy celého systému a se použije pro všechny odchozí přenosy HTTP/HTTPS.

Pokud jste nastavili proxy server na aktuální uživatelský účet (ne místní účet systému), použijte hello následující skript tooapply je tooSYSTEMACCOUNT:

```
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver
```

> [!NOTE]
> Pokud zjistíte v protokolu serveru proxy "(407) vyžadováno ověření proxy serveru", zkontrolujte, že ověřování je správně nastavená.
>
>

###### <a name="for-linux-machines"></a>Pro počítače se systémem Linux
Přidejte následující řádek toohello hello ```/etc/environment``` souboru:

```
http_proxy=http://<proxy IP>:<proxy port>
```

Přidejte následující řádky toohello hello ```/etc/waagent.conf``` souboru:

```
HttpProxy.Host=<proxy IP>
HttpProxy.Port=<proxy port>
```

#### <a name="step-2-allow-incoming-connections-on-hello-proxy-server"></a>Krok 2. Povolit příchozí připojení na hello proxy serveru:
1. Na serveru proxy hello otevřete bránu Windows Firewall. Hello nejjednodušší způsob, jak tooaccess hello je brána firewall toosearch pro bránu Windows Firewall s pokročilým zabezpečením.

    ![Otevřete hello brány Firewall](./media/backup-azure-vms-prepare/firewall-01.png)
2. V dialogovém okně hello brány Windows Firewall, klikněte pravým tlačítkem na **příchozí pravidla** a klikněte na tlačítko **nové pravidlo...** .

    ![Vytvořit nové pravidlo](./media/backup-azure-vms-prepare/firewall-02.png)
3. V hello **pravidla Průvodce vytvořením nového příchozího**, zvolte hello **vlastní** možnost pro hello **typ pravidla** a klikněte na tlačítko **Další**.
4. Na hello tooselect stránku hello **programu**, zvolte **všechny programy** a klikněte na tlačítko **Další**.
5. Na hello **protokol a porty** , zadejte hello následující informace a klikněte na tlačítko **Další**:

    ![Vytvořit nové pravidlo](./media/backup-azure-vms-prepare/firewall-03.png)

   * pro *protokolu typ* zvolte *TCP*
   * pro *místního portu* zvolte *specifické porty*, hello následujícího pole zadejte hello ```<Proxy Port>``` který byl nakonfigurovaný.
   * pro *vzdálených portů* vyberte *všechny porty*

     Pro hello zbytek hello průvodce klikněte na možnost všechny hello způsob toohello end a pojmenujte toto pravidlo.

#### <a name="step-3-add-an-exception-rule-toohello-nsg"></a>Krok 3. Přidejte toohello výjimky pravidla NSG:
V příkazovém řádku prostředí Azure PowerShell zadejte následující příkaz hello:

Hello následující příkaz přidá k výjimce toohello NSG. Tato výjimka umožňuje přenos TCP z jakéhokoli portu na 10.0.0.5 tooany internetové adresy na portu 80 (HTTP) nebo 443 (HTTPS). Pokud budete potřebovat specifického portu v hello veřejný Internet, že tooadd, který port toohello být ```-DestinationPortRange``` také.

```
Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
```


*Tyto kroky používají konkrétní názvy a hodnoty v tomto příkladu. Použijte prosím hello názvy a hodnoty pro nasazení, při zadávání, nebo vyjímání a vkládání podrobnosti do vašeho kódu.*

Teď, když víte, že máte síťové připojení, jsou připravené tooback provozu virtuálního počítače. V tématu [zálohování virtuálních počítačů nasazených Resource Managerem](backup-azure-arm-vms.md).

## <a name="questions"></a>Máte dotazy?
Pokud máte otázky, nebo pokud se některé funkce, které byste chtěli toosee zahrnuty, [pošlete nám svůj názor](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Další kroky
Teď připravíte prostředí pro zálohování virtuálního počítače, je dalším krokem logické toocreate zálohu. Hello plánování článek poskytuje podrobnější informace o zálohování virtuálních počítačů.

* [Zálohování virtuálních počítačů](backup-azure-vms.md)
* [Plánování vaší infrastruktury zálohování virtuálních počítačů](backup-azure-vms-introduction.md)
* [Správa záloh virtuálních počítačů](backup-azure-manage-vms.md)
