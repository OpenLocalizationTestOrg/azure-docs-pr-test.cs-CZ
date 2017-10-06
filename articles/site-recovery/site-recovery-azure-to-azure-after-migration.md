---
title: "aaaPrepare počítače tooset až zotavení po havárii mezi oblastmi Azure po migraci tooAzure pomocí Site Recovery | Microsoft Docs"
description: "Tento článek popisuje, jak tooprepare počítačů tooset až zotavení po havárii mezi oblastmi Azure po migraci tooAzure pomocí Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: ponatara
manager: abhemraj
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: ponatara
ms.openlocfilehash: b6274e3df210c1d8a7b8289cc85868ee6414e523
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-azure-vms-tooanother-region-after-migration-tooazure-by-using-azure-site-recovery"></a>Replikovat virtuální počítače Azure tooanother oblast po migraci tooAzure pomocí Azure Site Recovery

>[!NOTE]
> Azure Site Recovery replikaci pro virtuální počítače Azure (VM) je aktuálně ve verzi preview.

## <a name="overview"></a>Přehled

Tento článek vám pomůže připravit virtuální počítače Azure pro replikaci mezi dvěma oblastmi Azure po migraci těchto počítačů z prostředí tooAzure místní pomocí Azure Site Recovery.

## <a name="disaster-recovery-and-compliance"></a>Zotavení po havárii a dodržování předpisů
V současné době jsou více podniky přesunutí jejich tooAzure úlohy. S podniky přesunutí důležité místní produkční tooAzure úlohy, nastavení služby zotavení po havárii pro tyto úlohy je povinné pro dodržování předpisů a toosafeguard proti tak možnost narušení v oblasti Azure.

## <a name="steps-for-preparing-migrated-machines-for-replication"></a>Kroky pro přípravu migrované počítače pro replikaci
tooprepare migrovali počítače pro nastavení replikace tooanother oblast Azure:

1. Dokončení migrace.
2. Nainstalujte hello agent Azure v případě potřeby.
3. Odeberte služby Mobility hello.  
4. Restartujte hello virtuálních počítačů.

Tyto kroky jsou popsané v podrobněji hello následující části.

### <a name="step-1-migrate-workloads-running-on-hyper-v-vms-vmware-vms-and-physical-servers-toorun-on-azure-vms"></a>Krok 1: Migrace úloh běžících na virtuálních počítačů Hyper-V, virtuálních počítačů VMware a fyzické servery toorun na virtuálních počítačích Azure

tooset až replikace a migrace vaší místní technologie Hyper-V, VMware a fyzické úlohy tooAzure, postupujte podle hello kroky v hello [virtuální počítače Azure IaaS migraci mezi oblastmi Azure s Azure Site Recovery](site-recovery-migrate-to-azure.md) článku. 

Po migraci není nutné toocommit nebo odstranit převzetí služeb při selhání. Místo toho vyberte hello **dokončení migrace** možnost pro každý počítač má toomigrate:
1. V **replikované položky**, klikněte pravým tlačítkem na hello virtuálních počítačů a klikněte na **dokončení migrace**. Klikněte na tlačítko **OK** toocomplete hello krok. Můžete sledovat průběh ve vlastnostech virtuálního počítače hello sledováním hello úlohy dokončení migrace v **úlohy Site Recovery**.
2. Hello **dokončení migrace** akci dokončení procesu migrace hello, odebere replikaci pro počítač hello a zastaví Site Recovery fakturace pro počítač hello.

   ![completemigration](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

### <a name="step-2-install-hello-azure-vm-agent-on-hello-virtual-machine"></a>Krok 2: Nainstalujte agenta virtuálního počítače Azure hello hello virtuálního počítače
Hello Azure [agenta virtuálního počítače](../virtual-machines/windows/classic/agents-and-extensions.md#azure-vm-agents-for-windows-and-linux) musí být nainstalován na hello virtuálního počítače pro toowork rozšíření hello Site Recovery a toohelp chránit hello virtuálních počítačů.

>[!IMPORTANT]
>Počínaje verzí 9.7.0.0, na virtuální počítače s Windows, instalační program služby Mobility hello nainstaluje hello nejnovější dostupné agenta virtuálního počítače Azure. Na migraci hello virtuální počítač splňuje instalace agenta požadované pro používání všech rozšíření virtuálního počítače, včetně přípony hello Site Recovery. Hello virtuálního počítače Azure agenta potřebám toobe ručně nainstalovat jenom v případě, že hello služby Mobility na hello migrovaný počítač je verze 9,6 nebo starším.

Hello následující tabulka obsahuje další informace o instalaci agenta hello virtuálního počítače a ověřuje, zda byl nainstalován:

| **Operace** | **Windows** | **Linux** |
| --- | --- | --- |
| Instalace agenta virtuálního počítače hello |Stáhněte a nainstalujte hello [MSI agenta](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Potřebujete instalaci hello toocomplete oprávnění správce. |Nainstalujte nejnovější hello [agenta systému Linux](../virtual-machines/linux/agent-user-guide.md). Potřebujete instalaci hello toocomplete oprávnění správce. Doporučujeme nainstalovat agenta hello z distribuční úložiště. Jsme *nedoporučujeme* instalaci hello agenta virtuálního počítače s Linuxem přímo z Githubu.  |
| Ověření instalace agenta virtuálního počítače hello |1. Vyhledejte složku C:\WindowsAzure\Packages toohello hello virtuálního počítače Azure. Měli byste vidět hello přítomný soubor WaAppAgent.exe. <br>2. Klikněte pravým tlačítkem na soubor hello, přejděte příliš**vlastnosti**a potom vyberte hello **podrobnosti** kartě hello **verze produktu** pole by mělo být 2.6.1198.718 nebo vyšší. |Není k dispozici |


### <a name="step-3-remove-hello-mobility-service-from-hello-migrated-virtual-machine"></a>Krok 3: Odeberte hello služby Mobility ze hello migrovaný virtuální počítač

Pokud jste migrovali místní počítače VMware nebo fyzických serverů v systému Windows nebo Linux, je třeba toomanually odebrat nebo odinstalace hello služby Mobility ze hello migrovaný virtuální počítač.

>[!IMPORTANT]
>Tento krok není potřeba tooAzure migrovaných virtuálních počítačů Hyper-V.

#### <a name="uninstall-hello-mobility-service-on-a-windows-server-vm"></a>Odinstalujte službu Mobility hello na virtuální počítač Windows serveru
Použijte jeden z následujících metod toouninstall hello Mobility service na počítači s Windows serverem hello.

##### <a name="uninstall-by-using-hello-windows-ui"></a>Odinstalovat aplikaci pomocí uživatelského rozhraní Windows hello
1. V hello ovládacích panelů, vyberte **programy**.
2. Vyberte **Microsoft Azure Site Recovery Mobility služby nebo hlavní cílový server**a potom vyberte **odinstalovat**.

##### <a name="uninstall-at-a-command-prompt"></a>Odinstalujte na příkazovém řádku
1. Otevřete okno příkazového řádku jako správce.
2. toouninstall hello služby Mobility, spusťte následující příkaz hello:

   ```
   MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
   ```

#### <a name="uninstall-hello-mobility-service-on-a-linux-computer"></a>Odinstalujte službu Mobility hello na počítač se systémem Linux
1. Na serveru systému Linux, přihlaste se jako **kořenové** uživatele.
2. V terminálu přejděte příliš/uživatel/místní nebo automatické obnovení systému.
3. toouninstall hello služby Mobility, spusťte následující příkaz hello:

   ```
   uninstall.sh -Y
   ```

### <a name="step-4-restart-hello-vm"></a>Krok 4: Restartování hello virtuálních počítačů

Po odinstalování služby Mobility hello nastavit hello restartování virtuálního počítače předtím, než replikace tooanother oblast Azure.


## <a name="next-steps"></a>Další kroky
- Začněte chránit vaše úlohy [replikovat virtuální počítače Azure](site-recovery-azure-to-azure.md).
- Další informace o [sítě pokyny pro replikaci virtuálních počítačů Azure](site-recovery-azure-to-azure-networking-guidance.md).
