---
title: "Nejčastější dotazy k trezoru služeb aaaRecovery | Microsoft Docs"
description: "Tato verze hello nejčastější dotazy týkající se podporuje verze Public Preview hello služby hello služby zálohování Azure. Odpovědi toofrequently kladené dotazy týkající se hello agenta zálohování, zálohování a uchovávání, obnovení, zabezpečení a další běžné otázky o hello řešení Azure Backup."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "řešení zálohování; služba zálohování"
ms.assetid: 5f55b500-1ee9-4f64-9306-02d6f7a8eded
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/21/2016
ms.author: markgal;trinadhk;
ms.openlocfilehash: 882b2e67ed424dc9f3681a8870e6b4c7b4cdcaec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="recovery-services-vault---faq"></a>Trezor služby Recovery Services – nejčastější dotazy
Tento článek obsahuje informace o konkrétní tooRecovery služby trezoru a doplňuje hello [Azure Backup – nejčastější dotazy](backup-azure-backup-faq.md). Hello časté otázky týkající se Azure Backup poskytuje úplnou sadu hello otázky a odpovědi týkající se hello služby zálohování Azure.  

Otázky o Azure Backup můžete pokládat v části Disqus tohoto článku nebo u souvisejících článků hello. Také můžete pokládat dotazy týkající se služby Azure Backup hello v hello [diskusní fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>Trezory služby Recovery Services jsou založené na Resource Manageru. Jsou trezory služby Backup (v klasickém režimu) stále podporovány? <br/>
Ano, trezory Backup jsou stále podporovány. Trezory Backup vytvářejte na hello [portálu Classic](https://manage.windowsazure.com). Trezory služeb zotavení vytvářejte na hello [portál Azure](https://portal.azure.com). Ale důrazně doporučujeme vám toocreate trezoru služeb zotavení jako všechna budoucí vylepšení budou k dispozici pouze v trezoru služeb zotavení.

## <a name="can-i-migrate-a-backup-vault-tooa-recovery-services-vault-br"></a>Můžete migrovat úložiště záloh tooa trezoru služeb zotavení? <br/>
Bohužel Ne, v tuto chvíli nemůžete migrovat hello obsah tooa trezor zálohování, které trezor služeb zotavení. Ačkoli na přidání této funkce pracujeme, v rámci verze Public Preview není dostupná.

## <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Podporují trezory Recovery Services klasické virtuální počítače nebo virtuální počítače využívající Resource Manager? <br/>
Trezory Recovery Services podporují oba modely.  Můžete zálohovat virtuální počítač vytvořený na portálu Classic hello, (které jsou virtuální počítače v klasickém režimu) nebo virtuální počítač vytvořený v hello portálu Azure (které jsou založená na správci prostředků) trezor služeb zotavení tooa.

## <a name="i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-toomigrate-my-vms-from-classic-mode-tooresource-manager-mode--how-can-i-backup-them-in-recovery-services-vault"></a>Svoje klasické virtuální počítače jsem zálohoval v trezoru služby Backup. Nyní chci toomigrate virtuálních počítačů z režimu správce tooResource klasickém režimu.  Jakým způsobem je můžu zálohovat v trezoru Recovery Services?
Zálohy klasické virtuální počítače v trezoru záloh nebudou migrovat automaticky toorecovery trezor služeb, při migraci hello virtuálních počítačů ze classic tooResource režimu správce. Při migraci záloh virtuálních počítačů použijte tento postup:

1. V úložišti záloh, přejděte příliš**chráněné položky** a vyberte hello virtuálních počítačů. Klikněte na [Zastavit ochranu](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Políčko *Delete associated backup data* (Odstranit přidružená data záloh) ponechte **nezaškrtnuté**.
2. V hello [portál Azure](https://portal.azure.com), přejděte toohello **rozšíření** nabídky pro hello virtuálního počítače a odinstalace hello **VMSnapshot/VMSnapshotLinux** rozšíření.
3. Migrujte hello virtuální počítač z režimu správce tooResource klasickém režimu. Ujistěte se, úložiště a sítě odpovídající toovirtual počítače jsou také migrované tooResource Manager režimu.
4. Vytvoření trezoru služeb zotavení a konfigurace virtuálního počítače pomocí zálohování na hello migrovat **zálohování** akce nad panelu trezoru. Další informace o tom, příliš[povolení zálohování v trezoru služeb zotavení](backup-azure-vms-first-look-arm.md)
