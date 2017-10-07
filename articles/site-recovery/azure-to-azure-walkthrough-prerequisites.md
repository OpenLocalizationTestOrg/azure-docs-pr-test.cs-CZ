---
title: "spuštění replikace virtuálních počítačů Azure tooanother oblasti aaaBefore | Microsoft Docs"
description: "Shrnuje kroky hello nutné tootake před replikace virtuálních počítačů Azure mezi oblastí Azure pomocí služby Azure Site Recovery hello"
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
ms.openlocfilehash: 7fa633075eeb52d0c184a1dd8d53ccc15644f518
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-2-before-you-start"></a>Krok 2: Než začnete

Po zkontrolování hello [architektura](azure-to-azure-walkthrough-architecture.md) pro replikaci mezi oblastmi Azure s Azure virtuálních počítačů (VM) [Azure Site Recovery](site-recovery-overview.md), použijte tento článek tooverify požadavky. 

- Po dokončení hello článku byste měli mít clear porozumět tomu, co je potřeba toomake hello nasazení fungovat a dokončili hello požadované kroky.
- Odeslat všechny komentáře v dolní části hello tohoto článku nebo pokládání dotazů v hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

>[!NOTE]
>
> Azure replikace virtuálního počítače je aktuálně ve verzi preview.



## <a name="support-recommendations"></a>Podpora doporučení

Následující tabulka hello revize.

**Komponenta** | **Požadavek**
--- | ---
**Trezor služeb zotavení** | Doporučujeme vytvořit trezor služeb zotavení v cílové hello oblast Azure, který má toouse pro zotavení po havárii. Například pokud chcete tooreplicate zdrojové virtuální počítače ve východní USA tooCentral USA, vytvořte trezor hello v střed USA.
**Předplatné Azure** | Vaše předplatné Azure musí být povoleno toocreate virtuální počítače v cílovém umístění hello, které chcete toouse jako oblast pro obnovení po havárii hello. Obraťte se na podporu tooenable hello požadované kvóty.
**Cíl oblast kapacity** | V cílové hello oblast Azure by měl mít předplatné hello dostatečnou kapacitu pro virtuální počítače, účty úložiště a síťové součásti.
**Úložiště** | Použití hello [pokynů pro virtualizované úložiště](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) pro zdrojové virtuální počítače Azure, tooavoid problémy s výkonem.<br/><br/> Účty úložiště musí být v hello stejné oblasti jako trezor hello.<br/><br/> Nelze replikovat toopremium účty ve střední a – Jih, Indie.<br/><br/> Pokud nasadíte replikace s výchozím nastavením hello, Site Recovery vytvoří hello požadované účty úložiště na základě hello zdroj konfigurace. Pokud upravíte nastavení, postupujte podle hello [cíle škálovatelnosti pro disky virtuálních počítačů](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks).
**Sítě** | Tooallow odchozí připojení z virtuálních počítačů Azure, musíte pro konkrétní rozsahy adres URL/IP.<br/><br/> Účty sítě musí být v hello stejné oblasti jako trezor hello. 
**Virtuální počítač Azure** | Zkontrolujte, zda že jsou všechny hello nejnovější kořenové certifikáty na hello virtuálního počítače Azure Windows nebo Linuxem. Pokud nejsou, nebudete moct tooregister hello virtuálních počítačů ve službě Site Recovery z důvodu omezení zabezpečení.
**Azure uživatelský účet** | Azure uživatelský účet potřebuje toohave určité [oprávnění](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikaci virtuálního počítače Azure.

Získat úplný seznam požadavků na podporu v hello [matici podpory](site-recovery-support-matrix-azure-to-azure.md)


## <a name="set-permissions-on-hello-account"></a>Nastavení oprávnění pro účet hello

1. Přečtěte si informace o hello [oprávnění](site-recovery-role-based-linked-access-control.md) potřebujete pro replikaci.
2. Postupujte podle těchto [pokyny](../active-directory/role-based-access-control-configure.md#add-access) tooadd oprávnění.


## <a name="verify-target-resources"></a>Zkontrolujte cílové prostředky

1. Povolit tooallow vašeho předplatného Azure můžete toocreate virtuálních počítačů v hello cílovou oblast chcete toouse pro zotavení po havárii, které chcete toouse jako oblast pro obnovení po havárii hello. Obraťte se na podporu tooenable hello požadované kvóty.
2. Ujistěte se, že vaše předplatné má dostatek tooenable prostředky virtuálních počítačů s velikostí, které odpovídají zdrojové virtuální počítače. Ve výchozím nastavení, když nastavení replikace, Site Recovery vyskladnění hello stejnou velikost pro hello cíle virtuálního počítače nebo hello nejbližší možné velikost. Další informace o [řešení potíží s](site-recovery-azure-to-azure-troubleshoot-errors.md#azure-resource-quota-issues-error-code-150097) cílové prostředky.

## <a name="verify-azure-vm-certificates"></a>Ověření certifikátů virtuálního počítače Azure

1. Zkontrolujte, zda jsou všechny kořenové certifikáty nejnovější hello přítomen na hello Windows nebo chcete tooreplicate virtuální počítače s Linuxem. Pokud nejsou zadány hello nejnovější kořenové certifikáty, hello virtuálního počítače nemůže být registrovaný tooSite obnovení z důvodu omezení toosecurity.
2. U virtuálních počítačů Windows hello následující:

    - Nainstalujte všechny aktualizace nejnovější Windows hello na hello virtuálních počítačů tak, aby byly všechny hello důvěryhodných kořenových certifikátů na počítači hello.
    - V odpojeném prostředí postupujte podle hello standardní služby Windows Update certifikát procesu/procesu aktualizace v organizaci, tooget hello nejnovější kořenové certifikáty a aktualizovaného seznamu odvolaných certifikátů na virtuálních počítačích hello.
3. Virtuální počítače s Linuxem postupujte podle pokynů hello poskytované Linux distributora tooget hello nejnovější důvěryhodných kořenových certifikátů a hello nejnovější seznam odvolaných certifikátů na hello virtuálních počítačů. Další informace o [řešení potíží s](site-recovery-azure-to-azure-troubleshoot-errors.md#trusted-root-certificates-error-code-151066) důvěryhodné kořenové problémy.


## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 3: plánování sítě](azure-to-azure-walkthrough-network.md) tooset až odchozí připojení.
