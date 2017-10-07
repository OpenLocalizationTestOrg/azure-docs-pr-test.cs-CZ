---
title: 'Upgrade aplikace: serializace dat | Microsoft Docs'
description: "Osvědčené postupy pro serializaci dat a jak ovlivňuje postupné upgrady aplikací."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: a5f36366-a2ab-4ae3-bb08-bc2f9533bc5a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 1b65dfd3813423550631490640a81953864f58e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-data-serialization-affects-an-application-upgrade"></a>Jak ovlivňuje serializace dat upgradu aplikace
V [vrácení upgradu aplikace](service-fabric-application-upgrade.md), hello upgrade je použité tooa dílčí sadu uzlů, jednu upgradovací doménu najednou. Během tohoto procesu jsou některé domény upgrade na novější verzi aplikace hello a jsou některé upgradu domény na starší verzi aplikace hello. Při zavedení hello hello novou verzi vaší aplikace musí být schopný tooread hello starší verzi vaše data a hello starší verzi aplikace musí být schopný tooread hello novou verzi vaše data. Pokud formát dat hello není vpřed a zpětně kompatibilní, upgrade hello může selže nebo horší, data mohou být ke ztrátě nebo poškozený. Tento článek popisuje, co se považuje za vaše formát dat a nabízí osvědčené postupy pro zajištění, aby vaše data byla vpřed a zpětně kompatibilní.

## <a name="what-makes-up-your-data-format"></a>Co tvoří vaše formát dat?
V Azure Service Fabric hello data, která je trvale uložila a replikovala pochází z třídy jazyka C#. Pro aplikace, které používají [spolehlivé kolekce](service-fabric-reliable-services-reliable-collections.md), že data jsou hello objekty ve slovnících spolehlivé hello a fronty. Pro aplikace, které používají [Reliable Actors](service-fabric-reliable-actors-introduction.md), který je hello zálohování stavu objektu actor hello. Tyto třídy jazyka C# musí být serializovatelný toobe trvale uložila a replikovala. Proto hello formát dat je definován hello polí a vlastností, které jsou serializovány, a také o tom, že serializovat. Například v `IReliableDictionary<int, MyClass>` hello dat je serializovaný seznam `int` a serializovaný seznam `MyClass`.

### <a name="code-changes-that-result-in-a-data-format-change"></a>Kód změny, které způsobily změnu formátu dat
Vzhledem k tomu, že formát dat hello je určen podle třídy jazyka C#, může způsobit změny toohello třídy změně formátu data. Musí dát pozor tooensure, který může zpracovat postupného upgradu hello data formátu změnu. Příklady, které by mohly způsobit změny formátu dat:

* Přidání nebo odebrání vlastnosti nebo pole
* Přejmenování polí a vlastností
* Změna hello typy polí a vlastností
* Změna názvu třídy hello nebo obor názvů

### <a name="data-contract-as-hello-default-serializer"></a>Kontrakt dat jako výchozí serializátor hello
Serializátor Hello je obecně odpovědná za čtení dat hello a deserializaci do aktuální verze hello, i když hello data jsou ve starší nebo *novější* verze. Hello výchozí serializátor je hello [serializátor kontraktu dat](https://msdn.microsoft.com/library/ms733127.aspx), který má pravidla dobře definovaný verzí. Spolehlivé kolekce povolit toobe serializátor hello přepsat, ale Reliable Actors aktuálně nepodporují. Serializátor dat Hello hraje důležitou roli při povolování postupné upgrady. Serializátor kontraktu dat Hello je hello serializátoru, který doporučujeme pro aplikace Service Fabric.

## <a name="how-hello-data-format-affects-a-rolling-upgrade"></a>Jak ovlivňuje formát dat hello postupného upgradu
Během postupného upgradu, jsou dva základní scénáře, kde serializátor hello narazit na starší nebo *novější* verzi dat:

1. Po uzlu upgradu a spuštění zálohování, načte nové serializátor hello hello data, která byla trvalou toodisk hello staré verze.
2. Během hello vrácení upgradu hello cluster bude obsahovat kombinaci hello staré a nové verze vašeho kódu. Vzhledem k tomu, že repliky může umístěny v různých doménách upgradu a repliky odesílat data tooeach jiných, hello staré nebo nové verze vaše data mohou být zjištěny hello staré nebo nové verze vaší serializátor.

> [!NOTE]
> Hello "nové verze" a "staré verze" Zde naleznete toohello verzi váš kód, který běží. Hello "nové serializátor" odkazuje toohello serializátor kód, který spouští v nové verzi hello vaší aplikace. Hello "nová data" odkazuje toohello serializovat C# – třída z hello novou verzi vaší aplikace.
> 
> 

Hello dvě verze kódu a musí být ve formátu data vpřed a zpětně kompatibilní. Pokud nejsou kompatibilní, hello vrácení upgradu může selhat nebo data mohou být ztracena. vrácení upgradu Hello může selhat, protože hello kód nebo serializátor může výjimku výjimky nebo chybu při zjistí hello jinou verzi. Data mohou být ztracena, pokud například byla přidána nová vlastnost ale staré serializátor hello zahodí se během deserializace.

Kontrakt dat je hello doporučené řešení pro zajištění, aby vaše data byla kompatibilní. Obsahuje pravidla dobře definovaný verzí pro přidání, odebrání a změna pole. Je také podpora zabývají neznámé pole, zapojování do procesu hello serializace a deserializace a plánování práce s dědičnosti tříd. Další informace najdete v tématu [pomocí kontrakt dat](https://msdn.microsoft.com/library/ms733127.aspx).

## <a name="next-steps"></a>Další kroky
[Upgrade vaší aplikace pomocí sady Visual Studio](service-fabric-application-upgrade-tutorial.md) vás provede upgrade aplikace pomocí sady Visual Studio.

[Upgrade vaší aplikace pomocí prostředí Powershell](service-fabric-application-upgrade-tutorial-powershell.md) vás provede upgrade aplikace pomocí prostředí PowerShell.

Řídí, jak vaše aplikace upgraduje pomocí [Upgrade parametry](service-fabric-application-upgrade-parameters.md).

Zjistěte, jak toouse pokročilé funkce při upgradu vaší aplikace tím, že odkazuje příliš[Pokročilá témata](service-fabric-application-upgrade-advanced.md).

Řešení běžných potíží v upgradů aplikací tím, že odkazuje toohello kroky v [řešení potíží s aplikací upgrady ](service-fabric-application-upgrade-troubleshooting.md).

