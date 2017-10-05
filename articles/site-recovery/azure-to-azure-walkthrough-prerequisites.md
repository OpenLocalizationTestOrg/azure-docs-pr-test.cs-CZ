---
title: "Před zahájením replikace virtuálních počítačů Azure do jiné oblasti | Microsoft Docs"
description: "Shrnuje kroky, které je třeba provést před replikace virtuálních počítačů Azure mezi oblastí Azure pomocí služby Azure Site Recovery"
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
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: fc6682edc020796431324005e10ca4ff0c0fd2f0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="step-2-before-you-start"></a>Krok 2: Než začnete

Po zkontrolování [architektura](azure-to-azure-walkthrough-architecture.md) pro replikaci mezi oblastmi Azure s Azure virtuálních počítačů (VM) [Azure Site Recovery](site-recovery-overview.md), použijte tento článek ověřit požadované součásti. 

- Po dokončení článku měli vědět, co je potřeba, aby nasazení fungovat a dokončili požadovaných předchozích kroků.
- Případné připomínky můžete publikovat na konci tohoto článku nebo na [fóru služby Azure Site Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

>[!NOTE]
>
> Azure replikace virtuálního počítače je aktuálně ve verzi preview.



## <a name="support-recommendations"></a>Podpora doporučení

Zkontrolujte následující tabulka.

**Komponenta** | **Požadavek**
--- | ---
**Trezor služeb zotavení** | Doporučujeme vytvořit trezor služeb zotavení v cílové oblasti Azure, který chcete použít pro zotavení po havárii. Například pokud chcete replikovat zdrojové virtuální počítače ve východní USA na střed USA, vytvořte trezor v střed USA.
**Předplatné Azure** | Vašeho předplatného Azure by měly být povoleny k vytvoření virtuálních počítačů, v cílovém umístění, které chcete použít jako oblasti obnovení po havárii. Obraťte se na podporu, aby umožnil požadované kvótu.
**Cíl oblast kapacity** | V cílové oblasti Azure odběr musí mít dostatečnou kapacitu pro virtuální počítače, účty úložiště a síťové součásti.
**Storage** | Použití [pokynů pro virtualizované úložiště](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) pro zdrojové virtuální počítače Azure, aby se zabránilo problémům s výkonem.<br/><br/> Účty úložiště musí být ve stejné oblasti jako trezor.<br/><br/> Nelze se replikují na prémiové účty ve střední a – Jih, Indie.<br/><br/> Pokud nasadíte replikace s výchozím nastavením, Site Recovery vytvoří účty požadované úložiště na základě konfigurace zdroje. Pokud upravíte nastavení, postupujte podle kroků [cíle škálovatelnosti pro disky virtuálních počítačů](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks).
**Sítě** | Budete muset povolit odchozí připojení z virtuálních počítačích Azure, pro konkrétní rozsahy adres URL/IP.<br/><br/> Účty sítě musí být ve stejné oblasti jako trezor. 
**Virtuální počítač Azure** | Zkontrolujte, zda že jsou všechny nejnovější kořenové certifikáty pro virtuální počítač Azure Windows nebo Linuxem. Pokud nejsou, nebudete moci zaregistrovat virtuálního počítače ve službě Site Recovery z důvodu omezení zabezpečení.
**Azure uživatelský účet** | Azure uživatelský účet musí mít určité [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) k povolení replikace virtuálního počítače Azure.

Získat úplný seznam požadavků na podporu v [matici podpory](site-recovery-support-matrix-azure-to-azure.md)


## <a name="set-permissions-on-the-account"></a>Nastavení oprávnění pro účet

1. Přečtěte si informace o [oprávnění](site-recovery-role-based-linked-access-control.md) potřebujete pro replikaci.
2. Postupujte podle těchto [pokyny](../active-directory/role-based-access-control-configure.md#add-access) se přidat oprávnění.


## <a name="verify-target-resources"></a>Zkontrolujte cílové prostředky

1. Aktivujte předplatné Azure, aby bylo možné vytvořit virtuální počítače v cílové oblast, kterou chcete použít pro zotavení po havárii, který chcete použít jako oblasti obnovení po havárii. Obraťte se na podporu, aby umožnil požadované kvótu.
2. Zajistěte, aby že vaše předplatné nemá dostatek prostředků k povolení virtuální počítače s velikostí, které odpovídají zdrojové virtuální počítače. Ve výchozím nastavení, když nastavení replikace Site Recovery vybere stejnou velikost pro cílový počítač nebo nejbližší možné velikost. Další informace o [řešení potíží s](site-recovery-azure-to-azure-troubleshoot-errors.md#azure-resource-quota-issues-error-code-150097) cílové prostředky.

## <a name="verify-azure-vm-certificates"></a>Ověření certifikátů virtuálního počítače Azure

1. Zkontrolujte, zda všechny nejnovější kořenové certifikáty jsou k dispozici v systému Windows nebo je chcete replikovat virtuální počítače s Linuxem. Pokud nejsou zadány nejnovější kořenové certifikáty, virtuální počítač nelze zaregistrovat k Site Recovery z důvodu omezení zabezpečení.
2. Pro virtuální počítače Windows postupujte takto:

    - Nainstalujte všechny nejnovější aktualizace systému Windows ve virtuálním počítači tak, aby byly všechny důvěryhodných kořenových certifikátů na počítači.
    - V odpojeném prostředí, postupujte podle standardní služby Windows Update certifikát procesu/procesu aktualizace ve vaší organizaci, chcete-li získat nejnovější kořenové certifikáty a aktualizovat seznam CRL na virtuálních počítačích.
3. Pro virtuální počítače s Linuxem využijte tohoto vodítka poskytované vaší Linux distributora, chcete-li získat nejnovější důvěryhodných kořenových certifikátů a nejnovější seznam odvolaných certifikátů ve virtuálním počítači. Další informace o [řešení potíží s](site-recovery-azure-to-azure-troubleshoot-errors.md#trusted-root-certificates-error-code-151066) důvěryhodné kořenové problémy.


## <a name="next-steps"></a>Další kroky

Přejděte na [krok 3: plánování sítě](azure-to-azure-walkthrough-network.md) nastavit odchozí připojení.
