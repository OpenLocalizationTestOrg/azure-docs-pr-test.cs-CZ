---
title: "aaaOverview životního cyklu na základě objektu actor Azure mikroslužeb | Microsoft Docs"
description: "Popisuje spolehlivé objektu Actor prostředků infrastruktury služby životního cyklu, uvolňování paměti a ručně odstranit aktéři a jejich stavu"
services: service-fabric
documentationcenter: .net
author: amanbha
manager: timlt
editor: vturecek
ms.assetid: b91384cc-804c-49d6-a6cb-f3f3d7d65a8e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/13/2017
ms.author: amanbha
ms.openlocfilehash: a7926e372449048f0a579c2c58573754a4a82363
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="actor-lifecycle-automatic-garbage-collection-and-manual-delete"></a>Životní cyklus objektu actor, automatické uvolňování paměti a ruční delete
Aktivaci objektu actor hello poprvé Přišla žádost o tooany její metody. Objekt actor je deaktivované (paměti shromažďují hello aktéři runtime), pokud se nepoužívá pro nastaveném časovém intervalu. Objekt actor a její stav lze také odstranit ručně kdykoli.

## <a name="actor-activation"></a>Aktivace objektu actor
Pokud je objekt actor aktivován, dojde k následujícímu hello:

* Při volání je teď dostupná pro objekt actor a už není aktivní, se vytvoří nový objekt actor.
* Pokud se udržuje stav načtení stavu objektu actor Hello.
* Hello `OnActivateAsync` (C#) nebo `onActivateAsync` volání metody (Java), (které mohou být přepsána nastaveními v implementace objektu actor hello).
* objektu actor Hello je nyní považována za aktivní.

## <a name="actor-deactivation"></a>Deaktivace objektu actor
Při deaktivaci objektu actor, dojde k následujícímu hello:

* Pokud objekt actor nepoužívá pro nějaké časové období, odebere se z tabulky Active aktéři hello.
* Hello `OnDeactivateAsync` (C#) nebo `onDeactivateAsync` volání metody (Java), (které mohou být přepsána nastaveními v implementace objektu actor hello). Zruší všechny hello časovače pro objektu actor hello. Operace objektu actor jako stavu, ve kterém by neměl být volán změny z této metody.

> [!TIP]
> Hello Fabric aktéři runtime vysílá některé [událostí souvisejících tooactor aktivace a deaktivace](service-fabric-reliable-actors-diagnostics.md#list-of-events-and-performance-counters). Jsou užitečné v Diagnostika a sledování výkonu.
>
>

### <a name="actor-garbage-collection"></a>Uvolňování paměti objektu actor
Po deaktivaci objektu actor vydávají objektu actor toohello odkazy a může být shromažďují normálně modul hello common language runtime (CLR) nebo java virtual machine (JVM) systém uvolňování paměti. Uvolňování paměti pouze vyčistí objektu actor hello; provede **není** odebrat stavu uložené v objektu actor hello správce stavu. Hello další čas hello objektu actor se aktivuje, se vytvoří nový objekt actor a obnovit jeho stav.

Co se počítá jako "se používá" hello za účelem deaktivace a systém kolekce paměti?

* Přijímá volání
* `IRemindable.ReceiveReminderAsync`metody volané (platí jenom v případě objektu actor hello používá připomenutí)

> [!NOTE]
> Pokud objektu actor hello používá časovače a jeho zpětné volání časovače je volána, nemá **není** , se počítají jako "se používá".
>
>

Předtím, než jsme přejít do hello podrobnosti o deaktivaci, je důležité toodefine hello následující podmínky:

* *Interval sledování*. Toto je hello interval, ve které hello aktéři runtime hledá její tabulkou Active aktéři aktéři, které můžete deaktivovat a uklizeny. Hello výchozí hodnota je 1 minuta.
* *Časový limit nečinnosti*. Toto je hello množství času, že objekt actor potřebuje tooremain nepoužívané (nečinný), než je možné deaktivovat a uklizeny. Hello výchozí hodnota je 60 minut.

Obvykle není nutné toochange tyto výchozí hodnoty. Ale v případě potřeby tyto intervaly lze změnit prostřednictvím `ActorServiceSettings` při registraci vaší [služby objektu Actor](service-fabric-reliable-actors-platform.md):

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        ActorRuntime.RegisterActorAsync<MyActor>((context, actorType) =>
                new ActorService(context, actorType,
                    settings:
                        new ActorServiceSettings()
                        {
                            ActorGarbageCollectionSettings =
                                new ActorGarbageCollectionSettings(10, 2)
                        }))
            .GetAwaiter()
            .GetResult();
    }
}
```

```Java
public class Program
{
    public static void main(String[] args)
    {
        ActorRuntime.registerActorAsync(
                MyActor.class,
                (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
                timeout);
    }
}
```
Pro každou aktivní objektu actor uchovává informace hello objektu actor runtime o hello množství času, která byla nečinnosti (tzn. ne používá). modul runtime objektu actor Hello kontroluje každý hello aktéři každých `ScanIntervalInSeconds` toosee, pokud lze paměti shromážděných a shromažďuje ji, pokud to bylo nečinné `IdleTimeoutInSeconds`.

Kdykoliv se používá objektu actor, je jeho doba nečinnosti, po resetování too0. Potom může být objektu actor hello uvolnění z paměti pouze v případě, že znovu zůstane neaktivní, pro `IdleTimeoutInSeconds`. Odvolat, aby se považuje za objekt actor toohave byla použít, pokud se spustí metodu objektu actor rozhraní nebo zpětného volání objektu actor připomenutí. Objekt actor je **není** považována za toohave byla použít, pokud se spustí jeho zpětné volání časovače.

Hello následující diagram znázorňuje životní cyklus hello jednoho objektu actor tooillustrate tyto koncepty.

![Příklad nečinnosti][1]

Hello příklad ukazuje na hello životnost tohoto objektu actor hello dopad volání metody objektu actor, připomenutí a časovače. Hello následující body o příklad hello jsou důležité zmínit:

* ScanInterval a IdleTimeout se nastavují too5 a 10 v uvedeném pořadí. (Jednotky, není podstatné tady, protože naše účel je jenom koncept hello tooillustrate.)
* Hello kontrolu aktéři toobe uvolnění z paměti se odehrává na T = 0, 5, 10, 15, 20, 25, jak jsou definovány interval kontroly hello 5.
* Pravidelné časovač vyvolá v T = 4, 8, 12, 16, 20, 24, a provede zpětné volání. Doba nečinnosti hello hello actor neměla vliv.
* Volání metody objektu actor v T = 7 obnoví hello doba nečinnosti, po too0 a zpozdí hello uvolňování actor hello.
* Provede zpětné volání objektu actor připomenutí v T = 14 a další zpoždění hello uvolnění paměti objektu actor hello.
* Při kontrole kolekce hello uvolňování paměti na T = 25 čas nečinnosti hello objektu actor nakonec překročí časový limit nečinnosti hello 10 a objektu actor hello je uvolnění z paměti.

Objekt actor nebude nikdy uvolnění z paměti, zatímco je prováděna jednu z jeho metod, bez ohledu na to, jak dlouho je věnovaný provedení této metody. Jak už bylo zmíněno dříve, brání hello provádění metody rozhraní objektu actor a zpětná volání připomenutí uvolňování paměti resetováním too0 hello objektu actor doby nečinnosti. Hello provádění zpětných volání časovače neprovádí vynulování too0 hello doby nečinnosti. Uvolňování paměti hello actor hello je však odložené až zpětné volání časovače hello po dokončení provádění.

## <a name="deleting-actors-and-their-state"></a>Odstraňování aktéři a jejich stavu
Uvolnění paměti deaktivované aktéři pouze vyčistí hello objektu actor objektu, ale neodebere data, která je uložená v objektu actor správce stavu. Při opětovné aktivaci objektu actor přišla jeho data znovu k dispozici tooit prostřednictvím hello správce stavu. V případech, kdy aktéři ukládání dat do Správce stavu a jsou deaktivována ale nikdy znovu aktivovat může být nutné tooclean zálohovat svá data.

Hello [služby objektu Actor](service-fabric-reliable-actors-platform.md) poskytuje funkce pro odstranění aktéři ze vzdáleného volající:

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```
```Java
ActorId actorToDelete = new ActorId(id);

ActorService myActorServiceProxy = ActorServiceProxy.create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

myActorServiceProxy.deleteActorAsync(actorToDelete);
```

Odstranění objektu actor má následující důsledky v závislosti na tom, jestli je aktuálně aktivní objektu actor hello hello:

* **Aktivní objektu Actor**
  * Objektu actor se odebral ze seznamu active aktéři a je deaktivována.
  * Jeho stav se trvale odstraní.
* **Neaktivní objektu Actor**
  * Jeho stav se trvale odstraní.

Všimněte si, že objekt actor nelze volat odstranit sám na sobě z jednoho z jeho metody objektu actor, protože objektu actor hello nelze odstranit, při provádění v rámci kontextu volání objektu actor, ve které hello runtime má získat zámek kolem hello objektu actor volání tooenforce jednovláknové přístup.

## <a name="next-steps"></a>Další kroky
* [Časovače objektu actor a upomínek](service-fabric-reliable-actors-timers-reminders.md)
* [Události objektu actor](service-fabric-reliable-actors-events.md)
* [Vícenásobný přístup objektu actor](service-fabric-reliable-actors-reentrancy.md)
* [Objektu actor Diagnostika a sledování výkonu](service-fabric-reliable-actors-diagnostics.md)
* [Referenční dokumentace rozhraní API objektu actor](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [C# ukázkový kód](https://github.com/Azure/servicefabric-samples)
* [Java ukázkový kód](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-lifecycle/garbage-collection.png
