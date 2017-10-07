---
title: "aaaCreate první aplikace Azure mikroslužeb v systému Linux pomocí jazyka C# | Microsoft Docs"
description: "Vytvoření a nasazení aplikace Service Fabric pomocí jazyka C#"
services: service-fabric
documentationcenter: csharp
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 5a96d21d-fa4a-4dc2-abe8-a830a3482fb1
ms.service: service-fabric
ms.devlang: csharp
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/21/2017
ms.author: subramar
ms.openlocfilehash: 68d685e130be338ebcdb2f1af24b66d1e14f580a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-azure-service-fabric-application"></a>Vytvoření první aplikace Azure Service Fabric
> [!div class="op_single_selector"]
> * [C# – Windows](service-fabric-create-your-first-application-in-visual-studio.md)
> * [Java – Linux](service-fabric-create-your-first-linux-application-with-java.md)
> * [C# – Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
>
>

Service Fabric poskytuje sady SDK pro vytváření služeb v Linuxu pomocí .NET Core a Javy. V tomto kurzu se podíváme na to, jak toocreate aplikace pro systémy Linux a sestavení služby pomocí jazyka C# (.NET Core).

## <a name="prerequisites"></a>Požadavky
Než začnete, ujistěte se, že máte [v Linuxu nastavené vývojové prostředí](service-fabric-get-started-linux.md). Pokud používáte Mac OS X, můžete k [nastavení linuxového prostředí ve virtuálním počítači použít Vagrant](service-fabric-get-started-mac.md).

Můžete také tooinstall hello [Service Fabric rozhraní příkazového řádku](service-fabric-cli.md)

### <a name="install-and-set-up-hello-generators-for-csharp"></a>Instalace a nastavení hello generátory pro CSharp
Service Fabric nabízí nástroje pro generování uživatelského rozhraní, které vám pomůžou vytvořit aplikaci Service Fabric CSharp z terminálu pomocí generátoru šablon Yeoman. Postupujte podle kroků hello tooensure máte hello Service Fabric yeoman šablony generátor pro CSharp pracující na váš počítač.
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
  sudo npm install -g generator-azuresfcsharp
  ```

## <a name="create-hello-application"></a>Vytvoření aplikace hello
Aplikace Service Fabric může obsahovat jednu nebo více služeb, každý s určitou roli při poskytování funkcí aplikace hello. Hello Service Fabric [Yeoman](http://yeoman.io/) generátor pro CSharp, který jste nainstalovali v posledním kroku, umožňuje snadno toocreate první služby a tooadd více později. Jedinou službou použijeme Yeoman toocreate aplikace.

1. V terminálu zadejte následující příkaz toostart vytváření generování uživatelského rozhraní hello hello:`yo azuresfcsharp`
2. Pojmenujte svoji aplikaci.
3. Vyberte typ hello vaší první služby a pojmenujte ho. Pro účely hello tohoto kurzu vybereme možnost spolehlivé služby objektu Actor.

   ![Generátor Service Fabric Yeoman pro jazyk C#][sf-yeoman]

> [!NOTE]
> Další informace o možnostech hello najdete v tématu [Service Fabric přehled modelu programování](service-fabric-choose-framework.md).
>
>

## <a name="build-hello-application"></a>Vytvoření aplikace hello
šablony služby Fabric Yeoman Hello zahrňte skript sestavení, které můžete použít aplikace hello toobuild z terminálu hello (po přechodu složce toohello aplikace).

  ```sh
 cd myapp
 ./build.sh
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

Po nasazení aplikace hello, otevřete prohlížeč a přejděte do [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md) v [http://localhost: 19080/Explorer](http://localhost:19080/Explorer). Potom rozbalte hello **aplikace** uzel a Všimněte si, že nyní položka pro váš typ aplikace a druhý pro hello první instance tohoto typu.

## <a name="start-hello-test-client-and-perform-a-failover"></a>Spustit hello testovacího klienta a provést převzetí služeb při selhání
Projekty Actor samy o sobě nedělají nic. Vyžadují jiná služba nebo klienta toosend je zprávy. Šablona objektu actor Hello obsahuje jednoduchá testovací skriptu, které můžete použít toointeract službou objektu actor hello.

1. Spusťte skript hello pomocí výstup hello hello sledovat nástroj toosee služby objektu actor hello.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```
2. V Service Fabric Exploreru najděte uzel, který je hostitelem primární repliky hello služby objektu actor hello. Na snímku obrazovky hello níže je uzel 3.

    ![Hledání hello primární repliky v Service Fabric Exploreru][sfx-primary]
3. Klikněte na uzel hello v předchozím kroku hello najít a pak vyberte **deaktivovat (restartovat)** z nabídky akce hello. Tato akce restartuje jeden uzel v místním clusteru vynucení převzetí služeb při selhání tooa sekundární repliky spuštěna na jiném uzlu. Jak provedete tuto akci, věnujte pozornost toohello výstup z hello testovacího klienta a Všimněte si, že tento čítač hello pokračuje tooincrement navzdory hello převzetí služeb při selhání.

## <a name="adding-more-services-tooan-existing-application"></a>Přidání další služby tooan existující aplikace

tooadd jiná tooan aplikace služby již vytvořené pomocí `yo`, proveďte následující kroky hello:
1. Změnit kořenový adresář toohello hello existující aplikace.  Například `cd ~/YeomanSamples/MyApplication`, pokud `MyApplication` je vytvořený Yeoman aplikace hello.
2. Spusťte `yo azuresfcsharp:AddService`.

## <a name="migrating-from-projectjson-toocsproj"></a>Migrace z project.json too.csproj
1. Spuštění 'dotnet migraci"v kořenovém adresáři projektu bude migrovat všechny hello project.json toocsproj formátu.
2. Aktualizace hello projekt odkazuje na odpovídajícím způsobem toocsproj soubory v souborech projektu.
3. Aktualizujte hello toocsproj soubory názvy souborů projektu v build.sh.

## <a name="next-steps"></a>Další kroky

* [Další informace o Reliable Actors](service-fabric-reliable-actors-introduction.md)
* [Interakci s clusterů Service Fabric pomocí hello Service Fabric rozhraní příkazového řádku](service-fabric-cli.md)
* Informace o [možnostech podpory pro Service Fabric](service-fabric-support.md)
* [Začínáme se Service Fabric CLI](service-fabric-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-csharp/yeoman-csharp.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-csharp/sfx-primary.png
