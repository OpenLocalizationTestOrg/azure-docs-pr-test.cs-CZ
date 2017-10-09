---
title: "aaaPlan sítě pro fyzický server replikace tooAzure | Microsoft Docs"
description: "Tento článek popisuje plánování sítě požadované při replikaci tooAzure fyzických serverů"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 71db2435-b5ce-4263-83f6-093d10d1d4e1
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: e2ca2db2a1cb58ca5468d4ee2b0406f29ff09479
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="step-4-plan-networking-for-physical-server-replication-tooazure"></a>Krok 4: Plánování sítě pro replikaci tooAzure fyzického serveru

Tento článek shrnuje sítě replikace místní fyzických serverů tooAzure pomocí hello při plánování do úvahy [Azure Site Recovery](site-recovery-overview.md) služby.

Odeslat všechny komentáře v dolní části hello tohoto článku nebo pokládání dotazů v hello [fóru Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="connect-tooreplica-azure-vms"></a>Připojit virtuální počítače Azure tooreplica

Při plánování replikace a převzetí služeb při selhání, je mezi klíčové otázky hello jak tooconnect toohello virtuálního počítače Azure po převzetí služeb při selhání. Při navrhování strategie sítě pro virtuální počítače Azure repliky existuje několik možností:

- **Použít jinou IP adresu**: můžete vybrat toouse jiný rozsah IP adres pro síť virtuálních počítačů Azure replikovat hello. V tomto scénáři hello počítač získá nové adresy IP, po převzetí služeb při selhání a aktualizace služby DNS je vyžadován.
- **Použití hello stejnou IP adresu**: můžete chtít toouse hello stejného rozsahu IP adres, který ve vaší primární místní lokalitě pro hello síť Azure po převzetí služeb při selhání. Zachování hello stejné IP adresy zjednodušuje obnovení hello snížením sítě o problémech souvisejících s po převzetí služeb při selhání. Ale když replikujete tooAzure, budete potřebovat tooupdate tras se nové umístění hello hello IP adres po převzetí služeb při selhání.

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


Pro Woodgrove toobe možné tooreplicate, jeho servery tooAzure při zachování hello IP adresy zde je, jaké hello společnost potřebuje toodo:

1. Vytvoření virtuální sítě Azure. Je nutné rozšíření hello místní sítě, tak, aby aplikace můžete převzetí služeb při selhání bez problémů.
2. Azure umožňuje vám tooadd site-to-site VPN připojení, kromě připojení toopoint-to-site toohello virtuálním sítím vytvořeným v Azure.
3. Při nastavování připojení site-to-site hello, v hello Azure síť, pouze v případě, že se liší od rozsah adres IP místní hello hello rozsah IP adres je možné směrovat provoz toohello místní umístění (místní síť).
    - Je to proto, že Azure nepodporuje roztažené podsítě. Takže pokud máte podsíť 192.168.1.0/24 místně, nemůžete přidat 192.168.1.0/24 místní sítě v hello síť Azure.
    - Toto je očekávané, protože Azure není známo, že nejsou žádné aktivní počítače v podsíti hello a zotavení po havárii. jenom se vytváří této podsíti hello.
    - nesmí být v konfliktu toobe možné toocorrectly směrovat síťový provoz z podsítě hello síť Azure v síti hello a místní sítí hello.

![Před převzetí služeb při selhání podsíť](./media/physical-walkthrough-network/network-design7.png)

#### <a name="before-failover"></a>Před převzetí služeb při selhání

1. Vytvoření další sítě (například síť obnovení). Toto je hello sítě ve který převzal virtuální počítače byly vytvořeny.
2. tooensure, který hello IP adresu pro počítač se uchovávají po převzetí služeb ve vlastnostech počítače hello > **konfigurace**, zadejte hello stejnou IP adresu, která hello server má místní a klikněte na tlačítko **Uložit**.
3. Při hello počítače při selhání, přiřadí Azure Site Recovery poskytuje IP adresu tooit hello.
4. Po převzetí služeb při selhání je aktivační událost se aktivuje a hello virtuální počítače jsou vytvořené v Azure s IP adresou hello vyžaduje, můžete připojit pomocí sítě toohello [připojení virtuální sítě tooVnet](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). Tato akce může provádět skriptování.
5. Trasy potřebovat odpovídajícím způsobem upravit, toobe tooreflect této 192.168.1.0/24 teď přesunul tooAzure.

    ![Po převzetí služeb při selhání podsíť](./media/physical-walkthrough-network/network-design9.png)

#### <a name="after-failover"></a>Po převzetí služeb při selhání

Pokud nemáte síť Azure, jak je popsáno výše, můžete vytvořit připojení site-to-site VPN mezi primární lokalitou a Azure, po převzetí služeb při selhání.

## <a name="change-ip-addresses"></a>Změna IP adresy

To [příspěvku na blogu](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) vysvětluje, jak tooset až hello Azure síťová infrastruktura, pokud nepotřebujete tooretain IP adres po převzetí služeb při selhání. Popis aplikace spustí, vypadá na jak tooset až sítě místně a v Azure a s informacemi o spuštění převzetí služeb při selhání se ukončí.  

## <a name="next-steps"></a>Další kroky

Přejděte příliš[krok 5: Příprava Azure](physical-walkthrough-prepare-azure.md)
