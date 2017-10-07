---
title: "aaaHow toofail zpět z Azure tooVMware | Microsoft Docs"
description: "Po převzetí služeb při selhání tooAzure virtuální počítače můžete spustit navrácení služeb po obnovení toobring virtuální počítače zpět tooon místní. Přečtěte si postup hello jak toofail zpět."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: ruturajd
ms.openlocfilehash: b82abf6b15db9dccab49edbd14298b121e9fdc6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="fail-back-from-azure-tooan-on-premises-site"></a>Po obnovení vrátit Azure tooan místního serveru

Tento článek popisuje, jak toofail zálohování virtuálních počítačů z Azure Virtual Machines toohello místní lokality. Postupujte podle pokynů hello v této toofail článku zálohování virtuálních počítačů VMware nebo fyzických serverů Windows nebo Linuxem po jejich jste převzetí služeb při selhání z hello místní lokality tooAzure pomocí hello [VMware replikovat virtuální počítače a fyzické servery tooAzure s Azure Site Recovery](site-recovery-vmware-to-azure-classic.md) kurzu.

> [!WARNING]
> Pokud máte [dokončit migraci](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration)hello přesunutý virtuální počítač tooanother prostředek skupiny nebo odstraněné hello virtuální počítač Azure, nemůžete poté navrácení služeb po obnovení.

> [!NOTE]
> Pokud jste při selhání virtuálních počítačů VMware nemůžete navrácení služeb po obnovení tooa technologie Hyper-v hostiteli.

## <a name="overview-of-failback"></a>Přehled o navrácení služeb po obnovení
Zde je, jak funguje navrácení služeb po obnovení. Po převedla tooAzure selhání tooyour zpět na místní lokalitu v několika fázích:

1. [Znovu nastavte ochranu](site-recovery-how-to-reprotect.md) hello virtuální počítače na platformě Azure tak, aby jejich spuštění tooreplicate tooVMware virtuální počítače ve vaší místní lokalitě. Jako součást tohoto procesu musíte také:
    1. Nastavit místní hlavní cílový: Windows hlavnímu cíli pro virtuální počítače s Windows a [hlavního cíle Linuxu](site-recovery-how-to-install-linux-master-target.md) pro virtuální počítače s Linuxem.
    2. Nastavení [procesový server](site-recovery-vmware-setup-azure-ps-resource-manager.md).
    3. Zahájit [znovu nastavte ochranu](site-recovery-how-to-reprotect.md). Tato akce hello místní virtuální počítač vypnout a synchronizovat hello Azure dat virtuálního počítače s hello místní disky.
5. Po virtuální počítače v Azure jsou replikace tooyour místní lokality, zahájíte selhání přes z Azure toohello místní lokality.

Po vašich dat se nezdařila zpět, je znovu nastavte ochranu hello místní virtuální počítače, které při selhání zpátky do tak, aby jejich spuštění tooreplicate tooAzure.

Rychlý přehled sledujte hello následující video o tom, jak toofail přes z Azure tooan místní lokality.
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]

### <a name="fail-back-toohello-original-or-alternate-location"></a>Selhání back toohello původního nebo alternativního umístění

Pokud jste při selhání virtuálního počítače VMware, může selhat back toohello stejný zdroj místní virtuální počítač v případě ho stále existuje. V tomto scénáři se replikují jenom hello změny zpět. Tento scénář se označuje jako obnovení do původního umístění. Pokud hello na místním virtuálním počítači neexistuje, hello scénář je obnovení do alternativního umístění.

> [!NOTE]
> Je možné pouze původní vCenter toohello navrácení služeb po obnovení a konfigurační server. Nelze nasadit novou konfigurační server a použití navrácení služeb po obnovení. Navíc nelze přidat nový vCenter toohello existujícímu konfigurační server a navrácení služeb po obnovení do nové vCenter hello.

#### <a name="original-location-recovery"></a>Obnovení do původního umístění

Pokud tak back toohello původní virtuální počítač, je potřeba hello následující podmínky:
* Pokud hello virtuální počítač je spravovaný vCenter server, pak hello hostitele ESX hlavní cíl měli datastore přístup toohello virtuálního počítače.
* Pokud hello virtuálního počítače na hostiteli ESX, ale není spravován nástrojem vCenter, hello pevný disk hello virtuálního počítače musí být v úložiště dat hlavní zaměřeným hello hostitelé mají přístup.
* Pokud virtuální počítač na hostiteli ESX a nepoužívá vCenter, by měl předtím, než můžete znovu nastavte ochranu dokončit zjišťování hostitele ESX hello hello hlavního cíle. To platí, pokud po obnovení zpět fyzických serverů, příliš.
* Může selhat back tooa virtuální síť SAN (vSAN) nebo disk, který podle nezpracované zařízení mapování (RDM), pokud hello disky již existují a jsou připojené toohello místní virtuální počítač.

#### <a name="alternate-location-recovery"></a>Obnovení do náhradního umístění
Pokud hello místní virtuální počítač před opětovnou ochranu hello virtuálního počítače neexistuje, hello scénář nazývá obnovení do alternativního umístění. pracovní postup opětovné ochrany Hello vytvoří hello místní virtuální počítač znovu. Také to způsobí stahování úplná data.

* Pokud žádnou back tooan alternativní umístění, hello virtuální počítač bude obnovená toohello stejného hostitele ESX, na které hello je hlavním cílovém serveru nasazený. Hello úložiště dat, který byl použit toocreate hello disku bude hello stejné úložiště, který byl vybrán při opětovné povolení ochrany hello virtuálního počítače.
* Úložiště souborů systému (VMFS) zpět pouze tooa virtuálního počítače může selhat. Pokud máte síť vSAN nebo RDM, opětovné ochrany a navrácení služeb po obnovení nebude fungovat.
* Opětovné ochrany zahrnuje jeden velký počáteční přenos dat, za kterým následuje hello změny. Tento proces existuje, protože hello virtuálního počítače na místní neexistuje. kompletní datový Hello musí replikovat zpět toobe. Tato opětovné ochrany bude taky trvat déle než obnovení do původního umístění.
* Nelze označit zpět toovSAN RDM podle nebo disků. V úložišti dat VMFS lze vytvářet pouze nové disky virtuálního počítače (VMDKs).

Fyzický počítač, když převzal tooAzure, můžete se nezdařilo zpět pouze jako virtuální počítač VMware (také odkazované tooas P2A2V). Tento tok klesne pod obnovení do náhradního umístění hello.

* Fyzický server Windows Server 2008 R2 SP1, pokud ochranu a při selhání tooAzure, nemůže se zpět.
* Ujistěte se, že zjistíte alespoň jeden hlavní cílový server a hello nezbytné ESX/ESXi hostitelů toowhich potřebujete toofail zpět.

## <a name="have-you-completed-reprotection"></a>Dokončili jste vytvoření?
Než budete pokračovat, nastavte znovu dokončení hello kroky, aby hello virtuální počítače jsou ve stavu, replikované, a můžete zahájit převzetí služeb při selhání back tooan místní lokalitě. Další informace najdete v tématu [jak tooreprotect z Azure tooon místní](site-recovery-how-to-reprotect.md).

## <a name="prerequisites"></a>Požadavky

* Konfigurační server je vyžadováno místní uděláte navrácení služeb po obnovení. Během navrácení služeb po obnovení hello virtuální počítač, musí existovat v databázi serveru konfigurace hello nebo navrácení služeb po obnovení nebude úspěšné. Proto zajistěte podniknout pravidelně naplánované zálohování serveru. Pokud dojde havárii, budete potřebovat server hello toorestore s hello stejnou IP adresu pro toowork navrácení služeb po obnovení.
* hlavní cílový server Hello by neměl mít všechny snímky před spuštěním navrácení služeb po obnovení.

## <a name="steps-toofail-back"></a>Kroky toofail zpět

> [!IMPORTANT]
> Před spuštěním navrácení služeb po obnovení, ujistěte se, že jste dokončili vytvoření hello virtuálních počítačů. Hello virtuální počítače musí být v chráněném stavu, a jejich stavu by měl být **OK**. tooreprotect hello virtuální počítače, přečtěte si [jak tooreprotect](site-recovery-how-to-reprotect.md).

1. V hello replikované položky vyberte hello virtuálního počítače a klikněte pravým tlačítkem ji tooselect **neplánované převzetí služeb při selhání**.
2. V **potvrzení převzetí služeb při selhání**, ověřte hello směr převzetí služeb při selhání (z Azure) a pak vyberte hello bodu obnovení (nejnovější nebo nejnovější aplikace hello konzistentní) chcete toouse hello převzetí služeb při selhání. bod konzistentní s aplikací Hello je za hello nejnovějšího bodu v čase a způsobí ztrátu dat.
3. Během převzetí služeb při selhání Site Recovery vypne hello virtuální počítače na platformě Azure. Po můžete zkontrolovat, že tento navrácení služeb po obnovení je dokončená, podle očekávání, můžete zkontrolovat, že byla vypnuta hello virtuální počítače na platformě Azure.

### <a name="toowhat-recovery-point-can-i-fail-back-hello-virtual-machines"></a>bod obnovení toowhat může selhat back hello virtuální počítače?

Během navrácení služeb po obnovení máte dvě možnosti toofail back hello obnovení virtuálního počítače nebo naplánovat.

Pokud vyberete hello zpracovat nejnovější bod v čase, budou všechny virtuální počítače při selhání tootheir nejnovějšímu dostupnému bodu v čase. V případě, že je skupina replikace v rámci plánu obnovení hello, pak každý virtuální počítač hello replikační skupiny se převzetí služeb při selhání tooits nezávislé nejnovějšího bodu v čase.

Virtuální počítač nelze označit zpět, dokud má alespoň jeden bod obnovení. Plán obnovení nelze označit zpět, dokud všechny virtuální počítače mít alespoň jeden bodu obnovení.

> [!NOTE]
> Nejnovější bod obnovení je bod obnovení konzistentní při selhání.

Pokud vyberete hello bodu obnovení konzistentnímu s aplikací, bude navrácení jeden virtuální počítač obnovit tooits nejnovějšího bodu obnovení k dispozici konzistentních s aplikací. V případě hello plánu obnovení s replikační skupinou obnoví každou skupinu replikace tooits společný bod obnovení.
Všimněte si, že bodů obnovení konzistentních s aplikací mohou být za v čase a může být ztráty dat.

### <a name="what-happens-toovmware-tools-post-failback"></a>Co se stane, že tooVMware nástroje post navrácení služeb po obnovení?

Během převzetí služeb při selhání tooAzure nástroje VMware hello nesmí být nainstalován na hello virtuální počítač Azure. V případě virtuálního počítače s Windows zakáže automatické obnovení systému nástroje VMware hello během převzetí služeb při selhání. V případě virtuální počítač Linux automatické obnovení systému odinstaluje nástroje VMware hello během převzetí služeb při selhání.

Během navrácení služeb po obnovení virtuálního počítače Windows hello jsou nástroje VMware hello při navrácení služeb po obnovení znovu zapnout. Podobně pro virtuální počítač s linuxem nástroje VMware hello jsou přeinstalovány na počítači hello během navrácení služeb po obnovení.

## <a name="next-steps"></a>Další kroky

Po dokončení navrácení služeb po obnovení musíte toocommit tooensure hello virtuálního počítače, která obnovena virtuálních počítačů v Azure, se odstraní.

### <a name="commit"></a>Potvrzení
Potvrzení je požadovaná tooremove hello převzal virtuální počítač z Azure.
Klikněte pravým tlačítkem na položku hello chráněný a pak klikněte na **potvrdit**. Úlohy dojde k odebrání hello převzal virtuálních počítačů v Azure.

### <a name="reprotect-from-on-premises-tooazure"></a>Nastavte znovu z místní tooAzure

Po dokončení potvrzení virtuálního počítače je zpět na hello místní lokality, ale nebudou chráněné. toostart tooreplicate tooAzure znovu hello následující:

1. V **trezoru** > **nastavení** > **replikované položky**, vyberte hello virtuálních počítačů, které selhaly zpět a pak klikněte na tlačítko  **Znovu nastavit ochranu**.
2. Zadejte hodnotu hello hello procesového serveru, který potřebuje toobe používá toosend data zpět tooAzure.
3. Klikněte na tlačítko **OK** toobegin hello opětovné ochrany úlohy.

> [!NOTE]
> Poté, co se místní virtuální počítač se spustí, trvá nějakou dobu, než hello agent tooregister back toohello konfigurační server (až too15 minut). Během této doby znovu nastavte ochranu selže a vrátí chybovou zprávu s oznámením, že hello agent není nainstalován. Počkejte několik minut a pak zkuste znovu opětovné ochrany.

Po znovu nastavte ochranu hello dokončí úlohu, hello virtuální počítač se nereplikuje back tooAzure a můžete provést převzetí služeb při selhání.

## <a name="common-issues"></a>Běžné problémy
Zajistěte, aby byl tento hello vCenter v připojeném stavu před provedením navrácení služeb po obnovení. Jinak hodnota odpojení disků a jejich připojení back toohello virtuálního počítače selže.
