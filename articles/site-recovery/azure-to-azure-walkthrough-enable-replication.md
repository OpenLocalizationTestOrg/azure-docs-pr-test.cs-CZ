---
title: "Povolte replikaci pro virtuální počítače Azure na jinou oblast Azure s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky nutné k povolení replikace jiné oblasti Azure pro virtuální počítače Azure pomocí služby Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a309644f-d36b-4188-bba7-ad45a2d9bede
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 8/01/2017
ms.author: raynew
ms.openlocfilehash: f426ba4341e61d3c7da820d7d5097b217e94ca0e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="step-5-enable-replication-for-azure-vms"></a>Krok 5: Povolení replikace pro virtuální počítače Azure


Po nastavení [trezor služeb zotavení](azure-to-azure-walkthrough-vault.md), použijte tento článek k povolení replikace virtuálních počítačů (VM), do jiné oblasti Azure, s [Azure Site Recovery](site-recovery-overview.md). Povolení replikace, můžete nastavit zdroje a cíle, ověřte zásady replikace a vyberte virtuální počítače, které chcete replikovat.

- Po dokončení článek, virtuální počítače Azure by měla replikace do sekundární oblasti Azure.
- Odeslat všechny komentáře v dolní části tohoto článku nebo pokládání dotazů v [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)

>[!NOTE]
>
> Azure replikace virtuálního počítače je aktuálně ve verzi preview.


## <a name="select-the-source"></a>Vyberte zdroj 

1. V trezory služeb zotavení, klikněte na název trezoru > **+ replikovat**.
2. V **zdroj**, vyberte **Azure - PREVIEW**.
2. V **umístění zdroje**, vyberte zdroj oblast Azure, kde vaše virtuální počítače jsou aktuálně spuštěné.
3. Vyberte **modelu nasazení virtuálního počítače Azure** pro virtuální počítače: **Resource Manager** nebo **Classic**.
4. Vyberte **zdrojové skupiny prostředků** pro správce prostředků virtuálních počítačů, nebo **Cloudová služba** pro klasické virtuální počítače.
5. Kliknutím na **OK** uložte nastavení.

    ![Nastavit zdroj](./media/azure-to-azure-walkthrough-enable-replication/source.png)

## <a name="select-the-vms"></a>Vyberte virtuální počítače

Site Recovery načte seznam virtuálních počítačů spojené s předplatného a skupiny nebo cloudové služby prostředků.

1. V **virtuální počítače**, vyberte virtuální počítače, které chcete replikovat.
2. Klikněte na **OK**.

    ![Vyberte virtuální počítače](./media/azure-to-azure-walkthrough-enable-replication/vms.png)


## <a name="configure-settings"></a>Konfigurace nastavení

Zřizuje obnovení lokality výchozí nastavení pro cílová oblast (podle nastavení oblasti zdroje) a zásady replikace:

   - **Cílové umístění**: cílová oblast, kterou chcete použít pro zotavení po havárii. Doporučujeme vám, že cílové umístění odpovídá umístění trezoru Site Recovery.
   - **Cílová skupina prostředků**: skupinu prostředků, ke které virtuální počítače Azure v cílové oblasti patřit po převzetí služeb při selhání. Ve výchozím nastavení Site Recovery vytvoří novou skupinu prostředků v cílové oblasti s příponou "Automatické obnovení systému". 
   - **Cílová virtuální síť**: sítě, ve které virtuálních počítačích Azure v cílové oblasti budou umístěné po převzetí služeb při selhání. Ve výchozím nastavení Site Recovery vytvoří nové virtuální sítě (a podsítě) v oblasti cíl s příponou "Automatické obnovení systému". Tato síť je namapována na zdrojovou síť. Všimněte si, že po převzetí služeb při selhání virtuálního počítače, můžete přiřadit konkrétní IP adresu, pokud je potřeba zachovat stejnou IP adresu ve zdrojové a cílové umístění. 
   - **Účty úložiště mezipaměti**: Site Recovery používá účet úložiště v oblasti zdroje. Změny na zdrojové virtuální počítače jsou odesílány tento účet, než replikaci do cílového umístění. 
   - **Cíl účty úložiště**: ve výchozím nastavení, Site Recovery vytvoří nový účet úložiště v oblasti cíl pro zrcadlení zdrojový účet úložiště virtuálního počítače.
   -  **Cílové skupiny dostupnosti**: ve výchozím nastavení vytvoří Site Recovery novou skupinou dostupnosti ve cílová oblast s příponou "Automatické obnovení systému". 
   - **Název zásady replikace**: název zásady.
   - **Uchování bodu obnovení**: ve výchozím nastavení Site Recovery zachová body obnovení po dobu 24 hodin. Můžete nakonfigurovat hodnotu v rozmezí 1 až 72 hodin.
   - **Frekvence snímkování konzistentní aplikace vzhledem**: ve výchozím nastavení trvá Site Recovery snímky konzistentní s aplikací každé 4 hodiny. Můžete nakonfigurovat libovolnou hodnotu mezi 1 a 12 hodin. Data se replikují nepřetržitě:
    - Body obnovení konzistentní při selhání zachování konzistentních dat zápisu pořadí při vytvoření. Tento typ bod obnovení je obvykle stačí, pokud vaše aplikace je navržená pro zotavit havárie bez nekonzistenci dat.
    - Body obnovení konzistentní při selhání jsou generovány každých několik minut. Pomocí těchto bodů obnovení pro převzetí služeb při selhání a obnovení virtuálních počítačů poskytuje obnovení bodu cíl (RPO) v řádu minut.
    - Body obnovení konzistentní vzhledem k aplikaci (kromě zápisu pořadí konzistence) zkontrolujte, zda spuštěné aplikace dokončení všech operací a vyprázdní vyrovnávací paměti na disk (uvedení aplikace). Doporučujeme že použít tyto body obnovení pro databáze aplikace, jako je například SQL Server, Oracle a Exchange.
        
    ![Konfigurace nastavení](./media/azure-to-azure-walkthrough-enable-replication/settings.png)


### <a name="modify-settings"></a>Upravit nastavení

Pokud chcete upravit nastavení zásad cíle a replikace, postupujte takto:

1. Chcete-li zobrazit nebo upravit nastavení cíle, klikněte na tlačítko **nastavení**.
2. Chcete-li přepsat výchozí nastavení cíl, klikněte na tlačítko **přizpůsobit**. Můžete zadat cílová skupina prostředků, virtuální sítě, skupinu dostupnosti a cílový účet úložiště. Skupiny dostupnosti můžete přidat pouze pokud virtuální počítače jsou součástí sady v oblasti zdroje.

    ![Konfigurace nastavení](./media/azure-to-azure-walkthrough-enable-replication/customize-target.png)

3. Chcete-li přepsat nastavení replikace pro body obnovení a snímky konzistentní s aplikací, klikněte na tlačítko **přizpůsobit** vedle **zásady replikace**.
 
    ![Konfigurace nastavení](./media/azure-to-azure-walkthrough-enable-replication/customize-policy.png)

4. Chcete-li spustit zřizování cílové prostředky, klikněte na tlačítko **vytvoření cílové prostředky**. Zřizování trvá minutu. Nemáte zavřete okno při zřizování, nebo budete muset začít znovu.




## <a name="enable-replication"></a>Povolení replikace

1. V **nastavení**, klikněte na tlačítko **povolit replikaci**. To umožňuje počáteční replikace virtuálních počítačů, které jste vybrali. Aktualizovat stav počáteční replikace může chvíli trvat. Klikněte na tlačítko **aktualizovat** a získat nejnovější stav.

2. Můžete sledovat průběh **povolení ochrany** úlohy v **nastavení** > **úlohy** > **úlohy Site Recovery**.

3. V **nastavení** > **replikované položky**, můžete zobrazit stav virtuálních počítačů a probíhá počáteční replikace. Klikněte na virtuální počítač můžete rozbalit jeho nastavení.



## <a name="next-steps"></a>Další kroky

Přejděte na [krok 6: spustit testovací převzetí služeb](azure-to-azure-walkthrough-test-failover.md)
