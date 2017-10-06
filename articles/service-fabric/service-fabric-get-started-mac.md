---
title: "aaaSet vývojového prostředí v systému Mac OS X toowork s Azure Service Fabric | Microsoft Docs"
description: "Nainstalujte hello runtime, sadu SDK a nástroje a vytvořte místní vývojový cluster. Po dokončení této instalace, bude připravená toobuild aplikace v systému Mac OS X."
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
ms.date: 08/21/2017
ms.author: saysa
ms.openlocfilehash: 0b8a6c1fc1871fa76f3e21cefbc7f66f79072797
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-your-development-environment-on-mac-os-x"></a>Nastavení vývojového prostředí v Mac OS X
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

Můžete vytvořit toorun aplikace Service Fabric na clusterech Linux pomocí Mac OS X. Tento článek popisuje jak tooset si svůj počítač Mac pro vývoj.

## <a name="prerequisites"></a>Požadavky
Service Fabric nefunguje nativně na OS X. toorun místní cluster Service Fabric, poskytujeme předem nakonfigurovaná virtuálního počítače s Ubuntu pomocí Vagrant a VirtualBox. Než začnete, budete potřebovat:

* [Vagrant (verzi 1.8.4 nebo novější)](http://www.vagrantup.com/downloads.html)
* [VirtualBox](http://www.virtualbox.org/wiki/Downloads)

>[!NOTE]
> Je nutné toouse vzájemně podporované verze Vagrant a VirtualBox. Vagrant se může chovat chybně v nepodporované verzi VirtualBox.
>

## <a name="create-hello-local-vm"></a>Vytvoření hello Virtuálního počítače
toocreate hello obsahující cluster Service Fabric 5 uzlu Virtuálního počítače, proveďte následující kroky hello:

1. Klon hello `Vagrantfile` úložišti

    ```bash
    git clone https://github.com/azure/service-fabric-linux-vagrant-onebox.git
    ```
    Tento postup přineste seznamy hello soubor `Vagrantfile` obsahující hello virtuálních počítačů konfigurace společně s hello hello umístění virtuálních počítačů se stáhne ze.

2. Přejděte toohello místní klon hello úložišti

    ```bash
    cd service-fabric-linux-vagrant-onebox
    ```
3. (Volitelné) Úprava nastavení virtuálního počítače výchozí hello

    Ve výchozím nastavení, hello místní virtuální počítač je nakonfigurovaný následujícím způsobem:

   * 3 GB přidělené paměti
   * Síť privátní hostitele nakonfigurované na IP Adrese 192.168.50.50 povolení průchodu provozu z hostitele Mac hello

     Můžete buď z těchto nastavení změnit nebo přidat další toohello konfigurace virtuálních počítačů v hello `Vagrantfile`. V tématu hello [Vagrant dokumentace](http://www.vagrantup.com/docs) hello úplný seznam možností konfigurace.
4. Vytvoření hello virtuálních počítačů

    ```bash
    vagrant up
    ```

   Tento krok stáhne hello předkonfigurované image virtuálního počítače, spuštění, které ho místně a pak nastavte místní Service Fabric clusteru v ní. Byste měli očekávat ho tootake několik minut. Pokud se instalační program úspěšně dokončí, zobrazí se zpráva ve výstupu hello oznamující, že je tento cluster hello spuštění.

    ![Spouštění instalace clusteru po zřízení virtuálního počítače][cluster-setup-script]

    >[!TIP]
    > Pokud stahování virtuálních počítačů hello trvá dlouhou dobu, si můžete stáhnout pomocí wget nebo adresu curl nebo prostřednictvím prohlížeče tak, že přejdete toohello odkaz určeného **config.vm.box_url** v souboru hello `Vagrantfile`. Po stažení místně, upravit `Vagrantfile` toopoint toohello místní cestu, kam jste stáhli hello image. Například pokud jste stáhli hello too/home/users/test/azureservicefabric.tp8.box bitové kopie, pak nastavený **config.vm.box_url** toothat cesta.
    >

5. Testování této hello clusteru má správně nastaven tak, že přejdete tooService Fabric Explorer na http://192.168.50.50:19080/Explorer (za předpokladu, že jste u hello výchozí privátní síť IP).

    ![Zobrazit z hostitele hello Mac Service Fabric Exploreru][sfx-mac]


## <a name="create-application-on-mac-using-yeoman"></a>Vytvoření aplikace na počítači Mac pomocí Yeomanu
Service Fabric nabízí nástroje pro generování uživatelského rozhraní, které vám pomůžou vytvořit aplikaci Service Fabric z terminálu pomocí generátoru šablon Yeoman. Postupujte podle kroků hello tooensure máte hello Service Fabric yeoman šablony generátor pracující na váš počítač.

1. Potřebujete toohave Node.js a NPM, nainstalovaná v systému mac je. Pokud není můžete nainstalovat Node.js a NPM pomocí Homebrew pomocí následující hello. toocheck hello verze Node.js a NPM nainstalovaná na počítači Mac, můžete použít hello ``-v`` možnost.

  ```bash
  brew install node
  node -v
  npm -v
  ```
2. Instalace generátoru šablon [Yeoman](http://yeoman.io/) na počítač z NPM

  ```bash
  npm install -g yo
  ```
3. Nainstalujte hello Yeoman generátor chcete toouse, proveďte kroky hello v hello Začínáme [dokumentaci](service-fabric-get-started-linux.md). toocreate Service Fabric aplikací pomocí Yeoman, postupujte podle kroků hello-

  ```bash
  npm install -g generator-azuresfjava       # for Service Fabric Java Applications
  npm install -g generator-azuresfguest      # for Service Fabric Guest executables
  npm install -g generator-azuresfcontainer  # for Service Fabric Container Applications
  ```
4. toobuild aplikace Service Fabric Java v systému Mac, potřebovali byste - JDK 1.8 a nainstalovat na počítač hello Gradle.


## <a name="install-hello-service-fabric-plugin-for-eclipse-neon"></a>Instalace modulu plug-in hello Service Fabric pro Neónová Eclipse

Service Fabric nabízí o modul plug-in pro hello **Eclipse Neónová pro Java IDE** , může zjednodušit proces vytváření, sestavování a nasazení služeb Java hello. Můžete provést kroky pro instalaci hello uvedených v této Obecné [dokumentace](service-fabric-get-started-eclipse.md#install-or-update-the-service-fabric-plug-in-in-eclipse-neon) o instalaci nebo aktualizaci modulů plug-in služby Fabric Eclipse.

>[!TIP]
> Ve výchozím nastavení podporujeme hello výchozí IP jak je uvedeno v hello ``Vagrantfile`` v hello ``Local.json`` hello generované aplikace. V případě, že změnit a nasadíte Vagrant s jinou IP Adresou, aktualizujte prosím odpovídající IP hello v ``Local.json`` také vaší aplikace.

## <a name="next-steps"></a>Další kroky
<!-- Links -->
* [Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí Yeomana](service-fabric-create-your-first-linux-application-with-java.md)
* [Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí modulu plug-in Service Fabric pro Eclipse](service-fabric-get-started-eclipse.md)
* [Vytvoření clusteru Service Fabric v hello portálu Azure](service-fabric-cluster-creation-via-portal.md)
* [Vytvořit cluster Service Fabric pomocí hello Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
* [Pochopení modelu aplikace Service Fabric hello](service-fabric-application-model.md)

<!-- Images -->
[cluster-setup-script]: ./media/service-fabric-get-started-mac/cluster-setup-mac.png
[sfx-mac]: ./media/service-fabric-get-started-mac/sfx-mac.png
[sf-eclipse-plugin-install]: ./media/service-fabric-get-started-mac/sf-eclipse-plugin-install.png
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship
