---
title: "aaaSet vývojového prostředí pro Azure mikroslužeb | Microsoft Docs"
description: "Nainstalujte hello runtime, sadu SDK a nástroje a vytvořte místní vývojový cluster. Po dokončení této instalace, bude připravená toobuild aplikace."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: b94e2d2e-435c-474a-ae34-4adecd0e6f8f
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/10/2017
ms.author: ryanwi, mikhegn
ms.openlocfilehash: 9b0442778999d4c3d2b99adb98f6596dcbdc36d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-your-development-environment"></a>Příprava vývojového prostředí
> [!div class="op_single_selector"]
> * [Windows](service-fabric-get-started.md) 
> * [Linux](service-fabric-get-started-linux.md)
> * [OSX](service-fabric-get-started-mac.md)
> 
> 

 toobuild a spusťte [aplikace Azure Service Fabric] [ 1] na vývojovém počítači, nainstalujte hello runtime, sadu SDK a nástroje. Budete také potřebovat tooenable spouštění skriptů prostředí Windows PowerShell hello součástí hello SDK.

## <a name="prerequisites"></a>Požadavky
### <a name="supported-operating-system-versions"></a>Podporované verze operačních systémů
pro vývoj jsou podporovány následující verze operačního systému Hello:

* Windows 7
* Windows 8 / Windows 8.1
* Windows Server 2012 R2
* Windows Server 2016
* Windows 10

> [!NOTE]
> Windows 7 ve výchozím nastavení obsahuje jenom prostředí Windows PowerShell 2.0 Rutiny prostředí PowerShell pro Service Fabric vyžadují PowerShell 3.0 nebo novější. Můžete [stáhnout prostředí Windows PowerShell 5.0] [ powershell5-download] z hello Microsoft Download Center.
> 
> 

## <a name="install-hello-sdk-and-tools"></a>Instalace hello SDK a nástrojů
### <a name="toouse-visual-studio-2017"></a>toouse Visual Studio 2017
Nástroje pro služby prostředků infrastruktury jsou součástí hello Azure vývoj a správu úloh ve Visual Studio 2017. Povolte tuto úlohu jako součást instalace sady Visual Studio.
Kromě toho musíte tooinstall hello Microsoft Azure Service Fabric SDK, pomocí instalačního programu webové platformy.

* [Nainstalujte hello Microsoft Azure Service Fabric SDK][core-sdk]

### <a name="toouse-visual-studio-2015-requires-visual-studio-2015-update-2-or-later"></a>toouse Visual Studio 2015 (vyžaduje Visual Studio 2015 Update 2 nebo novější)
Pro Visual Studio 2015 jsou nainstalované nástroje Service Fabric společně s hello SDK, pomocí hello instalačního programu webové platformy:

* [Instalace hello Microsoft Azure Service Fabric SDK a nástrojů][full-bundle-vs2015]

### <a name="sdk-installation-only"></a>Jenom instalace sady SDK
Pokud potřebujete jenom hello SDK, můžete instalaci tohoto balíčku:
* [Nainstalujte hello Microsoft Azure Service Fabric SDK][core-sdk]

Hello aktuální verze jsou:
* Sada Service Fabric SDK 2.7.198
* Modul runtime Service Fabric 5.7.198
* Service Fabric Tools for Visual Studio 2015 1.7.50721
* Visual Studio 2017 s aktualizací Update 2 obsahuje Service Fabric Tools for Visual Studio 1.6.20170504
* Visual Studio 2017 s aktualizací Update 3 ve verzi Preview 7 (15.3.0 Preview 7.0) obsahuje Service Fabric Tools for Visual Studio 1.7.20170721

Seznam podporovaných verzí najdete v tématu [Podpora pro Service Fabric](service-fabric-support.md)

## <a name="enable-powershell-script-execution"></a>Povolení spouštění skriptů prostředí PowerShell
Platforma Service Fabric používá skripty prostředí Windows PowerShell k vytvoření místního vývojového clusteru a k nasazení aplikací ze sady Visual Studio. Systém Windows ve výchozím nastavení spouštění těchto skriptů blokuje. tooenable je, je třeba upravit zásady spouštění prostředí PowerShell. Otevřete PowerShell jako správce a zadejte následující příkaz hello:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a>Další kroky
Teď, když jste dokončili nastavení vývojového prostředí, můžete začít sestavovat a spouštět aplikace.

* [Vytvořte první aplikaci Service Fabric v sadě Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md)
* [Zjistěte, jak toodeploy a spravovat aplikace v místním clusteru](service-fabric-get-started-with-a-local-cluster.md)
* [Další informace o hello programovací modely: Reliable Services a Reliable Actors](service-fabric-choose-framework.md)
* [Podívejte se na hello Service Fabric ukázky kódu na Githubu](https://aka.ms/servicefabricsamples)
* [Vizualizujte cluster pomocí Service Fabric Exploreru](service-fabric-visualizing-your-cluster.md)
* [Postupujte podle hello Service Fabric učení cesta tooget platformu toohello obecný úvod](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)
* Informace o [možnostech podpory pro Service Fabric](service-fabric-support.md)

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Stránka kampaně Service Fabric"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VS RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "Odkaz na VS 2015 WebPI"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Odkaz na Dev15 WebPI"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Odkaz na Core SDK WebPI"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
