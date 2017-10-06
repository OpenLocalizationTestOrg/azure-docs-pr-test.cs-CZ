---
title: "aaaPlan sítě pro replikaci technologie Hyper-V (withSystem Center VMM) tooAzure s Azure Site Recovery | Microsoft Docs"
description: "Tento článek popisuje plánování sítě vyžaduje při replikaci virtuálních počítačů Hyper-V (s nástrojem VMM) tooAzure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 41c3c83e-6b1a-496a-8179-498c552ef0c7
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 51ca8b939b6f96880f83599ea8009eb0bfa5b957
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-4-plan-networking-for-hyper-v-with-vmm-tooazure-replication"></a>Krok 4: Plánování sítě pro replikaci tooAzure technologie Hyper-V (s nástrojem VMM)

Po provedení [plánování kapacity](vmm-to-azure-walkthrough-capacity.md) (Pokud provádíte úplné nasazení), přečtěte si tento článek toolearn o síti replikovat místní virtuální počítače Hyper-v System Center Virtual Machine Manager (VMM) při plánování do úvahy tooAzure cloudy, pomocí hello [Azure Site Recovery](site-recovery-overview.md) služby.

Odeslat všechny komentáře v dolní části hello tohoto článku nebo pokládání dotazů v hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="network-mapping-for-replication-tooazure"></a>Mapování sítě pro replikaci tooAzure

Mapování sítě se používá při replikaci virtuálních počítačů Hyper-V (spravované v nástroji VMM) tooAzure. Síťové mapování mapuje sítě virtuálních počítačů na zdrojovém serveru VMM a cílové sítě Azure. Mapování hello následující:

- **Síťové připojení**– po převzetí služeb při selhání, všechny replikované virtuální počítače Azure jsou připojené toohello namapované síti Azure. Všechny počítače, které převzetí služeb při selhání na hello stejné se mohou připojit tooeach jiných, i když při selhání v plánech různých obnovení.
- **Brána sítě**– Pokud na hello cílové síti Azure nastavená brána sítě, virtuální počítače můžete připojit síť virtuálních počítačů v hello namapované na některých místní tooother.

### <a name="prepare-vmm-for-network-mapping"></a>Příprava VMM mapování sítě

Připravte VMM mapování sítě následujícím způsobem:

1. Pokud nemáte, připravte se [logická síť VMM](https://docs.microsoft.com/system-center/vmm/network-logical) který je spojen s hello cloudu, ve které hello technologie Hyper-V jsou umístěni hostitelé.
2. Pokud nemáte, vytvoří [síť virtuálních počítačů](https://docs.microsoft.com/system-center/vmm/network-virtual) propojené toohello logické sítě připravený výše.
3. Připojte virtuální počítače na hello technologie Hyper-V serveru nebo clusteru hostitele v cloudu VMM, síť virtuálních počítačů toohello hello.

 
Poznámky: 
- Nové virtuální počítače jsou přidány toohello zdrojové síti virtuálních počítačů jsou připojené toohello namapované síti Azure, když dojde k replikaci.
- Pokud má hello Cílová síť více podsítí a jedna z těchto podsítí má hello stejný název jako podsíť, na které hello zdrojový virtuální počítač nachází, pak virtuální počítač repliky hello připojí po převzetí služeb při selhání toothat cílové podsíti.
- Pokud není žádná cílová podsíť s odpovídajícím názvem, hello virtuální počítač připojí toohello první podsíť v síti hello.
- Připravte sítě Azure a nastavit mapování sítě, jak nasadit hello scénář.

## <a name="connecting-tooazure-vms-after-failover"></a>Připojení tooAzure virtuální počítače po převzetí služeb při selhání

Při plánování replikace a převzetí služeb při selhání, je mezi klíčové otázky hello jak tooconnect toohello virtuálního počítače Azure po převzetí služeb při selhání. Při navrhování strategie sítě pro virtuální počítače Azure repliky existuje několik možností:

- **Použít jinou IP adresu**: můžete vybrat toouse jiný rozsah IP adres pro síť virtuálních počítačů Azure replikovat hello. V tento scénář hello počítač získá nové adresy IP po převzetí služeb při selhání a je nutná aktualizace DNS.
- **Zachovat hello stejnou IP adresu**: můžete chtít toouse hello stejného rozsahu IP adres, jako v primární místní sítě, pro hello síť Azure po převzetí služeb při selhání.  Zachování hello stejné IP adresy zjednodušuje obnovení hello snížením sítě o problémech souvisejících s po převzetí služeb při selhání. Ale když replikujete tooAzure, budete potřebovat tooupdate tras se nové umístění hello hello IP adres po převzetí služeb při selhání.


## <a name="retain-ip-addresses"></a>Zachovat IP adresy

Site Recovery poskytuje hello schopností tooretain pevné IP adresy při přebírání služeb při selhání tooAzure s převzetím služeb podsítě.

S převzetí služeb při selhání podsíť určité podsíti se nachází v lokalitě 1 nebo 2 lokality, ale nikdy v obou lokalitách současně. V pořadí toomaintain hello adresní prostor IP adres v hello události převzetí služeb při selhání můžete programově uspořádat pro hello směrovač infrastruktury toomove hello podsítě z jedné lokality tooanother. Během převzetí služeb při selhání související hello přesunutí podsítě s hello chráněných virtuálních počítačů. Hello Hlavní nevýhodou je, že v případě hello selhání, máte toomove hello celou podsíť.



### <a name="failover-example"></a>Příklad převzetí služeb při selhání

Podívejme se na příklad tooAzure převzetí služeb při selhání.

- Ficticious společnosti, společnosti Woodgrove Bank má místní infrastruktury hostování jejich obchodních aplikací. Své mobilní aplikace jsou hostované v Azure.
- Připojení mezi virtuálními počítači Woodgrove Bank v Azure a místními servery jsou poskytovány připojení site-to-site (VPN) mezi hello místní hraniční síti a hello virtuální síť Azure.
- To VPN znamená, že hello společnosti virtuální sítě v Azure se zobrazí jako rozšíření své místní sítě.
- Woodgrove chce toouse Site Recovery tooreplicate místní úlohy tooAzure.
 - Woodgrove má toodeal s aplikacemi a konfigurace, které závisí na pevně IP adresy a proto po převzetí služeb při selhání tooAzure potřebovat tooretain IP adresy pro své aplikace.
 - Woodgrove má přiřazené IP adres z rozsahu 172.16.1.0/24 172.16.2.0/24 tooits prostředky běžící v Azure.


Pro Woodgrove toobe možné tooreplicate, jeho tooAzure virtuálních počítačů při zachování hello IP adresy zde je, jaké hello společnost potřebuje toodo:

1. Vytvoření virtuální sítě Azure. Je nutné rozšíření hello místní sítě, tak, aby aplikace můžete převzetí služeb při selhání bez problémů.
2. Azure umožňuje vám tooadd site-to-site VPN připojení, kromě připojení toopoint-to-site toohello virtuálním sítím vytvořeným v Azure.
3. Při nastavování připojení site-to-site hello, v hello Azure síť, pouze v případě, že se liší od rozsah adres IP místní hello hello rozsah IP adres je možné směrovat provoz toohello místní umístění (místní síť).
    - Je to proto, že Azure nepodporuje roztažené podsítě. Takže pokud máte podsíť 192.168.1.0/24 místně, nemůžete přidat 192.168.1.0/24 místní sítě v hello síť Azure.
    - Toto je očekávané, protože Azure není známo, že nejsou žádné aktivní virtuální počítače v podsíti hello a zotavení po havárii. jenom se vytváří této podsíti hello.
    - nesmí být v konfliktu toobe možné toocorrectly směrovat síťový provoz z podsítě hello síť Azure v síti hello a místní sítí hello.

![Před převzetí služeb při selhání podsíť](./media/vmm-to-azure-walkthrough-network/network-design7.png)

### <a name="before-failover"></a>Před převzetí služeb při selhání

1. Vytvoření další sítě (například síť obnovení). Toto je hello sítě ve který převzal virtuální počítače byly vytvořeny.
2. tooensure, který hello IP adresu pro virtuální počítač se uchovávají po převzetí služeb ve vlastnostech virtuálního počítače hello > **konfigurace**, zadejte hello stejnou IP adresu tohoto hello má místní virtuální počítač a klikněte na tlačítko **Uložit**.
3. Když hello virtuálních počítačů při selhání, přiřadí Azure Site Recovery hello zadat IP adresu tooit.

    ![Vlastnosti sítě](./media/vmm-to-azure-walkthrough-network/network-design8.png)

4. Po převzetí služeb při selhání je aktivační událost se aktivuje a hello virtuální počítače jsou vytvořené v Azure s IP adresou hello vyžaduje, můžete připojit pomocí sítě toohello [Vnet tooVnet vonnection](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). Tato akce může provádět skriptování.
5. Trasy potřebovat odpovídajícím způsobem upravit, toobe tooreflect této 192.168.1.0/24 teď přesunul tooAzure.

    ![Po převzetí služeb při selhání podsíť](./media/vmm-to-azure-walkthrough-network/network-design9.png)

### <a name="after-failover"></a>Po převzetí služeb při selhání

Pokud nemáte síť Azure, jak je popsáno výše, můžete vytvořit připojení site-to-site VPN mezi primární lokalitou a Azure, po failvoer.

## <a name="change-ip-addresses"></a>Změna IP adresy

To [příspěvku na blogu](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) vysvětluje, jak tooset až hello Azure síťová infrastruktura, pokud nepotřebujete tooretain IP adres po převzetí služeb při selhání. Popis aplikace spustí, vypadá na jak tooset až sítě místně a v Azure a s informacemi o spuštění převzetí služeb při selhání se ukončí.  

## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 5: Příprava Azure](vmm-to-azure-walkthrough-prepare-azure.md)
