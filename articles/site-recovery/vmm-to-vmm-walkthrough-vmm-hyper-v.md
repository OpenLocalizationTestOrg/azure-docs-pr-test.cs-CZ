---
title: "aaaSet VMM a technologie Hyper-V pro replikaci tooa sekundární lokalitu s Azure Site Recovery | Microsoft Docs"
description: "Popisuje, jak tooset servery System Center VMM a hostitelů Hyper-V pro replikaci tooa sekundární lokalita VMM."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d0389e3b-3737-496c-bda6-77152264dd98
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 677bf6d38328ccc425e3b0f056d03159a52da428
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-4-set-up-vmm-and-hyper-v-for-hyper-v-vm-replication-tooa-secondary-site"></a>Krok 4: Nastavení VMM nebo Hyper-V pro virtuální počítač Hyper-V replikace tooa sekundární lokality 

Poté, co jste připravili pro sítě, nastavení servery System Center Virtual Machine Manager (VMM) a hostitele Hyper-V pro Hyper-V virtuální počítač (VM) replikace tooa sekundární lokalitu pomocí [Azure Site Recovery](site-recovery-overview.md) v hello portálu Azure. 

Po přečtení tohoto článku, post jakékoli komentáře v dolní části hello nebo na hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).



## <a name="prepare-vmm-servers"></a>Připravit servery VMM 

tooprepare pro nasazení:


1. Zajistěte, aby servery VMM dodržovat hello [podporu požadavků](site-recovery-support-matrix-to-sec-site.md#on-premises-servers), a [požadavky nasazení](vmm-to-vmm-walkthrough-prerequisites.md).
2. Zkontrolujte, zda jsou servery VMM připojené toohello internet a mít přístup toothese adresy URL.
    
    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
    - Pokud máte pravidla brány firewall založená na adresu IP, ověřte, že umožňují tooAzure komunikace.
    - Povolit hello [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653)a hello port HTTPS (443).
    - Povolte rozsahy IP adres pro hello oblast Azure svého předplatného a západní USA (používá se pro správu Identity a řízení přístupu).
3. Zkontrolujte, zda je hello VMM server [připravit mapování sítě](vmm-to-vmm-walkthrough-network.md#prepare-for-network-mapping)


## <a name="prepare-hyper-v-hostsclusters"></a>Příprava hostitele nebo Clustery Hyper-V

1. Zajistěte, aby hostitele nebo Clustery Hyper-V souladu s hello [podporu požadavků](site-recovery-support-matrix-to-sec-site.md#on-premises-servers), a [požadavky nasazení](vmm-to-vmm-walkthrough-prerequisites.md).
2. Ověřte požadavky hello [virtuálních počítačů Hyper-V](site-recovery-support-matrix-to-sec-site.md#support-for-replicated-machine-os-versions)
3. Ověřte [sítě](site-recovery-support-matrix-to-sec-site.md#network-configuration) a [úložiště](site-recovery-support-matrix-to-sec-site.md#storage) požadavky.
4. Zkontrolujte, zda hostitelé Hyper-V jsou připojené toohello internet a mít přístup toothese adresy URL.
    
    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
    - Pokud máte pravidla brány firewall založená na adresu IP, ověřte, že umožňují tooAzure komunikace.
    - Povolit hello [rozsahy IP Datacentra Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653)a hello port HTTPS (443).
    - Povolte rozsahy IP adres pro hello oblast Azure svého předplatného a západní USA (používá se pro správu Identity a řízení přístupu).

## <a name="prepare-for-single-server-deployment"></a>Příprava pro nasazení jednoho serveru


Pokud máte pouze jeden server VMM, můžete replikovat virtuální počítače v hostitelích technologie Hyper-V v cloudu VMM hello příliš[Azure](hyper-v-site-walkthrough-overview.md) nebo tooa sekundární cloudu VMM, jak je popsáno v tomto dokumentu. Protože replikovat mezi cloudy není bezproblémové doporučujeme první možnost hello.

Pokud chcete tooreplicate mezi cloudy, můžete replikovat jeden samostatný server VMM nebo s jeden server VMM nasazený v roztažené clusteru se systémem Windows

### <a name="replicate-with-a-standalone-vmm-server"></a>Replikovat s samostatný server VMM

V tomto scénáři nasazení hello jednoho serveru VMM jako virtuální počítač v primární lokalitě hello a replikovat tohoto virtuálního počítače tooa sekundárního webového serveru pomocí Site Recovery a replika technologie Hyper-V.

1. **Nastavit VMM na virtuálním počítači technologie Hyper-V**. Doporučujeme společné umístění instanci systému SQL Server hello používá VMM na hello stejného virtuálního počítače. Tím ušetříte čas, jako má toobe vytvořit pouze jeden virtuální počítač. Pokud chcete, aby toouse vzdálenou instanci systému SQL Server a dojde k výpadku, je třeba toorecover tuto instanci než bude možné obnovit VMM.
2. **Zkontrolujte má hello VMM server nakonfigurované alespoň dva cloudy**. Hello, které virtuální počítače mají tooreplicate a hello jiné cloudové bude sloužit jako hello sekundární umístění bude obsahovat jeden cloud. Hello cloudu, který obsahuje virtuální počítače hello chcete tooprotect, by měly splňovat [požadavky](#prerequisites).
3. Nastavte Site Recovery, jak je popsáno v tomto článku. Vytvoření a registrace serveru VMM hello v trezoru, nastavíte zásady replikace a povolení replikace. Hello zdrojové a cílové názvy VMM bude hello stejné. Zadejte že počáteční replikace probíhá přes síť hello.
4. Při nastavování mapování sítě namapujete hello síť virtuálních počítačů pro síť virtuálních počítačů toohello hello primárního cloudu pro cloud sekundární hello.
5. V konzole Správce technologie Hyper-V hello povolit replika technologie Hyper-V na hostiteli hello technologie Hyper-V, který obsahuje hello virtuálních počítačů nástroje VMM a zapnout replikaci na hello virtuálních počítačů. Zajistěte, aby že si nepřidáte tooclouds hello VMM virtuálního počítače, které jsou chráněné službou Site Recovery, tooensure nastavení repliky technologie Hyper-V nejsou přepsána Site Recovery.
6. Pokud vytvoříte plány obnovení pro převzetí služeb při selhání, které používáte hello stejnému serveru nástroje VMM zdroje a cíle.
7. V výpadku dokončení převzetí služeb při selhání a obnovit takto:

   1. Spusťte neplánované převzetí služeb při selhání v konzole Správce technologie Hyper-V hello v sekundární lokalitě hello toofail přes hello primárních virtuálních počítačů ve VMM toohello sekundární lokality.
   2. Ověřte, že hello virtuálních počítačů nástroje VMM je zapnutý a běží a v trezoru hello, spusťte toofail neplánované převzetí služeb při selhání přes hello virtuální počítače z primární toosecondary cloudů. Potvrdit hello převzetí služeb při selhání a vyberte alternativní bod obnovení v případě potřeby.
   3. Po hello neplánované převzetí služeb při selhání dokončení všechny prostředky jsou přístupné z primární lokality hello znovu.
   4. Pokud je k dispozici znovu v konzole Správce technologie Hyper-V hello v sekundární lokalitě hello hello primární lokality, povolte reverzní replikaci pro hello virtuálních počítačů nástroje VMM. Ze sekundární tooprimary spustí replikaci pro hello virtuálních počítačů.
   5. Spusťte plánované převzetí služeb při selhání v konzole Správce technologie Hyper-V hello v sekundární lokalitě hello toofail přes hello virtuálních počítačů ve VMM toohello primární lokality. Potvrzení převzetí služeb při selhání hello. Tak, aby hello virtuálních počítačů nástroje VMM je znovu replikace z primární toosecondary povolíte zpětnou replikaci.
   6. V trezoru služeb zotavení hello povolte reverzní replikaci pro hello zatížení virtuálních počítačů, je replikace z sekundární tooprimary toostart.
   7. V trezoru služeb zotavení hello spustit plánované převzetí služeb při selhání toofail back hello zatížení virtuálních počítačů toohello primární lokality. Potvrzení převzetí služeb při selhání toocomplete hello ho. Potom povolte zpětná replikace toostart replikaci hello zatížení z primárního toosecondary virtuálních počítačů.

### <a name="replicate-with-a-stretched-vmm-cluster"></a>Replikovat s clusterem s podporou roztažené VMM

Místo nasazování samostatný server VMM jako virtuální počítač, který replikuje tooa sekundární lokality, můžete provést VMM, které jsou vysoce dostupné po nasazení jako virtuální počítač v clusteru s podporou převzetí služeb při selhání systému Windows. To poskytuje pružnost úloh a ochranu proti selhání hardwaru. toodeploy pomocí Site Recovery hello virtuálních počítačů nástroje VMM musí být nasazené v clusteru s podporou funkce stretch geograficky vzdálených lokalit. toodo toto:

1. Instalace VMM na virtuálním počítači v clusteru s podporou převzetí služeb při selhání systému Windows a vyberte hello možnost toorun hello serveru jako vysoce dostupný během instalace.
2. Hello instance systému SQL Server, který je používán VMM by měla být replikována se skupinami dostupnosti AlwaysOn serveru SQL, tak, aby bylo repliku hello databáze v sekundární lokalitě hello.
3. Postupujte podle pokynů hello v toocreate Tento článek k trezoru, zaregistrujte hello server a nastavení ochrany. Je nutné tooregister každý server VMM v hello cluster v hello trezor služeb zotavení. toodo, nainstalujete hello zprostředkovatele na aktivním uzlu a registrace serveru VMM hello. Potom nainstalujte hello zprostředkovatele na jiných uzlech.
4. Když dojde k výpadku, hello VMM server a jeho odpovídající databázi systému SQL Server jsou převzetí služeb při selhání a k němu přistupovat z hello sekundární lokality.



## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 5: nastavení trezoru](vmm-to-vmm-walkthrough-create-vault.md).
