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
# <a name="update-your-previous-java-service-fabric-application-toofetch-java-libraries-from-maven"></a><span data-ttu-id="3b6a0-104">Aktualizovat předchozí Java Service Fabric aplikace toofetch Java knihovnách z Maven</span><span class="sxs-lookup"><span data-stu-id="3b6a0-104">Update your previous Java Service Fabric application toofetch Java libraries from Maven</span></span>
<span data-ttu-id="3b6a0-105">Nedávno jsme přesunuli binární soubory služby Fabric Java z hello Service Fabric Java SDK tooMaven hostování.</span><span class="sxs-lookup"><span data-stu-id="3b6a0-105">We have recently moved Service Fabric Java binaries from hello Service Fabric Java SDK tooMaven hosting.</span></span> <span data-ttu-id="3b6a0-106">Nyní můžete pomocí **mavencentral** toofetch hello nejnovější Service Fabric Java závislosti.</span><span class="sxs-lookup"><span data-stu-id="3b6a0-106">Now you can use **mavencentral** toofetch hello latest Service Fabric Java dependencies.</span></span> <span data-ttu-id="3b6a0-107">Tento úvodní pomůže aktualizovat existující Java aplikace, které jste dříve vytvořili toobe použít s Service Fabric Java SDK, pomocí buď Yeoman šablony nebo Eclipse, toobe kompatibilní s hello Maven na základě sestavení.</span><span class="sxs-lookup"><span data-stu-id="3b6a0-107">This quick-start helps you update your existing Java applications, which you earlier created toobe used with Service Fabric Java SDK, using either Yeoman template or Eclipse, toobe compatible with hello Maven based build.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3b6a0-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="3b6a0-108">Prerequisites</span></span>
1. <span data-ttu-id="3b6a0-109">Je třeba nejprve toouninstall hello existující sady Java SDK.</span><span class="sxs-lookup"><span data-stu-id="3b6a0-109">First you need toouninstall hello existing Java SDK.</span></span>

  ```bash
  sudo dpkg -r servicefabricsdkjava
  ```
2. <span data-ttu-id="3b6a0-110">Instalace hello nejnovější Service Fabric rozhraní příkazového řádku následující hello uvedených pokynů [zde](service-fabric-cli.md).</span><span class="sxs-lookup"><span data-stu-id="3b6a0-110">Install hello latest Service Fabric CLI following hello steps mentioned [here](service-fabric-cli.md).</span></span>

3. <span data-ttu-id="3b6a0-111">toobuild a práce na hello aplikací Service Fabric Java, je nutné tooensure, že máte JDK 1.8 a Gradle nainstalována.</span><span class="sxs-lookup"><span data-stu-id="3b6a0-111">toobuild and work on hello Service Fabric Java applications, you need tooensure that you have JDK 1.8 and Gradle installed.</span></span> <span data-ttu-id="3b6a0-112">Pokud dosud není nainstalován, můžete spustit hello následující tooinstall JDK 1.8 (openjdk-8-jdk) a Gradle -</span><span class="sxs-lookup"><span data-stu-id="3b6a0-112">If not yet installed, you can run hello following tooinstall JDK 1.8 (openjdk-8-jdk) and Gradle -</span></span>

 ```bash
 sudo apt-get install openjdk-8-jdk-headless
 sudo apt-get install gradle
 ```
4. <span data-ttu-id="3b6a0-113">Aktualizace hello instalace nebo odinstalace skripty toouse vaší aplikace hello nové služby Fabric rozhraní příkazového řádku následující uvedených pokynů hello [zde](service-fabric-application-lifecycle-sfctl.md).</span><span class="sxs-lookup"><span data-stu-id="3b6a0-113">Update hello install/uninstall scripts of your application toouse hello new Service Fabric CLI following hello steps mentioned [here](service-fabric-application-lifecycle-sfctl.md).</span></span> <span data-ttu-id="3b6a0-114">Naleznete v části Začínáme tooour [příklady](https://github.com/Azure-Samples/service-fabric-java-getting-started) pro referenci.</span><span class="sxs-lookup"><span data-stu-id="3b6a0-114">You can refer tooour getting-started [examples](https://github.com/Azure-Samples/service-fabric-java-getting-started) for reference.</span></span>

>[!TIP]
> <span data-ttu-id="3b6a0-115">Po odinstalaci hello Service Fabric Java SDK Yeoman nebude fungovat.</span><span class="sxs-lookup"><span data-stu-id="3b6a0-115">After uninstalling hello Service Fabric Java SDK, Yeoman will not work.</span></span> <span data-ttu-id="3b6a0-116">Postupujte podle uvedených požadavků hello [sem](service-fabric-create-your-first-linux-application-with-java.md) toohave Service Fabric Yeoman Java šablony generátor nahoru a práce.</span><span class="sxs-lookup"><span data-stu-id="3b6a0-116">Follow hello Prerequisites mentioned [here](service-fabric-create-your-first-linux-application-with-java.md) toohave Service Fabric Yeoman Java template generator up and working.</span></span>

## <a name="service-fabric-java-libraries-on-maven"></a><span data-ttu-id="3b6a0-117">Knihovny Service Fabric Java v Mavenu</span><span class="sxs-lookup"><span data-stu-id="3b6a0-117">Service Fabric Java libraries on Maven</span></span>
<span data-ttu-id="3b6a0-118">Hostitelem knihoven Service Fabric Java je Maven.</span><span class="sxs-lookup"><span data-stu-id="3b6a0-118">Service Fabric Java libraries have been hosted in Maven.</span></span> <span data-ttu-id="3b6a0-119">Můžete přidat závislosti hello v hello ``pom.xml`` nebo ``build.gradle`` knihoven vaše projekty toouse Service Fabric Java z **mavenCentral**.</span><span class="sxs-lookup"><span data-stu-id="3b6a0-119">You can add hello dependencies in hello ``pom.xml`` or ``build.gradle`` of your projects toouse Service Fabric Java libraries from **mavenCentral**.</span></span>

### <a name="actors"></a><span data-ttu-id="3b6a0-120">Objekty actor</span><span class="sxs-lookup"><span data-stu-id="3b6a0-120">Actors</span></span>

<span data-ttu-id="3b6a0-121">Podpora Service Fabric Reliable Actor pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3b6a0-121">Service Fabric Reliable Actor support for your application.</span></span>

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

### <a name="services"></a><span data-ttu-id="3b6a0-122">Služby</span><span class="sxs-lookup"><span data-stu-id="3b6a0-122">Services</span></span>

<span data-ttu-id="3b6a0-123">Podpora bezstavové služby Service Fabric pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3b6a0-123">Service Fabric Stateless Service support for your application.</span></span>

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

### <a name="others"></a><span data-ttu-id="3b6a0-124">Ostatní</span><span class="sxs-lookup"><span data-stu-id="3b6a0-124">Others</span></span>
#### <a name="transport"></a><span data-ttu-id="3b6a0-125">Přenos</span><span class="sxs-lookup"><span data-stu-id="3b6a0-125">Transport</span></span>

<span data-ttu-id="3b6a0-126">Podpora přenosové vrstvy pro aplikace Service Fabric Java.</span><span class="sxs-lookup"><span data-stu-id="3b6a0-126">Transport layer support for Service Fabric Java application.</span></span> <span data-ttu-id="3b6a0-127">Není nutné tooexplicitly přidat tuto závislost tooyour spolehlivé objektu Actor nebo aplikace služby, pokud program v přenosové vrstvě hello.</span><span class="sxs-lookup"><span data-stu-id="3b6a0-127">You do not need tooexplicitly add this dependency tooyour Reliable Actor or Service applications, unless you program at hello transport layer.</span></span>

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

#### <a name="fabric-support"></a><span data-ttu-id="3b6a0-128">Podpora prostředků infrastruktury</span><span class="sxs-lookup"><span data-stu-id="3b6a0-128">Fabric support</span></span>

<span data-ttu-id="3b6a0-129">Systém úrovně podpory pro Service Fabric, která komunikuje toonative modulu runtime Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="3b6a0-129">System level support for Service Fabric, which talks toonative Service Fabric runtime.</span></span> <span data-ttu-id="3b6a0-130">Není nutné tooexplicitly přidat tuto závislost tooyour spolehlivé objektu Actor nebo aplikace služby.</span><span class="sxs-lookup"><span data-stu-id="3b6a0-130">You do not need tooexplicitly add this dependency tooyour Reliable Actor or Service applications.</span></span> <span data-ttu-id="3b6a0-131">To získá automaticky načtených z Maven, jakmile zahrnete hello jiných závislostí uvedených výše.</span><span class="sxs-lookup"><span data-stu-id="3b6a0-131">This gets fetched automatically from Maven, when you include hello other dependencies above.</span></span>

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


## <a name="migrating-service-fabric-stateless-service"></a><span data-ttu-id="3b6a0-132">Migrace bezstavové služby Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3b6a0-132">Migrating Service Fabric Stateless Service</span></span>

<span data-ttu-id="3b6a0-133">možnost toobuild toobe existující Service Fabric bezstavové Java service pomocí Service Fabric závislosti načtených z Maven, je nutné tooupdate hello ``build.gradle`` souboru uvnitř hello služby.</span><span class="sxs-lookup"><span data-stu-id="3b6a0-133">toobe able toobuild your existing Service Fabric stateless Java service using Service Fabric dependencies fetched from Maven, you need tooupdate hello ``build.gradle`` file inside hello Service.</span></span> <span data-ttu-id="3b6a0-134">Dříve takto - toobe jako použije</span><span class="sxs-lookup"><span data-stu-id="3b6a0-134">Previously it used toobe like as follows -</span></span>
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
<span data-ttu-id="3b6a0-135">Nyní, závislostí hello toofetch z Maven, hello **aktualizovat** ``build.gradle`` by měla mít odpovídající části hello takto -</span><span class="sxs-lookup"><span data-stu-id="3b6a0-135">Now, toofetch hello dependencies from Maven, hello **updated** ``build.gradle`` would have hello corresponding parts as follows -</span></span>
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
<span data-ttu-id="3b6a0-136">Obecně platí tooget celkovou představu o tom, jak sestavit hello skript bude vypadat Service Fabric bezstavové služby Java, tooany ukázka můžete odkazovat z našich úvodních příklady.</span><span class="sxs-lookup"><span data-stu-id="3b6a0-136">In general, tooget an overall idea about how hello build script would look like for a Service Fabric stateless Java service, you can refer tooany sample from our getting-started examples.</span></span> <span data-ttu-id="3b6a0-137">Tady je hello [build.gradle](https://github.com/Azure-Samples/service-fabric-java-getting-started/blob/master/Services/EchoServer/EchoServer1.0/EchoServerService/build.gradle) pro ukázku EchoServer hello.</span><span class="sxs-lookup"><span data-stu-id="3b6a0-137">Here is hello [build.gradle](https://github.com/Azure-Samples/service-fabric-java-getting-started/blob/master/Services/EchoServer/EchoServer1.0/EchoServerService/build.gradle) for hello EchoServer sample.</span></span>

## <a name="migrating-service-fabric-actor-service"></a><span data-ttu-id="3b6a0-138">Migrace Service Fabric Actor Service</span><span class="sxs-lookup"><span data-stu-id="3b6a0-138">Migrating Service Fabric Actor Service</span></span>

<span data-ttu-id="3b6a0-139">možnost toobuild toobe existující Service Fabric objektu Actor aplikace v jazyce Java pomocí Service Fabric závislosti načtených z Maven, je nutné tooupdate hello ``build.gradle`` soubor uvnitř balíčku rozhraní hello a v balíčku služby hello.</span><span class="sxs-lookup"><span data-stu-id="3b6a0-139">toobe able toobuild your existing Service Fabric Actor Java application using Service Fabric dependencies fetched from Maven, you need tooupdate hello ``build.gradle`` file inside hello interface package and in hello Service package.</span></span> <span data-ttu-id="3b6a0-140">Pokud máte TestClient balíčku, musíte to také tooupdate.</span><span class="sxs-lookup"><span data-stu-id="3b6a0-140">If you have a TestClient package, you need tooupdate that as well.</span></span> <span data-ttu-id="3b6a0-141">Ano, pro vaše objektu actor ``Myactor``, následující hello by hello míst, kde je nutné tooupdate -</span><span class="sxs-lookup"><span data-stu-id="3b6a0-141">So, for your actor ``Myactor``, hello following would be hello places where you need tooupdate -</span></span>
```
./Myactor/build.gradle
./MyactorInterface/build.gradle
./MyactorTestClient/build.gradle
```

#### <a name="updating-build-script-for-hello-interface-project"></a><span data-ttu-id="3b6a0-142">Aktualizace sestavení skript pro hello rozhraní projektu</span><span class="sxs-lookup"><span data-stu-id="3b6a0-142">Updating build script for hello interface project</span></span>

<span data-ttu-id="3b6a0-143">Dříve takto - toobe jako použije</span><span class="sxs-lookup"><span data-stu-id="3b6a0-143">Previously it used toobe like as follows -</span></span>
```
dependencies {
    compile fileTree(dir: '/opt/microsoft/sdk/servicefabric/java/packages/lib', include: ['*.jar'])
}
.
.
```
<span data-ttu-id="3b6a0-144">Nyní, závislostí hello toofetch z Maven, hello **aktualizovat** ``build.gradle`` by měla mít odpovídající části hello takto -</span><span class="sxs-lookup"><span data-stu-id="3b6a0-144">Now, toofetch hello dependencies from Maven, hello **updated** ``build.gradle`` would have hello corresponding parts as follows -</span></span>
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

#### <a name="updating-build-script-for-hello-actor-project"></a><span data-ttu-id="3b6a0-145">Aktualizace sestavení skript pro projekt hello objektu actor</span><span class="sxs-lookup"><span data-stu-id="3b6a0-145">Updating build script for hello actor project</span></span>

<span data-ttu-id="3b6a0-146">Dříve takto - toobe jako použije</span><span class="sxs-lookup"><span data-stu-id="3b6a0-146">Previously it used toobe like as follows -</span></span>
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
<span data-ttu-id="3b6a0-147">Nyní, závislostí hello toofetch z Maven, hello **aktualizovat** ``build.gradle`` by měla mít odpovídající části hello takto -</span><span class="sxs-lookup"><span data-stu-id="3b6a0-147">Now, toofetch hello dependencies from Maven, hello **updated** ``build.gradle`` would have hello corresponding parts as follows -</span></span>
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

#### <a name="updating-build-script-for-hello-test-client-project"></a><span data-ttu-id="3b6a0-148">Aktualizace sestavení skript pro hello testovacího klienta projektu</span><span class="sxs-lookup"><span data-stu-id="3b6a0-148">Updating build script for hello test client project</span></span>

<span data-ttu-id="3b6a0-149">Změny tady jsou podobné toohello změny popsané v předchozí části, tedy hello objektu actor projektu.</span><span class="sxs-lookup"><span data-stu-id="3b6a0-149">Changes here are similar toohello changes discussed in previous section, that is, hello actor project.</span></span> <span data-ttu-id="3b6a0-150">Dříve hello Gradle toobe skriptu použít jako takto-</span><span class="sxs-lookup"><span data-stu-id="3b6a0-150">Previously hello Gradle script used toobe like as follows -</span></span>
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
<span data-ttu-id="3b6a0-151">Nyní, závislostí hello toofetch z Maven, hello **aktualizovat** ``build.gradle`` by měla mít odpovídající části hello takto -</span><span class="sxs-lookup"><span data-stu-id="3b6a0-151">Now, toofetch hello dependencies from Maven, hello **updated** ``build.gradle`` would have hello corresponding parts as follows -</span></span>
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

## <a name="next-steps"></a><span data-ttu-id="3b6a0-152">Další kroky</span><span class="sxs-lookup"><span data-stu-id="3b6a0-152">Next steps</span></span>

* [<span data-ttu-id="3b6a0-153">Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí Yeomana</span><span class="sxs-lookup"><span data-stu-id="3b6a0-153">Create and deploy your first Service Fabric Java application on Linux by using Yeoman</span></span>](service-fabric-create-your-first-linux-application-with-java.md)
* [<span data-ttu-id="3b6a0-154">Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí modulu plug-in Service Fabric pro Eclipse</span><span class="sxs-lookup"><span data-stu-id="3b6a0-154">Create and deploy your first Service Fabric Java application on Linux by using Service Fabric Plugin for Eclipse</span></span>](service-fabric-get-started-eclipse.md)
* [<span data-ttu-id="3b6a0-155">Komunikovat s clusterů Service Fabric pomocí hello Service Fabric rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="3b6a0-155">Interact with Service Fabric clusters using hello Service Fabric CLI</span></span>](service-fabric-cli.md)
