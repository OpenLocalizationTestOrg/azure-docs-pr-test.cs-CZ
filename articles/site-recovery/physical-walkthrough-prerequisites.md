---
title: "aaaPrerequisites pro fyzický server tooAzure replikaci s využitím Azure Site Recovery | Microsoft Docs"
description: "Shrnuje hello požadavky na replikaci úlohy běžící na fyzických serverech tooAzure Windows nebo Linuxem pomocí služby Azure Site Recovery hello."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 318156ba-793b-48d0-98d4-cc5436bf28a3
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: b0ed53a143079877a2ad21ee17aae5510e0dc058
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-2-review-hello-prerequisites-for-physical-server-tooazure-replication"></a>Krok 2: Kontrola hello požadavky pro replikaci tooAzure fyzického serveru

Kontrola předpokladů hello shrnuté v tabulce hello.


**Požadavek** | **Podrobnosti**
--- | ---
**Azure** | Další informace o [požadavky pro Azure](site-recovery-prereq.md#azure-requirements)
**Lokální konfigurační server** | Je třeba fyzického serveru (nebo virtuálních počítačů VMware) s Windows serverem 2012 R2 nebo novější. Nastavit tento server během nasazování Site Recovery.<br/><br/> Proces hello výchozí server a hlavní cílový server jsou také v tomto počítači nainstalován. Pokud jste vertikální navýšení kapacity, bude pravděpodobně třeba samostatný procesový server. V takovém případě má hello stejné požadavky jako hello konfigurační server.<br/><br/> [Další informace](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements).
**Místní virtuální počítače** | Počítače, které chcete tooreplicate by měla být spuštěná [podporovaný operační systém](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) a v souladu s [požadavky Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
**Adresy URL** | konfigurační server Hello potřebuje přístup k toothese adresy URL:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Pokud máte pravidla brány firewall založená na adresu IP, ověřte, že umožňují tooAzure komunikace.<br/></br> Povolit hello [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653)a hello port HTTPS (443).<br/></br> Povolte rozsahy IP adres pro hello oblast Azure svého předplatného a západní USA (používá se pro správu Identity a řízení přístupu).<br/><br/> Povolit tuto adresu URL pro stahování MySQL hello: http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi.
**Služba mobility** | Nainstalovat na každý server replikované.




## <a name="limitations"></a>Omezení

Poznámka: omezení hello shrnuté v tabulce hello před nasazením.

**Omezení** | **Podrobnosti**
--- | ---
**Azure** | Účty úložiště a sítě musí být v hello stejné oblasti jako trezor hello<br/><br/> Pokud používáte prémiový účet úložiště, musíte také standardní ukládat protokoly replikace toostore účtu<br/><br/> Nelze replikovat toopremium účty ve střední a – Jih, Indie.
**Lokální konfigurační server** | Pokud používáte virtuální počítače VMware jako hello konfigurace serveru počítač, musí být hello typ adaptéru virtuálního počítače VMware VMXNET3. Pokud tomu tak není, [tuto aktualizaci nainstalovat](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1)<br/><br/> Pro virtuální počítače VMware by měly být nainstalovány vSphere PowerCLI 6.0.<br/><br> Hello počítač by neměl být řadič domény.<br/><br/> Hello počítač by měl mít statickou IP adresu.<br/><br/> název hostitele Hello by měl být maximálně 15 znaků nebo méně, a operační systém by měl být v angličtině.
**Replikovat počítače** | Ověřte [omezení virtuálního počítače Azure](site-recovery-prereq.md#azure-requirements)<br/><br/> Nelze replikovat virtuální počítače s šifrované disky nebo virtuální počítače pomocí rozhraní UEFI nebo EFI.<br/><br> Sdílený disk, clustery nejsou podporované. Pokud hello zdrojového virtuálního počítače má seskupování síťových adaptérů, převede se tooa jeden síťový adaptér po převzetí služeb při selhání.<br/><br/> Pokud virtuální počítače iSCSI disk, Site Recovery převede souboru virtuálního pevného disku tooa po převzetí služeb při selhání. Pokud hello cíle iSCSI, dosažitelný z hello virtuálního počítače Azure, připojí tooit a zobrazí se i hello virtuálního pevného disku. Pokud k tomu dojde, odpojte hello cíle iSCSI.<br/><br/> Pokud chcete, aby tooenable více virtuálních počítačů konzistence, která umožňuje virtuální počítače se systémem hello stejné zatížení toobe obnovit společně tooa konzistentní datový bod, otevřete port 20004 hello virtuálních počítačů.<br/><br/> Windows musí být nainstalován na jednotce hello C. disk s operačním systémem Hello by měl být základní a nikoli dynamické. Hello datový disk může být dynamické.<br/><br/> Soubory Linux/etc/hosts na virtuálních počítačích by měly obsahovat položky, které mapují hello místního hostitele název tooIP adres přidružených všechny síťové adaptéry. Hello název hostitele, přípojné body, název zařízení, systémové cesty a názvy souborů (/ etc; USR) by měly být pouze v angličtině.<br/><br/> Konkrétní typy [Linux úložiště](site-recovery-support-matrix-to-azure.md#support-for-storage) jsou podporovány.<br/><br/>Vytvořit nebo nastavit **disk.enableUUID=true** v nastavení virtuálního počítače hello. To poskytuje konzistentní UUID toohello VMDK, tak, aby správně připojí a zajišťuje, aby jenom rozdílové změny přenášená back místní tooon během navrácení služeb po obnovení bez úplná replikace.


## <a name="next-steps"></a>Další kroky

- Pokud jste to úplné nasazení, přejděte příliš[krok 3: plánování kapacity](physical-walkthrough-capacity.md)
- Pokud jste to jednoduchá testovací nasazení, přejděte příliš[krok 4: plánování sítě](physical-walkthrough-network.md).
