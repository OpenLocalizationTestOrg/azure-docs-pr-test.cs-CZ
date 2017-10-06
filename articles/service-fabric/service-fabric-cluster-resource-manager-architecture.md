---
title: "aaaResource architektury správce | Microsoft Docs"
description: "Přehled architektury portálu Service Fabric clusteru Resource Manager."
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: 6c4421f9-834b-450c-939f-1cb4ff456b9b
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 9ea80273d0566a2ac25143ada3662875656b57b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="cluster-resource-manager-architecture-overview"></a>Přehled architektury správce prostředků clusteru
Hello správce prostředků clusteru Service Fabric je centrální služba, která běží v clusteru hello. Spravuje hello požadovaného stavu služby hello v clusteru hello, zejména s ohledem tooresource využívání a pravidla pro umístění. 

hello správce prostředků clusteru Service Fabric toomanage hello prostředky v clusteru, musí mít několik informací:

- Služby, které aktuálně neexistuje.
- Každá služba je aktuální (nebo výchozí) spotřeby prostředků 
- Hello zbývající kapacity na clusteru 
- kapacita Hello hello uzlů v clusteru hello 
- Hello množství prostředků na každém uzlu

časem změnit spotřeby prostředků Hello dané služby a služby obvykle zajímají více než jeden typ prostředku. Mezi různé služby může být skutečně fyzických i fyzické prostředky se měří. Služby mohou sledovat fyzické metriky jako spotřeba paměti a disku. Služby mohou běžně, zajímají logické metriky - věcmi, jako jsou "WorkQueueDepth" nebo "TotalRequests". Logické a fyzické metriky mohou být používány hello stejného clusteru. Metriky můžete sdílet mezi mnoha služeb nebo být konkrétní tooa konkrétní službu.

## <a name="other-considerations"></a>Další důležité informace
Hello vlastníků a operátory hello clusteru může lišit od hello autoři služby a aplikace, nebo na minimální jsou hello stejné používání různých klobouky osoby. Při vývoji aplikace víte, o co vyžaduje pár věcí. Máte odhad Dobrý den by měly být nasazeny jak různé služby a prostředky budou využívat. Například hello webová vrstva musí toorun na uzly, které jsou zveřejněné toohello Internetu, při hello databázové služby, by měly není. Další příklad hello webových služeb jsou pravděpodobně omezené procesoru a sítě, při hello datové vrstvy služby pozor Další informace o spotřebě paměti a disku. Zpracování za provozu lokality incident pro služby v produkčním prostředí, nebo který spravuje služby upgradu toohello osoba hello však má toodo různé úlohy a vyžaduje různých nástrojů. 

Jsou dynamické hello clusteru a služby:

- můžete zvýšit nebo snížit Hello počet uzlů v clusteru hello
- Může pocházet a přejděte uzly různých velikostí a typů
- Služby mohou být vytvořeny, odebrat a změnit jejich přidělení požadované zdroje a pravidla pro umístění
- Upgrady nebo jiné operace správy, můžete vrátit prostřednictvím hello clusteru aplikace hello na úrovních infrastruktury
- Selhání může dojít kdykoliv.

## <a name="cluster-resource-manager-components-and-data-flow"></a>Součásti Správce prostředků clusteru a toku dat
Hello správce prostředků clusteru má tootrack hello požadavky jednotlivých služby a hello spotřeby prostředků objektem každé služby v rámci těchto služeb. Hello správce prostředků clusteru má dvě části koncepční: agentů, kteří běží na každém uzlu a služby odolné proti chybám. Hello agenty na každý uzel sledování zatížení sestavy ze služeb, agregační je a pravidelně sestavy je. Hello služby Správce prostředků clusteru slučuje všechny informace hello od agentů místní hello a odpovědí podle aktuální konfigurace.

Podívejme se na následující diagram hello:

<center>
![Architektura vyrovnávání prostředků][Image1]
</center>

Během modul runtime existuje mnoho změn, které by mohly nastat. Předpokládejme například, že hello množství prostředků některé služby využívat změny, některé služby nezdaří, a některé uzly připojení a nechte hello cluster. Všechny změny hello na uzlu se agregovat a pravidelně odesílají služby Správce prostředků clusteru sítě toohello (1,2), kde jsou agregovat znovu, analyzovat a uložené. Každých několik sekund, které služba zjistí hello změny a určuje, zda se všechny akce nezbytné (3). Například může Všimněte si, že některé prázdný uzly byly přidány toohello clusteru. V důsledku toho se rozhodne toomove některé uzly toothose služby. Hello správce prostředků clusteru může také Všimněte si, že je přetížena konkrétním uzlu, nebo že máte určité služby se nezdařila nebo byla odstraněna, uvolnění prostředků jinde.

Pojďme podívejte se na následující diagram hello a zjistěte, co se stane dále. Pojďme vyslovte této hello clusteru Resource Manager určuje, je nutné provést změny. Ho koordinuje pomocí jiné služby (v konkrétní hello Správce převzetí služeb při selhání) toomake hello nezbytné změny systému. Hello potřebné příkazy se pak odešlou toohello odpovídající uzly (4). Předpokládejme například že hello Resource Manager si všimli, že počítač Uzel5 byl přetížený a proto se rozhodli, službu toomove B tooNode4 počítač Uzel5. Na konci hello tohoto Rekonfigurace hello (5) hello cluster vypadat třeba takto:

<center>
![Architektura vyrovnávání prostředků][Image2]
</center>

## <a name="next-steps"></a>Další kroky
- Hello správce prostředků clusteru má mnoho možností pro popisující hello clusteru. toofind Další informace o jejich, projděte si tento článek na [popisující cluster Service Fabric](./service-fabric-cluster-resource-manager-cluster-description.md)
- primární povinností Hello clusteru správce prostředků jsou vyrovnává hello clusteru a vynucovat pravidla pro umístění. Další informace o konfiguraci těchto chování najdete v tématu [vyrovnávání cluster Service Fabric](./service-fabric-cluster-resource-manager-balancing.md)

[Image1]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-1.png
[Image2]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-2.png
