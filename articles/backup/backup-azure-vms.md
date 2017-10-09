---
title: "aaaBack do trezoru záloh tooa classic nasazené virtuální počítače Azure | Microsoft Docs"
description: "Zjistit, zaregistrujte a zálohování virtuálních počítačů s tyto postupy pro zálohování virtuálních počítačů Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "zálohování virtuálních počítačů; zálohování virtuálního počítače; zálohování a zotavení po havárii; zálohování virtuálních počítačů"
ms.assetid: c0ab5469-65fd-4af5-ae9b-f5d183f82ce8
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: 048e32d9b2bd5bdd7a125225a71a6d805bb4fbd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-azure-virtual-machines-classic-portal"></a>Vytvořte zálohu virtuálních počítačích Azure (klasický portál)
> [!div class="op_single_selector"]
> * [Zálohování trezor služeb tooRecovery virtuální počítače](backup-azure-arm-vms.md)
> * [Zálohování virtuálních počítačů tooBackup trezoru](backup-azure-vms.md)
>
>

Tento článek obsahuje hello postupy pro zálohování úložiště záloh tooa Classic nasadit virtuální počítač Azure (VM). Existuje několik úloh, které potřebujete tootake znak c/o předtím, než můžete zálohovat virtuální počítač Azure. Pokud jste již neučinili Ano, dokončení hello [požadavky](backup-azure-vms-prepare.md) tooprepare prostředí pro zálohování virtuálních počítačů.

Další informace najdete v tématu články hello na [plánování infrastruktury zálohování virtuálních počítačů v Azure](backup-azure-vms-introduction.md) a [virtuální počítače Azure](https://azure.microsoft.com/documentation/services/virtual-machines/).

> [!NOTE]
> Azure obsahuje dva modely nasazení pro vytváření a práci s prostředky: [Resource Manager a Classic](../azure-resource-manager/resource-manager-deployment-model.md). Úložiště záloh lze chránit pouze virtuálních počítačů nasazených Classic. Nelze chránit virtuálních počítačů nasazených Resource Manager pomocí úložiště záloh. V tématu [zálohování virtuálních počítačů trezor služeb tooRecovery](backup-azure-arm-vms.md) podrobnosti o práci se službou obnovení trezory.
>
>

Zálohování virtuálních počítačů Azure zahrnuje tři základní kroky:

![Tři kroky tooback zálohu virtuálního počítače Azure IaaS](./media/backup-azure-vms/3-steps-for-backup.png)

> [!NOTE]
> Zálohování virtuálních počítačů je místní proces. Nelze zálohovat virtuální počítače v jedné oblasti úložiště tooa záloh, které v jiné oblasti. Proto je nutné vytvořit úložiště záloh v každé oblasti Azure, kde existují virtuální počítače, které se bude zálohovat.
>
> [!IMPORTANT]
> Od března 2017 se už můžete trezory Backup portálu classic toocreate hello.
> Teď můžete upgradovat vaše trezory služeb tooRecovery trezory Backup. Podrobnosti najdete v tématu hello článku [upgradu tooa trezoru zálohování trezor služeb zotavení](backup-azure-upgrade-backup-to-recovery-services.md). Společnost Microsoft doporučuje tooupgrade zálohování trezory tooRecovery trezory služeb.<br/> Po 15 říjen 2017 nemůžete použít trezory Backup toocreate prostředí PowerShell. **Do 1. listopadu 2017:**
>- Všechny zbývající trezory Backup bude automaticky upgradovaný tooRecovery trezory služeb.
>- Můžete nebudou moct tooaccess zálohovaných dat na portálu classic hello. Místo toho použijte hello Azure portálu tooaccess zálohovaných dat v trezory služeb zotavení.
>

## <a name="step-1---discover-azure-virtual-machines"></a>Krok 1 – zjistit virtuální počítače Azure
tooensure žádné nové předplatné přidané toohello virtuální počítače (VM) jsou identifikovány před registrací, spusťte proces zjišťování hello. Hello proces se dotáže Azure hello seznam virtuálních počítačů v rámci předplatného hello, společně s dalšími informacemi, jako třeba název hello cloudové služby a oblast hello.

1. Přihlaste se toohello [portál Classic](http://manage.windowsazure.com/)
2. V seznamu hello služeb Azure, klikněte na **služeb zotavení** tooopen hello seznamu trezorů zálohování a obnovení lokality.
    ![Otevřít trezor seznamu](./media/backup-azure-vms/choose-vault-list.png)
3. V seznamu hello trezory Backup vyberte hello trezoru tooback zálohu virtuálního počítače.

    Pokud je to nový trezor hello portál otevře toohello **rychlý Start** stránky.

    ![Otevřete registrovaná položky nabídky](./media/backup-azure-vms/vault-quick-start.png)

    Pokud byl dříve nakonfigurován hello trezoru, hello portál ho otevře nabídky toohello naposledy použili.
4. V nabídce trezoru hello (u hello horní části stránky hello), klikněte na tlačítko **registrované položky**.

    ![Otevřete registrovaná položky nabídky](./media/backup-azure-vms/vault-menu.png)
5. Z hello **typ** nabídce vyberte možnost **virtuální počítač Azure**.

    ![Výběr úlohy](./media/backup-azure-vms/discovery-select-workload.png)
6. Klikněte na tlačítko **DISCOVER** v hello dolní části stránky hello.
    ![Tlačítko Vyhledat](./media/backup-azure-vms/discover-button-only.png)

    proces zjišťování Hello může trvat několik minut hello virtuální počítače jsou se v tabulce. Je oznámení dole hello úvodní obrazovka, která vás informuje, zda je spuštěn proces hello.

    ![Vyhledání virtuálních počítačů](./media/backup-azure-vms/discovering-vms.png)

    Hello oznámení změny po dokončení procesu hello. Pokud proces zjišťování hello nenalezl hello virtuální počítače, nejdříve se ujistěte, že existují hello virtuálních počítačů. Pokud existují hello virtuálních počítačů, ujistěte se, hello virtuální počítače jsou v hello stejné oblasti jako trezor záloh hello. Pokud hello virtuálních počítačů existují a jsou ve stejné oblasti text hello, ujistěte se, že hello virtuální počítače ještě nejsou registrované tooa úložiště záloh. Pokud je virtuální počítač přiřazený tooa úložiště záloh není k dispozici toobe přiřazené tooother záloh.

    ![Vyhledávání dokončeno](./media/backup-azure-vms/discovery-complete.png)

    Po zjištění nové položky hello přejděte tooStep 2 a zaregistrujte virtuální počítače.

## <a name="step-2---register-azure-virtual-machines"></a>Krok 2 – registrace virtuálních počítačů Azure
Zaregistrovat tooassociate virtuální počítač Azure se službou hello služby zálohování Azure. Obvykle se jedná o jednorázové aktivity.

1. Přejděte toohello zálohy trezoru pod **služeb zotavení** v hello portálu Azure a potom klikněte na **registrované položky**.
2. Vyberte **virtuální počítač Azure** z rozevírací nabídky hello.

    ![Výběr úlohy](./media/backup-azure-vms/discovery-select-workload.png)
3. Klikněte na tlačítko **ZAREGISTROVAT** v hello dolní části stránky hello.
    ![Tlačítko Registrovat](./media/backup-azure-vms/register-button-only.png)
4. V hello **registrovat položky** místní nabídky, vyberte hello virtuálních počítačů, které chcete tooregister. Pokud existují dvě nebo více virtuálních počítačů s hello stejný název, použijte hello cloudové služby toodistinguish mezi nimi.

   > [!TIP]
   > Najednou může být registrováno více virtuálních počítačů.
   >
   >

    Pro každý virtuální počítač, který jste vybrali, se vytvoří úloha.
5. Klikněte na tlačítko **zobrazit úlohy** v hello oznámení toogo toohello **úlohy** stránky.

    ![Registrace úlohy](./media/backup-azure-vms/register-create-job.png)

    virtuální počítač Hello se také zobrazí v hello seznamu registrovaných položek společně hello stav operace Registrování hello.

    ![Stav registrace 1](./media/backup-azure-vms/register-status01.png)

    Po dokončení operace hello hello stav se změní tooreflect hello *zaregistrován* stavu.

    ![Stav registrace 2](./media/backup-azure-vms/register-status02.png)

## <a name="step-3---protect-azure-virtual-machines"></a>Krok 3 – chránit virtuální počítače Azure
Nyní můžete nastavit zásadu zálohování a uchovávání hello virtuálního počítače. Více virtuálních počítačů může být chráněn pomocí jedné chránit akce.

Azure trezory Backup vytvořené po květen 2015 dodává s výchozí zásady integrovaný do trezoru hello. Tato výchozí zásada se dodává s výchozí uchování 30 dnů a plán zálohování jednou denně.

1. Přejděte toohello zálohy trezoru pod **služeb zotavení** v hello portálu Azure a potom klikněte na **registrované položky**.
2. Vyberte **virtuální počítač Azure** z rozevírací nabídky hello.

    ![Výběr úlohy v portálu](./media/backup-azure-vms/select-workload.png)
3. Klikněte na tlačítko **CHRÁNIT** v hello dolní části stránky hello.

    Hello **Průvodce ochranou položek** se zobrazí. Průvodce Hello je uveden pouze virtuální počítače, které jsou zaregistrované a nechráněné. Vyberte hello virtuálních počítačů, které chcete tooprotect.

    Pokud existují dvě nebo více virtuálních počítačů s hello stejný název, použijte hello cloudové služby toodistinguish mezi virtuálními počítači hello.

   > [!TIP]
   > Najednou můžete chránit více virtuálních počítačů.
   >
   >

    ![Škálování konfigurace ochrany](./media/backup-azure-vms/protect-at-scale.png)

4. Vyberte **plán zálohování** tooback až hello virtuálních počítačů, které jste vybrali. Můžete vybrat z existující sady zásad nebo zadejte nový.

    Ke každé zásadě zálohování může být přidružených více virtuálních počítačů. Však hello virtuálního počítače lze přidružit pouze s jednu zásadu v libovolném bodě v čase.

    ![Ochrana pomocí nové zásady](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > Zásada zálohování obsahuje schéma uchovávání pro hello naplánované zálohování. Pokud vyberete existující zásady zálohování, nelze změnit možnosti uchovávání hello v dalším kroku hello.
   >
   >

5. Vyberte **rozsah uchování** tooassociate s hello zálohy.

    ![Chránit pomocí flexibilní uchování](./media/backup-azure-vms/policy-retention.png)

    Zásady uchovávání informací určuje hello dobu pro uložení zálohy. Můžete zadat různé zásady uchovávání informací na základě při pořízení zálohy hello. Například bod zálohy pořizovat denně (který slouží jako bod obnovení provozní) může být zachována 90 dní. Porovnání bod zálohy prováděné na konci hello každé čtvrtletí (pro účely auditu) může být nutné toobe zachovají pro mnoho měsíců nebo let.

    ![Virtuální počítač je zálohovaný s bodem obnovení](./media/backup-azure-vms/long-term-retention.png)

    Na tomto obrázku příklad:

   * **Denní zásady uchovávání informací**: zálohy pořizovat denně se uchovávají po dobu 30 dnů.
   * **Týdenní zásady uchovávání informací**: zálohy pořízené každý týden v neděli zachovány 104 týdny.
   * **Měsíční zásady uchovávání informací**: zálohy pořízené hello poslední neděle každý měsíc zachovány po dobu 120 měsíců.
   * **Roční zásady uchovávání informací**: zálohy pořízené hello První neděle každých leden se zachovají pro 99 let.

     Úloha se vytvoří zásady ochrany hello tooconfigure a přidružit zásady toothat hello virtuálních počítačů pro každý virtuální počítač, který jste vybrali.
6. seznam hello tooview **Konfigurace ochrany** úlohy v nabídce hello trezory, klikněte na tlačítko **úlohy** a vyberte **Konfigurace ochrany** z hello **operace**  filtru.

    ![Úloha konfigurace ochrany](./media/backup-azure-vms/protect-configureprotection.png)

## <a name="initial-backup"></a>Prvotní zálohování
Jakmile hello virtuální počítač je chráněný pomocí zásad, se zobrazí v části hello **chráněné položky** karta s hello stav *chráněno – (čekání na prvotní zálohování)*. Ve výchozím nastavení, je prvním plánovaným zálohováním hello hello *prvotní zálohování*.

hello prvotní zálohování tootrigger okamžitě po konfiguraci ochrany:

1. Na konci hello hello **chráněné položky** klikněte na tlačítko **zálohovat nyní**.

    Hello služba Azure Backup vytvoří úlohu zálohování pro operaci prvotního zálohování hello.
2. Klikněte na tlačítko hello **úlohy** kartě tooview hello seznamu úloh.

    ![Probíhá zálohování](./media/backup-azure-vms/protect-inprogress.png)

> [!NOTE]
> Během operace zálohování hello vydává hello služba Azure Backup příkaz toohello rozšířením zálohování v každé virtuální počítač tooflush všechny zápisu úlohy a pořízení konzistentního snímku.
>
>

Po prvotní zálohování hello dokončení, stav hello hello virtuálního počítače v hello **chráněné položky** karta je *chráněné*.

![Virtuální počítač je zálohovaný s bodem obnovení](./media/backup-azure-vms/protect-backedupvm.png)

## <a name="viewing-backup-status-and-details"></a>Zobrazení stavu zálohy a podrobnosti
Jakmile chráněný, počet virtuálních počítačů hello také nárůstu hello **řídicí panel** stránky Souhrn. Hello **řídicí panel** stránka zobrazuje také počet hello úlohy z hello posledních 24 hodin, které byly *úspěšné*, mají *se nezdařilo*a jsou *v průběhu*. Na hello **úlohy** stránky, použijte hello **stav**, **operace**, nebo **z** a **k** toofilter nabídky Hello úlohy.

![Stav služby backup v stránka řídicího panelu](./media/backup-azure-vms/dashboard-protectedvms.png)

Hodnoty v řídicím panelu hello se aktualizuje každých 24 hodin.

## <a name="troubleshooting-errors"></a>Řešení potíží s chybami
Pokud narazíte na problémy při zálohování virtuálního počítače, podívejte se na hello [článku Poradce při potížích virtuálních počítačů](backup-azure-vms-troubleshoot.md) nápovědu.

## <a name="next-steps"></a>Další kroky
* [Správa a monitorování virtuálních počítačů](backup-azure-manage-vms.md)
* [Obnovení virtuálních počítačů](backup-azure-restore-vms.md)
