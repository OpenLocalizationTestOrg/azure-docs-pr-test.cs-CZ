---
title: "Nejčastější dotazy k zálohování virtuálních počítačů aaaAzure | Microsoft Docs"
description: "Odpovědi toocommon otázky týkající se: Jak funguje zálohování virtuálního počítače Azure, omezení a co se stane, když změny toopolicy dojít"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
keywords: azure vm backup, azure vm restore, backup policy
ms.assetid: c4cd7ff6-8206-45a3-adf5-787f64dbd7e1
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/18/2017
ms.author: trinadhk;pullabhk;
ms.openlocfilehash: a1ad2cb3a379577a8c4258c8207ce75809e11a4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="questions-about-hello-azure-vm-backup-service"></a>Dotazy týkající se hello služby zálohování virtuálních počítačů Azure
Tento článek obsahuje odpovědi toocommon otázky toohelp rychle pochopit součásti hello zálohování virtuálních počítačů Azure. V některých hello odpovědi jsou články toohello odkazy, které mají komplexní informace. Také můžete pokládat dotazy týkající se služby Azure Backup hello v hello [diskusní fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="configure-backup"></a>Konfigurace zálohování
### <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Podporují trezory Recovery Services klasické virtuální počítače nebo virtuální počítače využívající Resource Manager? <br/>
Trezory Recovery Services podporují oba modely.  Můžete zálohovat klasické virtuální počítač (vytvořit na portálu Classic hello) nebo tooa Resource Manager virtuální počítač (vytvořený na portálu Azure hello), které trezor služeb zotavení.

### <a name="what-configurations-are-not-supported-by-azure-vm-backup-"></a>Jaký konfigurace zálohování virtuálních počítačů Azure nepodporuje?
Přečtěte si témata [Podporované operační systémy](backup-azure-arm-vms-prepare.md#supported-operating-system-for-backup) a [Omezení zálohování virtuálních počítačů](backup-azure-arm-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm).

### <a name="why-cant-i-see-my-vm-in-configure-backup-wizard"></a>Proč se můj virtuální počítač nezobrazuje v průvodci konfigurací zálohování?
V průvodci konfigurací zálohování služba Azure Backup uvádí pouze virtuální počítače, které:
* Již není chráněná – můžete ověřit hello zálohování stav virtuálního počítače mezi aktualizacemi tooVM okno o zálohu stavu z nabídky nastavení okna hello. Další informace o tom, příliš[zkontrolujte zálohování stav virtuálního počítače](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-vm-management-blade)
* Patří toosame oblasti jako virtuální počítač

## <a name="backup"></a>Zálohování
### <a name="will-on-demand-backup-job-follow-same-retention-schedule-as-scheduled-backups"></a>Bude úloha zálohování na vyžádání používat stejný plán uchovávání jako plánovaní zálohování?
Ne. Rozsah uchování hello toospecify potřebujete pro úlohu zálohování na vyžádání. Ve výchozím nastavení se bude uchovávat po dobu 30 dnů, pokud ji aktivujete z portálu. 

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-toowork"></a>Na některých virtuálních počítačích byla nedávno povolena služba Azure Disk Encryption. Moje zálohování bude pokračovat toowork?
Potřebujete oprávnění toogive pro tooaccess služby zálohování Azure Key Vault. Tato oprávnění můžete zadat v PowerShellu pomocí kroků popsaných v části *Povolení zálohování* v dokumentaci k [PowerShellu](backup-azure-vms-automation.md).

### <a name="i-migrated-disks-of-a-vm-toomanaged-disks-will-my-backups-continue-toowork"></a>Můžu migrovat disky disky toomanaged virtuálních počítačů. Moje zálohování bude pokračovat toowork?
Ano, zálohování funguje bez problémů a bez nutnosti toore-konfigurace zálohování. 

## <a name="restore"></a>Obnovení
### <a name="how-do-i-decide-between-restoring-disks-versus-full-vm-restore"></a>Jak se rozhodnout mezi obnovením disků a úplným obnovením virtuálního počítače?
Úplné obnovení virtuálního počítače Azure si můžete představit jako určitou možnost rychlého vytvoření pro obnovený virtuální počítač. Obnovení virtuálních počítačů možnost změní hello názvy disků, kontejnery používané disky, veřejné IP adresy, síťové názvy jedinečnosti prostředky získávání vytvořené jako součást vytvoření virtuálního počítače. Také nepřidá nastavení tooavailability Virtuálního hello. 

Obnovení disků použijte k:
* Přizpůsobení hello virtuálních počítačů, která je vytvořena z bodu v konfiguraci čas, jako je změna velikosti hello z konfigurace zálohy.
* Přidání konfigurace, které se nenacházejí v době zálohování hello 
* Ovládací prvek hello zásady vytváření názvů pro prostředky získávání vytvořené
* Přidat sadu tooavailability virtuálních počítačů
* Použití konfigurace, které lze dosáhnout jenom pomocí PowerShellu nebo definice deklarativní šablony.

## <a name="manage-vm-backups"></a>Správa záloh virtuálních počítačů
### <a name="what-happens-when-i-change-a-backup-policy-on-vms"></a>Co se stane, když že se na virtuálních počítačích změní zásady zálohování?
Pokud je nové zásady použity na virtuální počítače, bude pak plán a jejich uchovávání hello nové zásady. Pokud je rozšířeno uchování, stávajících bodů obnovení budou označeny tookeep je závislý na nové zásady. Pokud je snížen uchovávání, jsou označeny pro vyřazení v další úlohy čištění hello a budou odstraněny. 
