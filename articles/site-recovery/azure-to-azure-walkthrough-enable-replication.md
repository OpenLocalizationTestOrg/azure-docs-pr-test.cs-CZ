---
title: "aaaEnable replikace pro virtuální počítače Azure tooanother oblast Azure s Azure Site Recovery | Microsoft Docs"
description: "Shrnuje kroky hello nutné tooenable replikace tooanother oblast Azure pro virtuální počítače Azure pomocí služby Azure Site Recovery hello"
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
ms.openlocfilehash: 2fa03db45a18ccb8b9f31ed05589be0dd6d5f031
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-enable-replication-for-azure-vms"></a>Krok 5: Povolení replikace pro virtuální počítače Azure


Po nastavení [trezor služeb zotavení](azure-to-azure-walkthrough-vault.md), použijte tento článek tooenable replikace virtuálních počítačů (VM), tooanother oblast Azure, s [Azure Site Recovery](site-recovery-overview.md). tooenable replikace, můžete nastavit zdroje a cíle, ověřte hello zásadu replikace a vyberte virtuální počítače chcete tooreplicate.

- Po dokončení hello článek, virtuální počítače Azure musí být replikace toohello sekundární oblasti Azure.
- Odeslat všechny komentáře v dolní části hello tohoto článku nebo pokládání dotazů v hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)

>[!NOTE]
>
> Azure replikace virtuálního počítače je aktuálně ve verzi preview.


## <a name="select-hello-source"></a>Vyberte zdroj hello 

1. V trezory služeb zotavení, klikněte na název trezoru hello > **+ replikovat**.
2. V **zdroj**, vyberte **Azure - PREVIEW**.
2. V **umístění zdroje**, vyberte zdroj hello oblast Azure, kde vaše virtuální počítače jsou aktuálně spuštěné.
3. Vyberte hello **modelu nasazení virtuálního počítače Azure** pro virtuální počítače: **Resource Manager** nebo **Classic**.
4. Vyberte hello **zdrojové skupiny prostředků** pro správce prostředků virtuálních počítačů, nebo **Cloudová služba** pro klasické virtuální počítače.
5. Klikněte na tlačítko **OK** toosave hello nastavení.

    ![Nakonfigurujte zdroj hello](./media/azure-to-azure-walkthrough-enable-replication/source.png)

## <a name="select-hello-vms"></a>Vyberte virtuální počítače hello

Site Recovery načte seznam hello, které virtuální počítače spojené s hello předplatného a skupiny nebo cloudové služby prostředků.

1. V **virtuální počítače**, vyberte virtuální počítače hello chcete tooreplicate.
2. Klikněte na **OK**.

    ![Vyberte virtuální počítače](./media/azure-to-azure-walkthrough-enable-replication/vms.png)


## <a name="configure-settings"></a>Konfigurace nastavení

Site Recovery poskytuje výchozí nastavení pro cílová oblast hello (podle nastavení oblasti zdroje hello) a zásady replikace hello:

   - **Cílové umístění**: cílová oblast hello chcete toouse pro zotavení po havárii. Doporučujeme vám, že hello cílové umístění odpovídá hello umístění hello trezoru Site Recovery.
   - **Cílová skupina prostředků**: toowhich skupiny prostředků virtuálních počítačů Azure ve hello cílová oblast bude patřit po převzetí služeb při selhání. Ve výchozím nastavení Site Recovery vytvoří novou skupinu prostředků v hello cílová oblast s příponou "Automatické obnovení systému". 
   - **Cílová virtuální síť**: hello sítě, ve které virtuálních počítačích Azure v hello cílová oblast budou umístěné po převzetí služeb při selhání. Ve výchozím nastavení Site Recovery vytvoří nové virtuální sítě (a podsítě) v oblasti cíl hello s příponou "Automatické obnovení systému". Tato síť je namapované tooyour zdrojovou síť. Všimněte si, že po převzetí služeb při selhání virtuálního počítače můžete přiřadit konkrétní IP adresu, pokud potřebujete tooretain hello stejné IP adres v hello zdrojové a cílové umístění. 
   - **Účty úložiště mezipaměti**: Site Recovery používá účet úložiště v oblasti zdroj hello. Změny na zdrojové virtuální počítače se budou odesílat toothis účtu před replikace toohello cílové umístění. 
   - **Cíl účty úložiště**: ve výchozím nastavení, Site Recovery vytvoří nový účet úložiště v hello cílová oblast toomirror hello zdrojový účet úložiště virtuálního počítače.
   -  **Cílové skupiny dostupnosti**: ve výchozím nastavení vytvoří Site Recovery novou skupinou dostupnosti ve hello cílová oblast s příponou "Automatické obnovení systému" hello. 
   - **Název zásady replikace**: název zásady.
   - **Uchování bodu obnovení**: ve výchozím nastavení Site Recovery zachová body obnovení po dobu 24 hodin. Můžete nakonfigurovat hodnotu v rozmezí 1 až 72 hodin.
   - **Frekvence snímkování konzistentní aplikace vzhledem**: ve výchozím nastavení trvá Site Recovery snímky konzistentní s aplikací každé 4 hodiny. Můžete nakonfigurovat libovolnou hodnotu mezi 1 a 12 hodin. Data se replikují nepřetržitě:
    - Body obnovení konzistentní při selhání zachování konzistentních dat zápisu pořadí při vytvoření. Tento typ bod obnovení je obvykle stačí, pokud je vaše aplikace navrženou toorecover z havárie bez nekonzistenci dat.
    - Body obnovení konzistentní při selhání jsou generovány každých několik minut. Pomocí těchto toofail body obnovení a obnovit poskytuje virtuální počítače obnovení bodu cíl (RPO) v pořadí hello minut.
    - Body obnovení konzistentní vzhledem k aplikaci (v přidání toowrite pořadí konzistence) zkontrolujte, zda spuštěné aplikace dokončení všech operací a vyprázdní vyrovnávací paměti toodisk (uvedení aplikace). Doporučujeme že použít tyto body obnovení pro databáze aplikace, jako je například SQL Server, Oracle a Exchange.
        
    ![Konfigurace nastavení](./media/azure-to-azure-walkthrough-enable-replication/settings.png)


### <a name="modify-settings"></a>Upravit nastavení

Pokud chcete nastavení zásad toomodify cíle a replikace, hello následující:

1. tooview nebo upravit nastavení cíle, klikněte na **nastavení**.
2. toooverride hello výchozí cílový nastavení, klikněte na tlačítko **přizpůsobit**. Můžete zadat cílová skupina prostředků, virtuální sítě, skupinu dostupnosti a cílový účet úložiště. Skupiny dostupnosti můžete přidat pouze pokud virtuální počítače jsou součástí sady v oblasti zdroj hello.

    ![Konfigurace nastavení](./media/azure-to-azure-walkthrough-enable-replication/customize-target.png)

3. nastavení replikace toooverride pro body obnovení a snímky konzistentní s aplikací, klikněte na tlačítko **přizpůsobit** další příliš**zásady replikace**.
 
    ![Konfigurace nastavení](./media/azure-to-azure-walkthrough-enable-replication/customize-policy.png)

4. Klikněte na tlačítko toostart zřizování hello cílové prostředky, **vytvoření cílové prostředky**. Zřizování trvá minutu. Nemáte zavřete okno hello při zřizování, nebo budete mít toostart přes.




## <a name="enable-replication"></a>Povolení replikace

1. V **nastavení**, klikněte na tlačítko **povolit replikaci**. To umožňuje počáteční replikace hello virtuálních počítačů, které jste vybrali. Počáteční replikace stavu může trvat některé toorefresh čas. Klikněte na tlačítko **aktualizovat** tooget hello nejnovější stav.

2. Můžete sledovat průběh hello **povolení ochrany** úlohy v **nastavení** > **úlohy** > **úlohy Site Recovery**.

3. V **nastavení** > **replikované položky**, můžete zobrazit stav hello virtuálních počítačů a hello probíhá počáteční replikace. Klikněte na tlačítko hello virtuálních počítačů toodrill dolů do jeho nastavení.



## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 6: spustit testovací převzetí služeb](azure-to-azure-walkthrough-test-failover.md)
