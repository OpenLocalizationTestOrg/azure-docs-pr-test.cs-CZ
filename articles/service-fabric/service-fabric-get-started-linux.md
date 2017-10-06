---
title: "aaaSet vývojového prostředí v systému Linux | Microsoft Docs"
description: "Nainstalujte modul runtime hello a sady SDK a vytvoření místního vývojového clusteru v systému Linux. Po dokončení této instalace, bude připravená toobuild aplikace."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/23/2017
ms.author: subramar
ms.openlocfilehash: 9d82c2015f9e2c6fb55f2052c7cdb1e906c5deeb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-your-development-environment-on-linux"></a>Příprava vývojového prostředí v Linuxu
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md)
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
>
>  

toodeploy a spusťte [aplikace Azure Service Fabric](service-fabric-application-model.md) na vývojovém počítači Linux, nainstalujte hello runtime a společné sady SDK. Můžete také nainstalovat volitelné sady SDK pro Javu a .NET Core.

## <a name="prerequisites"></a>Požadavky

pro vývoj jsou podporovány následující verze operačního systému Hello:

* Ubuntu 16.04 (`Xenial Xerus`)

## <a name="update-your-apt-sources"></a>Aktualizace zdrojů APT
tooinstall hello SDK a hello přidružené runtime balíčku pomocí nástroje příkazového řádku hello výstižný get, nejprve je nutné aktualizovat vaše zdroje byt pokročilé nástroje balení (č).

1. Otevřete terminál.
2. Přidejte hello Service Fabric úložišti tooyour zdroje seznamu.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] http://apt-mo.trafficmanager.net/repos/servicefabric/ xenial main" > /etc/apt/sources.list.d/servicefabric.list'
    ```

3. Přidat hello `dotnet` seznamu zdrojů tooyour úložišti.

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
    ```

4. Přidat hello nové Gnu ochrana osobních údajů (GnuPG nebo GPG) klíče tooyour VÝSTIŽNÝ Správce klíčů.

    ```bash
    sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 417A0893
    ```

5. Přidejte hello oficiální Docker GPG klíče tooyour VÝSTIŽNÝ Správce klíčů.

    ```bash
    sudo apt-get install curl
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```

6. Nastavení úložiště Docker hello.

    ```bash
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    ```

7. Aktualizace vašeho balíčku seznamy založené na hello nově přidány úložiště.

    ```bash
    sudo apt-get update
    ```

## <a name="install-and-set-up-hello-sdk-for-local-cluster-setup"></a>Instalace a nastavení hello SDK pro nastavení místního clusteru

Po dokončení aktualizace vašich zdrojů, můžete nainstalovat hello SDK. Nainstalujte balíček Service Fabric SDK hello, potvrzení instalace hello a toohello licenční smlouvy souhlasím.

```bash
sudo apt-get install servicefabricsdkcommon
```

>   [!TIP]
>   Hello následující příkazy automatizovat přijímá hello licenci pro Service Fabric balíčky:
>   ```bash
>   echo "servicefabric servicefabric/accepted-eula-v1 select true" | sudo debconf-set-selections
>   echo "servicefabricsdkcommon servicefabricsdkcommon/accepted-eula-v1 select true" | sudo debconf-set-selections
>   ```

## <a name="set-up-a-local-cluster"></a>Nastavení místního clusteru
  Pokud hello instalace proběhne úspěšně, musí být schopný toostart místní cluster.

  1. Spusťte instalační skript clusteru hello.

      ```bash
      sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
      ```

  2. Otevřete webový prohlížeč a přejděte příliš[Service Fabric Explorer](http://localhost:19080/Explorer). Pokud byl zahájen hello clusteru, měli byste vidět řídicí panel hello Service Fabric Exploreru.

      ![Service Fabric Explorer v Linuxu][sfx-linux]

  V tuto chvíli můžete nasadit předem sestavené balíčky aplikací Service Fabric nebo nové balíčky založené na kontejnerech nebo spustitelných souborech hostů. toobuild nové služby pomocí hello Java nebo .NET Core SDK, postupujte podle kroků hello volitelné nastavení, které jsou uvedené v následujících částech.


  > [!NOTE]
  > Samostatné clustery nejsou podporované v systému Linux. Hello preview podporuje pouze jeden pole a clustery pro víc počítačů Azure Linux.
  >

## <a name="set-up-hello-service-fabric-cli"></a>Nastavit hello Service Fabric rozhraní příkazového řádku

Hello [Service Fabric rozhraní příkazového řádku](service-fabric-cli.md) obsahuje příkazy pro interakci s entitami Service Fabric, včetně clusterů a aplikací. Je založen na python, takže se toohave python a pip nainstalovat předtím, než budete pokračovat s hello následující příkaz:

```bash
pip install sfctl
```

## <a name="install-and-set-up-hello-generators-for-containers-and-guest-executables"></a>Instalace a nastavení hello generátory pro kontejnery a Host-spustitelné soubory
Service Fabric nabízí nástroje pro generování uživatelského rozhraní, které vám pomůžou vytvořit aplikace Service Fabric z terminálu pomocí generátoru šablon Yeoman. Postupujte podle kroků hello tooensure máte hello Service Fabric yeoman šablony generátor pro práci na váš počítač.

1. Instalace nodejs a NPM na počítači

  ```bash
  sudo apt-get install npm
  sudo apt install nodejs-legacy
  ```
2. Instalace generátoru šablon [Yeoman](http://yeoman.io/) na počítač z NPM

  ```bash
  sudo npm install -g yo
  ```
3. Instalace generátor kontejneru hello jo s háčkem nad Service Fabric a hosta execuatble generátor z NPM

  ```bash
  sudo npm install -g generator-azuresfcontainer  # for Service Fabric container application
  sudo npm install -g generator-azuresfguest      # for Service Fabric guest executable application
  ```

Po instalaci hello výše generátory, měli byste být aplikací mít toocreate s spustitelný soubor nebo kontejneru služby hosta spuštěním `yo azuresfguest` nebo `yo azuresfcontainer` v uvedeném pořadí.

## <a name="install-hello-necessary-java-artifacts-optional-if-you-want-toouse-hello-java-programming-models"></a>Nainstalujte hello nezbytné Java artefakty (volitelné tam, pokud chcete toouse hello Java programovací modely)

toobuild služeb Service Fabric pomocí Java, ujistěte se, že máte 1.8 JDK, které jsou nainstalovány společně s Gradle, který se používá pro spuštění úlohy sestavení. Následující fragment kódu Hello nainstaluje otevřete 1.8 JDK společně s Gradle. knihovny Service Fabric Java Hello jsou vyžádány od Maven.

  ```bash
  sudo apt-get install openjdk-8-jdk-headless
  sudo apt-get install gradle
  ```

## <a name="install-hello-eclipse-neon-plug-in-optional"></a>Nainstalujte hello Eclipse Neónová modulu plug-in (volitelné)

Hello Eclipse modulu plug-in pro Service Fabric z můžete nainstalovat v rámci hello **IDE Eclipse pro vývojáře v jazyce Java**. V aplikacích Fabric Java tooService přidání můžete použít Eclipse toocreate Service Fabric Host spustitelný soubor aplikace a aplikace typu kontejner.

1. V prostředí Eclipse, ujistěte se, že máte nejnovější Neónová Eclipse a hello nejnovější verzi Buildship (1.0.17 nebo novější) nainstalována. Hello verze nainstalované součásti můžete zkontrolovat výběrem **pomoci** > **podrobné informace o instalaci**. Buildship můžete aktualizovat pomocí pokynů hello [Eclipse Buildship: Eclipse moduly plug-in pro Gradle][buildship-update].

2. Vyberte modul plug-in, Service Fabric hello tooinstall **pomoci** > **instalace nového softwaru**.

3. V hello **pracovat s** zadejte **http://dl.microsoft.com/eclipse**.

4. Klikněte na tlačítko **Přidat**.

    ![stránku Hello dostupného softwaru][sf-eclipse-plugin]

5. Vyberte hello **ServiceFabric** modul plug-in a potom klikněte na **Další**.

6. Dokončete kroky instalace hello a přijměte licenční smlouvu hello.

Pokud již máte hello Service Fabric prostředí Eclipse modulu plug-in nainstalována, ujistěte se, že máte nejnovější verzi hello. Můžete zkontrolovat výběrem **pomoci** > **podrobné informace o instalaci** a pak hledání Service Fabric hello seznamu nainstalovaných modulů plug-in. Pokud je k dispozici novější verze, vyberte **Update** (Aktualizovat).

Další informace najdete v tématu [Modul plug-in Service Fabric pro vývoj aplikací v Eclipse Javě](service-fabric-get-started-eclipse.md).


## <a name="install-hello-net-core-sdk-optional-if-you-want-toouse-hello-net-core-programming-models"></a>Nainstalujte hello .NET Core SDK (volitelné tam, pokud chcete, aby toouse hello .NET Core programovací modely)
Hello .NET Core SDK poskytuje šablony, které jsou požadované toobuild Service Fabric služby s .NET Core a hello knihovny. Instalovat balíček .NET Core SDK hello ve spuštěné následující hello-

   ```bash
   sudo apt-get install servicefabricsdkcsharp
   ```

## <a name="update-hello-sdk-and-runtime"></a>Aktualizace hello SDK a prostředí runtime

tooupdate toohello nejnovější verzi hello SDK a modul runtime, spusťte následující příkazy hello (hello sady SDK, které nechcete použít, zrušte výběr):

```bash
sudo apt-get update
sudo apt-get install servicefabric servicefabricsdkcommon servicefabricsdkcsharp
```
tooupdate binární soubory hello sady Java SDK z Maven, je nutné tooupdate hello verze podrobnosti odpovídající binární hello v hello ``build.gradle`` soubor toopoint toohello nejnovější verzi. tooknow přesně, kde potřebujete tooupdate hello verzi, naleznete v části tooany ``build.gradle`` souboru v Service Fabric Začínáme ukázky [zde](https://github.com/Azure-Samples/service-fabric-java-getting-started).

> [!NOTE]
> Aktualizaci balíčků hello může způsobit vaše místního vývojového clusteru toostop systémem. Restartujte místním clusteru po upgradu podle pokynů hello na této stránce.

## <a name="next-steps"></a>Další kroky

* [Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí Yeomana](service-fabric-create-your-first-linux-application-with-java.md)
* [Vytvoření a nasazení první aplikace Service Fabric v Javě v Linuxu pomocí modulu plug-in Service Fabric pro Eclipse](service-fabric-get-started-eclipse.md)
* [Vytvoření první aplikace v CSharp v Linuxu](service-fabric-create-your-first-linux-application-with-csharp.md)
* [Příprava vývojového prostředí v OSX](service-fabric-get-started-mac.md)
* [Pomocí aplikace hello toomanage Service Fabric rozhraní příkazového řádku](service-fabric-application-lifecycle-sfctl.md)
* [Rozdíly Service Fabric pro Windows a Linux](service-fabric-linux-windows-differences.md)
* [Začínáme s rozhraním příkazového řádku Service Fabric](service-fabric-cli.md)

<!-- Links -->

[azure-xplat-cli-github]: https://github.com/Azure/azure-xplat-cli
[install-node]: https://nodejs.org/en/download/package-manager/#installing-node-js-via-package-manager
[buildship-update]: https://projects.eclipse.org/projects/tools.buildship

<!--Images -->

[sf-eclipse-plugin]: ./media/service-fabric-get-started-linux/service-fabric-eclipse-plugin.png
[sfx-linux]: ./media/service-fabric-get-started-linux/sfx-linux.png
