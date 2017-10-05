---
title: "Vytvoření vaší první na základě objektu actor Azure mikroslužbu v Javě | Microsoft Docs"
description: "Tento kurz vás provede kroky při vytváření, ladění a nasazení jednoduchého službu založenou na objektu actor pomocí Service Fabric Reliable Actors."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d31dc8ab-9760-4619-a641-facb8324c759
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/04/2017
ms.author: vturecek
ms.openlocfilehash: 288f1ed1016f50031065e66444d2562427194dc7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-reliable-actors"></a>Začínáme s Reliable Actors
> [!div class="op_single_selector"]
> * [C# v systému Windows](service-fabric-reliable-actors-get-started.md)
> * [Java v Linuxu](service-fabric-reliable-actors-get-started-java.md)
> 
> 

Tento článek vysvětluje základy Azure Service Fabric Reliable Actors a provede vás vytvořením a nasazením jednoduchou aplikaci spolehlivé objektu Actor v jazyce Java.

## <a name="installation-and-setup"></a>Instalace a nastavení
Než začnete, ujistěte se, že máte vývojového prostředí Service Fabric na váš počítač.
Pokud potřebujete nastavit tak, přejděte na [Začínáme v systému Mac](service-fabric-get-started-mac.md) nebo [Začínáme v systému Linux](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Základní koncepty
Začít s Reliable Actors, potřebujete jenom pochopit několik základní koncepty:

* **Služby objektu actor**. Spolehlivé služby, které můžou být nasazené v Service Fabric infrastruktury jsou součástí Reliable Actors. Instance objektu actor aktivují v instanci služby s názvem.
* **Registrace objektu actor**. Jako se službami Reliable Services spolehlivé objektu Actor služby musí být registrováno v modulu runtime Service Fabric. Typ objektu actor kromě toho musí být registrováno s modulem runtime objektu Actor.
* **Rozhraní objektu actor**. Rozhraní objektu actor se používá k definování silného typu veřejné rozhraní objektu actor. Rozhraní objektu actor v terminologii modelu objektu Actor spolehlivé, definuje typy zprávy, které můžete porozumět objektu actor a proces. Rozhraní objektu actor slouží ostatní aktéři a klientské aplikace "Odeslat" (asynchronně) zpráv do objektu actor. Reliable Actors můžete implementovat více rozhraní.
* **Třída ActorProxy**. Třída ActorProxy se používá klientskými aplikacemi volat metody vystavenou přes rozhraní objektu actor. Třída ActorProxy poskytuje dvě důležité funkce:
  
  * Překlad názvů: je možné najít objektu actor v clusteru (Najít uzlu clusteru, který je hostitelem).
  * Zpracování selhání: mohou zkuste volání metod a znovu přeložit umístění objektu actor po, například selhání vyžadující objektu actor pro přemístit do jiného uzlu v clusteru.

Následující pravidla, které se týkají objektu actor rozhraní jsou důležité zmínit:

* Metody rozhraní objektu actor nemohou být přetíženy.
* Rozhraní objektu actor, které se nesmí mít metody, ref nebo volitelné parametry.
* Obecná rozhraní nejsou podporovány.

## <a name="create-an-actor-service"></a>Vytvoření služby objektu actor
Začněte vytvořením nové aplikace Service Fabric. Sada Service Fabric SDK pro Linux zahrnuje Yeoman generátor zajistit generování uživatelského rozhraní pro aplikace Service Fabric pomocí bezstavové služby. Spusťte následující Yeoman spuštěním příkazu:

```bash
$ yo azuresfjava
```

Postupujte podle pokynů vytvořte **spolehlivé služby objektu Actor**. V tomto kurzu, název aplikace "HelloWorldActorApplication" a "HelloWorldActor." objektu actor Vytvoří se následující generování uživatelského rozhraní:

```bash
HelloWorldActorApplication/
├── build.gradle
├── HelloWorldActor
│   ├── build.gradle
│   ├── settings.gradle
│   └── src
│       └── reliableactor
│           ├── HelloWorldActorHost.java
│           └── HelloWorldActorImpl.java
├── HelloWorldActorApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldActorPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   ├── _readme.txt
│       │   └── Settings.xml
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── HelloWorldActorInterface
│   ├── build.gradle
│   └── src
│       └── reliableactor
│           └── HelloWorldActor.java
├── HelloWorldActorTestClient
│   ├── build.gradle
│   ├── settings.gradle
│   ├── src
│   │   └── reliableactor
│   │       └── test
│   │           └── HelloWorldActorTestClient.java
│   └── testclient.sh
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="reliable-actors-basic-building-blocks"></a>Spolehlivé aktéři základních stavebních bloků
Základní koncepty dříve popisované převede na základních stavebních bloků služby objektu Actor spolehlivé.

### <a name="actor-interface"></a>Rozhraní objektu actor
Tato položka obsahuje definici rozhraní objektu actor. Toto rozhraní definuje kontrakt objektu actor, který sdílí implementace objektu actor a klienti volání objektu actor, proto obvykle má smysl definovat na místě, která je oddělená od objektu actor implementaci a může být sdílen více jiných služeb nebo klientské aplikace.

`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:

```java
public interface HelloWorldActor extends Actor {
    @Readonly   
    CompletableFuture<Integer> getCountAsync();

    CompletableFuture<?> setCountAsync(int count);
}
```

### <a name="actor-service"></a>Služby objektu actor
Tato položka obsahuje implementace objektu actor a objektu actor registrační kód. Třída objektu actor implementuje rozhraní objektu actor. Toto je, kde vaše objektu actor nemá svou práci.

`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:

```java
@ActorServiceAttribute(name = "HelloWorldActor.HelloWorldActorService")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class HelloWorldActorImpl extends ReliableActor implements HelloWorldActor {
    Logger logger = Logger.getLogger(this.getClass().getName());

    protected CompletableFuture<?> onActivateAsync() {
        logger.log(Level.INFO, "onActivateAsync");

        return this.stateManager().tryAddStateAsync("count", 0);
    }

    @Override
    public CompletableFuture<Integer> getCountAsync() {
        logger.log(Level.INFO, "Getting current count value");
        return this.stateManager().getStateAsync("count");
    }

    @Override
    public CompletableFuture<?> setCountAsync(int count) {
        logger.log(Level.INFO, "Setting current count value {0}", count);
        return this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value);
    }
}
```

### <a name="actor-registration"></a>Registrace objektu actor
Služby objektu actor musí být zaregistrován s typem služby v modulu runtime Service Fabric. V pořadí pro službu objektu Actor ke spuštění vaše instance objektu actor musí být typu vašeho objektu actor také zaregistrován u služby objektu Actor. `ActorRuntime` Metoda registrace provede tuto práci pro aktéři.

`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:

```java
public class HelloWorldActorHost {

    public static void main(String[] args) throws Exception {

        try {
            ActorRuntime.registerActorAsync(HelloWorldActorImpl.class, (context, actorType) -> new ActorServiceImpl(context, actorType, ()-> new HelloWorldActorImpl()), Duration.ofSeconds(10));

            Thread.sleep(Long.MAX_VALUE);

        } catch (Exception e) {
            e.printStackTrace();
            throw e;
        }
    }
}
```

### <a name="test-client"></a>Testovacího klienta
Toto je jednoduchá testovací klientskou aplikaci můžete spustit samostatně z aplikace Service Fabric testování služby objektu actor. Jedná se o příklad použití ActorProxy pro aktivaci a komunikaci s instancí objektu actor. Získat není nasazené pomocí služby.

### <a name="the-application"></a>Aplikace
Nakonec balíčky aplikace služby objektu actor a dalším službám, které byste mohli přidat v budoucnu společně pro nasazení. Obsahuje *ApplicationManifest.xml* a zástupného pro balíček služby objektu actor.

## <a name="run-the-application"></a>Spuštění aplikace

Yeoman generování uživatelského rozhraní obsahuje skript gradle sestavení aplikace a skripty pro nasazení a odeberte aplikaci bash. Pokud chcete nasadit aplikaci, nejprve sestavení aplikace s gradlem:

```bash
$ gradle
```

To vytvoří balíček aplikace Service Fabric, které se dá nasadit pomocí nástrojů příkazového řádku služby prostředků infrastruktury.

### <a name="deploy-service-fabric-cli"></a>Nasazení Service Fabric rozhraní příkazového řádku

Install.sh skript obsahuje potřebné příkazy Service Fabric rozhraní příkazového řádku (sfctl) k nasazení balíčku aplikace.
Spusťte skript install.sh k nasazení aplikace.

```bash
$ ./install.sh
```

## <a name="next-steps"></a>Další kroky

* [Začínáme se Service Fabric CLI](service-fabric-cli.md)
