---
title: "aaaHow tooconfigure geografická replikace pro Azure Redis Cache | Microsoft Docs"
description: "Zjistěte, jak tooreplicate Azure Redis Cache instancí v zeměpisných oblastech."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 375643dc-dbac-4bab-8004-d9ae9570440d
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: sdanie
ms.openlocfilehash: edcd6f202b51055d1a4e47ecaf11f9977d50aa81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-geo-replication-for-azure-redis-cache"></a>Jak tooconfigure geografická replikace pro Azure Redis Cache

Geografická replikace poskytuje mechanismus pro propojení dvě instance vrstvy Azure Redis Cache Premium. Jeden mezipaměti je určený jako primární propojené mezipaměti hello a hello jiných jako sekundární propojené mezipaměti hello. Hello sekundární propojené mezipaměti se stane, jen pro čtení a data napsané toohello primární mezipaměť je replikují toohello sekundární propojené mezipaměti. Tuto funkci lze použít tooreplicate mezipaměti nad oblastmi Azure. Tento článek obsahuje Průvodce tooconfiguring geografická replikace pro vaše instance služby Azure Redis Cache Premium vrstvy.

## <a name="geo-replication-prerequisites"></a>Geografická replikace požadavky

musí být splněné tooconfigure geografická replikace mezi dvěma mezipamětí, hello následující požadavky:

- Musí být obě mezipamětí [úroveň Premium](cache-premium-tier-intro.md) ukládá do mezipaměti.
- Obě mezipaměti musí být v hello stejného předplatného Azure.
- Hello sekundární propojené mezipaměti musí být buď hello stejné cenová úroveň nebo větší cenovou úroveň než primární propojené mezipaměť hello.
- Pokud primární propojené mezipaměti hello povoleným clusteringem, clustering povolit hello hello sekundární propojené mezipaměti musí mít stejný počet horizontálních oddílů jako primární propojené mezipaměti hello.
- Obě mezipaměti musí být vytvořený a ve spuštěném stavu.
- U buď mezipaměti nesmí být povolena trvalost.
- Geografická replikace mezi mezipaměti v hello stejnou virtuální síť je podporována. Geografická replikace mezi mezipaměti v různých virtuálních sítí se také podporuje, hello dvě virtuální sítě jsou nakonfigurovány tak, aby byly prostředky ve virtuální sítě hello možné tooreach navzájem prostřednictvím připojení TCP.

Po dokončení konfigurace geografická replikace hello platí následující omezení pár tooyour propojené mezipaměti:

- Hello sekundární propojené mezipaměť je jen pro čtení; si můžete přečíst z něj však nelze zapsat všechny tooit data. 
- Odeberou se všechna data, která byla v hello sekundární propojené mezipaměti před přidáním odkazu hello. Pokud hello geografická replikace je následně odstraněny ale, hello replikují data zůstanou v mezipaměti pro sekundární propojené hello.
- Nelze inicializovat, [škálování operaci](cache-how-to-scale.md) na buď mezipaměti nebo [změnit hello počet horizontálních oddílů](cache-how-to-premium-clustering.md) Pokud mezipaměti hello povoleným clusteringem.
- Nelze zapnout stálost na buď mezipaměti.
- Můžete použít [exportovat](cache-how-to-import-export-data.md#export) s buď mezipaměti, ale můžete pouze [Import](cache-how-to-import-export-data.md#import) do hello primární propojené mezipaměti.
- Nelze odstranit propojené mezipaměti nebo hello skupinu prostředků, který obsahuje, dokud neodeberete hello geografická replikace propojení. Další informace najdete v tématu [proč hello operace selhat, když jsem se toodelete Moje propojené mezipaměti?](#why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache)
- Pokud dva mezipamětí hello jsou v různých oblastech, bude použita náklady na celkový výstup sítě toohello data se replikují přes oblasti toohello sekundární propojené mezipaměti. Další informace najdete v tématu [kolik nemá ho náklady tooreplicate svá data v oblastech Azure?](#how-much-does-it-cost-to-replicate-my-data-across-azure-regions)
- Neexistuje propojené mezipaměť automatické převzetí služeb při selhání toohello sekundární, pokud primární mezipaměti hello (a její replika) přejděte. V pořadí toofailover klientské aplikace potřebovali byste toomanually odebrat hello geografická replikace odkaz a bod hello klienta aplikace toohello mezipaměti, který byl dříve hello sekundární propojené mezipaměti. Další informace najdete v tématu [jak selhání přes toohello sekundární propojené mezipaměti funguje?](#how-does-failing-over-to-the-secondary-linked-cache-work)

## <a name="add-a-geo-replication-link"></a>Přidat odkaz geografická replikace

1. toolink dva premium ukládá do mezipaměti, společně za geografickou replikaci, klikněte na tlačítko **geografická replikace** z nabídky prostředků hello hello mezipaměti určený jako primární hello propojené mezipaměti a pak klikněte na tlačítko **odkaz replikace mezipaměti přidat**z hello **geografická replikace** okno.

    ![Přidání odkazu](./media/cache-how-to-geo-replication/cache-geo-location-menu.png)

2. Klikněte na název hello hello potřeby sekundární mezipaměti z hello **kompatibilní mezipamětí** seznamu. Pokud v seznamu hello nezobrazí požadované mezipaměti, ověřte, že hello [geografická replikace požadavky](#geo-replication-prerequisites) pro potřeby hello splnění sekundární mezipaměti. toofilter hello mezipamětí podle oblasti, klikněte na požadovanou oblast hello v toodisplay mapy hello pouze ty ukládá do mezipaměti v hello **kompatibilní mezipamětí** seznamu.

    ![Geografická replikace kompatibilní mezipaměti](./media/cache-how-to-geo-replication/cache-geo-location-select-link.png)
    
    Lze také zahájit hello propojení procesu nebo zobrazení podrobností o sekundární mezipaměti hello pomocí hello kontextové nabídky.

    ![Geografická replikace kontextové nabídky](./media/cache-how-to-geo-replication/cache-geo-location-select-link-context-menu.png)

3. Klikněte na tlačítko **odkaz** toolink hello dvě mezipamětí společně a spusťte proces replikace hello.

    ![Ukládá do mezipaměti odkaz](./media/cache-how-to-geo-replication/cache-geo-location-confirm-link.png)

4. Hello průběh procesu hello replikace můžete zobrazit na hello **geografická replikace** okno.

    ![Stav propojení](./media/cache-how-to-geo-replication/cache-geo-location-linking.png)

    Můžete také zobrazit hello propojení stavu na hello **přehled** okno pro obě primární a sekundární mezipamětí hello.

    ![Stav mezipaměti](./media/cache-how-to-geo-replication/cache-geo-location-link-status.png)

    Po dokončení procesu replikace hello hello **propojit stav** změní příliš**úspěšné**.

    ![Stav mezipaměti](./media/cache-how-to-geo-replication/cache-geo-location-link-successful.png)

    Během hello proces propojení zůstane hello primární propojené mezipaměti k dispozici pro použití ale hello sekundární propojené mezipaměti není k dispozici, dokud se nedokončí hello proces propojení.

## <a name="remove-a-geo-replication-link"></a>Odebrat odkaz geografická replikace

1. Klikněte na tlačítko tooremove hello propojení mezi dvěma mezipaměti a ukončení geografická replikace, **zrušit propojení mezipamětí** z hello **geografická replikace** okno.
    
    ![Zrušit propojení mezipaměti](./media/cache-how-to-geo-replication/cache-geo-location-unlink.png)

    Po dokončení procesu zrušení propojení hello hello sekundární mezipaměti je k dispozici pro obě čte a zapisuje.

>[!NOTE]
>Pokud je odebrán odkaz hello geografická replikace, hello replikovaných dat z hello primární propojené mezipaměti zůstanou v mezipaměti sekundární hello.
>
>

## <a name="geo-replication-faq"></a>Geografická replikace – nejčastější dotazy

- [Můžete použít geografická replikace s mezipamětí úroveň Standard nebo Basic?](#can-i-use-geo-replication-with-a-standard-or-basic-tier-cache)
- [Je k dispozici pro použití při propojování hello nebo zrušení propojení proces Moje mezipaměti?](#is-my-cache-available-for-use-during-the-linking-or-unlinking-process)
- [Můžete propojit více než dva mezipamětí společně?](#can-i-link-more-than-two-caches-together)
- [Můžete propojit dvě mezipaměti z různých předplatných Azure?](#can-i-link-two-caches-from-different-azure-subscriptions)
- [Můžete propojit dvě mezipaměti s jinou velikostí?](#can-i-link-two-caches-with-different-sizes)
- [Můžete použít geografická replikace s povoleným clusteringem?](#can-i-use-geo-replication-with-clustering-enabled)
- [Můžete použít geografická replikace s Moje mezipamětí ve virtuální síti?](#can-i-use-geo-replication-with-my-caches-in-a-vnet)
- [Můžete použít PowerShell nebo rozhraní příkazového řádku Azure toomanage geografická replikace?](#can-i-use-powershell-or-azure-cli-to-manage-geo-replication)
- [Kolik stojí tooreplicate svá data v oblastech Azure?](#how-much-does-it-cost-to-replicate-my-data-across-azure-regions)
- [Proč hello operace selhat, když jsem se toodelete Moje propojené mezipaměti?](#why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache)
- [Jaké oblasti je vhodné použít pro moje sekundární propojené mezipaměti?](#what-region-should-i-use-for-my-secondary-linked-cache)
- [Jak funguje selhání přes toohello sekundární propojené mezipaměti?](#how-does-failing-over-to-the-secondary-linked-cache-work)

### <a name="can-i-use-geo-replication-with-a-standard-or-basic-tier-cache"></a>Můžete použít geografická replikace s mezipamětí úroveň Standard nebo Basic?

Ne, geografická replikace je k dispozici pouze pro prémiových mezipamětí vrstvy.

### <a name="is-my-cache-available-for-use-during-hello-linking-or-unlinking-process"></a>Je k dispozici pro použití při propojování hello nebo zrušení propojení proces Moje mezipaměti?

- Při propojení dvou mezipamětí společně za geografickou replikaci, zůstane hello primární propojené mezipaměti k dispozici pro použití ale hello sekundární propojené mezipaměti není k dispozici, dokud se nedokončí hello proces propojení.
- Při odebírání hello geografická replikace propojení mezi dvěma mezipamětí, zůstávají dostupné k použití obou mezipamětí.

### <a name="can-i-link-more-than-two-caches-together"></a>Můžete propojit více než dva mezipamětí společně?

Ne, při použití geografická replikace můžete propojit pouze dvě mezipamětí společně.

### <a name="can-i-link-two-caches-from-different-azure-subscriptions"></a>Můžete propojit dvě mezipaměti z různých předplatných Azure?

Ne, obě mezipaměti musí být v hello stejného předplatného Azure.

### <a name="can-i-link-two-caches-with-different-sizes"></a>Můžete propojit dvě mezipaměti s jinou velikostí?

Ano, tak dlouho, dokud sekundární propojené mezipaměti hello je větší než hello primární propojené mezipaměti.

### <a name="can-i-use-geo-replication-with-clustering-enabled"></a>Můžete použít geografická replikace s povoleným clusteringem?

Ano, tak dlouho, dokud obě mezipaměti mají hello stejný počet horizontálních oddílů.

### <a name="can-i-use-geo-replication-with-my-caches-in-a-vnet"></a>Můžete použít geografická replikace s Moje mezipamětí ve virtuální síti?

Ano, jsou podporovány geografická replikace mezipamětí ve virtuálních sítí. 

- Geografická replikace mezi mezipaměti v hello stejnou virtuální síť je podporována.
- Geografická replikace mezi mezipaměti v různých virtuálních sítí se také podporuje, hello dvě virtuální sítě jsou nakonfigurovány tak, aby byly prostředky ve virtuální sítě hello možné tooreach navzájem prostřednictvím připojení TCP.

### <a name="can-i-use-powershell-or-azure-cli-toomanage-geo-replication"></a>Můžete použít PowerShell nebo rozhraní příkazového řádku Azure toomanage geografická replikace?

V tuto chvíli, které můžete spravovat pouze hello geografická replikace pomocí portálu Azure.

### <a name="how-much-does-it-cost-tooreplicate-my-data-across-azure-regions"></a>Kolik stojí tooreplicate svá data v oblastech Azure?

Při použití geografická replikace, data z primární propojené mezipaměti hello je sekundární replikované toohello propojené mezipaměti. Pokud hello dvě propojené mezipamětí jsou v hello stejné oblasti Azure, je bezplatná pro přenos dat hello. Pokud hello dvě propojené mezipamětí jsou v různých oblastech Azure, hello poplatků přenos dat geografické replikace je náklady na šířku pásma hello replikace tohoto data toohello jiné oblasti Azure. Další informace najdete v tématu [podrobnosti o cenách šířky pásma](https://azure.microsoft.com/pricing/details/bandwidth/).

### <a name="why-did-hello-operation-fail-when-i-tried-toodelete-my-linked-cache"></a>Proč hello operace selhat, když jsem se toodelete Moje propojené mezipaměti?

Při dvou mezipamětí jsou spojeny dohromady, nelze odstranit mezipaměti nebo hello skupinu prostředků, který je obsahuje, dokud neodeberete hello geografická replikace propojení. Pokud se pokusíte skupiny prostředků hello toodelete, který obsahuje jedno nebo obě hello propojené mezipamětí, hello další prostředky ve skupině prostředků hello se odstraní, ale skupinu prostředků hello zůstává v hello `deleting` stavu a všechny propojené mezipamětí ve skupině prostředků hello zůstat v hello `running` stavu. toocomplete hello odstranění skupiny prostředků hello a hello propojené mezipaměti v něm zalomení hello geografická replikace propojení, jak je popsáno v [odebrat odkaz geografická replikace](#remove-a-geo-replication-link).

### <a name="what-region-should-i-use-for-my-secondary-linked-cache"></a>Jaké oblasti je vhodné použít pro moje sekundární propojené mezipaměti?

Obecně se doporučuje pro vaší mezipaměti tooexist v hello stejné oblasti Azure jako hello aplikace, který přistupuje k ho. Pokud vaše aplikace má primární a záložní oblast, musí existovat své mezipaměti primární a sekundární v těchto oblastech stejné. Další informace o spárované oblastech najdete v tématu [osvědčené postupy – Azure spárované oblasti](../best-practices-availability-paired-regions.md).

### <a name="how-does-failing-over-toohello-secondary-linked-cache-work"></a>Jak funguje selhání přes toohello sekundární propojené mezipaměti?

V hello počáteční verze geografická replikace Azure Redis Cache automatické převzetí služeb při selhání v oblastech Azure nepodporuje. Geografická replikace se používá hlavně ve scénáři zotavení po havárii. Ve scénáři zotavení distater zákazníci měli zprovoznit hello celý zásobník aplikací v oblasti zálohování v koordinované spíše než umožníte jednotlivé součásti aplikace při rozhodování, kdy tooswitch tootheir zálohy na své vlastní. To je obzvláště důležité tooRedis. Jedním z klíčových výhod Redis hello je, že se jedná o velmi nízkou latencí úložiště. Pokud Redis používá aplikace převezme tooa jiné oblasti Azure, ale hello výpočetní vrstvě nemá, hello přidat zaokrouhlí času by mít znatelný dopad na výkon. Z tohoto důvodu rádi bychom znali selhání Redis tooavoid přes automaticky z důvodu problémů s dostupností tootransient.

V současné době tooinitiate hello převzetí služeb při selhání, je nutné tooremove hello geografická replikace na odkaz v hello portál Azure a poté změňte koncový bod připojení hello v klientovi Redis hello z hello primární propojené mezipaměti toohello (dříve propojené) sekundární mezipaměti. Když hello, které nejsou přidruženy k dvě mezipamětí, hello replika se stane běžný pro čtení a zápis mezipaměti znovu a přijímá požadavky přímo z klientů Redis.


## <a name="next-steps"></a>Další kroky

Další informace o hello [Azure Redis Cache na úrovni Premium](cache-premium-tier-intro.md).

