---
title: "aaaPrerequisites pro VMware tooAzure replikaci s využitím Azure Site Recovery | Microsoft Docs"
description: "Shrnuje hello požadavky na replikaci úloh běžících na virtuálních počítačích VMware tooAzure, pomocí služby Azure Site Recovery hello."
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
ms.openlocfilehash: 14f8e9713b42a1d8da71223fbbb7a6b7c4303adb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-2-review-hello-prerequisites-for-vmware-tooazure-replication"></a>Krok 2: Kontrola hello požadavky pro replikaci tooAzure VMware

Přečtěte si požadavky hello shrnuté v následující tabulce hello.

**Požadavek** | **Podrobnosti**
--- | ---
**Azure** | Další informace o [požadavky pro Azure](site-recovery-prereq.md#azure-requirements)
**Lokální konfigurační server** | Je třeba virtuální počítače VMware s Windows serverem 2012 R2 nebo novější. Nastavit tento server během nasazování Site Recovery.<br/><br/> Výchozí proces hello jsou server a hlavní cílový server také nainstalované na tomto virtuálním počítači. Když jste vertikální navýšení kapacity, může být nutné samostatný procesový server a má hello stejné požadavky jako hello konfigurační server.<br/><br/> Další informace o těchto součástí [sem](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements)
**Na místní servery VMware** | Jeden nebo více VMware vSphere serverů, 6.5, 6.0, 5.5, 5.1 s nejnovější aktualizace. Servery musí nacházet ve stejné sítě jako konfigurační server hello (nebo samostatný procesový server) hello.<br/><br/> VCenter server doporučujeme toomanage hostitele, kde běží verze 6.5, 6.0 nebo 5.5 s nejnovějšími aktualizacemi hello.
**Místní virtuální počítače** | Virtuální počítače chcete tooreplicate by měla být spuštěná [podporovaný operační systém](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)a v souladu s [požadavky Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements). Virtuální počítač by měl mít spuštění nástroje VMware.
**Adresy URL** | konfigurační server Hello potřebuje přístup k toothese adresy URL:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Pokud máte pravidla brány firewall založená na adresu IP, ověřte, že umožňují tooAzure komunikace.<br/></br> Povolit hello [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653)a hello port HTTPS (443).<br/></br> Povolte rozsahy IP adres pro hello oblast Azure svého předplatného a západní USA (používá se pro správu Identity a řízení přístupu).<br/><br/> Povolit tuto adresu URL pro stahování MySQL hello: http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi.
**Služba mobility** | Nainstalovat na každý replikovaných virtuálních počítačů.




## <a name="limitations"></a>Omezení

Ujistěte se, že rozumíte hello omezení shrnuté v tabulce hello před nasazením.

**Omezení** | **Podrobnosti**
--- | ---
**Azure** | Účty úložiště a sítě musí být v hello stejné oblasti jako trezor hello<br/><br/> Pokud používáte prémiový účet úložiště, musíte také standardní ukládat protokoly replikace toostore účtu<br/><br/> Nelze replikovat toopremium účty ve střední a – Jih, Indie.
**Lokální konfigurační server** | Typ adaptéru virtuálního počítače VMware musí být VMXNET3. Pokud tomu tak není, [tuto aktualizaci nainstalovat](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1)<br/><br/> vSphere PowerCLI 6.0 by měly být nainstalovány.<br/><br> Hello počítač by neměl být řadič domény. Hello počítač by měl mít statickou IP adresu.<br/><br/> název hostitele Hello by měl být maximálně 15 znaků nebo méně, a operační systém by měl být v angličtině.
**VMware** | Site Recovery nepodporuje nové vCenter a vSphere 6.5 a 6.0 funkce, jako křížové řešení vMotion vCenter, virtuální svazky a úložiště DRS.
**Virtuální počítače** | Ověřte [omezení virtuálního počítače Azure](site-recovery-prereq.md#azure-requirements)<br/><br/> Nelze replikovat virtuální počítače s šifrované disky nebo virtuální počítače pomocí rozhraní UEFI nebo EFI.<br/><br> Sdílený disk, clustery nejsou podporované. Pokud hello zdrojového virtuálního počítače má seskupování síťových adaptérů, převede se tooa jeden síťový adaptér po převzetí služeb při selhání.<br/><br/> Pokud virtuální počítače iSCSI disk, Site Recovery převede souboru virtuálního pevného disku tooa po převzetí služeb při selhání. Pokud hello cíle iSCSI, dosažitelný z hello virtuálního počítače Azure, připojí tooit a zobrazí se i hello virtuálního pevného disku. Pokud k tomu dojde, odpojte hello cíle iSCSI.<br/><br/> Pokud chcete, aby tooenable více virtuálních počítačů konzistence, která umožňuje virtuální počítače se systémem hello stejné zatížení toobe obnovit společně tooa konzistentní datový bod, otevřete port 20004 hello virtuálních počítačů.<br/><br/> Windows musí být nainstalován na jednotce hello C. disk s operačním systémem Hello by měl být základní a nikoli dynamické. Hello datový disk může být dynamické.<br/><br/> Soubory Linux/etc/hosts na virtuálních počítačích by měly obsahovat položky, které mapují hello místního hostitele název tooIP adres přidružených všechny síťové adaptéry. Hello název hostitele, přípojné body, název zařízení, systémové cesty a názvy souborů (/ etc; USR) by měly být pouze v angličtině.<br/><br/> Konkrétní typy [Linux úložiště](site-recovery-support-matrix-to-azure.md#support-for-storage) jsou podporovány.<br/><br/>Vytvořit nebo nastavit **disk.enableUUID=true** v nastavení virtuálního počítače hello. To poskytuje konzistentní UUID toohello VMDK, tak, aby správně připojí a zajišťuje, aby jenom rozdílové změny přenášená back místní tooon během navrácení služeb po obnovení bez úplná replikace.


## <a name="next-steps"></a>Další kroky

- Pokud jste to úplné nasazení, přejděte příliš[krok 3: plánování kapacity](vmware-walkthrough-capacity.md)
- Pokud jste to jednoduchá testovací nasazení, přejděte příliš[krok 4: plánování sítě](vmware-walkthrough-network.md).
