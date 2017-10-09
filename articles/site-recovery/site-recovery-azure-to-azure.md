---
title: "Replikovat virtuální počítače Azure mezi oblastmi Azure pro zotavení po havárii vyžaduje: Azure tooAzure | Microsoft Docs"
description: "Shrnuje kroky hello musíte virtuální počítače Azure tooreplicate mezi oblastmi Azure (Azure Azure) se službou Azure Site Recovery hello pro potřebovat obnovení po havárii."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: raynew
ms.openlocfilehash: c4c425af260a9bcc3dd4dcc13da26109e20f03bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-azure-vms-between-regions-with-azure-site-recovery"></a>Replikace virtuálních počítačů Azure mezi oblastmi s Azure Site Recovery

>[!NOTE]
>
> Azure Site Recovery replikaci pro virtuální počítače Azure (VM) je aktuálně ve verzi preview.

Tento článek popisuje, jak tooreplicate virtuálních počítačích Azure mezi oblastmi Azure pomocí hello [Site Recovery](site-recovery-overview.md) služby v hello portálu Azure.

POST dotazy a na konci hello tohoto článku nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="disaster-recovery-in-azure"></a>Zotavení po havárii v Azure

Předdefinované infrastrukturu Azure možností a funkcí přispívat strategie pro dostupnost robustní a odolné tooa pro úlohy, které běží na virtuálních počítačích Azure. Existují však mnoha důvodů, proč potřebujete tooplan pro zotavení po havárii mezi oblastmi Azure sami:

- Potřebujete toomeet předpisy pro konkrétní aplikace a úlohy, které vyžadují kontinuity podnikových procesů a strategie po havárii (BCDR).
- Chcete hello možnost tooprotect a obnovení virtuálních počítačů Azure podle obchodních rozhodnutí a to není pouze podle integrované funkce Azure.
- Potřebujete v souladu s potřebám vaší firmy a dodržování předpisů bez dopadu na produkční tootest převzetí služeb při selhání a obnovení.
- Potřebují toofail přes toohello oblasti obnovení v případě havárie hello a bezproblémově nezdaří back toohello původní zdrojový oblast.

Pomocí Site Recovery pro virtuální počítač Azure Azure replikace toohelp provést tyto úlohy.


## <a name="why-use-site-recovery"></a>Proč používat Site Recovery?      

Site Recovery poskytuje jednoduchý způsob tooreplicate virtuálních počítačích Azure mezi oblastmi:

- **Automatické nasazení**. Na rozdíl od model replikace aktivní aktivní není nutné pro složité a nákladné infrastruktury v sekundární oblasti hello. Když povolíte replikaci, Site Recovery ve automaticky vytvoří hello požadované prostředky hello cílová oblast, na základě nastavení oblasti zdroje.
- **Řízení oblasti**. Pomocí Site Recovery můžete replikovat z libovolné oblasti tooany oblasti v rámci kontinentě. Toto porovnání s přístupem pro čtení geograficky redundantní úložiště, která asynchronně replikuje mezi standard [spárovat oblasti](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) pouze. Geograficky redundantní úložiště s přístupem pro čtení poskytuje přístup jen pro čtení toohello data v hello cílová oblast.
- **Automatizované replikace**. Site Recovery poskytuje automatizované průběžnou replikaci. Převzetí služeb při selhání a navrácení služeb po obnovení můžete spustit s jedním kliknutím.
- **RTO a plánovaný bod obnovení**. Site Recovery využívá výhod hello Azure síťové infrastruktuře propojující oblasti tookeep RTO a velmi nízké RPO.
- **Testování**. Cvičení zotavení po havárii můžete spustit s na vyžádání testovací převzetí služeb při selhání podle potřeby, aniž by to ovlivňovalo produkční zatížení nebo probíhající replikace.
- **Plány obnovení**. Můžete použít obnovení plány tooorchestrate převzetí služeb při selhání a navrácení služeb po obnovení hello celé aplikace běžící na několika virtuálních počítačích. Funkce plán obnovení Hello má bohaté prvotřídní integrace s runbooky služby Azure automation.


## <a name="deployment-summary"></a>Souhrn nasazení

Zde je uveden seznam, co potřebujete toodo tooset replikaci virtuálních počítačů mezi oblastmi Azure:

1. Vytvořte trezor služby Recovery Services. Trezor Hello obsahuje nastavení konfigurace a orchestruje replikaci.
2. Povolení replikace pro virtuální počítače Azure hello.
3. Spusťte toomake testovací převzetí služeb při selhání, se, zda vše funguje podle očekávání.

>[!IMPORTANT]
>
> Můžete zkontrolovat hello [matici podpory pro replikaci virtuálního počítače Azure](./site-recovery-support-matrix-azure-to-azure.md).

>[!IMPORTANT]
>
> Informace o tom, jak tooconfigure hello požadované odchozí připojení k síti pro virtuální počítače Azure Site Recovery replikace najdete v tématu hello [sítě pokyny dokumentu](./site-recovery-azure-to-azure-networking-guidance.md).

### <a name="before-you-start"></a>Než začnete

* Azure uživatelský účet potřebuje toohave určité [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikaci virtuálního počítače Azure.
* Vaše předplatné Azure musí být povoleno toocreate virtuální počítače v cílovém umístění hello, které chcete toouse jako oblast pro obnovení po havárii hello. Obraťte se na podporu tooenable hello požadované kvóty.

## <a name="create-a-recovery-services-vault"></a>Vytvoření trezoru Služeb zotavení

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> Doporučujeme vytvořit trezor služeb zotavení hello v hello umístění, kam má tooreplicate vaše virtuální počítače. Například pokud cílové umístění je hello střed USA, vytvořte trezor hello v **střed USA**.

## <a name="enable-replication"></a>Povolení replikace

V **trezory služeb zotavení**, klikněte na název trezoru hello. V hello trezoru, klikněte na tlačítko hello **+ replikovat** tlačítko.

### <a name="step-1-configure-hello-source"></a>Krok 1. Nakonfigurujte zdroj hello
1. V **zdroj**, vyberte **Azure - PREVIEW**.
2. V **umístění zdroje**, vyberte zdroj hello oblast Azure, kde vaše virtuální počítače jsou aktuálně spuštěné.
3. Model nasazení vyberte hello virtuální počítače: **Resource Manager** nebo **Classic**.
4. Vyberte hello **zdrojové skupiny prostředků** pro virtuální počítače správce prostředků nebo **Cloudová služba** pro klasické virtuální počítače.

    ![Nakonfigurujte zdroj hello](./media/site-recovery-azure-to-azure/source.png)

### <a name="step-2-select-virtual-machines"></a>Krok 2. Vyberte virtuální počítače

* Vyberte virtuální počítače hello tooreplicate a pak klikněte na **OK**.

    ![Vyberte virtuální počítače](./media/site-recovery-azure-to-azure/vms.png)

### <a name="step-3-configure-settings"></a>Krok 3. Konfigurace nastavení

1. Výchozí hello toooverride cíle nastavení a zadejte nastavení hello podle vaší volby, klikněte na tlačítko **přizpůsobit**. Další informace najdete v tématu [přizpůsobit cílové prostředky](site-recovery-replicate-azure-to-azure.md##customize-target-resources).

    ![Konfigurace nastavení](./media/site-recovery-azure-to-azure/settings.png)


2. Site Recovery ve výchozím nastavení vytvoří zásadu replikace, která přebírá snímky konzistentní aplikace každé 4 hodiny a uchovává body obnovení po dobu 24 hodin. toocreate zásad s různá nastavení, klikněte na tlačítko **přizpůsobit** další příliš**zásady replikace**.

    ![Úpravy zásad](./media/site-recovery-azure-to-azure/customize-policy.png)

3. Klikněte na tlačítko toostart zřizování hello cílové prostředky, **vytvoření cílové prostředky**. Zřizování trvá minutu. Nemáte zavřete okno hello při zřizování nebo potřebujete toostart než.

4. tootrigger replikaci hello vybraný virtuální počítač, klikněte na tlačítko **povolit replikaci**.

5. Můžete sledovat průběh hello **povolení ochrany** úlohy v **nastavení** > **úlohy** > **úlohy Site Recovery**.

6. V **nastavení** > **replikované položky**, můžete zobrazit stav hello virtuálních počítačů a hello probíhá počáteční replikace. Klikněte na tlačítko hello virtuálních počítačů toodrill dolů do jeho nastavení.

## <a name="run-a-test-failover"></a>Spuštění testovacího převzetí služeb při selhání

Po nastavení všechno spustit toomake testovací převzetí služeb při selhání, zda vše funguje podle očekávání:

1. toofail přes jeden počítač, v **nastavení** > **replikované položky**, klikněte na tlačítko hello virtuálních počítačů **+ testovací převzetí služeb při selhání** ikonu.

2. plánování toofail přes obnovení, **nastavení** > **plány obnovení**, klikněte pravým tlačítkem na hello plán **testovací převzetí služeb při selhání**. plán obnovení toocreate [postupujte podle těchto pokynů](site-recovery-create-recovery-plans.md). 

3. V **testovací převzetí služeb při selhání**, vyberte připojení hello cílový virtuální síť Azure toowhich virtuální počítače Azure po převzetí služeb při selhání hello.

4. toostart hello převzetí služeb při selhání, klikněte na tlačítko **OK**. tootrack průběhu, klikněte na virtuální počítač tooopen hello jeho vlastnosti. Nebo můžete kliknout na hello **testovací převzetí služeb při selhání** úlohy v název trezoru hello > **nastavení** > **úlohy** > **úlohy Site Recovery**.

5. Po hello dokončení převzetí služeb při selhání, hello repliky počítač Azure se zobrazí v hello portálu Azure > **virtuální počítače**. Zajistěte, aby byl tento hello virtuálních počítačů hello odpovídající velikost, byl připojený toohello příslušnou síť, a zda je spuštěna.

6. Klikněte na tlačítko toodelete hello virtuálních počítačů, které byly vytvořeny během hello testovací převzetí služeb při selhání, **vyčistit testovací převzetí služeb při selhání** na hello replikované položky nebo hello plánu obnovení. V **poznámky**, zaznamenejte a uložte jakékoli připomínky související s hello testovací převzetí služeb při selhání. 

[Další informace](site-recovery-test-failover-to-azure.md) o testovací převzetí služeb při selhání.


## <a name="next-steps"></a>Další kroky

Po testovací nasazení hello:

- [Další informace](site-recovery-failover.md) o různých typech převzetí služeb při selhání a jak toorun je.
- Další informace o [pomocí plánů obnovení](site-recovery-create-recovery-plans.md) tooreduce RTO.
