---
title: "aaaCreate aplikace Java spolehlivé aktéři Azure Service Fabric v systému Linux | Microsoft Docs"
description: "Zjistěte, jak toocreate a nasazení aplikace Java Service Fabric spolehlivé aktéři za pět minut."
services: service-fabric
documentationcenter: java
author: rwike77
manager: timlt
editor: 
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/23/2017
ms.author: ryanwi
ms.openlocfilehash: 11496b767811c89969c65d1682d843448eb6a922
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-java-service-fabric-reliable-actors-application-on-linux"></a>Vytvoření první aplikace Service Fabric Reliable Actors v Javě v Linuxu
> [!div class="op_single_selector"]
> * [C# – Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java – Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# – Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

Tento rychlý start vám pomůže během několika minut vytvořit první aplikaci Azure Service Fabric v Javě v linuxovém vývojovém prostředí.  Jakmile budete hotovi, budete mít jednoduchou aplikaci Java jedním služby systémem hello místní vývojový cluster.  

## <a name="prerequisites"></a>Požadavky
Než začnete, instalaci hello Service Fabric SDK, hello Service Fabric rozhraní příkazového řádku a nastavit cluster vývoj ve vaší [vývojového prostředí Linux](service-fabric-get-started-linux.md). Pokud používáte Mac OS X, můžete k [nastavení linuxového vývojového prostředí ve virtuálním počítači použít Vagrant](service-fabric-get-started-mac.md).

Můžete také tooinstall hello [Service Fabric rozhraní příkazového řádku](service-fabric-cli.md).

### <a name="install-and-set-up-hello-generators-for-java"></a>Instalace a nastavení hello generátory pro jazyk Java
Service Fabric nabízí nástroje pro generování uživatelského rozhraní, které vám pomůžou vytvořit aplikaci Service Fabric Java z terminálu pomocí generátoru šablon Yeoman. Postupujte podle kroků hello tooensure máte hello Service Fabric yeoman šablony generátor pro jazyk Java pracující na váš počítač.
1. Instalace nodejs a NPM na počítači

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. Instalace generátoru šablon [Yeoman](http://yeoman.io/) na počítač z NPM

  ```bash
  sudo npm install -g yo
  ```
3. Nainstalujte generátor aplikace Service Fabric jo s háčkem nad Java hello z NPM

  ```bash
  sudo npm install -g generator-azuresfjava
  ```

## <a name="create-hello-application"></a>Vytvoření aplikace hello
Aplikace Service Fabric obsahuje jednu nebo více služeb, každý s určitou roli při poskytování funkcí aplikace hello. Generátor Hello jste nainstalovali v poslední části hello umožňuje snadno toocreate první služby a tooadd více později.  Vytvořit, sestavit a nasadit aplikace Service Fabric v Javě můžete také pomocí modulu plug-in pro Eclipse. Viz [Vytvoření a nasazení první aplikace v Javě pomocí Eclipse](service-fabric-get-started-eclipse.md). Pro tento rychlý start použijte Yeoman toocreate aplikace jedinou službou, která uchovává a získá hodnotu čítače.

1. V terminálu zadejte ``yo azuresfjava``.
2. Pojmenujte svoji aplikaci.
3. Vyberte typ hello vaší první služby a pojmenujte ho. Pro účely tohoto kurzu zvolte službu Reliable Actor. Další informace o hello jiné typy služeb, naleznete v části [Service Fabric přehled modelu programování](service-fabric-choose-framework.md).
   ![Generátor Service Fabric Yeoman pro Javu][sf-yeoman]

## <a name="build-hello-application"></a>Vytvoření aplikace hello
šablony služby Fabric Yeoman Hello zahrnují sestavení skript pro [Gradle](https://gradle.org/), které můžete použít aplikaci hello toobuild z hello terminálu.
Závislosti Service Fabric Java se získávají z Mavenu. toobuild a práce na hello aplikací Service Fabric Java, je nutné tooensure, že máte JDK a Gradle nainstalována. Pokud dosud není nainstalován, můžete spustit hello tooinstall JDK(openjdk-8-jdk) a Gradle -

  ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

toobuild a balíček hello aplikace, spusťte následující hello:

  ```bash
  cd myapp
  gradle
  ```

## <a name="deploy-hello-application"></a>Nasazení aplikace hello
Po hello aplikace, můžete ho nasadit toohello místní cluster.

1. Připojte toohello místní cluster Service Fabric.

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. Spusťte instalační skript hello zadaný v toocopy hello šablony aplikace hello balíček toohello clusteru úložiště bitových kopií, registrace typu aplikace hello a vytvoření instance aplikace hello.

    ```bash
    ./install.sh
    ```

Nasazení aplikace hello vytvořené je hello stejně jako všechny ostatní aplikace Service Fabric. Naleznete v dokumentaci k hello na [aplikace Service Fabric s hello Service Fabric rozhraní příkazového řádku pro správu](service-fabric-application-lifecycle-sfctl.md) podrobné pokyny.

Příkazy toothese parametrů naleznete v manifesty hello generuje uvnitř balíčku aplikace hello.

Po nasazení aplikace hello, otevřete prohlížeč a přejděte do [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) v [http://localhost: 19080/Explorer](http://localhost:19080/Explorer).
Potom rozbalte hello **aplikace** uzel a Všimněte si, že nyní položka pro váš typ aplikace a druhý pro hello první instance tohoto typu.

## <a name="start-hello-test-client-and-perform-a-failover"></a>Spustit hello testovacího klienta a provést převzetí služeb při selhání
Aktéři udělat nic samostatně, vyžadují jiná služba nebo klienta toosend je zprávy. Šablona objektu actor Hello obsahuje jednoduchá testovací skriptu, které můžete použít toointeract službou objektu actor hello.

1. Spusťte skript hello pomocí výstup hello hello sledovat nástroj toosee služby objektu actor hello.  Hello zkušební skript volá hello `setCountAsync()` volá metodu tooincrement objektu actor hello čítač, hello `getCountAsync()` metodu hello objektu actor tooget hello nová hodnota čítače a zobrazí, které hodnoty toohello konzoly.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. V Service Fabric Explorer vyhledejte hello uzly hostování hello primární replika služby objektu actor hello. Na snímku obrazovky hello níže je uzel 3. obslužné rutiny repliky primární služba Hello operacích čtení a zápisu.  Změny ve stavu service jsou poté replikovány na sekundárních replikách toohello, spuštěná na uzlech 0 a 1 v úvodní obrazovka snímek níže.

    ![Hledání hello primární repliky v Service Fabric Exploreru][sfx-primary]

3. V **uzly**, klikněte na uzel hello v předchozím kroku hello najít a pak vyberte **deaktivovat (restartovat)** z nabídky akce hello. Tato akce restartuje hello uzlu se systémem hello primární služba repliky a vynutí tooone převzetí služeb při selhání replik sekundární hello systémem v jiném uzlu.  Tato sekundární replika je propagovaných tooprimary, jiné sekundární repliky se vytvoří na jiný uzel a primární repliky hello začne tootake operace čtení a zápisu. Při restartování uzlu hello, sledujte hello výstup hello testovacího klienta a Všimněte si, že tento čítač hello pokračuje tooincrement navzdory hello převzetí služeb při selhání.

## <a name="remove-hello-application"></a>Odebrání aplikace hello
Použít skript odinstalovat hello uvedený v instanci aplikace hello šablony toodelete hello, zrušení registrace balíčku aplikace hello a odebrání balíčku aplikace hello z úložiště bitových kopií hello clusteru.

```bash
./uninstall.sh
```

V Service Fabric explorer uvidíte, že aplikace hello a typ aplikace se v již hello **aplikace** uzlu.

## <a name="service-fabric-java-libraries-on-maven"></a>Knihovny Service Fabric Java v Mavenu
Hostitelem knihoven Service Fabric Java je Maven. Můžete přidat závislosti hello v hello ``pom.xml`` nebo ``build.gradle`` knihoven vaše projekty toouse Service Fabric Java z **mavenCentral**.

### <a name="actors"></a>Objekty actor

Podpora Service Fabric Reliable Actor pro vaši aplikaci.

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-actors-preview</artifactId>
      <version>0.10.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-actors-preview:0.10.0'
  }
  ```

### <a name="services"></a>Služby

Podpora bezstavové služby Service Fabric pro vaši aplikaci.

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-services-preview</artifactId>
      <version>0.10.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-services-preview:0.10.0'
  }
  ```

### <a name="others"></a>Ostatní
#### <a name="transport"></a>Přenos

Podpora přenosové vrstvy pro aplikace Service Fabric Java. Není nutné tooexplicitly přidat tuto závislost tooyour spolehlivé objektu Actor nebo aplikace služby, pokud program v přenosové vrstvě hello.

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-transport-preview</artifactId>
      <version>0.10.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-transport-preview:0.10.0'
  }
  ```

#### <a name="fabric-support"></a>Podpora prostředků infrastruktury

Systém úrovně podpory pro Service Fabric, která komunikuje toonative modulu runtime Service Fabric. Není nutné tooexplicitly přidat tuto závislost tooyour spolehlivé objektu Actor nebo aplikace služby. To získá automaticky načtených z Maven, jakmile zahrnete hello jiných závislostí uvedených výše.

  ```XML
  <dependency>
      <groupId>com.microsoft.servicefabric</groupId>
      <artifactId>sf-preview</artifactId>
      <version>0.10.0</version>
  </dependency>
  ```

  ```gradle
  repositories {
      mavenCentral()
  }
  dependencies {
      compile 'com.microsoft.servicefabric:sf-preview:0.10.0'
  }
  ```

## <a name="migrating-old-service-fabric-java-applications-toobe-used-with-maven"></a>Migrace starého toobe aplikace Service Fabric Java použít s Maven
Nedávno jsme přesunuli knihovny Service Fabric Java z úložiště tooMaven Service Fabric Java SDK. Při hello nové aplikace, který generovat pomocí Yeoman nebo Eclipse, vygeneruje nejnovější aktualizované projekty (které bude možné toowork s Maven), můžete aktualizovat existující Service Fabric bezstavové nebo objektu actor aplikací Java, které byly pomocí hello služby Fabric Java SDK dříve, toouse hello Service Fabric Java závislostí z Maven. Postupujte podle kroků hello [sem](service-fabric-migrate-old-javaapp-to-use-maven.md) tooensure starší aplikace funguje s Maven.

## <a name="next-steps"></a>Další kroky

* [Vytvoření první aplikace Service Fabric v Javě v Linuxu pomocí Eclipse](service-fabric-get-started-eclipse.md)
* [Další informace o Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Komunikovat s clusterů Service Fabric pomocí hello Service Fabric rozhraní příkazového řádku](service-fabric-cli.md)
* Informace o [možnostech podpory pro Service Fabric](service-fabric-support.md)
* [Začínáme se Service Fabric CLI](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png
