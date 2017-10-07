---
title: "aaaMigrate ze sady Java SDK tooMaven - aktualizovat staré aplikací v jazyce Java Azure Service Fabric toouse Maven | Microsoft Docs"
description: "Aktualizujte hello starší Java aplikace, které používá toouse hello Service Fabric Java SDK závislostí služby Fabric Java toofetch z Maven. Po dokončení této instalace, bude vaše starší aplikace Java možné toobuild."
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: 
ms.assetid: bf84458f-4b87-4de1-9844-19909e368deb
ms.service: service-fabric
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/23/2017
ms.author: saysa
ms.openlocfilehash: 11b979facd7b3865141a6d3a035a6021dd06ca0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="update-your-previous-java-service-fabric-application-toofetch-java-libraries-from-maven"></a>Aktualizovat předchozí Java Service Fabric aplikace toofetch Java knihovnách z Maven
Nedávno jsme přesunuli binární soubory služby Fabric Java z hello Service Fabric Java SDK tooMaven hostování. Nyní můžete pomocí **mavencentral** toofetch hello nejnovější Service Fabric Java závislosti. Tento úvodní pomůže aktualizovat existující Java aplikace, které jste dříve vytvořili toobe použít s Service Fabric Java SDK, pomocí buď Yeoman šablony nebo Eclipse, toobe kompatibilní s hello Maven na základě sestavení.

## <a name="prerequisites"></a>Požadavky
1. Je třeba nejprve toouninstall hello existující sady Java SDK.

  ```bash
  sudo dpkg -r servicefabricsdkjava
  ```
2. Instalace hello nejnovější Service Fabric rozhraní příkazového řádku následující hello uvedených pokynů [zde](service-fabric-cli.md).

3. toobuild a práce na hello aplikací Service Fabric Java, je nutné tooensure, že máte JDK 1.8 a Gradle nainstalována. Pokud dosud není nainstalován, můžete spustit hello následující tooinstall JDK 1.8 (openjdk-8-jdk) a Gradle -

 ```bash
 sudo apt-get install openjdk-8-jdk-headless
 sudo apt-get install gradle
 ```
4. Aktualizace hello instalace nebo odinstalace skripty toouse vaší aplikace hello nové služby Fabric rozhraní příkazového řádku následující uvedených pokynů hello [zde](service-fabric-application-lifecycle-sfctl.md). Naleznete v části Začínáme tooour [příklady](https://github.com/Azure-Samples/service-fabric-java-getting-started) pro referenci.

>[!TIP]
> Po odinstalaci hello Service Fabric Java SDK Yeoman nebude fungovat. Postupujte podle uvedených požadavků hello [sem](service-fabric-create-your-first-linux-application-with-java.md) toohave Service Fabric Yeoman Java šablony generátor nahoru a práce.

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


## <a name="migrating-service-fabric-stateless-service"></a>Migrace bezstavové služby Service Fabric

možnost toobuild toobe existující Service Fabric bezstavové Java service pomocí Service Fabric závislosti načtených z Maven, je nutné tooupdate hello ``build.gradle`` souboru uvnitř hello služby. Dříve takto - toobe jako použije
```
dependencies {
    compile fileTree(dir: '/opt/microsoft/sdk/servicefabric/java/packages/lib', include: ['*.jar'])
    compile project(':Interface')
}
.
.
.
jar {
    manifest {
    attributes(
                'Main-Class': 'statelessservice.MyStatelessServiceHost',
                "Class-Path": configurations.compile.collect { 'lib/' + it.getName() }.join(' '))
    baseName "MyStateless"
    destinationDir = file('./../MyStatelessApplication/MyStatelessPkg/Code')
}
.
.
.
task copyDeps <<{
    copy {
        from("/opt/microsoft/sdk/servicefabric/java/packages/lib")
        into("./../MyStatelessApplication/MyStatelessPkg/Code/lib")
        include('*.jar')
    }
    copy {
        from("/opt/microsoft/sdk/servicefabric/java/packages/lib")
        into("./../MyStatelessApplication/MyStatelessPkg/Code/lib")
        include('libj*.so')
    }
}
```
Nyní, závislostí hello toofetch z Maven, hello **aktualizovat** ``build.gradle`` by měla mít odpovídající části hello takto -
```
repositories {
        mavenCentral()
}

configurations {
    azuresf
}

dependencies {
    compile project(':Interface')
    azuresf ('com.microsoft.servicefabric:sf-services-preview:0.10.0')
    compile fileTree(dir: 'lib', include: '*.jar')
}

task explodeDeps(type: Copy, dependsOn:configurations.azuresf) { task ->
    configurations.azuresf.filter { it.toString().contains("native-preview") }.each{
        from zipTree(it)
    }
    configurations.azuresf.filter { !it.toString().contains("native-preview") }.each {
        from it
    }
    into "lib"
    include "libj*.so", "*.jar"
}

compileJava.dependsOn(explodeDeps)
.
.
.
jar {
    manifest {
        def mpath = configurations.compile.collect {'lib/'+it.getName()}.join (' ')
        mpath = mpath + ' ' + configurations.azuresf.collect {'lib/'+it.getName()}.join (' ')
        attributes(
                'Main-Class': 'statelessservice.MyStatelessServiceHost',
                "Class-Path": mpath)
    baseName "MyStateless"
    destinationDir = file('./../MyStatelessApplication/MyStatelessPkg/Code')
   }
}
.
.
.
task copyDeps <<{
    copy {
        from("lib/")
        into("./../MyStatelessApplication/MyStatelessPkg/Code/lib")
        include('*')
    }
}
```
Obecně platí tooget celkovou představu o tom, jak sestavit hello skript bude vypadat Service Fabric bezstavové služby Java, tooany ukázka můžete odkazovat z našich úvodních příklady. Tady je hello [build.gradle](https://github.com/Azure-Samples/service-fabric-java-getting-started/blob/master/Services/EchoServer/EchoServer1.0/EchoServerService/build.gradle) pro ukázku EchoServer hello.

## <a name="migrating-service-fabric-actor-service"></a>Migrace Service Fabric Actor Service

možnost toobuild toobe existující Service Fabric objektu Actor aplikace v jazyce Java pomocí Service Fabric závislosti načtených z Maven, je nutné tooupdate hello ``build.gradle`` soubor uvnitř balíčku rozhraní hello a v balíčku služby hello. Pokud máte TestClient balíčku, musíte to také tooupdate. Ano, pro vaše objektu actor ``Myactor``, následující hello by hello míst, kde je nutné tooupdate -
```
./Myactor/build.gradle
./MyactorInterface/build.gradle
./MyactorTestClient/build.gradle
```

#### <a name="updating-build-script-for-hello-interface-project"></a>Aktualizace sestavení skript pro hello rozhraní projektu

Dříve takto - toobe jako použije
```
dependencies {
    compile fileTree(dir: '/opt/microsoft/sdk/servicefabric/java/packages/lib', include: ['*.jar'])
}
.
.
```
Nyní, závislostí hello toofetch z Maven, hello **aktualizovat** ``build.gradle`` by měla mít odpovídající části hello takto -
```
repositories {
    mavenCentral()
}

configurations {
    azuresf
}

dependencies {
    azuresf ('com.microsoft.servicefabric:sf-actors-preview:0.10.0')
    compile fileTree(dir: 'lib', include: '*.jar')
}

task explodeDeps(type: Copy, dependsOn:configurations.azuresf) { task ->
    configurations.azuresf.filter { it.toString().contains("native-preview") }.each{
        from zipTree(it)
    }
    configurations.azuresf.filter { !it.toString().contains("native-preview") }.each {
        from it
    }
    into "lib"
    include "libj*.so", "*.jar"
}

compileJava.dependsOn(explodeDeps)
.
.
```

#### <a name="updating-build-script-for-hello-actor-project"></a>Aktualizace sestavení skript pro projekt hello objektu actor

Dříve takto - toobe jako použije
```
dependencies {
    compile fileTree(dir: '/opt/microsoft/sdk/servicefabric/java/packages/lib', include: ['*.jar'])
    compile project(':MyactorInterface')
}
.
.
.
jar {
    manifest {
    attributes(
                'Main-Class': 'reliableactor.MyactorHost',
                "Class-Path": configurations.compile.collect { 'lib/' + it.getName() }.join(' '))
      baseName "myactor"
    destinationDir = file('./../myjavaapp/MyactorPkg/Code')
    }
}
.
.
.
task copyDeps<< {
    copy {
        from("/opt/microsoft/sdk/servicefabric/java/packages/lib")
        into("./../myjavaapp/MyactorPkg/Code/lib")
        include('*.jar')
    }
    copy {
        from("/opt/microsoft/sdk/servicefabric/java/packages/lib")
        into("./../myjavaapp/MyactorPkg/Code/lib")
        include('libj*.so')
    }
    copy {
        from("../MyactorInterface/out/lib")
        into("./../myjavaapp/MyactorPkg/Code/lib")
        include('*.jar')
    }
}
```
Nyní, závislostí hello toofetch z Maven, hello **aktualizovat** ``build.gradle`` by měla mít odpovídající části hello takto -
```
repositories {
    mavenCentral()
}

configurations {
    azuresf
}

dependencies {
    compile project(':MyactorInterface')
    azuresf ('com.microsoft.servicefabric:sf-actors-preview:0.10.0')
    compile fileTree(dir: 'lib', include: '*.jar')
}

task explodeDeps(type: Copy, dependsOn:configurations.azuresf) { task ->
    configurations.azuresf.filter { it.toString().contains("native-preview") }.each{
        from zipTree(it)
    }
    configurations.azuresf.filter { !it.toString().contains("native-preview") }.each {
        from it
    }
    into "lib"
    include "libj*.so", "*.jar"
}

compileJava.dependsOn(explodeDeps)
.
.
.
jar {
    manifest {
        def mpath = configurations.compile.collect {'lib/'+it.getName()}.join (' ')
        mpath = mpath + ' ' + configurations.azuresf.collect {'lib/'+it.getName()}.join (' ')
        attributes(
                'Main-Class': 'reliableactor.MyactorHost',
                "Class-Path": mpath)
    baseName "myactor"
    destinationDir = file('../myjavaapp/MyactorPkg/Code')}
 }
.
.
.
task copyDeps<< {
      copy {
              from("lib/")
              into("../myjavaapp/MyactorPkg/Code/lib")
              include('*')
      }
      copy {
              from("../MyactorInterface/out/lib")
              into("../myjavaapp/MyactorPkg/Code/lib")
              include('*.jar')
      }
}
```

#### <a name="updating-build-script-for-hello-test-client-project"></a>Aktualizace sestavení skript pro hello testovacího klienta projektu

Změny tady jsou podobné toohello změny popsané v předchozí části, tedy hello objektu actor projektu. Dříve hello Gradle toobe skriptu použít jako takto-
```
dependencies {
    compile fileTree(dir: '/opt/microsoft/sdk/servicefabric/java/packages/lib', include: ['*.jar'])
      compile project(':MyactorInterface')
}
.
.
.
jar
{
    manifest {
    attributes(
        'Main-Class': 'reliableactor.test.MyactorTestClient',
        "Class-Path": configurations.compile.collect { 'lib/' + it.getName() }.join(' '))
    }
    baseName "myactor-test"
  destinationDir = file('out/lib')
}
.
.
.
task copyDeps<< {
        copy {
                from("/opt/microsoft/sdk/servicefabric/java/packages/lib")
                into("./out/lib/lib")
                include('*.jar')
        }
        copy {
                from("/opt/microsoft/sdk/servicefabric/java/packages/lib")
                into("./out/lib/lib")
                include('libj*.so')
        }
        copy {
                from("../MyactorInterface/out/lib")
                into("./out/lib/lib")
                include('*.jar')
        }
}
```
Nyní, závislostí hello toofetch z Maven, hello **aktualizovat** ``build.gradle`` by měla mít odpovídající části hello takto -
```
repositories {
    mavenCentral()
}

configurations {
    azuresf
}

dependencies {
    compile project(':MyactorInterface')
    azuresf ('com.microsoft.servicefabric:sf-actors-preview:0.10.0')
    compile fileTree(dir: 'lib', include: '*.jar')
}

task explodeDeps(type: Copy, dependsOn:configurations.azuresf) { task ->
    configurations.azuresf.filter { it.toString().contains("native-preview") }.each{
        from zipTree(it)
    }
    configurations.azuresf.filter { !it.toString().contains("native-preview") }.each {
        from it
    }
    into "lib"
    include "libj*.so", "*.jar"
}

compileJava.dependsOn(explodeDeps)
.
.
.
jar
{
    manifest {
        def mpath = configurations.compile.collect {'lib/'+it.getName()}.join (' ')
        mpath = mpath + ' ' + configurations.azuresf.collect {'lib/'+it.getName()}.join (' ')
    attributes(
                'Main-Class': 'reliableactor.test.MyactorTestClient',
                "Class-Path": mpath)
    baseName "myactor-test"
    destinationDir = file('./out/lib')
        }
}
.
.
.
task copyDeps<< {
        copy {
                from("lib/")
                into("./out/lib/lib")
                include('*')
        }
        copy {
                from("../MyactorInterface/out/lib")
                into("./out/lib/lib")
                include('*.jar')
        }
}
```

## <a name="next-steps"></a>Další kroky

* [Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí Yeomana](service-fabric-create-your-first-linux-application-with-java.md)
* [Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí modulu plug-in Service Fabric pro Eclipse](service-fabric-get-started-eclipse.md)
* [Komunikovat s clusterů Service Fabric pomocí hello Service Fabric rozhraní příkazového řádku](service-fabric-cli.md)
