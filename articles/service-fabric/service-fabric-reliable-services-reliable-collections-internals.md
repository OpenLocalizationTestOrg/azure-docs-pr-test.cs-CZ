---
title: "Správce spolehlivé stavu Service Fabric a spolehlivé kolekce internals aaaAzure | Microsoft Docs"
description: "Podrobné informace o konceptech spolehlivé kolekce a návrhu v Azure Service Fabric."
services: service-fabric
documentationcenter: .net
author: mcoskun
manager: timlt
editor: rajak
ms.assetid: 62857523-604b-434e-bd1c-2141ea4b00d1
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 5/1/2017
ms.author: mcoskun
ms.openlocfilehash: 651bfb52785a2475e4840cd471e87220d1936391
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-reliable-state-manager-and-reliable-collection-internals"></a>Interní informace o stavu spolehlivé infrastruktury Azure Service Manager a spolehlivé kolekce
Tento dokument obsahuje podrobné uvnitř spolehlivé správce stavu a spolehlivé kolekce toosee fungování komponenty jádra pozadí hello.

> [!NOTE]
> Tento dokument je práce v průběhu. Přidejte komentář toothis článek tootell nám jaké tématu chcete více informací o toolearn.
>

##  <a name="local-persistence-model-log-and-checkpoint"></a>Model místní trvalost: kontrolního bodu a protokolu
Hello spolehlivé správce stavu a spolehlivé kolekce podle trvalost model, který se nazývá kontrolního bodu a protokolu.
V tomto modelu je každé změně stavu nejprve zaznamenána na disku a potom se použijí v paměti.
sám dokončení stát Hello je nastavené jako trvalé jen občas (také známa jako Kontrolní bod).
Hello výhodou je, že rozdíly jsou převedena na sekvenční připojovacího zápisy na disk pro zlepšení výkonu.

toobetter pochopit hello protokolu a modelu kontrolního bodu, nejdříve Podíváme se na scénář nekonečné disku hello.
Hello spolehlivé správce stavu zaznamená každé operace předtím, než se replikují.
Protokolování umožňuje hello spolehlivé kolekce tooapply hello operace jenom v paměti.
Vzhledem k tomu, že protokoly jsou nastavené jako trvalé, i v případě, že repliky hello selže a je třeba toobe restartování hello spolehlivé správce stavu má dostatek informací v jeho tooreplay protokolu všechny hello operace, které hello repliky ztratil.
Jako hello disk je nekonečno, záznamy protokolu nikdy nepotřebují toobe odebrat a hello spolehlivé kolekce musí toomanage pouze hello v paměti stavu.

Nyní Podíváme se na scénář konečné disku hello.
Jako záznamy protokolu hromadit, hello spolehlivé správce stavu spustí nedostatek místa na disku.
Předtím, než k tomu dojde, hello spolehlivé správce stavu musí tootruncate jeho toomake místo protokolu pro hello novější záznamy.
Požadavky správce stavu spolehlivé hello spolehlivé kolekce toocheckpoint toodisk jejich stav v paměti.
V tomto okamžiku by hello spolehlivé kolekce zachována jeho stav v paměti.
Jakmile spolehlivé kolekce hello dokončit jejich kontrolní body, hello spolehlivé správce stavu můžete zkrátit hello protokolu toofree místa na disku.
Když toobe restartovat, je potřeba hello repliky, spolehlivé kolekce obnovit jejich kontrolní bod stavu a hello spolehlivé správce stavu obnoví hraje zpět všechny změny stavu hello, kterým došlo od posledního kontrolního bodu hello.

Jiné hodnoty add vytváření kontrolních bodů je, že vylepšuje časy obnovení v běžné scénáře. Protokol obsahuje všechny operace, které došlo od posledního kontrolního bodu hello.
Proto může zahrnovat víc verzí položky jako více hodnot pro daný řádek v spolehlivé slovníku.
Naproti tomu kontrolní body spolehlivé kolekce hello pouze nejnovější verze jednotlivých hodnot pro klíč.

## <a name="next-steps"></a>Další kroky
* [Transakce a zámky.](service-fabric-reliable-services-reliable-collections-transactions-locks.md)

