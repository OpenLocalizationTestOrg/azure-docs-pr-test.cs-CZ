---
title: "aaaOverview trezorů služeb zotavení | Microsoft Docs"
description: "Přehled a porovnání trezory služeb zotavení a trezory Azure Backup."
services: backup
documentationcenter: " "
author: markgalioto
manager: carmonm
ms.assetid: 38d4078b-ebc8-41ff-9bc8-47acf256dc80
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/15/2017
ms.author: markgal;arunak
ms.openlocfilehash: 77dd9ece7fe86429cc6f9a47a68b5150a1f4af71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="recovery-services-vaults-overview"></a>Přehled trezory služeb zotavení

Tento článek popisuje funkce hello trezoru služeb zotavení. Trezor služeb zotavení je entita úložiště v Azure, kde data. Hello dat je obvykle kopie dat, nebo informace o konfiguraci pro virtuální počítače (VM), úlohy, servery nebo pracovní stanice. Trezor služeb zotavení je hello verze správce prostředků úložiště záloh. Společnost Microsoft doporučuje toouse trezory služeb zotavení a tooconvert trezory služeb tooRecovery trezory jakékoli zálohy.

## <a name="what-is-a-recovery-services-vault"></a>Co je trezor služby Recovery Services?

Trezor služeb zotavení je že entita online úložiště v Azure používá toohold data, jako jsou záložní kopie, body obnovení a zásady zálohování. Zálohování dat pro toohold trezory služeb zotavení můžete použít k různým službám Azure, jako jsou virtuální počítače IaaS (Linux nebo Windows) a databází Azure SQL. Obnovení služby trezory podporu System Center DPM, Windows Server, Server pro zálohování Azure a další. Trezory služeb zotavení bylo snadné tooorganize zálohovaných dat, a současně minimalizujete její režie na správu.

V rámci předplatného Azure můžete vytvořit libovolný počet trezorů služeb zotavení, jak se vám líbí.

## <a name="comparing-recovery-services-vaults-and-backup-vaults"></a>Trezory porovnáním různých služeb zotavení a trezory Backup

Trezory služeb zotavení jsou založené na modelu Azure Resource Manager hello Azure, zatímco trezory Backup jsou založené na modelu hello Azure Service Manager. Když upgradujete trezoru služeb zotavení tooa trezor zálohování, zálohování data hello zůstanou nezměněna během a po upgradu hello. Trezory služeb zotavení zadejte funkce není k dispozici pro trezory Backup, jako třeba:

- **Rozšířené možnosti toohelp zabezpečených zálohování dat**: trezory služeb zotavení s, Azure Backup poskytuje zabezpečení zálohování do cloudu tooprotect možnosti. Tyto funkce zabezpečení zajistěte, aby vám zabezpečit vaše zálohy a bezpečně obnovit data z cloudu záloh, i když jsou v ohrožení produkčního prostředí a zálohování serverů. [Další informace](backup-azure-security-feature.md)

- **Centrální monitorování pro vaše prostředí IT hybridní**: trezory služeb zotavení s, můžete monitorovat nejenom vaše [virtuální počítače Azure IaaS](backup-azure-manage-vms.md) , ale také vaše [místní prostředky](backup-azure-manage-windows-server.md#manage-backup-items) z centrální portálu. [Další informace](http://azure.microsoft.com/blog/alerting-and-monitoring-for-azure-backup)

- **Řízení přístupu na základě rolí (RBAC)**: RBAC poskytuje správu řízení podrobných přístupu v Azure. [Azure poskytuje různé integrované role](../active-directory/role-based-access-built-in-roles.md), a zálohování Azure má tři [body obnovení toomanage předdefinované role](backup-rbac-rs-vault.md). Trezory služeb zotavení jsou kompatibilní s RBAC, který omezuje zálohování a obnovení přístup toohello definované sadu rolí uživatele. [Další informace](backup-rbac-rs-vault.md)

- **Chránit všechny konfigurace virtuálních počítačů Azure**: trezory služeb zotavení ochrana založené na správci prostředků virtuálních počítačů včetně prémiové disky, spravované disky a virtuální počítače šifrovaná. Upgrade tooa trezoru zálohování trezor služeb zotavení poskytuje hello možnost tooupgrade vaše virtuální počítače na základě portálu Service Manager tooResource založené na správci virtuálních počítačů. Při upgradu hello trezoru, můžete zachovat body obnovení virtuálních počítačů na bázi portálu Service Manager a nakonfigurujte ochranu pro hello upgradovat (Resource Manager povolené) virtuálních počítačů. [Další informace](http://azure.microsoft.com/blog/azure-backup-recovery-services-vault-ga)

- **Rychlé obnovení pro virtuální počítače IaaS**: trezory služeb zotavení pomocí, můžete obnovit soubory a složky z virtuálního počítače IaaS bez obnovení hello celý virtuální počítač, který umožňuje rychlejší obnovení. Rychlé obnovení pro virtuální počítače IaaS je k dispozici pro systém Windows a virtuální počítače s Linuxem. [Další informace](http://azure.microsoft.com/blog/instant-file-recovery-from-azure-linux-vm-backup-using-azure-backup-preview)

## <a name="managing-your-recovery-services-vaults-in-hello-portal"></a>Správa vašeho trezory služeb zotavení hello portálu
Protože hello služby Backup je integrována do hello okno nastavení Azure, je snadné vytváření a správa trezorů služeb zotavení v hello portálu Azure. Tato integrace znamená můžete vytvořit ani spravovat trezoru služeb zotavení *v kontextu hello hello cílovou službu*. Například body obnovení hello tooview pro virtuální počítač, vyberte ho a klikněte na **zálohování** v okně Nastavení hello. konkrétní toothat Hello informace o zálohování virtuálních počítačů se zobrazí. V následujícím příkladu hello **ContosoVM** je název hello hello virtuálního počítače. **ContosoVM demovault** je název hello hello trezor služeb zotavení. Nepotřebujete tooremember hello název trezoru služeb zotavení hello, která ukládá hello body obnovení, tyto informace můžete přistupovat z hello virtuálního počítače.  

![Podrobnosti trezoru služeb zotavení virtuálních počítačů](./media/backup-azure-recovery-services-vault-overview/rs-vault-in-context.png)

Pokud několik serverů jsou chráněné pomocí hello trezoru stejné služeb zotavení, může být více logických toolook v hello trezor služeb zotavení. Můžete vyhledat všechny trezory služeb zotavení v hello předplatného a vyberte jednu ze seznamu hello.

Hello následující části obsahují tooarticles odkazy, které popisují, jak trezoru služeb zotavení toouse v každý typ aktivity.

### <a name="back-up-data"></a>Zálohování dat
- [Zálohování virtuálního počítače Azure](backup-azure-vms-first-look-arm.md)
- [Zálohování Windows serveru nebo pracovní stanice systému Windows](backup-try-azure-backup-in-10-mins.md)
- [Zálohování aplikace DPM tooAzure úlohy](backup-azure-dpm-introduction.md)
- [Příprava tooback až úlohy pomocí serveru Azure Backup](backup-azure-microsoft-azure-backup.md)

### <a name="manage-recovery-points"></a>Správa bodů obnovení
- [Spravovat zálohy virtuálního počítače Azure](backup-azure-manage-vms.md)
- [Správa souborů a složek](backup-azure-manage-windows-server.md)

### <a name="restore-data-from-hello-vault"></a>Obnovení dat z trezoru hello
- [Obnovit jednotlivé soubory z virtuálního počítače Azure](backup-azure-restore-files-from-vm.md)
- [Obnovení virtuálního počítače Azure](backup-azure-arm-restore-vms.md)

### <a name="secure-hello-vault"></a>Zabezpečené hello trezoru
- [Zabezpečení cloudu zálohování dat v trezory služeb zotavení](backup-azure-security-feature.md)



## <a name="next-steps"></a>Další kroky
Použijte následující článek hello:</br>
[Zálohování virtuálních počítačů IaaS](backup-azure-arm-vms-prepare.md)</br>
[Zálohování serveru Azure Backup](backup-azure-microsoft-azure-backup.md)</br>
[Zálohování serveru Windows](backup-configure-vault.md)
