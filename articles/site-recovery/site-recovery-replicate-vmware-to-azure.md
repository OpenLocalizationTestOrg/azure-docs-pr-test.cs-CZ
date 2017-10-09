---
title: Replikovat aplikace (VMware tooAzure) | Microsoft Docs
description: "Tento článek popisuje, jak tooset replikaci virtuálního počítače, spuštěná na VMware do Azure."
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: asgang
ms.openlocfilehash: b07aabdacec521c7bd89e50f6a1427a774ff0287
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-applications-running-on-vmware-vms-tooazure"></a>Replikovat aplikací běžících na virtuálních počítačích VMware tooAzure



Tento článek popisuje, jak tooset replikaci virtuálního počítače, spuštěná na VMware do Azure.
## <a name="prerequisites"></a>Požadavky

Hello článek předpokládá, že máte již

1.  [Instalační program místní zdrojové prostředí](site-recovery-set-up-vmware-to-azure.md)
2.  [Instalační program cílové prostředí v Azure](site-recovery-prepare-target-vmware-to-azure.md)


## <a name="enable-replication"></a>Povolení replikace
#### <a name="before-you-start"></a>Než začnete
Při replikaci virtuálních počítačů VMware, Všimněte si, že:

* Azure uživatelský účet potřebuje toohave určité [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikaci nové tooAzure virtuálního počítače.
* Virtuální počítače VMware, jsou zjišťovány každých 15 minut. Může trvat 15 minut nebo déle pro ně tooappear hello portálu po zjišťování. Podobně zjišťování může trvat 15 minut nebo déle při přidání nového hostitele serveru nebo vSphere vCenter.
* Změny prostředí na virtuálním počítači hello (jako je třeba instalace nástroje VMware) může trvat 15 minut nebo další toobe aktualizovat hello portálu.
* Můžete zkontrolovat hello poslední zjištěné čas pro virtuální počítače VMware v hello **poslední obraťte se na** pole pro hello hostitelů server vSphere vCenter, na hello **konfigurační servery** okno.
* tooadd počítače pro replikaci bez čekání na hello naplánovaného zjišťování, zvýrazněte hello konfigurační server (nemáte klikněte na něj) a klikněte na tlačítko hello **aktualizovat** tlačítko.
* Když povolíte replikaci, pokud hello počítač připravený, hello procesový server automaticky nainstaluje služba Mobility hello na něm.


**Teď následujícím způsobem povolte replikaci**:

1. Klikněte na **Krok 2: Replikujte aplikaci** > **Zdroj**. Po povolení replikace pro hello poprvé, klikněte na tlačítko **+ replikovat** v hello trezoru tooenable replikaci pro další počítače.
2. V hello **zdroj** okno > **zdroj**vyberte hello konfigurační server.
3. V **počítač typ**, vyberte **virtuální počítače** nebo **fyzických počítačů**.
4. V **vCenter vSphere Hypervisor**, vyberte hello vCenter server, který spravuje hostitelů vSphere hello nebo vyberte hello hostitele. Toto nastavení není relevantní, pokud replikujete fyzických počítačů.
5. Vyberte procesový server hello. Pokud jste nevytvořili žádné další proces serverů bude název hello hello konfigurační server. Pak klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-replication2.png)

6. V **cíl** vyberte hello předplatné a skupina prostředků hello místo hello toocreate při selhání virtuálního počítače. Vyberte model nasazení hello, který chcete toouse v Azure (klasický nebo prostředek management) pro hello při selhání virtuálního počítače.
7. Vyberte účet úložiště Azure hello, že který má toouse pro replikaci dat. Poznámky:

   * Můžete vybrat premium nebo standardní účet úložiště. Pokud vyberete prémiový účet, budete potřebovat toospecify další standardní účet úložiště pro protokoly probíhající replikace. Účty musí být v hello stejné oblasti jako hello trezoru služeb zotavení.
   * Pokud chcete toouse jiný účet úložiště než ty, které máte, můžete ji vytvořit*zástupný symbol odkaz pro vytvoření účtu úložiště pomocí resource manager, který se bude vztahovat v části Začínáme*. Klikněte na tlačítko toocreate účet úložiště pomocí Správce prostředků, **vytvořit nový**. Pokud chcete účet úložiště pomocí klasického modelu hello toocreate, můžete udělat [v hello portál Azure](../storage/common/storage-create-storage-account.md).

8. Vyberte hello Azure toowhich síť a podsíť virtuálních počítačů Azure se připojí, když se zprovozní po převzetí služeb při selhání. Hello síť musí mít hello stejné oblasti jako hello trezoru služeb zotavení. Vyberte **nakonfigurovat pro vybrané počítače**, tooapply hello nastavení tooall počítače sítě je vybrat pro ochranu. Vyberte **nakonfigurovat později** tooselect hello síť Azure pro konkrétní počítač. Pokud nemáte k síti, je třeba příliš[vytvořit](#set-up-an-azure-network). toocreate síť pomocí Resource Manager, klikněte na tlačítko **vytvořit nový**. Pokud chcete toocreate síť pomocí klasického modelu hello, udělat [v hello portál Azure](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). V odpovídajícím případě vyberte podsíť. Pak klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-rep3.png)
9. V **virtuální počítače** > **vybrat virtuální počítače**, klikněte na tlačítko a vyberte každý počítač má tooreplicate. Můžete vybrat pouze počítače, pro které je možné povolit replikaci. Pak klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-replication5.png)
10. V **vlastnosti** > **konfigurovat vlastnosti**vyberte hello účet, který se použije hello proces serveru tooautomatically instalaci služby Mobility hello na počítači hello.  
11. Ve výchozím nastavení jsou všechny disky replikovat. Klikněte na tlačítko tooexclude disky z replikace, **všechny disky** a zrušte zaškrtnutí všech disků, které nechcete tooreplicate.  Pak klikněte na **OK**. Později můžete nastavit další vlastnosti. [Další informace](site-recovery-exclude-disk.md) o s výjimkou disků.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-replication6.png)

12. V **nastavení replikace** > **nakonfigurujete nastavení replikace**, ověřte, že hello správné zásady replikace je vybrána. Můžete upravit nastavení zásad replikace v **nastavení** > **zásady replikace** > název zásady > **upravit nastavení**. Změny použít tooa zásady budou použité tooreplicating a nové počítače.
13. Povolit **konzistence pro víc Virtuálních** Pokud mají toogather počítače do replikační skupiny a zadejte název pro skupinu hello. Pak klikněte na **OK**. Poznámky:

    * Počítače v replikační skupiny replikovat společně a mít sdílené body obnovení konzistentní při selhání a konzistentní s aplikací, když se převzetí služeb při selhání.
    * Doporučujeme vám, že shromáždíte virtuálních počítačů a fyzických serverů společně, aby odpovídaly vašich úloh. Povolení konzistence více virtuálních počítačů může ovlivnit výkon pracovního vytížení a musí být použit pouze pokud počítače běží hello stejné úlohy a potřebujete konzistence.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-replication7.png)
14. Klikněte na tlačítko **povolit replikaci**. Můžete sledovat průběh hello **povolení ochrany** úlohy v **nastavení** > **úlohy** > **úlohy Site Recovery**. Po hello **dokončení ochrany** úloha spustí počítač hello je připraven na převzetí služeb při selhání.

> [!NOTE]
> Pokud počítač hello je připravená pro nabízenou instalaci hello Mobility součást služby bude nainstalována v případě, že je povolena ochrana. Po hello je součást nainstalována na počítač hello, které spustí úlohu ochrany a selže. Po selhání hello musíte toomanually každý počítač restartovat. Po hello restartování hello ochrany úlohy začne znovu a dojde k počáteční replikaci.
>
>

## <a name="view-and-manage-vm-properties"></a>Zobrazení a správa vlastností virtuálního počítače

Doporučujeme ověřit vlastnosti hello hello zdrojového počítače. Mějte na paměti, že název virtuálního počítače Azure hello musí být v souladu s [požadavky na virtuální počítač Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. Klikněte na tlačítko **nastavení** > **replikované položky** > a vyberte hello počítač. Hello **Essentials** okno zobrazuje informace o nastavení počítače a stav.
2. V **vlastnosti**, můžete zobrazit replikace a informace o převzetí služeb při selhání pro hello virtuálních počítačů.
3. V **výpočty a síť** > **výpočetní vlastnosti**, můžete zadat název a cílovou velikost virtuálního počítače Azure hello. Upravte název toocomply hello s požadavky na Azure, pokud potřebujete.
    ![Povolení replikace](./media/site-recovery-vmware-to-azure/VMProperties_AVSET.png)
 
4.  Můžete vybrat [skupiny prostředků](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-resource-groups-guidelines) převzetí služeb při selhání z počítače, který se stane součástí post. Toto nastavení kdykoli před selhání přes, můžete změnit. POST převzít služby, pokud provádíte migraci hello počítač tooa jiné skupině prostředků, pak by došlo k přerušení nastavení ochrany počítače.
5. Můžete vybrat [skupinu dostupnosti](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) potřeby váš počítač toobe být součástí jedné post převzetí služeb při selhání. Při výběru dostupnost sady, prosím mějte na paměti, že:

    * Pouze patřící toohello zadané sady dostupnosti bude uvedena skupiny prostředků  
    * Počítače s jinou virtuální sítě nemůže být součástí stejné skupiny dostupnosti
    * Pouze virtuální počítače stejnou velikost může být součástí stejné skupiny dostupnosti
5. Můžete také zobrazit a přidejte informace o hello Cílová síť, podsíť a IP adresu, která bude přiřazena toohello virtuálního počítače Azure.
6. V **disky**, uvidíte hello operačního systému a datové disky na hello virtuální počítač, který bude replikován.

### <a name="network-adapters-and-ip-addressing"></a>Síťové adaptéry a adresování IP 

- Můžete nastavit hello cílová IP adresa. Pokud adresu nezadáte, použije hello převzal počítač DHCP. Pokud nastavíte adresu, která není k dispozici na převzetí služeb při selhání, převzetí služeb při selhání hello nebude fungovat. Dobrý den, které lze použít stejnou cílovou IP adresu pro testovací převzetí služeb při selhání, pokud není k dispozici v hello testovací převzetí služeb při selhání sítě hello adresa.
- Hello počet síťových adaptérů závisí hello velikost, který jste zadali pro hello cílový virtuální počítač následujícím způsobem:
    - Pokud hello počet síťových adaptérů na zdrojovém počítači hello je menší než nebo rovna toohello počet adaptérů povoleno pro velikost cílového počítače hello pak bude mít cíl hello hello stejný počet adaptérů jako zdroj hello.
    - Pokud hello počet adaptérů pro hello zdrojový virtuální počítač překračuje počet hello povolený pro cílovou velikost hello pak maximální velikost cíle hello se použije.
    - Například pokud má zdrojový počítač dva síťové adaptéry a velikost hello cílového počítače podporuje čtyři, bude mít hello cílový počítač dva adaptéry. Pokud má hello zdrojový počítač dva adaptéry, ale hello podporovaná velikost cíle podporuje pouze jeden, bude mít cílový počítač hello jenom jeden adaptér.
    - Pokud hello virtuální počítač více síťových adaptérů připojí se všechny toohello stejné síti.
    - Pokud hello virtuální počítač více síťových adaptérů pak hello jeden uvedené v seznamu hello se stal hello *výchozí* síťový adaptér v hello virtuální počítač Azure.
   



## <a name="common-issues"></a>Běžné problémy

* Každý disk by měla být menší než velikost 1TB.
* Hello operačního systému disku by měla být základního disku a není dynamický disk
* Generace 2/UEFI povoleno virtuálních počítačů, hello operační systém řady musí být Windows a spouštěcí disk by měla být menší než 300GB

## <a name="next-steps"></a>Další kroky

Po dokončení hello ochrany, můžete zkusit [převzetí služeb při selhání](site-recovery-failover.md) toocheck jestli vaše aplikace se zobrazí v Azure nebo ne.

V případě, že chcete toodisable ochrany, zkontrolujte, jak příliš[vyčistit nastavení registrace a ochrany](site-recovery-manage-registration-and-protection.md)
