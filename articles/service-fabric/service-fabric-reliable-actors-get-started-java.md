---
title: "aaaCreate vaše první na základě objektu actor Azure mikroslužbu v Javě | Microsoft Docs"
description: "Tento kurz vás provede kroky hello při vytváření, ladění a nasazení jednoduchého službu založenou na objektu actor pomocí Service Fabric Reliable Actors."
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
ms.openlocfilehash: 24718a8d7034360c53597f139169580f1a6ce732
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

Tento článek vysvětluje základy hello Azure Service Fabric Reliable Actors a provede vás vytvořením a nasazením jednoduchou aplikaci spolehlivé objektu Actor v jazyce Java.

## <a name="installation-and-setup"></a>Instalace a nastavení
Než začnete, ujistěte se, že máte hello Service Fabric vývojového prostředí nastavit na vašem počítači.
Pokud potřebujete tooset ho, přejděte příliš[Začínáme v systému Mac](service-fabric-get-started-mac.md) nebo [Začínáme v systému Linux](service-fabric-get-started-linux.md).

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

## <a name="create-an-actor-service"></a>Vytvoření služby objektu actor
Začněte vytvořením nové aplikace Service Fabric. zahrnuje Hello Service Fabric SDK pro Linux Yeoman generování generátor tooprovide hello uživatelského rozhraní pro aplikace Service Fabric pomocí bezstavové služby. Spusťte spuštěním hello následující Yeoman příkaz:

```bash
$ yo azuresfjava
```

Postupujte podle pokynů toocreate hello **spolehlivé služby objektu Actor**. V tomto kurzu název hello aplikace "HelloWorldActorApplication" a hello objektu actor "HelloWorldActor." Vytvoří Hello následující generování uživatelského rozhraní:

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
základní koncepty Hello dříve popisované převede na hello základní stavební bloky služby objektu Actor spolehlivé.

### <a name="actor-interface"></a>Rozhraní objektu actor
Tato položka obsahuje definici rozhraní hello objektu actor hello. Toto rozhraní definuje kontrakt objektu actor hello, který sdílí hello objektu actor implementace a volání objektu actor hello, takže obvykle má smysl toodefine jej do umístění, které je oddělené od implementace objektu actor hello a může být sdílen více jiných klientů hello služby nebo aplikace klienta.

`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:

```java
public interface HelloWorldActor extends Actor {
    @Readonly   
    CompletableFuture<Integer> getCountAsync();

    CompletableFuture<?> setCountAsync(int count);
}
```

### <a name="actor-service"></a>Služby objektu actor
Tato položka obsahuje implementace objektu actor a objektu actor registrační kód. Třída objektu actor Hello implementuje rozhraní objektu actor hello. Toto je, kde vaše objektu actor nemá svou práci.

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
služby objektu actor Hello musí být zaregistrován s typem služby v modulu runtime Service Fabric hello. V pořadí pro hello služby objektu Actor toorun vaše instance objektu actor, vašeho typu objektu actor musí být zaregistrovaná taky hello služby objektu Actor. Hello `ActorRuntime` metoda registrace provede tuto práci pro aktéři.

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
Toto je jednoduchá testovací klientskou aplikaci můžete spustit samostatně z tootest aplikace Service Fabric hello služby objektu actor. Jedná se o příklad kde hello ActorProxy můžou být použité tooactivate a komunikaci s instancí objektu actor. Získat není nasazené pomocí služby.

### <a name="hello-application"></a>aplikace Hello
Nakonec balíčky aplikací hello hello služby objektu actor a dalším službám, které byste mohli přidat v hello budoucí společně pro nasazení. Obsahuje hello *ApplicationManifest.xml* a zástupného pro balíček služby objektu actor hello.

## <a name="run-hello-application"></a>Spuštění aplikace hello

Hello Yeoman generování uživatelského rozhraní obsahuje gradle skriptu toobuild hello aplikace a bash skripty toodeploy a aplikaci odebrat. toodeploy hello aplikace, první aplikace hello sestavení s gradlem:

```bash
$ gradle
```

To vytvoří balíček aplikace Service Fabric, které se dá nasadit pomocí nástrojů příkazového řádku služby prostředků infrastruktury.

### <a name="deploy-service-fabric-cli"></a>Nasazení Service Fabric rozhraní příkazového řádku

Hello install.sh skript obsahuje hello nezbytné Service Fabric rozhraní příkazového řádku (sfctl) příkazy toodeploy hello balíčku aplikace.
Spuštění aplikace hello hello install.sh skriptu toodeploy.

```bash
$ ./install.sh
```

## <a name="next-steps"></a>Další kroky

* [Začínáme se Service Fabric CLI](service-fabric-cli.md)
