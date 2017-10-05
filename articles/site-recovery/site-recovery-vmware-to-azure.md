---
title: "Replikace virtuálních počítačů VMware do Azure | Microsoft Docs"
description: "Shrnuje kroky, které potřebujete pro replikaci úlohy běžící na virtuálních počítačích VMware do úložiště Azure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ROBOTS: NOINDEX, NOFOLLOW
redirect_url: vmware-walkthrough-overview
ms.openlocfilehash: 8a312f3c1a65b2a9bb91abbfd8b18fb0ec0279b7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-vmware-virtual-machines-to-azure-with-site-recovery"></a>Replikace virtuálních počítačů VMware do Azure pomocí Site Recovery

> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmware-to-azure.md)
> * [Azure Classic](site-recovery-vmware-to-azure-classic.md)


Tento článek popisuje, jak replikovat na lokální virtuální počítače VMware do Azure, pomocí [Azure Site Recovery](site-recovery-overview.md) službu na portálu Azure.

Pokud chcete provést migraci virtuálních počítačů VMware do Azure (převzetí služeb při selhání pouze), přečtěte si [v tomto článku](site-recovery-migrate-to-azure.md) Další informace.

POST dotazy a na konci tohoto článku nebo na [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="deployment-steps"></a>Kroky nasazení

Zde je, co musíte udělat:

1. Ověřte požadavky a omezení.
2. Nastavte síť Azure a účty úložiště.
3. Příprava na místním počítači, který chcete nasadit jako konfigurační server.
4. Připravte VMware účty používané pro automatické zjišťování virtuálních počítačů a volitelně pro nabízená instalace služby Mobility.
4. Vytvořte trezor služby Recovery Services. Trezor obsahuje konfigurační nastavení a orchestruje replikaci.
5. Zadejte nastavení zdroje, cíl a replikace.
6. Nasazení služby Mobility na virtuálních počítačích, které chcete replikovat.
7. Povolte replikaci pro virtuální počítače.
7. Otestujete převzetí služeb při selhání, abyste měli jistotu, že všechno funguje, jak má.

## <a name="prerequisites"></a>Požadavky

**Požadavek na podporu** | **Podrobnosti**
--- | ---
**Azure** | Další informace o [požadavky pro Azure](site-recovery-prereq.md#azure-requirements)
**Lokální konfigurační server** | Je třeba virtuální počítače VMware s Windows serverem 2012 R2 nebo novější. Nastavit tento server během nasazování Site Recovery.<br/><br/> Ve výchozím nastavení jsou procesový server a hlavní cílový server také nainstalované na tomto virtuálním počítači. Pokud jste vertikální navýšení kapacity, možná budete muset samostatný procesový server a má stejné požadavky jako konfigurační server.<br/><br/> Další informace o těchto součástí [sem](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements)
**Na místní servery VMware** | Jeden nebo více VMware vSphere serverů, 6.0, 5.5, 5.1 s nejnovější aktualizace. Servery musí nacházet ve stejné síti jako konfigurační server (nebo samostatný procesový server).<br/><br/> Doporučujeme, abyste server vCenter pro správu hostitele, kde běží 6.0 nebo 5.5 s nejnovějšími aktualizacemi. Při nasazení verze 6.0 jsou podporovány pouze funkce, které jsou k dispozici v 5.5.
**Místní virtuální počítače** | Na virtuálních počítačích, které chcete replikovat, musí běžet [podporovaný operační systém](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) a počítače musí splňovat [požadavky na Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements). Virtuální počítač by měl mít spuštění nástroje VMware.
**Adresy URL** | Konfigurační server potřebuje přístup k tyto adresy URL:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Pokud máte zavedená pravidla brány firewall založená na IP adrese, zkontrolujte, že tato pravidla umožňují komunikaci s Azure.<br/></br> Povolte [Rozsahy IP adres datového centra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) a port HTTPS (443).<br/></br> Povolte rozsahy IP adres pro oblast Azure svého předplatného a pro oblast Západní USA (používá se pro řízení přístupu a správu identit).<br/><br/> Povolit tuto adresu URL pro stažení MySQL: http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi.
**Služba mobility** | Nainstalovat na každý replikovaných virtuálních počítačů.




## <a name="limitations"></a>Omezení

**Omezení** | **Podrobnosti**
--- | ---
**Azure** | Účty úložiště a sítě musí být ve stejné oblasti jako trezor<br/><br/> Pokud používáte účet úložiště premium, potřebujete účet standardního úložiště pro ukládání protokolů replikace<br/><br/> Nelze se replikují na prémiové účty ve střední a – Jih, Indie.
**Lokální konfigurační server** | Typ adaptéru virtuálního počítače VMware musí být VMXNET3. Pokud tomu tak není, [tuto aktualizaci nainstalovat](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1)<br/><br/> vSphere PowerCLI 6.0 by měly být nainstalovány.<br/><br> Tento počítač by neměl být řadič domény. Tento počítač by měl mít statickou IP adresu.<br/><br/> Název hostitele musí být maximálně 15 znaků nebo méně, a operační systém by měl být v angličtině.
**VMware** | Jsou podporovány pouze verzi 5.5 funkce v systému vCenter 6.0. Site Recovery nepodporuje nové vCenter a vSphere 6.0 funkce, jako křížové řešení vMotion vCenter, virtuální svazky a úložiště DRS.
**Virtuální počítače** | Ověřte [omezení virtuálního počítače Azure](site-recovery-prereq.md#azure-requirements)<br/><br/> Nelze replikovat virtuální počítače s šifrované disky nebo virtuální počítače pomocí rozhraní UEFI nebo EFI.<br/><br> Sdílený disk, clustery nejsou podporované. Pokud virtuální počítač má zdroj seskupování síťových adaptérů, jsou převedeny na jednu síťovou kartu po převzetí služeb při selhání.<br/><br/> Pokud virtuální počítače iSCSI disk, Site Recovery převede jej na soubor virtuálního pevného disku po převzetí služeb při selhání. Pokud cíle iSCSI, dosažitelný z virtuálního počítače Azure, se k němu připojuje a zobrazí se i virtuální pevný disk. Pokud k tomu dojde, odpojte cíle iSCSI.<br/><br/> Pokud chcete povolit konzistence více virtuálních počítačů, která umožňuje virtuální počítače běží stejné zatížení dohromady obnovit konzistentní datový bod, otevřete port 20004 ve virtuálním počítači.<br/><br/> Windows musí být nainstalován na jednotce C. Disk operačního systému musí být základní a nikoli dynamické. Datový disk může být dynamické.<br/><br/> Soubory Linux/etc/hosts na virtuálních počítačích by měly obsahovat položky, které mapování názvu místního hostitele na IP adresy přidružené všechny síťové adaptéry. Název hostitele, přípojné body, název zařízení, systémové cesty a názvy souborů (/ etc; USR) by měly být pouze v angličtině.<br/><br/> Konkrétní typy [Linux úložiště](site-recovery-support-matrix-to-azure.md#support-for-storage) jsou podporovány.<br/><br/>Vytvořit nebo nastavit **disk.enableUUID=true** v nastavení virtuálního počítače. To poskytuje konzistentní UUID VMDK, tak, aby správně připojí a zajišťuje, že jenom rozdílové změny jsou přenášeny zpět na místní během navrácení služeb po obnovení bez úplná replikace.

## <a name="set-up-azure"></a>Nastavení Azure

1. [Nastavit síť Azure](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).
    - Virtuální počítače Azure budou umístěny v této síti, když jste vytvořili po převzetí služeb při selhání.
    - Můžete nastavit síť v [Resource Manager](../resource-manager-deployment-model.md), nebo v klasickém režimu.

2. Nastavení [účtu úložiště Azure](../storage/storage-create-storage-account.md#create-a-storage-account) pro replikovaná data.
    - Účet může být standardní nebo [premium](../storage/storage-premium-storage.md).
    - Můžete nastavit účet ve službě Správce prostředků, nebo v klasickém režimu.

3. [Příprava účet](#prepare-for-automatic-discovery-and-push-installation) vCenter server nebo hostitelů vSphere, tak obnovení lokality může automaticky zjistit virtuální počítače VMware.

## <a name="prepare-the-configuration-server"></a>Příprava na konfiguračním serveru

1. Nainstalujte Windows Server 2012 R2 nebo novější, na virtuální počítače VMware.
2. Zajistěte, aby měl virtuální počítač přístup k adresám URL uvedené v [požadavky](#prerequisites).
3. Nainstalujte [VMware vSphere PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1).


## <a name="prepare-for-automatic-discovery-and-push-installation"></a>Příprava pro automatické zjišťování a vynucená instalace

- **Příprava účtu pro automatické zjišťování**: Site Recovery procesový server automaticky vyhledá virtuální počítače. K tomuto účelu musí Site Recovery přihlašovací údaje, které mají přístup k servery vCenter server a hostitelů vSphere ESXi.

    1. Chcete-li použít vyhrazený účet, vytvořte roli (na úrovni vCenter, pomocí těchto [oprávnění](#vmware-account-permissions). Pojmenujte ji jako **Azure_Site_Recovery**.
    2. Potom vytvořte uživatele na serveru hostitele/vCenter vSphere a přiřadit role uživatele. Tento uživatelský účet je zadat během nasazování Site Recovery.

- **Příprava účet k nabízení služby Mobility**: Pokud chcete k nabízení služby Mobility na virtuální počítače, potřebujete účet, který lze použít procesní server přístup k virtuálnímu počítači. Účet se používá pouze pro nabízené instalace. Můžete použít domény nebo místní účet:

    - Pro systém Windows Pokud nepoužíváte účet domény, je nutné zakázat řízení vzdáleného přístupu uživatele v místním počítači. Chcete-li to provést, v registru podle **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, přidejte položku DWORD **LocalAccountTokenFilterPolicy**, s hodnotou 1.
    - Pokud chcete přidat položku registru pro Windows z rozhraní příkazového řádku, zadejte:``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
    - Pro systémy Linux musí být účet root na zdrojovém serveru Linux.

## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-the-protection-goal"></a>Výběr cíle ochrany

Vyberte, jak chcete počítače replikovat a kam je chcete replikovat.

1. Klikněte na tlačítko **trezory služeb zotavení** > trezoru.
2. V nabídce prostředků, klikněte na tlačítko **Site Recovery** > **krok 1: připravte infrastrukturu** > **cíl ochrany**.

    ![Zvolte cíle.](./media/site-recovery-vmware-to-azure/choose-goals.png)
3. V **cíl ochrany**, vyberte **do Azure** > **Ano, s hypervisoru VMware vSphere**.

    ![Zvolte cíle.](./media/site-recovery-vmware-to-azure/choose-goals2.png)

## <a name="set-up-the-source-environment"></a>Nastavení zdrojového prostředí

Nastavení konfigurace serveru, registrace v trezoru a vyhledání virtuálních počítačů.

1. Klikněte na tlačítko **lokality obnovení** > **krok 1: Příprava infrastruktury** > **zdroj**.
2. Pokud nemáte konfigurační server, klikněte na tlačítko **+ konfigurační server**.

    ![Nastavení zdroje](./media/site-recovery-vmware-to-azure/set-source1.png)
3. V **přidat Server**, zkontrolujte, zda **konfigurační Server** se zobrazí v **typ serveru**.
4. Stáhněte instalační soubor nástroje Unified instalace nástroje Site Recovery.
5. Stáhnout registrační klíč trezoru Tuto funkci potřebujete po spuštění Unified instalace. Klíč je platný pět dní od jeho vygenerování.

   ![Nastavení zdroje](./media/site-recovery-vmware-to-azure/set-source2.png)


## <a name="run-site-recovery-unified-setup"></a>Sjednocený instalační program spusťte Site Recovery

Předtím, než můžete spustit, a potom spuštěním Unified instalačního programu nainstalujte konfigurační server, procesový server a hlavní cílový server, postupujte takto.
    - Vám zajistí rychlý přehled video

        > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video1-Source-Infrastructure-Setup/player]

    - Na serveru, konfigurace virtuálního počítače, zkontrolujte, jestli se systémové hodiny synchronizované se [čas serveru](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Měla by se shodovat. Pokud je 15 minut před nebo za instalace může selhat.
    - Spusťte instalační program jako místní správce na serveru konfigurace virtuálního počítače.
    - Ujistěte se, že je povolená TLS 1.0 ve virtuálním počítači.


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> Konfigurační server lze také nainstalovat [z příkazového řádku](http://aka.ms/installconfigsrv).



### <a name="add-the-account-for-automatic-discovery"></a>Přidejte účet pro automatické zjišťování

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="connect-to-vmware-servers"></a>Připojení k serverům VMware

Připojte hostitele ESXi vSphere nebo servery vCenter server, k vyhledání virtuálních počítačů VMware.

- Pokud přidáte vCenter server nebo hostitelů vSphere pomocí účtu bez oprávnění správce na serveru, že účet potřebuje těchto oprávnění povoleno:
    - Datacenter, úložiště, složka, hostitele, sítě, prostředků, virtuální počítač, vSphere distribuované přepínače.
    - VCenter server potřebuje oprávnění úložišť zobrazení.
- Když přidáte servery VMware, může trvat 15 minut nebo déle se objevily na portálu.
Povolit Azure Site Recovery se zjistit virtuální počítače spuštěné v místním prostředí, kterou potřebujete připojit VMware vCenter Server nebo hostitelů vSphere ESXi službou Site Recovery.

Vyberte **+ vCenter** zahájíte připojení serveru VMware vCenter server nebo hostiteli ESXi VMware vSphere.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]

Site Recovery se připojuje k VMware servery pomocí zadané nastavení a vyhledá virtuální počítače.

## <a name="set-up-the-target"></a>Nastavit cíl

Před nastavením cílovém prostředí, zkontrolujte, máte [účtu úložiště Azure a sítě](#set-up-azure)

1. Klikněte na **Připravit infrastrukturu** > **Cíl** a vyberte předplatné Azure, které chcete použít.
2. Určit, jestli je váš model nasazení cílového využívající Resource Manager a classic.
3. Site Recovery zkontroluje, že máte minimálně jednu kompatibilní síť a účet úložiště Azure.

   ![cíl](./media/site-recovery-vmware-to-azure/gs-target.png)
4. Pokud jste dosud nevytvořili účet úložiště nebo sítě, klikněte na tlačítko **+ účet úložiště** nebo **+ síť**, abyste mohli vytvořit účet správce prostředků nebo sítě vložené.

## <a name="set-up-replication-settings"></a>Nakonfigurování nastavení replikace

Vám zajistí rychlý přehled videa, než začnete:
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video2-vCenter-Server-Discovery-and-Replication-Policy/player]

1. Chcete-li vytvořit novou zásadu replikace, klikněte na tlačítko **infrastruktura Site Recovery** > **zásady replikace** > **+ zásady replikace**.
2. V **vytvořit zásadu replikace**, zadejte název zásady.
3. V části **Prahová hodnota cíle bodu obnovení (RPO)** zadejte omezení RPO. Tato hodnota určuje, jak často jsou vytvořeny body obnovení data. Pokud tento limit překračuje průběžnou replikaci, je vygenerována výstraha.
4. V **uchování bodu obnovení**, zadejte (v hodinách), jak dlouho je okno uchování pro každý bod obnovení. Replikované virtuální počítače můžete obnovit do libovolného bodu v okně. Až 24 hodin uchování je podporována pro počítače, na které se replikují do úložiště úrovně premium a 72 hodin, než standardní úložiště.
5. V **frekvence snímkování konzistentní aplikace vzhledem**, určit, jak často (v minutách) budou vytvořeny body obnovení obsahující snímky konzistentní s aplikacemi. Klikněte na tlačítko **OK** k vytvoření zásad.

    ![Zásady replikace](./media/site-recovery-vmware-to-azure/gs-replication2.png)
8. Když vytvoříte novou zásadu je automaticky přiřazen s konfiguračním serverem. Ve výchozím nastavení je pro navrácení služeb po obnovení automaticky vytvoří odpovídající zásady. Například, pokud je zásada replikace **rep zásad** bude navrácení služeb po obnovení zásad **rep. zásady navrácení**. Tyto zásady se nepoužije, dokud nespustíte navrácení služeb po obnovení z Azure.  



## <a name="plan-capacity"></a>Plánování kapacity

1. Teď, když máte základní infrastrukturu, můžete nastavit můžete myslíte o plánování kapacity a zjistit, jestli nepotřebujete další prostředky. [Další informace](site-recovery-plan-capacity-vmware.md).
2. Když jste hotovi s plánování kapacity, vyberte **Ano** v **dokončili jste plánování kapacity?**

   ![Plánování kapacity](./media/site-recovery-vmware-to-azure/gs-capacity-planning.png)


## <a name="prepare-vms-for-replication"></a>Příprava pro replikaci virtuálních počítačů

Služba Mobility musí být nainstalovaná na všech virtuálních počítačích VMware, které chcete replikovat. Služba Mobility můžete nainstalovat v několika způsoby:

1. Instalace pomocí nabízené instalace z procesového serveru. Je nutné připravit virtuální počítače, které chcete použít tuto metodu.
2. Instalace pomocí nástroje nasazení, například System Center Configuration Manager nebo služby Azure automation DSC.
3.  Nainstalujte ručně.

[Další informace](site-recovery-vmware-to-azure-install-mob-svc.md)


## <a name="enable-replication"></a>Povolení replikace

Než začnete, potřebujete:

- Azure uživatelský účet musí mít určité [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) k povolení replikace nového virtuálního počítače do Azure.
- Když přidáváte nebo odebíráte virtuálních počítačů, může trvat až 15 minut nebo déle pro změny vstoupily v platnost a se objevily na portálu.
- Čas poslední zjištěné můžete zkontrolovat pro virtuální počítače v **konfigurační servery** > **poslední obraťte se na**.
- Chcete-li přidat virtuální počítače bez čekání na naplánovaného zjišťování, zvýrazněte konfigurační server (nemáte klikněte na něj) a klikněte na tlačítko **aktualizovat**.
- Virtuální počítač připravený na nabízenou instalaci, procesový server automaticky nainstaluje služba Mobility při povolení replikace.


### <a name="exclude-disks-from-replication"></a>Vyloučení disků z replikace

Ve výchozím nastavení jsou všechny disky na počítači replikují. Disky můžete z replikace vyloučit. Například možná nebudete chtít replikovat disky s dočasná data nebo data, která se má aktualizovat pokaždé, když je počítač nebo restartování aplikace (například pagefile.sys nebo databázi tempdb systému SQL Server).

### <a name="replicate-vms"></a>Replikace virtuálních počítačů

Než začnete, podívejte se na rychlé video s přehledem

>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video3-Protect-VMware-Virtual-Machines/player]

1. Klikněte na **Krok 2: Replikujte aplikaci** > **Zdroj**.
2. V **zdroj**, vyberte konfigurační server.
3. V **počítač typ**, vyberte **virtuální počítače**.
4. V **vCenter vSphere Hypervisor**, vyberte server vCenter, který spravuje hostitelů vSphere nebo vyberte hostitele.
5. Vyberte procesový server. Pokud jste nevytvořili žádné servery další proces bude konfigurační server. Pak klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-replication2.png)

6. V **cíl**, vyberte předplatné a skupině prostředků, ve kterém chcete vytvořit neúspěšný přes virtuální počítače. Vyberte model nasazení, kterou chcete použít v Azure (klasický nebo prostředek management), pro selhání virtuálních počítačů.


7. Vyberte účet úložiště Azure, který chcete použít pro replikaci dat. Pokud nechcete použít účet, který jste již nastavili, můžete vytvořit nový.

8. Vyberte síť Azure a podsíť, ke kterým se připojí virtuální počítače Azure, když se po převzetí služeb při selhání vytvoří. Výběrem možnosti **Nakonfigurovat pro vybrané počítače** použijte nastavení sítě pro všechny počítače, které jste vybrali pro ochranu. Vyberte **Nakonfigurovat později** a vyberte síť Azure pro konkrétní počítač. Pokud nechcete použít stávající síť, můžete vytvořit jeden.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-rep3.png)
9. V nastavení **Virtuální počítače** > **Výběr virtuálních počítačů** klikněte a vyberte každý počítač, který chcete replikovat. Můžete vybrat pouze počítače, pro které je možné povolit replikaci. Pak klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-replication5.png)
10. V **vlastnosti** > **konfigurovat vlastnosti**, vyberte účet, který se použije pro automaticky instalaci služby Mobility na počítači procesní server.
11. Ve výchozím nastavení jsou všechny disky replikovat. Klikněte na tlačítko **všechny disky** a zrušte zaškrtnutí všech disků, které nechcete, aby k replikaci. Pak klikněte na **OK**. Později můžete nastavit další vlastnosti virtuálního počítače.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-replication6.png)
11. V **nastavení replikace** > **nakonfigurujete nastavení replikace**, ověřte, zda je vybrán správný replikace zásad. Pokud změníte zásady, změny se použijí pro replikaci počítač a pro nové počítače.
12. Povolit **konzistence pro víc Virtuálních** Pokud chcete shromažďovat počítače do replikační skupiny a zadejte název pro skupinu. Pak klikněte na **OK**. Poznámky:

    * Počítače v replikační skupiny replikovat společně a mít sdílené body obnovení konzistentní při selhání a konzistentní s aplikací, když se převzetí služeb při selhání.
    * Doporučujeme vám, že shromáždíte virtuálních počítačů a fyzických serverů společně, aby odpovídaly vašich úloh. Povolení konzistence více virtuálních počítačů může ovlivnit výkon pracovního vytížení a musí být použit pouze pokud počítače běží stejné zatížení a potřebujete konzistenci.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-replication7.png)
13. Klikněte na tlačítko **povolit replikaci**. Můžete sledovat průběh **povolení ochrany** úlohy v **nastavení** > **úlohy** > **úlohy Site Recovery**. Po spuštění úlohy **Dokončit ochranu** je počítač připravený k převzetí služeb při selhání.

### <a name="view-and-manage-vm-properties"></a>Zobrazení a správa vlastností virtuálního počítače

Doporučujeme, abyste ověřte vlastnosti virtuálního počítače a proveďte změny, které potřebujete.

1. Klikněte na tlačítko **replikované položky** > a vyberte počítač. **Essentials** okno zobrazuje informace o nastavení počítače a stav.
2. V části **Vlastnosti** můžete zobrazit informace o replikaci a převzetí služeb při selhání pro virtuální počítač.
3. V části **Výpočty a síť** > **Výpočetní vlastnosti** můžete zadat název a cílovou velikost virtuálního počítače Azure. Podle potřeby upravte název tak, aby byl souladu s [požadavky Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
4. Upravte nastavení pro Cílová síť, podsíť a IP adresu, která bude přiřazena virtuálnímu počítači Azure:

   - Můžete nastavit cílovou IP adresu.

    - Pokud adresu nezadáte, bude počítač, který převezme služby při selhání, používat DHCP.
    - Pokud nastavíte adresu, která není k dispozici na převzetí služeb při selhání, převzetí služeb při selhání nebude fungovat.
    - Stejnou cílovou IP adresu lze pro testovací převzetí služeb při selhání, pokud je adresa k dispozici v testovací síti převzetí služeb při selhání.

   - Počet síťových adaptérů je závisí na velikosti, kterou zadáte pro cílový virtuální počítač:

     - Pokud počet síťových adaptérů na zdrojovém počítači je stejný jako nebo menší než počet adaptérů povoleno pro velikost cílového počítače a potom cíl bude mít stejný počet adaptérů jako zdroj.
     - Pokud počet adaptérů pro zdrojový virtuální počítač překračuje počet povolený pro cílovou velikost, pak se použije maximální velikost cíle.
     - Pokud má například zdrojový počítač dva síťové adaptéry a velikost cílového počítače podporuje čtyři, bude mít cílový počítač dva adaptéry. Pokud má zdrojový počítač dva adaptéry, ale podporovaná velikost cíle podporuje pouze jeden, bude mít cílový počítač pouze jeden adaptér.     
   - Pokud má virtuální počítač více síťových adaptérů připojí se všechny ke stejné síti.
   - Pokud virtuální počítač má několik síťových adaptérů se pak stane první z nich uvedené v seznamu *výchozí* síťový adaptér ve virtuálním počítači Azure.
4. V **disky**, uvidíte operační systém virtuálního počítače a data disky, který bude replikován.

#### <a name="managed-disks"></a>Managed Disks

V **výpočty a síť** > **výpočetní vlastnosti**, "Použití spravovaných disků" nastavení "Ano" pro virtuální počítač můžete nastavit, pokud chcete připojit k počítači na převzetí služeb při selhání do Azure spravované disky. Spravované disky zjednodušují správu disků virtuálních počítačů Azure IaaS tím, že spravují účty úložiště přidružené k diskům virtuálních počítačů. [Další informace o spravovaných disky.](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview)

   - Spravované disky se vytvoří a připojí k virtuálnímu počítači jenom v případě, že při selhání služby převezme Azure. Když zapnete ochranu, data z místních počítačů se budou i nadále replikovat do účtů úložiště.  Spravované disky je možné vytvořit pouze pro virtuální počítače nasazené pomocí modelu nasazení Resource Manageru.  

   - Když nastavíte vlastnost Použít spravované disky na Ano, budete moci vybrat jenom skupiny dostupnosti ve skupině prostředků, která má vlastnost Použít spravované disky nastavenou na Ano. Důvodem je to, že virtuální počítače se spravovanými disky můžou být součástí jenom skupin dostupnosti, které mají vlastnost Použít spravované disky nastavenou na Ano. Při vytváření skupin dostupnosti nezapomeňte vlastnost Použít spravované disky nastavit podle toho, jak chcete se spravovanými disky nakládat při převzetí služeb při selhání.  Stejně tak když nastavíte Použít spravované disky na Ne, budete moci vybrat jenom skupiny dostupnosti ve skupině prostředků, která má vlastnost Použít spravované disky nastavenou na Ne. [Přečtěte si další informace o spravovaných discích a skupinách dostupnosti](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set).

  > [!NOTE]
  > Pokud byl někdy účet úložiště používaný pro replikaci zašifrován pomocí šifrování služby Storage, vytvoření spravovaných disků se během převzetí služeb při selhání nezdaří. Buď můžete vlastnost Použít spravované disky nastavit na Ne a pokusit se o převzetí služeb při selhání, nebo můžete zakázat ochranu virtuálního počítače a chránit ho pomocí účtu úložiště, který nikdy neměl povoleno šifrování služby Storage.
  > [Další informace o šifrování služby Storage a o spravovaných discích](https://docs.microsoft.com/en-us/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption)


## <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání


Poté, co jste nastavili vše, spusťte převzetí služeb při selhání a ujistěte se všechno funguje podle očekávání. Vám zajistí rychlý přehled videa, než začnete
>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video4-Recovery-Plan-DR-Drill-and-Failover/player]


1. K převzetí služeb při selhání jednom počítači, v **nastavení** > **replikované položky**, klikněte na virtuální počítač > **+ testovací převzetí služeb při selhání** ikonu.

    ![Testovací převzetí služeb při selhání](./media/site-recovery-vmware-to-azure/TestFailover.png)

1. Pokud chcete pro převzetí služeb při selhání použít plán obnovení, klikněte v **Nastavení** > **Plány obnovení** pravým tlačítkem myši na plán > **Testovací převzetí služeb při selhání**. Pokud chcete vytvořit plán obnovení, [postupujte podle těchto pokynů](site-recovery-create-recovery-plans.md).  

1. V **testovací převzetí služeb při selhání**, vyberte síť Azure, ke které virtuální počítače Azure připojí po převzetí služeb při selhání.

1. Kliknutím na **OK** zahajte převzetí služeb při selhání. Průběh můžete sledovat kliknutím na virtuálním počítači otevřete jeho vlastnosti, nebo na **testovací převzetí služeb při selhání** úlohy v název trezoru > **nastavení** > **úlohy** > **úlohy Site Recovery**.

1. Po dokončení převzetí služeb při selhání by se vám také měl zobrazit počítač Azure repliky na portálu Azure Portal > **Virtuální počítače**. Měli byste zajistit, aby měl virtuální počítač odpovídající velikost, byl připojený k odpovídající síti a aby běžel.

1. Pokud jste [připravili připojení po převzetí služeb při selhání](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover), měli byste být schopni se k virtuálnímu počítači Azure připojit.

1. Až budete hotovi, klikněte v plánu obnovení na **Cleanup test failover** (Vyčistit po testu převzetí při selhání). V **poznámky**, zaznamenejte a uložte jakékoli připomínky související s testovací převzetí služeb. Tato akce odstraní virtuální počítače, které byly vytvořeny během testovacího převzetí služeb při selhání.

[Další informace](site-recovery-test-failover-to-azure.md) o testovací převzetí služeb při selhání.


## <a name="vmware-account-permissions"></a>Oprávnění pro uživatelský účet VMware

Site Recovery potřebuje přístup k VMware u procesového serveru na automatické zjišťování virtuálních počítačů a převzetí služeb při selhání a navrácení služeb po obnovení virtuálních počítačů.

- **Migrace**: Pokud chcete migrovat virtuální počítače VMware do Azure, aniž by někdy je selhání zpět, je použít účet VMware s rolí jen pro čtení. Tyto role můžete spustit převzetí služeb při selhání, ale nelze vypnout chráněných zdrojových počítačů. Tato akce není nezbytné pro migraci.
- **Replikovat nebo obnovit**: Pokud chcete nasadit úplná replikace (replikace, převzetí služeb při selhání, navrácení služeb po obnovení) účet musí být schopen spuštění operací, jako je například vytváření a odebrání disků, zapínání virtuální počítače atd.
- **Automatické zjišťování**: je požadován alespoň účet jen pro čtení.


**Úkol** | **Požadovaný účet nebo role** | **Oprávnění** | **Podrobnosti**
--- | --- | --- | ---
**Procesový server automaticky vyhledá virtuální počítače VMware** | Je třeba alespoň uživatele jen pro čtení | Objekt datového centra –> Propagate pro podřízený objekt role = jen pro čtení | Uživatel přiřazené úrovni datacenter a má přístup ke všem objektům v datovém centru.<br/><br/> Pokud chcete omezit přístup, přiřadit **žádný přístup** role s **Propagate na podřízené** objekt, pro podřízený objekt (hostitelů vSphere, datastores, virtuální počítače a sítě).
**Převzetí služeb při selhání** | Je třeba alespoň uživatele jen pro čtení | Objekt datového centra –> Propagate pro podřízený objekt role = jen pro čtení | Uživatel přiřazené úrovni datacenter a má přístup ke všem objektům v datovém centru.<br/><br/> Pokud chcete omezit přístup, přiřadit **žádný přístup** role s **Propagate na podřízené** objekt, který má podřízené objekty (hostitelů vSphere, datastores, virtuální počítače a sítě).<br/><br/> Tato možnost je užitečná pro účely migrace, ale není úplná replikace, převzetí služeb při selhání a navrácení služeb po obnovení.
**Převzetí služeb při selhání a navrácení služeb po obnovení** | Doporučujeme vytvořit roli (Azure_Site_Recovery) s požadovanými oprávněními a potom přiřadit roli VMware uživatele nebo skupinu | Objekt datového centra –> Propagate pro podřízený objekt role = Azure_Site_Recovery<br/><br/> Úložiště dat -> přidělte místo, procházet úložiště dat, operace se soubory nízké úrovně, odstraňte soubor, aktualizovat soubory virtuálního počítače<br/><br/> Síť -> přiřazení sítě<br/><br/> Zdroj -> Přiřazení virtuálního počítače do fondu zdrojů, migrovat napájený vypnout virtuální počítač, migrace napájený na virtuálním počítači<br/><br/> Úlohy -> Vytvořit úlohu, úloha aktualizace<br/><br/> Virtuální počítač -> Konfigurace<br/><br/> Virtuální počítač -> interakcí -> odpovědí otázku, připojení zařízení, nakonfigurovat média CD, nakonfigurovat disketová média, vypnout, zapnutí, instalaci nástroje VMware<br/><br/> Virtuální počítač -> inventáře -> vytvořit, registraci, zrušení registrace<br/><br/> Virtuální počítač -> zřizování -> Povolit stahování virtuálního počítače, povolí nahrát soubory virtuálního počítače<br/><br/> Virtuální počítač -> snímky -> odebrat snímky | Uživatel přiřazené úrovni datacenter a má přístup ke všem objektům v datovém centru.<br/><br/> Pokud chcete omezit přístup, přiřadit **žádný přístup** role s **Propagate na podřízené** objekt, pro podřízený objekt (hostitelů vSphere, datastores, virtuální počítače a sítě).


## <a name="next-steps"></a>Další kroky

Po získání replikace nahoru a spuštěna, když dojde k výpadku selhání do Azure a virtuálních počítačích Azure jsou vytvořené pomocí replikovaná data. Pak můžete přistupovat úlohy a aplikace v Azure, dokud se nesplní zpět do primárního umístění, až se obnoví normální provozní podmínky.

- [Další informace](site-recovery-failover.md) o různých typech převzetí služeb při selhání a jak je spouštět.
- Pokud se migrace počítačů namísto replikace a selhání zpět, [Další](site-recovery-migrate-to-azure.md#migrate-on-premises-vms-and-physical-servers).
- [Přečtěte si informace o navrácení služeb po obnovení](site-recovery-failback-azure-to-vmware.md)navrácení služeb po obnovení a replikovat virtuální počítače Azure zpět na primární místního webu.

## <a name="third-party-software-notices-and-information"></a>Oznámení software třetích stran a informace
Nechcete převede nebo lokalizaci

Software a firmware spuštěné v produktu společnosti Microsoft nebo služba je založena na nebo zahrnuje materiálu z projekty uvedené níže (dále souhrnně nazývané "kód třetí strany").  Společnost Microsoft se není původní autor kód třetích stran.  Původní o autorských právech a licenci, pod kterým společnost Microsoft takový kód třetích stran, jsou nastavené níže uvedenými.

Informace v části A je týkající se součásti kód třetích stran z projekty uvedené níže. Tyto licence a informace jsou uvedené pro pouze pro informační účely.  Tento kód třetích stran, je právě relicensed vám společnost Microsoft pod licenční podmínky softwaru společnosti Microsoft pro produkty společnosti Microsoft nebo služby.  

Informace v části B je týkající se součástí kód třetích stran, které jsou určeny jako dostupné pro vás společností Microsoft v rámci původní licenční podmínky.

Dokončení soubor najdete na [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=529428). Společnost Microsoft si vyhrazuje všechna práva, která nejsou výslovně udělena, zda implicitně, jako překážku uplatnění nároku či jiným způsobem.
