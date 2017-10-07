---
title: "zálohy virtuálních počítačů nasazených Resource Managerem aaaManage | Microsoft Docs"
description: "Zjistěte, jak toomanage a monitorování záloh virtuálních počítačů nasazených Resource Managerem"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: f3050283-d60f-472d-b464-cb844e70d67e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: trinadhk;markgal
ms.openlocfilehash: 241fc4b2a29c5cd8b8b0ee8efbf8ba4e96c6a7ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-virtual-machine-backups"></a>Správa záloh virtuálních počítačů Azure
> [!div class="op_single_selector"]
> * [Spravovat zálohy virtuálního počítače Azure](backup-azure-manage-vms.md)
> * [Spravovat zálohy Classic virtuálních počítačů](backup-azure-manage-vms-classic.md)
>
>

Tento článek obsahuje pokyny týkající se správy zálohování virtuálních počítačů a vysvětluje hello výstrahy zálohy informace k dispozici v hello řídicí panel portálu. Hello pokyny v tomto článku se vztahuje toousing virtuálních počítačů s trezory služeb zotavení. Tento článek nepopisuje hello vytváření virtuálních počítačů, ani se vysvětluje jak tooprotect virtuálních počítačů. Úvod do o ochraně nasazení Azure Resource Manager virtuálních počítačů v Azure k trezoru služeb zotavení, najdete v části [první pohled: zálohování virtuálních počítačů tooa trezor služeb zotavení](backup-azure-vms-first-look-arm.md).

## <a name="manage-vaults-and-protected-virtual-machines"></a>Správa úložišť a chráněné virtuální počítače
V hello portál Azure poskytuje panelu trezoru služeb zotavení hello přístup tooinformation o hello trezoru včetně:

* Hello poslední snímek zálohy, který je taky nejnovější bod obnovení hello < br\>
* zásady zálohování Hello < br\>
* Celková velikost všech snímků zálohy < br\>
* počet virtuálních počítačů, které jsou chráněné službou hello trezoru < br\>

Mnoho úlohy správy počítačů s zálohování virtuálních počítačů začínat otevírání hello trezoru hello řídicím panelu. Ale vzhledem k trezory můžou být použité tooprotect více položek (nebo víc virtuálních počítačů), tooview podrobnosti o konkrétní virtuální počítač, otevřete hello trezoru položek řídicího panelu. Hello následující postup ukazuje, jak tooopen hello *panelu trezoru* a pak pokračujte toohello *položky panelu trezoru*. V obou postupy, které bod jak tooadd hello trezoru a trezoru toohello položky řídicí panel Azure pomocí příkazu toodashboard hello kódu Pin je "tipy". Toodashboard kódu PIN je způsob vytvoření trezoru toohello zástupce nebo položky. Běžné příkazy můžete spustit také z hello zástupce.

> [!TIP]
> Pokud máte více řídicí panely a otevřete oken, pomocí posuvníku světlý blue hello dole hello hello okno tooslide hello řídicí panel Azure a zpět.
>
>

![Úplné zobrazení s posuvníku](./media/backup-azure-manage-vms/bottom-slider.png)

### <a name="open-a-recovery-services-vault-in-hello-dashboard"></a>Otevřete hello řídicím panelu trezoru služeb zotavení:
1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. V nabídce centra hello, klikněte na tlačítko **Procházet** a v hello seznamu prostředků zadejte **služeb zotavení**. Během zadávání hello seznamu filtrů podle vašeho zadání. Klikněte na **Trezor Recovery Services**.

    ![Vytvoření trezoru Recovery Services – krok 1](./media/backup-azure-manage-vms/browse-to-rs-vaults.png) <br/>

    Zobrazí se Hello seznamu trezorů služeb zotavení.

    ![Trezory seznam služeb zotavení ](./media/backup-azure-manage-vms/list-o-vaults.png) <br/>

   > [!TIP]
   > Pokud připnete trezoru toohello řídicího panelu Azure, je tento trezor okamžitě dostupné při otevření hello portálu Azure. toopin trezoru toohello řídicího panelu, v seznamu hello trezoru, klikněte pravým tlačítkem na hello trezoru a vyberte **Pin toodashboard**.
   >
   >
3. Hello seznamu trezorů vyberte trezor tooopen hello jeho řídicího panelu. Když vyberete hello trezoru, hello panelu trezoru a hello **nastavení** okno otevřít. V hello následující bitové kopie, hello **Contoso trezoru** zvýrazní řídicího panelu.

    ![Otevření panelu trezoru a okna nastavení](./media/backup-azure-manage-vms/full-view-rs-vault.png)

### <a name="open-a-vault-item-dashboard"></a>Otevřete položku řídicího panelu trezoru
V předchozím postupu hello otevřít hello panelu trezoru. tooopen hello trezoru položek řídicího panelu:

1. V panelu trezoru hello na hello **zálohování položek** dlaždici, klikněte na tlačítko **virtuální počítače Azure**.

    ![Otevřete položky zálohování dlaždice](./media/backup-azure-manage-vms/contoso-vault-1606.png)

    Hello **položky zálohování** okno uvádí hello poslední úloha zálohování pro každou položku. V tomto příkladu je jeden virtuální počítač, demovm-markgal chráněn tento trezor.  

    ![Dlaždice položky zálohování](./media/backup-azure-manage-vms/backup-items-blade.png)

   > [!TIP]
   > Pro usnadnění přístupu budete moct připnout trezoru položky toohello řídicího panelu Azure. toopin položku trezoru v seznamu položek hello trezoru, klikněte pravým tlačítkem na položku hello a vyberte **Pin toodashboard**.
   >
   >
2. V hello **zálohování položek** okně klikněte na tlačítko hello položky tooopen hello trezoru položek řídicího panelu.

    ![Dlaždice položky zálohování](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

    Hello trezoru položek řídicího panelu a jeho **nastavení** okno otevřít.

    ![Řídicí panel položky zálohování se okno nastavení](./media/backup-azure-manage-vms/item-dashboard-settings.png)

    Z řídicího panelu trezoru položky hello můžete provést celou řadu úloh správy klíčů, jako například:

   * změnit zásady nebo vytvořte nové zásady zálohování < br\>
   * Zobrazit body obnovení a zobrazí jejich stavu konzistence < br\>
   * zálohování na vyžádání virtuálního počítače < br\>
   * Zastavte ochranu virtuálních počítačů < br\>
   * Obnovte ochranu virtuálního počítače < br\>
   * odstranit záložní data (nebo bodu obnovení) < br\>
   * [obnovit záložní disky](backup-azure-arm-restore-vms.md#restore-backed-up-disks) < br\>

Pro hello následující postupy je výchozí bod hello hello trezoru položek řídicího panelu.

## <a name="manage-backup-policies"></a>Správa zásad zálohování
1. Na hello [položky panelu trezoru](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikněte na tlačítko **všechna nastavení** tooopen hello **nastavení** okno.

    ![Okno zásady zálohování](./media/backup-azure-manage-vms/all-settings-button.png)
2. Na hello **nastavení** okně klikněte na tlačítko **zálohování zásad** tooopen tohoto okna.

    V okně hello jsou uvedeny hello zálohování četnost a uchování podrobnosti rozsahu.

    ![Okno zásady zálohování](./media/backup-azure-manage-vms/backup-policy-blade.png)
3. Z hello **vyberte zásady zálohování** nabídky:

   * Vyberte jinou zásadu toochange zásady a klikněte na **Uložit**. nové zásady Hello je okamžitě použité toohello trezoru. < br\>
   * Vyberte zásadu, toocreate **vytvořit nový**.

     ![Záloha virtuálního počítače](./media/backup-azure-manage-vms/backup-policy-create-new.png)

     Pokyny pro vytvoření zásady zálohování, naleznete v části [definování zásad zálohování](backup-azure-manage-vms.md#defining-a-backup-policy).

[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

> [!NOTE]
> Při správě zásady zálohování, ujistěte se, zda text hello toofollow [osvědčené postupy](backup-azure-vms-introduction.md#best-practices) pro optimální výkon zálohování
>
>

## <a name="on-demand-backup-of-a-virtual-machine"></a>Zálohování na vyžádání virtuálního počítače
Na vyžádání můžete provést zálohování virtuálního počítače, jakmile je nakonfigurován pro ochranu. Pokud probíhá hello prvotní zálohování, zálohování na vyžádání vytvoří úplnou kopii hello virtuálního počítače v hello trezor služeb zotavení. Pokud dokončení prvotního zálohování hello odeslat zálohu na vyžádání pouze změny z předchozího snímku hello toohello trezor služeb zotavení. To znamená jsou následné zálohy vždy přírůstkové.

> [!NOTE]
> Hello rozsah uchování pro zálohu na vyžádání je hello uchování hodnota zadaná pro hello denního bodu zálohy v zásadách hello. Pokud je vybrána žádná denního bodu zálohy, se používá hello týdenního bodu zálohy.
>
>

tootrigger na vyžádání zálohování virtuálního počítače:

* Na hello [položky panelu trezoru](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikněte na tlačítko **zálohovat nyní**.

    ![Zálohování teď tlačítko.](./media/backup-azure-manage-vms/backup-now-button.png)

    portál Hello zajišťuje, že chcete toostart úlohu zálohování na vyžádání. Klikněte na tlačítko **Ano** úlohu zálohování toostart hello.

    ![Zálohování teď tlačítko.](./media/backup-azure-manage-vms/backup-now-check.png)

    Úloha zálohování Hello vytvoří bod obnovení. Hello uchování bodu obnovení hello je hello stejné jako rozsah uchování určená v zásadách hello spojené s virtuálním počítačem hello. průběh hello tootrack pro úlohu hello v hello panelu trezoru, klikněte na tlačítko hello **úlohy zálohování** dlaždici.  

## <a name="stop-protecting-virtual-machines"></a>Zastavte ochranu virtuálních počítačů
Pokud si zvolíte toostop ochranu virtuálního počítače, zobrazí se výzva, pokud chcete, aby se body obnovení tooretain hello. Existují dva způsoby toostop ochranu virtuálních počítačů:

* Zastavit všechny budoucí úlohy zálohování a odstranění všech bodů obnovení, nebo
* Zastavit všechny budoucí úlohy zálohování, ale ponechte hello bodů obnovení <br/>

Není k dispozici s náklady spojené s ponechat hello bodů obnovení v úložišti. Výhodou hello ponechat hello bodů obnovení je však, že později, můžete obnovit hello virtuálního počítače v případě potřeby. Informace o hello náklady a hello body obnovení, najdete v části hello [podrobnosti o cenách](https://azure.microsoft.com/pricing/details/backup/). Pokud si zvolíte toodelete všech bodů obnovení, nelze obnovit hello virtuálního počítače.

toostop ochranu pro virtuální počítač:

1. Na hello [položky panelu trezoru](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikněte na tlačítko **zastavení zálohování**.

    ![Zastavení zálohování tlačítko](./media/backup-azure-manage-vms/stop-backup-button.png)

    Otevře se okno zastavení zálohování Hello.

    ![Zastavení zálohování okno](./media/backup-azure-manage-vms/stop-backup-blade.png)
2. Na hello **zastavení zálohování** okně zvolte, zda tooretain nebo odstranění hello zálohovaná data. pole informace Hello poskytuje podrobnosti o svého výběru.

    ![Zastavení ochrany](./media/backup-azure-manage-vms/retain-or-delete-option.png)
3. Pokud jste zvolili tooretain hello zálohovaná data, přeskočte toostep 4. Pokud jste zvolili toodelete zálohovaná data, potvrďte mají úlohy zálohování hello toostop a odstranit hello body obnovení – název typu hello hello položky.

    ![Zastavit ověření](./media/backup-azure-manage-vms/item-verification-box.png)

    Pokud si nejste jisti hello položka názvu, pozastavte ukazatel myši nad hello vykřičník tooview hello název. Navíc je hello název položky hello pod **zastavení zálohování** hello horní části okna hello.
4. Volitelně můžete zadat **důvod** nebo **komentář**.
5. Úloha zálohování toostop hello hello aktuální položky, klikněte na tlačítko ![tlačítko zastavení zálohování](./media/backup-azure-manage-vms/stop-backup-button-blue.png)

    Oznámení umožňuje vědět, že byly zastaveny úlohy zálohování hello.

    ![Potvrďte zastavení ochrany](./media/backup-azure-manage-vms/stop-message.png)

## <a name="resume-protection-of-a-virtual-machine"></a>Pokračovat v ochraně virtuálního počítače
Pokud hello **zachovat Data zálohování** možnost jste vybrali při ochranu pro virtuální počítač hello byla zastavena, pak je možné tooresume ochrany. Pokud hello **odstranit Data zálohování** jste vybrali možnost a pak ochranu pro virtuální počítač hello nelze obnovit.

tooresume ochranu pro virtuální počítač hello

1. Na hello [položky panelu trezoru](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikněte na tlačítko **obnovit zálohu**.

    ![Pokračovat v ochraně](./media/backup-azure-manage-vms/resume-backup-button.png)

    Otevře se okno zásady zálohování Hello.

   > [!NOTE]
   > Při ochranu znovu hello virtuálního počítače, můžete zvolit jinou zásadu než hello zásada, ke které byl virtuální počítač původně chráněný.
   >
   >
2. Postupujte podle kroků hello v [spravovat zásady zálohování](backup-azure-manage-vms.md#manage-backup-policies) tooassign hello zásady pro virtuální počítač hello.

    Jakmile zásady zálohování hello je použité toohello virtuální počítač, zobrazí hello následující zprávou.

    ![Úspěšně chráněných virtuálních počítačů](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a>Odstranění dat zálohy
Odstraněním hello zálohovaná data související s virtuálním počítačem během hello **zastavení zálohování** úlohy, nebo můžete kdykoli po hello dokončení úlohy zálohování. I to může být výhodné toowait dny nebo týdny před odstraněním hello body obnovení. Na rozdíl od bodů obnovení, obnovení při odstraňování zálohovaná data, nemůžete vybrat konkrétní obnovení toodelete body. Pokud si zvolíte toodelete zálohovaných dat, odstraníte všechny body obnovení, které jsou přidružené k položce hello.

Hello následujícím postupu se předpokládá hello úlohy zálohování pro virtuální počítač hello byla zastavena nebo zakázána. Jakmile úloha zálohování hello je zakázána, hello **obnovit zálohu** a **odstranění zálohování** možnosti jsou dostupné na panelu položky hello trezoru.

![Opětovné spuštění a odstranění tlačítka](./media/backup-azure-manage-vms/resume-delete-buttons.png)

toodelete zálohování dat na virtuální počítač s hello *zálohy zakázané*:

1. Na hello [položky panelu trezoru](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klikněte na tlačítko **zálohování odstranit**.

    ![Typ virtuálního počítače](./media/backup-azure-manage-vms/delete-backup-buttom.png)

    Hello **odstranit Data zálohování** otevře se okno.

    ![Typ virtuálního počítače](./media/backup-azure-manage-vms/delete-backup-blade.png)
2. Název typu hello tooconfirm hello položku, kterou chcete body obnovení toodelete hello.

    ![Zastavit ověření](./media/backup-azure-manage-vms/item-verification-box.png)

    Pokud si nejste jisti hello položka názvu, pozastavte ukazatel myši nad hello vykřičník tooview hello název. Navíc je hello název položky hello pod **odstranit Data zálohování** hello horní části okna hello.
3. Volitelně můžete zadat **důvod** nebo **komentář**.
4. toodelete hello zálohování dat pro aktuální položku hello, klikněte na tlačítko ![tlačítko zastavení zálohování](./media/backup-azure-manage-vms/delete-button.png)

    Oznámení umožňuje vědět, že byl odstraněn hello zálohovaná data.

## <a name="next-steps"></a>Další kroky
Informace o opětovné vytvoření virtuálního počítače z bodu obnovení, podívejte se na [obnovení virtuálních počítačů Azure](backup-azure-restore-vms.md). Pokud potřebujete informace o ochraně virtuálních počítačů, přečtěte si [první pohled: zálohování virtuálních počítačů tooa trezor služeb zotavení](backup-azure-vms-first-look-arm.md). Informace o sledování událostí najdete v tématu [monitorování výstrahy pro virtuální počítač Azure zálohy](backup-azure-monitor-vms.md).
