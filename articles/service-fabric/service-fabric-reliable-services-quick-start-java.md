---
title: "aaaCreate vaše první spolehlivé Azure mikroslužbu v Javě | Microsoft Docs"
description: "Úvod toocreating aplikace Microsoft Azure Service Fabric s bezzstavovými i stavovými službami."
services: service-fabric
documentationcenter: java
author: vturecek
manager: timlt
editor: 
ms.assetid: 7831886f-7ec4-4aef-95c5-b2469a5b7b5d
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 577d96591797bbfe6be5c1094426b5f1435cca0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-reliable-services"></a>Začínáme s Reliable Services
> [!div class="op_single_selector"]
> * [C# v systému Windows](service-fabric-reliable-services-quick-start.md)
> * [Java v Linuxu](service-fabric-reliable-services-quick-start-java.md)
>
>

Tento článek vysvětluje hello Základy služby Azure Service Fabric spolehlivé a provede vás vytvořením a nasazením jednoduchou spolehlivá služba aplikaci napsanou v jazyce Java. Toto video Microsoft Virtual Academy také ukazuje, jak toocreate bezstavové spolehlivé služby:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965">  
<img src="./media/service-fabric-reliable-services-quick-start-java/ReliableServicesJavaVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="installation-and-setup"></a>Instalace a nastavení
Než začnete, ujistěte se, že máte hello Service Fabric vývojového prostředí nastavit na vašem počítači.
Pokud potřebujete tooset ho, přejděte příliš[Začínáme v systému Mac](service-fabric-get-started-mac.md) nebo [Začínáme v systému Linux](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Základní koncepty
spuštění se službami Reliable Services, můžete pouze tooget potřebovat toounderstand pár základních konceptech:

* **Typ služby**: Toto je implementace služby. Je definována pomocí třídy hello napíšete, která rozšiřuje `StatelessService` a ostatní kódu nebo v něm, použít název a číslo verze závislosti.
* **S názvem instance služby**: toorun vaší služby, vytvoříte pojmenované instance typu služby mnohem jako vytvoření instance objektu typu třídy. Instance služby, které jsou ve skutečnosti instancí objektu možnosti vaší služby třídu, která můžete zapsat.
* **Hostitel služby**: hello s názvem instance služby vytvoříte toorun nutné v hostiteli. Hostitel služby Hello je právě proces, kde můžete spustit instance služby.
* **Registrace služby**: registrace soustřeďuje všechny informace dohromady. Hello typ služby musí být zaregistrován s hello Service Fabric runtime ve službě hostitele tooallow Service Fabric toocreate instance ho toorun.  

## <a name="create-a-stateless-service"></a>Vytvoření bezstavové služby
Začněte vytvořením aplikace Service Fabric. zahrnuje Hello Service Fabric SDK pro Linux Yeoman generování generátor tooprovide hello uživatelského rozhraní pro aplikace Service Fabric pomocí bezstavové služby. Spusťte spuštěním hello následující Yeoman příkaz:

```bash
$ yo azuresfjava
```

Postupujte podle pokynů toocreate hello **spolehlivé bezstavové služby**. V tomto kurzu aplikace hello název "HelloWorldApplication" a hello služby "HelloWorld". výsledek Hello zahrnuje adresářů pro hello `HelloWorldApplication` a `HelloWorld`.

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="implement-hello-service"></a>Implementace služby hello
Otevřete **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**. Tato třída definuje typ hello služby a všechny kód můžete spustit. rozhraní API služby Hello poskytuje dva vstupní body kódu:

* Metodu zprostředkovává vstupního bodu, názvem `runAsync()`, kde můžete začít provádění jakékoli úlohy, včetně dlouho běžící výpočetních úloh.

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {
    ...
}
```

* Ke komunikaci vstupnímu bodu kde zařaďte vaší komunikačního balíku výběru. Toto je, kde můžete spustit přijímá žádosti od uživatelů a dalších služeb.

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

V tomto kurzu budeme soustředit na hello `runAsync()` metody vstupní bod. Toto je, kde můžete okamžitě začít kód spuštěný.

### <a name="runasync"></a>RunAsync
Platforma Hello volá tuto metodu, pokud instance služby je umístěný a připravené tooexecute. Bezstavové služby, která jednoduše znamená po otevření hello instance služby. Token zrušení získáte toocoordinate při instanci služby musí toobe uzavřen. V Service Fabric může tento cyklus otevření nebo uzavření instance služby k mnohokrát průběhu životnosti hello hello služby jako celek. Toto může nastat z různých důvodů, včetně:

* Hello systém přesune vaší instance služby pro vyrovnávání prostředků.
* K chybám dochází v kódu.
* Hello aplikace nebo systému byla upgradována.
* základní hardware Hello dojde k výpadku.

Tato orchestration spravuje Service Fabric tookeep služby vysoce dostupné a správně vyrovnáváním.

`runAsync()`by neměly blokovat synchronně. Implementaci runAsync by měl vrátit toocontinue CompletableFuture tooallow hello modulu runtime. Pokud vaše úlohy potřebuje tooimplement dlouhotrvající úlohu, která se má provést uvnitř hello CompletableFuture.

#### <a name="cancellation"></a>Zrušení
Zrušení úlohy je spolupráci úsilí řízená hello zadaný token zrušení. systém Hello počká na vaše úloha tooend (podle úspěšné dokončení, zrušení nebo selhání) předtím, než ji přesune. Je důležité toohonor hello zrušení tokenu, dokončit všechny fungovat a ukončete `runAsync()` provést co nejrychleji při hello systému požadavky zrušení. Hello následující příklad ukazuje, jak toohandle zrušení událostí:

```java
    @Override
    protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {

        // TODO: Replace hello following sample code with your own logic
        // or remove this runAsync override if it's not needed in your service.

        CompletableFuture.runAsync(() -> {
          long iterations = 0;
          while(true)
          {
            cancellationToken.throwIfCancellationRequested();
            logger.log(Level.INFO, "Working-{0}", ++iterations);

            try
            {
              Thread.sleep(1000);
            }
            catch (IOException ex) {}
          }
        });
    }
```

### <a name="service-registration"></a>Registrace služby
Typů služeb musí být zaregistrované v modulu runtime Service Fabric hello. Typ služby Hello je definována v hello `ServiceManifest.xml` a třídě služby, který implementuje `StatelessService`. Registrace služby se provádí v hello proces hlavní vstupní bod. V tomto příkladu hello proces hlavní vstupní bod je `HelloWorldServiceHost.java`:

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    }
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration:", ex);
        throw ex;
    }
}
```

## <a name="run-hello-application"></a>Spuštění aplikace hello

Hello Yeoman generování uživatelského rozhraní obsahuje gradle skriptu toobuild hello aplikace a bash skripty toodeploy a aplikaci odebrat. toorun hello aplikace, první aplikace hello sestavení s gradlem:

```bash
$ gradle
```

Tímto se vytvoří balíček aplikace Service Fabric, které se dá nasadit pomocí Service Fabric rozhraní příkazového řádku.

### <a name="deploy-with-service-fabric-cli"></a>Nasazení pomocí Service Fabric rozhraní příkazového řádku

Hello install.sh skript obsahuje hello nezbytné Service Fabric rozhraní příkazového řádku příkazy toodeploy hello balíčku aplikace. Spusťte aplikaci install.sh skriptu toodeploy hello.

```bash
$ ./install.sh
```

## <a name="next-steps"></a>Další kroky

* [Začínáme se Service Fabric CLI](service-fabric-cli.md)
