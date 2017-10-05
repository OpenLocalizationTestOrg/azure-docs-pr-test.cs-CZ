---
title: Replikovat aplikace (VMware do Azure) | Microsoft Docs
description: "Tento článek popisuje, jak nastavit replikaci virtuálních počítačů spuštěných na VMware do Azure."
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
ms.openlocfilehash: e0047a996c9bfd7d950b32f0871ddd7608924b42
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="replicate-applications-running-on-vmware-vms-to-azure"></a>Replikovat aplikací běžících na virtuálních počítačích VMware do Azure



Tento článek popisuje, jak nastavit replikaci virtuálních počítačů spuštěných na VMware do Azure.
## <a name="prerequisites"></a>Požadavky

Článek předpokládá, že máte již

1.  [Instalační program místní zdrojové prostředí](site-recovery-set-up-vmware-to-azure.md)
2.  [Instalační program cílové prostředí v Azure](site-recovery-prepare-target-vmware-to-azure.md)


## <a name="enable-replication"></a>Povolení replikace
#### <a name="before-you-start"></a>Než začnete
Při replikaci virtuálních počítačů VMware, Všimněte si, že:

* Azure uživatelský účet musí mít určité [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) k povolení replikace nového virtuálního počítače do Azure.
* Virtuální počítače VMware, jsou zjišťovány každých 15 minut. Může trvat 15 minut nebo déle se objevily na portálu po zjišťování. Podobně zjišťování může trvat 15 minut nebo déle při přidání nového hostitele serveru nebo vSphere vCenter.
* Změny prostředí na virtuálním počítači (jako je třeba instalace nástroje VMware) může trvat 15 minut nebo déle aktualizovat na portálu.
* Čas poslední zjištěné můžete zkontrolovat pro virtuální počítače VMware v **poslední obraťte se na** pole pro vCenter server vSphere hostitele, na **konfigurační servery** okno.
* Chcete-li přidat počítače pro replikaci bez čekání na naplánovaného zjišťování, zvýrazněte konfigurační server (nemáte klikněte na něj) a klikněte na tlačítko **aktualizovat** tlačítko.
* Když povolíte replikaci, pokud je počítač připravený, procesový server automaticky nainstaluje služba Mobility na něm.


**Teď následujícím způsobem povolte replikaci**:

1. Klikněte na **Krok 2: Replikujte aplikaci** > **Zdroj**. Po prvním povolení replikace kliknutím na **+Replikovat** v trezoru povolte replikaci pro další počítače.
2. V **zdroj** okno > **zdroj**, vyberte konfigurační server.
3. V **počítač typ**, vyberte **virtuální počítače** nebo **fyzických počítačů**.
4. V **vCenter vSphere Hypervisor**, vyberte server vCenter, který spravuje hostitelů vSphere nebo vyberte hostitele. Toto nastavení není relevantní, pokud replikujete fyzických počítačů.
5. Vyberte procesový server. Pokud jste nevytvořili žádné další procesu serverů to bude název konfigurační server. Pak klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-replication2.png)

6. V **cíl** vyberte předplatné a skupině prostředků, kde chcete vytvořit neúspěšný přes virtuální počítače. Vyberte model nasazení (Classic nebo Resource Manager), který chcete v Azure použít pro virtuální počítače, pro které bylo provedeno převzetí služeb při selhání.
7. Vyberte účet úložiště Azure, který chcete použít pro replikaci dat. Poznámky:

   * Můžete vybrat premium nebo standardní účet úložiště. Pokud vyberete prémiový účet, budete muset zadat účet další standardní úložiště pro protokoly probíhající replikace. Účty musí být ve stejné oblasti jako trezor služeb zotavení.
   * Pokud chcete použít jiný účet úložiště než ty, které máte, můžete vytvořit jeden*zástupný symbol odkaz pro vytvoření účtu úložiště pomocí resource manager, který se bude vztahovat v části Začínáme*. Chcete-li vytvořit účet úložiště pomocí Správce prostředků, klikněte na tlačítko **vytvořit nový**. Pokud chcete vytvořit účet úložiště pomocí klasického modelu, můžete to udělat [na webu Azure Portal](../storage/common/storage-create-storage-account.md).

8. Vyberte síť Azure a podsíť, ke kterému se připojí virtuální počítače Azure, když se zprovozní po převzetí služeb při selhání. Síť musí být ve stejné oblasti jako trezor Služeb zotavení. Výběrem možnosti **Nakonfigurovat pro vybrané počítače** použijte nastavení sítě pro všechny počítače, které jste vybrali pro ochranu. Vyberte **Nakonfigurovat později** a vyberte síť Azure pro konkrétní počítač. Pokud nemáte k síti, budete muset [vytvořit](#set-up-an-azure-network). Chcete-li vytvořit síť pomocí Resource Manager, klikněte na tlačítko **vytvořit nový**. Pokud chcete vytvořit síť pomocí klasického modelu, udělejte to [na portálu Azure Portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). V odpovídajícím případě vyberte podsíť. Pak klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-rep3.png)
9. V nastavení **Virtuální počítače** > **Výběr virtuálních počítačů** klikněte a vyberte každý počítač, který chcete replikovat. Můžete vybrat pouze počítače, pro které je možné povolit replikaci. Pak klikněte na **OK**.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-replication5.png)
10. V **vlastnosti** > **konfigurovat vlastnosti**, vyberte účet, který se použije pro automaticky instalaci služby Mobility na počítači procesní server.  
11. Ve výchozím nastavení jsou všechny disky replikovat. Pokud chcete z replikace vyloučit disky, klikněte na tlačítko **všechny disky** a zrušte zaškrtnutí všech disků, které nechcete, aby k replikaci.  Pak klikněte na **OK**. Později můžete nastavit další vlastnosti. [Další informace](site-recovery-exclude-disk.md) o s výjimkou disků.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-replication6.png)

12. V **nastavení replikace** > **nakonfigurujete nastavení replikace**, ověřte, zda je vybrán správný replikace zásad. Můžete upravit nastavení zásad replikace v **nastavení** > **zásady replikace** > název zásady > **upravit nastavení**. Změny, které použijete pro zásady se použijí pro replikaci a nové počítače.
13. Povolit **konzistence pro víc Virtuálních** Pokud chcete shromažďovat počítače do replikační skupiny a zadejte název pro skupinu. Pak klikněte na **OK**. Poznámky:

    * Počítače v replikační skupiny replikovat společně a mít sdílené body obnovení konzistentní při selhání a konzistentní s aplikací, když se převzetí služeb při selhání.
    * Doporučujeme vám, že shromáždíte virtuálních počítačů a fyzických serverů společně, aby odpovídaly vašich úloh. Povolení konzistence více virtuálních počítačů může ovlivnit výkon pracovního vytížení a musí být použit pouze pokud počítače běží stejné zatížení a potřebujete konzistenci.

    ![Povolení replikace](./media/site-recovery-vmware-to-azure/enable-replication7.png)
14. Klikněte na tlačítko **povolit replikaci**. Můžete sledovat průběh **povolení ochrany** úlohy v **nastavení** > **úlohy** > **úlohy Site Recovery**. Po spuštění úlohy **Dokončit ochranu** je počítač připravený k převzetí služeb při selhání.

> [!NOTE]
> Pokud je počítač připravený na nabízenou instalaci, které službu mobility bude nainstalována v případě, že je povolena ochrana. Po součást se nainstaluje do počítače, které spustí úlohu ochrany a selže. Po selhání je potřeba restartovat ručně každý počítač. Po restartování začne znovu úlohu ochrany a dojde k počáteční replikaci.
>
>

## <a name="view-and-manage-vm-properties"></a>Zobrazení a správa vlastností virtuálního počítače

Doporučujeme ověřit vlastnosti zdrojového počítače. Mějte na paměti, že název virtuálního počítače Azure musí být v souladu s [požadavky na virtuální počítače Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. Klikněte na tlačítko **nastavení** > **replikované položky** > a vyberte počítač. **Essentials** okno zobrazuje informace o nastavení počítače a stav.
2. V části **Vlastnosti** můžete zobrazit informace o replikaci a převzetí služeb při selhání pro virtuální počítač.
3. V části **Výpočty a síť** > **Výpočetní vlastnosti** můžete zadat název a cílovou velikost virtuálního počítače Azure. Název pro dosažení souladu s požadavky na Azure, pokud potřebujete změňte.
    ![Povolení replikace](./media/site-recovery-vmware-to-azure/VMProperties_AVSET.png)
 
4.  Můžete vybrat [skupiny prostředků](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-resource-groups-guidelines) převzetí služeb při selhání z počítače, který se stane součástí post. Toto nastavení kdykoli před selhání přes, můžete změnit. POST převzít služby, pokud provádíte migraci počítače k jiné skupině prostředků a dojde k porušení nastavení ochrany počítače.
5. Můžete vybrat [skupinu dostupnosti](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) Pokud váš počítač, musí být součástí jedné post převzetí služeb při selhání. Při výběru dostupnost sady, prosím mějte na paměti, že:

    * Objeví se pouze skupiny dostupnosti patřící do zadaná skupina prostředků  
    * Počítače s jinou virtuální sítě nemůže být součástí stejné skupiny dostupnosti
    * Pouze virtuální počítače stejnou velikost může být součástí stejné skupiny dostupnosti
5. Můžete také zobrazit a přidejte informace o Cílová síť, podsíť a IP adresu, která bude přiřazena virtuálnímu počítači Azure.
6. V **disky**, uvidíte operačního systému a datové disky na virtuální počítač, který bude replikován.

### <a name="network-adapters-and-ip-addressing"></a>Síťové adaptéry a adresování IP 

- Můžete nastavit cílovou IP adresu. Pokud adresu nezadáte, bude počítač, který převezme služby při selhání, používat DHCP. Pokud nastavíte adresu, která není k dispozici na převzetí služeb při selhání, převzetí služeb při selhání nebude fungovat. Stejnou cílovou IP adresu je možné použít pro testovací převzetí služeb při selhání, pokud je adresa k dispozici v testovací síti převzetí služeb při selhání.
- Počet síťových adaptérů závisí na velikosti, kterou zadáte pro cílový virtuální počítač, a to následujícím způsobem:
    - Pokud je počet síťových adaptérů na zdrojovém počítači menší nebo roven počtu adaptérů, které jsou povolené pro velikost cílového počítače, pak bude mít cíl stejný počet adaptérů jako zdroj.
    - Pokud počet adaptérů pro zdrojový virtuální počítač překračuje počet povolený pro cílovou velikost, použije se maximální velikost cíle.
    - Pokud má například zdrojový počítač dva síťové adaptéry a velikost cílového počítače podporuje čtyři, bude mít cílový počítač dva adaptéry. Pokud má zdrojový počítač dva adaptéry, ale podporovaná velikost cíle podporuje pouze jeden, bude mít cílový počítač pouze jeden adaptér.
    - Pokud má virtuální počítač více síťových adaptérů připojí se všechny ke stejné síti.
    - Pokud virtuální počítač má několik síťových adaptérů se pak stane první z nich uvedené v seznamu *výchozí* síťový adaptér ve virtuálním počítači Azure.
   



## <a name="common-issues"></a>Běžné problémy

* Každý disk by měla být menší než velikost 1TB.
* Jako disk operačního systému by měl být použit základní disk, a ne dynamický disk.
* Pro virtuální počítače s podporou generace 2/UEFI by jako operační systém měl být použitý systém Windows a spouštěcí disk by měl být menší než 300 GB.

## <a name="next-steps"></a>Další kroky

Po dokončení ochrany, můžete zkusit [převzetí služeb při selhání](site-recovery-failover.md) ke kontrole, jestli vaše aplikace se zobrazí v Azure nebo ne.

V případě, že chcete zakažte ochranu, zkontrolujte, jak [vyčistit nastavení registrace a ochrany](site-recovery-manage-registration-and-protection.md)
