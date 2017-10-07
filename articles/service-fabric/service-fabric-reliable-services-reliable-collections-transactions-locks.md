---
title: "aaaTransactions a režim zámku Azure Service Fabric spolehlivé kolekce | Microsoft Docs"
description: "Azure Service Fabric spolehlivé stavu Manager a spolehlivé kolekce transakce a uzamyká."
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: masnider,rajak
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: mcoskun
ms.openlocfilehash: 340e029aa98f43ad6e46b48f687dad01f9d96f69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="transactions-and-lock-modes-in-azure-service-fabric-reliable-collections"></a>Transakce a uzamčení režimy v Azure Service Fabric spolehlivé kolekce

## <a name="transaction"></a>Transakce
Transakce je posloupnost operací provést jako jednu logickou jednotku práce.
Transakce musí mít květy hello následující ACID vlastnosti. (viz: https://technet.microsoft.com/en-us/library/ms190612)
* **Nedělitelnost**: transakce musí být atomické jednotky práce. Jinými slovy se provádí jeho změny dat, nebo žádná z nich probíhá.
* **Konzistence**: Po dokončení transakce musí zůstat všechna data v konzistentním stavu. Všechny interních datových strukturách musí být správné na konci hello hello transakce.
* **Izolace**: úpravy provedené souběžných transakcí musí být izolované od hello změny provedené při dalších souběžných transakcí. úroveň izolace Hello používá pro operace v rámci ITransaction je určen podle hello IReliableState provádění operace hello.
* **Odolnost**: Po dokončení transakce jeho účinky jsou trvale zavedené v systému hello. úpravy Hello zachovat ani v případě selhání systému hello.

### <a name="isolation-levels"></a>Úrovně izolace
Definuje úroveň hello úroveň izolace transakce hello toowhich musí být izolované od změny provedené při dalších transakcí.
Existují dvě úrovně izolace, které jsou podporovány v spolehlivé kolekce:

* **Opakovatelných čtení**: Určuje, že příkazy nelze číst data, která byla upravena, ale ještě nebyly potvrzeny podle dalších transakcí a že žádné další transakce můžete upravit data, která byla přečtena hello aktuální transakce až do aktuálního hello dokončení transakce. Další podrobnosti najdete v tématu [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).
* **Snímek**: Určuje, že data načtená žádné příkazem v transakci je hello stavu transakční konzistence verzi hello data, která byla při spuštění hello hello transakce.
  transakce Hello poznáte pouze změny dat, které nebyly potvrzeny před hello začátku transakce hello.
  Změny dat provedené dalších transakcí po hello začátek hello aktuální transakce nejsou viditelné toostatements provádění v aktuální transakci hello.
  Hello efekt působí, jako kdyby hello příkazy v transakci získat snímek hello potvrzené dat tak, jak byly hello začátku transakce hello.
  Snímky jsou konzistentní v rámci spolehlivé kolekce.
  Další podrobnosti najdete v tématu [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx).

Spolehlivé kolekce automaticky zvolte hello toouse úroveň izolace pro danou operaci čtení v závislosti na operaci hello a hello role repliky hello v době vytvoření transakce hello.
Následuje hello tabulku, která znázorňuje úrovni výchozí nastavení izolace pro operace spolehlivé slovník a fronty.

| Operace \ Role | Primární | Sekundární |
| --- |:--- |:--- |
| Čtení jedné Entity |Opakovatelných pro čtení |Snímek |
| Výčet, počet |Snímek |Snímek |

> [!NOTE]
> Běžných příkladů pro jednu entitu operace jsou `IReliableDictionary.TryGetValueAsync`, `IReliableQueue.TryPeekAsync`.
> 

Hello spolehlivé slovník i hello spolehlivé fronty podporují vaše zapíše pro čtení.
Jinými slovy, všechny zápisu v rámci transakce budou viditelné tooa následující číst, patří toohello stejné transakci.

## <a name="locks"></a>Zámky
V kolekcích spolehlivé, všechny transakce implementace přísných dvě fáze uzamčení: transakce není uvolnit zámky hello získala dokud neskončí hello transakce s přerušení nebo potvrzení.

Spolehlivé slovník používá uzamčení pro všechny operace jedné entity na úrovni řádků.
Spolehlivé fronty ztrátách souběžnosti pro striktní transakční FIFO vlastnost.
Spolehlivé fronty používá úrovně zámky operace povolení jednu transakci s `TryPeekAsync` nebo `TryDequeueAsync` a jednu transakci s `EnqueueAsync` najednou.
Všimněte si, že toopreserve FIFO, pokud `TryPeekAsync` nebo `TryDequeueAsync` někdy zjistí, že hello spolehlivé fronty je prázdný, budou také zamknout `EnqueueAsync`.

Zápisu operace vždy provést výhradní zámky.
Pro operace čtení uzamčení hello závisí na několika faktorech.
Všechny operace čtení provádí pomocí izolace snímku je zamykání.
Všechny operace čtení opakovatelných ve výchozím nastavení trvá sdílené zámky.
Pro všechny operace čtení, podporující opakovatelných pro čtení, ale hello uživatele můžete požádat o aktualizační zámek místo hello Sdílený zámek.
Aktualizační Zámek znamená že zámek asymetrické používaných tooprevent běžnou zablokování, který nastane, když více transakcí zamknutí prostředků pro potenciální aktualizace později.

Matice kompatibility zámku Hello lze nalézt v hello následující tabulka:

| Žádosti o \ udělena | Žádný | Shared | Aktualizace | Exkluzivní |
| --- |:--- |:--- |:--- |:--- |
| Shared |Ke konfliktu |Ke konfliktu |Konflikt |Konflikt |
| Aktualizace |Ke konfliktu |Ke konfliktu |Konflikt |Konflikt |
| Exkluzivní |Ke konfliktu |Konflikt |Konflikt |Konflikt |

Časový limit argument hello spolehlivé rozhraní API kolekce se používá pro zjišťování zablokování.
Například dvě transakce (T1 a T2) se snažíte tooread a aktualizovat K1.
Je možné, že je toodeadlock, protože obě skončili s hello Sdílený zámek.
Jedna nebo obě hello operací v tomto případě bude časový limit.

Tento scénář vzájemného zablokování je skvělým příkladem jak aktualizační zámek může zabránit zablokování.

## <a name="next-steps"></a>Další kroky
* [Práce s Reliable Collections](service-fabric-work-with-reliable-collections.md)
* [Spolehlivé služby oznámení](service-fabric-reliable-services-notifications.md)
* [Spolehlivé služby zálohování a obnovení (zotavení po havárii)](service-fabric-reliable-services-backup-restore.md)
* [Configuration Manager spolehlivé stavu](service-fabric-reliable-services-configuration.md)
* [Referenční informace pro vývojáře pro spolehlivé kolekce](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)

