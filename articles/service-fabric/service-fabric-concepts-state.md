---
title: "aaaDefinine a spravovat stav v Azure mikroslužeb | Microsoft Docs"
description: "Jak toodefine a spravovat stav služby v Service Fabric"
services: service-fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: 
ms.assetid: f5e618a5-3ea3-4404-94af-122278f91652
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 4a24696da71753d0f343a86df3556b5b7c964834
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-state"></a>Stav služby
**Služba stavu** odkazuje toohello v paměti nebo na diskových dat, že služba vyžaduje toofunction. Obsahuje, například hello datové struktury a členské proměnné služby hello čte a zapisuje toodo práci. V závislosti na tom, jak je navržen hello služby se mohly by také zahrnovat souborům nebo jiným prostředkům, které jsou uložené na disku. Například hello soubory databáze využije toostore dat a protokoly transakcí.

Jako příklad služba zvažte kalkulačky. Základní kalkulačky služby trvá dvou čísel a vrátí jejich součet. Provádění tohoto výpočtu zahrnuje žádné proměnné členů nebo Další informace.

Nyní zvažte, zda text hello stejné kalkulačky, ale s další metodu pro ukládání a vrací poslední součet hello má počítaný. Tato služba je nyní stateful. Stateful znamená, že obsahuje některé stavu, který zapíše toowhen se vypočítá nové sum a čte z při požádejte tooreturn hello poslední vypočítaný součet.

V Azure Service Fabric nazývá první službě hello bezstavové služby. druhý služba Hello se nazývá stavové služby.

## <a name="storing-service-state"></a>Ukládání stavu služby
Stav můžete externalized nebo umístěn společně s hello kód, který je manipulace s hello stavu. Externalization stavu se obvykle provádí pomocí externí databáze nebo jiné datové úložiště, že hello běží na různé počítače přes síť hello nebo mimo proces na stejném počítači. V našem příkladu kalkulačky úložiště dat hello může být SQL database nebo instanci Azure tabulka úložiště. Každý požadavek toocompute hello součet provede aktualizace na tato data a požadavky hodnota hello toohello službou tooreturn mají za následek hello aktuální hodnota je načtena z úložiště hello. 

Stav může být také umístěné společně se službou hello kód, který zpracovává hello stavu. Stavové služby v Service Fabric se obvykle vytvořená s využitím tohoto modelu. Service Fabric nabízí hello infrastruktury tooensure tento stav je vysoce dostupný, konzistentní a odolné, a že služby hello integrované tímto způsobem můžete snadno škálovat.

## <a name="next-steps"></a>Další kroky
Další informace o konceptech Service Fabric najdete v tématu hello následující články:

* [Dostupnost služeb Service Fabric](service-fabric-availability-services.md)
* [Škálovatelnost služby Service Fabric](service-fabric-concepts-scalability.md)
* [Vytváření oddílů služby Service Fabric](service-fabric-concepts-partitioning.md)
* [Spolehlivé služby Service Fabric](service-fabric-reliable-services-introduction.md)
