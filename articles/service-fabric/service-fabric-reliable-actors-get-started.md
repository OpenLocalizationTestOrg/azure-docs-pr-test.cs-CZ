---
title: "aaaCreate vaše první na základě objektu actor Azure mikroslužbu v jazyce C# | Microsoft Docs"
description: "Tento kurz vás provede kroky hello při vytváření, ladění a nasazení jednoduchého službu založenou na objektu actor pomocí Service Fabric Reliable Actors."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d4aebe72-1551-4062-b1eb-54d83297f139
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: ab4f75bef0adb6e70f0ead587475b3fb51e6e6a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-reliable-actors"></a>Začínáme s Reliable Actors
> [!div class="op_single_selector"]
> * [C# v systému Windows](service-fabric-reliable-actors-get-started.md)
> * [Java v Linuxu](service-fabric-reliable-actors-get-started-java.md)
> 
> 

Tento článek vysvětluje základy hello Azure Service Fabric Reliable Actors a provede vás vytváření, ladění a nasazování jednoduchou aplikaci spolehlivé objektu Actor v sadě Visual Studio.

## <a name="installation-and-setup"></a>Instalace a nastavení
Než začnete, ujistěte se, zda máte na počítači nastavit hello Service Fabric vývojové prostředí.
Pokud potřebujete tooset ho, najdete podrobné pokyny na [jak tooset hello vývojového prostředí](service-fabric-get-started.md).

## <a name="basic-concepts"></a>Základní koncepty
tooget začít s Reliable Actors, můžete pouze potřebovat toounderstand několik základní koncepty:

* **Služby objektu actor**. Spolehlivé služby, které můžou být nasazené v infrastruktuře Service Fabric hello jsou součástí Reliable Actors. Instance objektu actor aktivují v instanci služby s názvem.
* **Registrace objektu actor**. Jako služba spolehlivé objektu Actor potřebuje se službami Reliable Services toobe zaregistrována modulu runtime Service Fabric hello. Typ objektu actor hello kromě toho musí toobe zaregistrována hello objektu Actor runtime.
* **Rozhraní objektu actor**. rozhraní objektu actor Hello je použité toodefine silného typu veřejné rozhraní objektu actor. Rozhraní objektu actor hello v hello terminologie modelu objektu Actor spolehlivé, definuje hello typy zpráv, které hello objektu actor můžete pochopit a zpracovat. rozhraní objektu actor Hello používá jiné aktéři a klientské aplikace příliš "Odeslat" (asynchronně) objektu actor toohello zprávy. Reliable Actors můžete implementovat více rozhraní.
* **Třída ActorProxy**. používá Hello ActorProxy třídy klienta aplikace tooinvoke hello metody vystavenou přes rozhraní objektu actor hello. Hello ActorProxy třída poskytuje dvě důležité funkce:
  
  * Překlad názvů: je možné toolocate hello objektu actor v clusteru hello (Najít hello uzel hello clusteru, který je hostitelem).
  * Zpracování selhání: můžete opakujte volání metod a znovu přeložit umístění objektu actor hello po, například selhání, který vyžaduje hello objektu actor toobe přemístění tooanother uzlu v clusteru hello.

Hello následující pravidla, které se týkají tooactor rozhraní jsou důležité zmínit:

* Metody rozhraní objektu actor nemohou být přetíženy.
* Rozhraní objektu actor, které se nesmí mít metody, ref nebo volitelné parametry.
* Obecná rozhraní nejsou podporovány.

## <a name="create-a-new-project-in-visual-studio"></a>Vytvořte nový projekt v sadě Visual Studio
Spusťte Visual Studio 2015 nebo 2017 Visual Studio jako správce a vytvořit nový projekt aplikace Service Fabric:

![Service Fabric nástrojů pro Visual Studio – nový projekt][1]

V hello další dialogové okno můžete si zvolit hello typ projektu, které chcete toocreate.

![Šablony projektů Service Fabric][5]

Pro projekt Hello World hello použijeme služby Service Fabric Reliable Actors hello.

Po vytvoření hello řešení, měli byste vidět hello strukturu:

![Struktura projektu Service Fabric][2]

## <a name="reliable-actors-basic-building-blocks"></a>Spolehlivé aktéři základních stavebních bloků
Typické Reliable Actors řešení se skládá ze tří projektů:

* **projekt aplikace Hello (MyActorApplication)**. Toto je hello projekt, který balíčky všechny společně hello služby pro nasazení. Obsahuje hello *ApplicationManifest.xml* a skriptů prostředí PowerShell pro správu aplikace hello.
* **Hello rozhraní projektu (MyActor.Interfaces)**. Toto je hello projekt, který obsahuje definici rozhraní hello objektu actor hello. V projektu MyActor.Interfaces hello můžete definovat hello rozhraní, které se použijí hello aktéři v řešení hello. Vašich rozhraní objektu actor lze definovat v jakékoli projektu s libovolným názvem, ale hello rozhraní definuje kontrakt objektu actor hello, který sdílí implementace objektu actor hello a hello klientům volání objektu actor hello, takže obvykle má smysl toodefine v sestavení, které je oddělené od implementace objektu actor hello a může být sdílen více další projekty.

```csharp
public interface IMyActor : IActor
{
    Task<string> HelloWorld();
}
```

* **projekt služby objektu actor Hello (MyActor)**. Toto je hello projektu používá toodefine hello Service Fabric služba, která se má toohost hello objektu actor. Obsahuje hello implementace objektu actor hello. Implementace objektu actor je třída, která je odvozena od základního typu hello `Actor` a implementuje hello alespoň jedno rozhraní, která jsou definována v projektu MyActor.Interfaces hello. Třídu objektu actor musí také implementovat konstruktor, který přijímá `ActorService` instance a `ActorId` a předá je toohello základní `Actor` třídy. To umožňuje vkládání závislostí konstruktor platformy závislostí.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<string> HelloWorld()
    {
        return Task.FromResult("Hello world!");
    }
}
```

služby objektu actor Hello musí být zaregistrován s typem služby v modulu runtime Service Fabric hello. V pořadí pro hello služby objektu Actor toorun vaše instance objektu actor, vašeho typu objektu actor musí být zaregistrovaná taky hello služby objektu Actor. Hello `ActorRuntime` metoda registrace provede tuto práci pro aktéři.

```csharp
internal static class Program
{
    private static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<MyActor>(
                (context, actorType) => new ActorService(context, actorType, () => new MyActor())).GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}

```

Pokud spustíte z nový projekt v sadě Visual Studio a máte jenom jednu definici objektu actor, hello registrace je zahrnuta ve výchozím nastavení v hello kód, který generuje Visual Studio. Pokud definujete jiných aktéři v hello služby, je třeba tooadd hello objektu actor registrace pomocí:

```csharp
 ActorRuntime.RegisterActorAsync<MyOtherActor>();

```

> [!TIP]
> Hello modulu runtime Service Fabric aktéři vysílá některé [události a čítače výkonu související metody tooactor](service-fabric-reliable-actors-diagnostics.md#actor-method-events-and-performance-counters). Jsou užitečné v Diagnostika a sledování výkonu.
> 
> 

## <a name="debugging"></a>Ladění
Hello Service Fabric tools pro Visual Studio podporují ladění na místním počítači. Můžete začít relaci ladění pomocí hello stiskne klávesu F5. Visual Studio vytvoří (v případě potřeby) balíčky. Také nasadí aplikaci hello na místní cluster Service Fabric hello a připojí ladicí program hello.

Během procesu nasazení hello, uvidíte průběh hello hello **výstup** okno.

![Service Fabric ladicích výstup – okno][3]

## <a name="next-steps"></a>Další kroky
Další informace o [jak Reliable Actors pomocí platformy Service Fabric hello](service-fabric-reliable-actors-platform.md).

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject.PNG
[2]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-projectstructure.PNG
[3]: ./media/service-fabric-reliable-actors-get-started/debugging-output.PNG
[4]: ./media/service-fabric-reliable-actors-get-started/vs-context-menu.png
[5]: ./media/service-fabric-reliable-actors-get-started/reliable-actors-newproject1.PNG
