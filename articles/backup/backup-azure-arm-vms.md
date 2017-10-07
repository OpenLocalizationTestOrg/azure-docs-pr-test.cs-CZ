---
title: "aaaBack virtuální počítače Azure | Microsoft Docs"
description: "Zjistit, zaregistrujte a zálohovat trezoru služeb zotavení tooa virtuální počítače Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "zálohování virtuálních počítačů; zálohování virtuálního počítače; zálohování a zotavení po havárii; zálohování virtuálních počítačů arm"
ms.assetid: 5c68481d-7be3-4e68-b87c-0961c267053e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/15/2017
ms.author: trinadhk;jimpark;markgal;
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a204a42726450a7fd89b5563a786b5070578b113
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-azure-virtual-machines-tooa-recovery-services-vault"></a>Zálohování virtuálních počítačů Azure trezor služeb zotavení tooa
> [!div class="op_single_selector"]
> * [Zálohování trezor služeb tooRecovery virtuální počítače](backup-azure-arm-vms.md)
> * [Zálohování virtuálních počítačů tooBackup trezoru](backup-azure-vms.md)
>
>

Tento článek podrobně popisuje, jak trezoru tooback zálohu virtuálních počítačích Azure (nasazení Resource Manager i nasazení Classic) tooa služeb zotavení. Většina práce hello pro zálohování virtuálních počítačů je hello přípravy. Před zahájením zálohování nebo chránit virtuální počítač, je nutné dokončit hello [požadavky](backup-azure-arm-vms-prepare.md) tooprepare prostředí pro ochranu virtuálních počítačů. Po dokončení hello požadavky, můžete zahájit hello operace zálohování tootake snímky virtuálního počítače.


[!INCLUDE [learn about backup deployment models](../../includes/backup-deployment-models.md)]

Další informace najdete v tématu články hello na [plánování infrastruktury zálohování virtuálních počítačů v Azure](backup-azure-vms-introduction.md) a [virtuální počítače Azure](https://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="triggering-hello-backup-job"></a>Aktivuje úlohu zálohování hello
zásady zálohování Hello přidružené k trezoru služeb zotavení hello definuje, jak často a kdy běží hello operace zálohování. Ve výchozím nastavení je prvním plánovaným zálohováním hello hello prvotní zálohování. Dokud prvotního zálohování hello hello stav poslední zálohy na hello **úlohy zálohování** ukazovat **upozornění (nedokončené prvotní zálohování)**.

![Zálohování čeká na zpracování](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

Pokud prvotní zálohování toobegin brzy, doporučujeme spustit **zálohovat nyní**. Hello následující postup spustí z panelu trezoru hello. Tento postup slouží ke spuštění úlohu prvotního zálohování hello po dokončení všech požadavků. Pokud již byl spuštěn hello úlohu prvotního zálohování, tento postup není k dispozici. Hello přidružené zásady zálohování určuje hello další úloha zálohování.  

toorun hello úlohu prvotního zálohování:

1. Na řídicím panelu trezoru hello, klikněte na číslo hello pod **zálohování položek**, nebo klikněte na tlačítko hello **zálohování položek** dlaždici. <br/>
  ![Ikona nastavení](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)

  Hello **zálohování položek** otevře se okno.

  ![Zálohování položek](./media/backup-azure-vms-first-look-arm/back-up-items-list.png)

2. Na hello **zálohování položek** okně, vyberte hello položky.

  ![Ikona nastavení](./media/backup-azure-vms-first-look-arm/back-up-items-list-selected.png)

  Hello **zálohování položek** seznamu otevře. <br/>

  ![Aktivovaná úloha zálohování.](./media/backup-azure-vms-first-look-arm/backup-items-not-run.png)

3. Na hello **zálohování položek** klikněte na symbol tří teček hello **...**  tooopen hello kontextové nabídky.

  ![Místní nabídka](./media/backup-azure-vms-first-look-arm/context-menu.png)

  Zobrazí se Hello kontextové nabídky.

  ![Místní nabídka](./media/backup-azure-vms-first-look-arm/context-menu-small.png)

4. V místní nabídce hello, klikněte na tlačítko **zálohovat nyní**.

  ![Místní nabídka](./media/backup-azure-vms-first-look-arm/context-menu-small-backup-now.png)

  Otevře se okno zálohovat nyní Hello.

  ![Zobrazí okno zálohovat nyní hello](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

5. Na hello zálohovat nyní okně klikněte na ikonu hello kalendáře, použijte hello kalendáře řízení tooselect hello poslední den tohoto bodu obnovení se zachovává a klikněte na tlačítko **zálohování**.

  ![Sada hello poslední den hello zálohovat nyní bod obnovení je zachován.](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  Nasazení oznámení umožňují vědět byla spuštěna úloha zálohování hello, a že můžete monitorovat průběh hello hello úlohy na stránce úloh zálohování hello. V závislosti na velikosti hello vašeho virtuálního počítače vytváření hello prvotní zálohování může chvíli trvat.

6. tooview nebo sledovat stav hello hello prvotní zálohování, na panelu trezoru hello na hello **úlohy zálohování** klikněte na dlaždici **v průběhu**.

  ![Dlaždice Úlohy zálohování](./media/backup-azure-vms-first-look-arm/open-backup-jobs-1.png)

  Otevře se okno úlohy zálohování Hello.

  ![Dlaždice Úlohy zálohování](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view-1.png)

  V hello **zálohování úloh** okně uvidíte hello stav všech úloh. Zkontrolujte, zda hello úloha zálohování pro virtuální počítač stále probíhá, nebo pokud se nedokončí. Po dokončení úlohy zálohování se stav hello je *dokončeno*.

  > [!NOTE]
  > Jako součást operace zálohování hello vydá hello služba Azure Backup příkaz toohello rozšířením zálohování v jednotlivých virtuálních počítačů tooflush všech zápisů a pořízení konzistentního snímku.
  >
  >

## <a name="troubleshooting-errors"></a>Řešení potíží s chybami
Pokud narazíte na problémy při zálohování virtuálního počítače, přečtěte si téma hello [článku Poradce při potížích virtuálních počítačů](backup-azure-vms-troubleshoot.md) nápovědu.

## <a name="next-steps"></a>Další kroky
Teď, když chráníte virtuální počítač, najdete v části hello následující články toolearn o úlohách správy virtuálních počítačů a jak toorestore virtuálních počítačů.

* [Správa a monitorování virtuálních počítačů](backup-azure-manage-vms.md)
* [Obnovení virtuálních počítačů](backup-azure-arm-restore-vms.md)
