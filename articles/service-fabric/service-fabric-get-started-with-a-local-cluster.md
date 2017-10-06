---
title: "aaaDeploy a upgradujte místně Azure mikroslužeb | Microsoft Docs"
description: "Zjistěte, jak nasadit existující aplikaci tooit tooset si místní cluster Service Fabric a potom tuto aplikaci upgradujte."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 60a1f6a5-5478-46c0-80a8-18fe62da17a8
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/13/2017
ms.author: ryanwi;mikhegn
ms.openlocfilehash: e5f5adc9edb71433b2a7635e9d661ff92a4b18ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-deploying-and-upgrading-applications-on-your-local-cluster"></a>Začínáme s nasazením a upgradem aplikací v místním clusteru
Hello Azure Service Fabric SDK zahrnuje úplné místní vývojové prostředí, který můžete použít tooquickly Začínáme s nasazením a správou aplikací v místním clusteru. V tomto článku vytvoříte místní cluster, nasazení tooit existující aplikaci a pak upgradovat aplikaci tooa nové verze, všechny z prostředí Windows PowerShell.

> [!NOTE]
> Tento článek vychází z předpokladu, že už máte [nastavené vývojové prostředí](service-fabric-get-started.md).
> 
> 

## <a name="create-a-local-cluster"></a>Vytvoření místního clusteru
Cluster Service Fabric tvoří sada hardwarových prostředků, na které můžete nasazovat aplikace. Obvykle clusteru je tvořen kdekoli z pěti toomany tisíc počítačů. Hello Service Fabric SDK ale obsahuje konfiguraci clusteru, můžete spustit na jednom počítači.

Je důležité, že toounderstand, který hello místní cluster Service Fabric není emulátor ani simulátor. Ji spustí hello stejný kód platformy, která se nachází na víc počítačů clustery. Hello jediným rozdílem je, že běží procesy platformy hello, které jsou normálně rozložené mezi pěti počítačů na jednom počítači.

Hello SDK nabízí dva způsoby tooset si místní cluster: prostředí Windows PowerShell skriptu a hello Local Cluster Manager systému aplikaci na hlavním panelu. V tomto kurzu používáme skript prostředí PowerShell hello.

> [!NOTE]
> Pokud jste už nasazením aplikace z Visual Studia vytvořili místní cluster, můžete tuto část přeskočit.
> 
> 

1. Jako správce spusťte nové okno prostředí PowerShell.
2. Spusťte instalační skript clusteru hello ze složky sady SDK hello:
   
    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"
    ```
   
    Instalace clusteru bude chvíli trvat. Po dokončení instalace byste měli vidět výstup podobný tomuhle:
   
    ![Výstup po instalaci clusteru][cluster-setup-success]
   
    Nyní je připraven tootry nasazení clusteru služby tooyour aplikace.

## <a name="deploy-an-application"></a>Nasazení aplikace
Hello Service Fabric SDK zahrnuje celou řadu architektur a vývojářských nástrojů pro vytváření aplikací. Pokud vás zajímá dozvědět, jak zjistit, toocreate aplikací v sadě Visual Studio, [vytvoření první aplikace Service Fabric v sadě Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).

V tomto kurzu budete používat existující ukázkovou aplikaci (s názvem WordCount) tak, aby se mohli zaměřit na aspekty správy hello hello platformy: nasazení, monitorování a upgradu.

1. Jako správce spusťte nové okno prostředí PowerShell.
2. Importujte modul Service Fabric SDK PowerShell hello.
   
    ```powershell
    Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
    ```
3. Vytvořte aplikaci hello toostore adresáře, která můžete stáhnout a nasadit, třeba C:\ServiceFabric.
   
    ```powershell
    mkdir c:\ServiceFabric\
    cd c:\ServiceFabric\
    ```
4. [Stáhněte aplikaci WordCount hello](http://aka.ms/servicefabric-wordcountapp) toohello umístění, které jste vytvořili.  Poznámka: prohlížeč Microsoft Edge hello uloží soubor hello s *.zip* rozšíření.  Změnit příponu souboru hello příliš*.sfpkg*.
5. Připojte toohello místní cluster:
   
    ```powershell
    Connect-ServiceFabricCluster localhost:19000
    ```
6. Vytvoření nové aplikace pomocí příkazu hello SDK nasazení s názvem a balíček aplikace toohello cesta.
   
    ```powershell  
   Publish-NewServiceFabricApplication -ApplicationPackagePath c:\ServiceFabric\WordCountV1.sfpkg -ApplicationName "fabric:/WordCount"
    ```
   
    Pokud všechno proběhne správně, měli byste vidět hello následující výstup:
   
    ![Nasazení místního clusteru toohello aplikace][deploy-app-to-local-cluster]
7. toosee hello aplikaci v akci, spusťte hello prohlížeče a přejděte příliš[http://localhost: 8081/WordCount/index.HTML](http://localhost:8081/wordcount/index.html). Měli byste vidět tohle:
   
    ![Uživatelské rozhraní nasazené aplikace][deployed-app-ui]
   
    Hello aplikace WordCount je jednoduché. Obsahuje klienta JavaScript kód toogenerate náhodných pěti znacích "slova", které jsou pak přes předávací službu toohello aplikace přes webové rozhraní API ASP.NET. Stavové služby sleduje hello počtu slov počítají. Dělí do oddílů podle hello první znak hello slov. Můžete najít hello zdrojového kódu pro aplikaci WordCount hello v hello [classic Začínáme ukázky](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount).
   
    Hello aplikace, kterou jsme nasadili obsahuje čtyři oddíly. Takže slova začínající písmeny A až G se ukládají do hello prvního oddílu, slova začínající písmeny H až N jsou uloženy v hello druhý oddíl a tak dále.

## <a name="view-application-details-and-status"></a>Zobrazení podrobností a stavu aplikace
Teď, když jsme nasadili aplikaci hello, podíváme se na některé podrobnosti o aplikaci hello v prostředí PowerShell.

1. Dotaz na všechny nasazené aplikace v clusteru hello:
   
    ```powershell
    Get-ServiceFabricApplication
    ```
   
    Za předpokladu, že jste nasadili jenom aplikaci WordCount hello, zobrazí se o něco podobného jako:
   
    ![Dotaz na všechny nasazené aplikace v prostředí PowerShell][ps-getsfapp]
2. Přejděte další úroveň toohello dotazováním hello sadu služeb, které jsou součástí aplikace WordCount hello.
   
    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```
   
    ![Výpis seznamu služeb aplikace hello v prostředí PowerShell][ps-getsfsvc]
   
    Hello aplikace se skládá z dvě služby, hello webového uživatelského rozhraní a hello stavové služby, která spravuje slova hello.
3. Nakonec podívejte se na hello seznam oddílů služby WordCountService:
   
    ```powershell
    Get-ServiceFabricPartition 'fabric:/WordCount/WordCountService'
    ```
   
    ![Zobrazení oddílů služby hello v prostředí PowerShell][ps-getsfpartitions]
   
    Dobrý den sadu příkazů, které jste použili, stejně jako všechny příkazy Service Fabric prostředí PowerShell, jsou k dispozici pro všechny clusteru, který jste se může připojit k místní nebo vzdálené.
   
    Pro víc vizuální způsob toointeract s hello clusteru, můžete použít hello webový nástroj Service Fabric Explorer tak, že přejdete příliš[http://localhost: 19080/Explorer](http://localhost:19080/Explorer) v prohlížeči hello.
   
    ![Zobrazení podrobností aplikace v Service Fabric Exploreru][sfx-service-overview]
   
   > [!NOTE]
   > toolearn Další informace o Service Fabric Explorer najdete v části [vizualizace vašeho clusteru pomocí Service Fabric Exploreru](service-fabric-visualizing-your-cluster.md).
   > 
   > 

## <a name="upgrade-an-application"></a>Upgrade aplikace
Service Fabric nabízí upgrady bez výpadků díky monitorování stavu hello aplikace hello během zavádění v clusteru hello. Proveďte upgrade aplikace WordCount hello.

Hello nové verze aplikace hello teď počty jenom slova, která začínají samohláskou. Jako hello upgradu vidíme dvě změny v chování aplikace hello. Nejdřív by se měla zpomalit hello rychlost, jakou hello Zaprvé, protože se počítá míň slov. Druhý protože hello první oddíl obsahuje dvě samohlásky (A a E) a všechny ostatní oddíly jenom po jedné, jeho počet nakonec začněte toooutpace hello ostatní.

1. [Stáhněte balíček WordCount verze 2 hello](http://aka.ms/servicefabric-wordcountappv2) toohello stejné umístění, kam jste stáhli balíček verze 1 hello.
2. Vrátí tooyour okno prostředí PowerShell a použít příkaz pro upgrade hello SDK tooregister hello novou verzi v clusteru hello. Pak začněte upgradovat hello fabric: / WordCount aplikace.
   
    ```powershell
    Publish-UpgradedServiceFabricApplication -ApplicationPackagePath C:\ServiceFabric\WordCountV2.sfpkg -ApplicationName "fabric:/WordCount" -UpgradeParameters @{"FailureAction"="Rollback"; "UpgradeReplicaSetCheckTimeout"=1; "Monitored"=$true; "Force"=$true}
    ```
   
    Měli byste vidět, že začne hello následující výstup v prostředí PowerShell jako hello upgradu.
   
    ![Průběh upgradu v prostředí PowerShell][ps-appupgradeprogress]
3. Při upgradu hello možná bude snazší toomonitor jeho stav v Service Fabric Exploreru. Spusťte okno prohlížeče a přejděte příliš[http://localhost: 19080/Explorer](http://localhost:19080/Explorer). Rozbalte položku **aplikace** ve stromu hello na levé straně hello zvolte **WordCount**a v neposlední řadě **fabric: / WordCount**. Na kartě hello essentials je zobrazena hello stav upgradu hello pokračuje prostřednictvím domén upgradu hello clusteru.
   
    ![Průběh upgradu v Service Fabric Exploreru][sfx-upgradeprogress]
   
    Hello upgradu v každé doméně, jsou kontroly stavu provádí tooensure, který aplikace hello nepracuje správně.
4. Pokud hello dříve dotazu hello sadu služeb v hello fabric: / WordCount aplikace, Všimněte si, že hello změnit verze WordCountService, ale verze WordCountWebService hello nebyla:
   
    ```powershell
    Get-ServiceFabricService -ApplicationName 'fabric:/WordCount'
    ```
   
    ![Dotaz na aplikační služby po upgradu][ps-getsfsvc-postupgrade]
   
    Tento příklad ukazuje, jak Service Fabric spravuje upgrady aplikací. Dotýká se jenom hello sada služeb (nebo balíčků kódu/konfigurace v těchto službách) změnily, což může proces hello upgrade rychlejší a spolehlivější.
5. Nakonec se vraťte toohello prohlížeče tooobserve hello chování nové verze aplikace hello. Podle očekávání, hello počet postupuje pomaleji a hello první oddíl nakonec získá o něco větší hello svazku.
   
    ![Zobrazení hello nové verze aplikace hello v prohlížeči hello][deployed-app-ui-v2]

## <a name="cleaning-up"></a>Čištění
Před zabalení je důležité, že jsou skutečná tooremember, který hello místní cluster. Aplikace pokračovat toorun hello pozadí, dokud je odebrat.  V závislosti na povaze vašich aplikací hello spuštěné aplikaci může spotřebovávat značné množství prostředků na počítači. Máte několik možností toomanage aplikace a hello clusteru:

1. tooremove jednotlivou aplikaci a všechny jeho je daty spusťte následující příkaz hello:
   
    ```powershell
    Unpublish-ServiceFabricApplication -ApplicationName "fabric:/WordCount"
    ```
   
    Nebo odstranění aplikace hello z hello Service Fabric Explorer **akce** nabídky nebo hello místní nabídky v zobrazení seznamu levé aplikace hello.
   
    ![Odstranění aplikace v Service Fabric Exploreru][sfe-delete-application]
2. Po odstranění aplikace hello z hello clusteru, zrušit registraci verze 1.0.0 a 2.0.0 hello typ aplikace WordCount. Odstranění odebere hello balíčky aplikací, včetně hello kódu a konfigurace, z úložiště bitových kopií hello clusteru.
   
    ```powershell
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 2.0.0
    Remove-ServiceFabricApplicationType -ApplicationTypeName WordCount -ApplicationTypeVersion 1.0.0
    ```
   
    Nebo v Service Fabric Exploreru zvolte **Unprovision typ** aplikace hello.
3. Klikněte na tlačítko tooshut dolů hello clusteru, ale data aplikací hello zachovat a trasování, **Stop Local Cluster** v aplikaci na hlavním panelu systému hello.
4. zcela, klikněte na tlačítko toodelete hello clusteru **odebrat místní Cluster** v aplikaci na hlavním panelu systému hello. Tato možnost způsobí jiný hello pomalé nasazení příštím stisknutí klávesy F5 ve Visual Studio. Odeberte místní cluster hello pouze v případě, že nechystáte toouse, je po určitý čas nebo pokud potřebujete tooreclaim prostředky.

## <a name="one-node-and-five-node-cluster-mode"></a>Režim clusteru s jedním uzlem a s pěti uzly
Při vývoji aplikací často potřebujete rychle střídat psaní kódu, ladění, změny kódu a ladění. toohelp optimalizovat tento proces, hello místní cluster může běžet ve dvou režimech: jedním uzlem nebo pěti uzlů. Oba režimy mají své výhody. Režim pěti uzly clusteru umožňuje toowork s clusterem s podporou skutečné. Můžete testovat scénáře převzetí služeb při selhání, pracovat s více instancemi a replikami služeb. Režim jednouzlového clusteru je optimalizované toodo rychlé nasazení a registrace služby, toohelp rychle ověření kódu pomocí modulu runtime Service Fabric hello.

Cluster s jedním uzlem ani cluster s pěti uzly není emulátor ani simulátor. Hello místního vývojového clusteru spustí hello stejný kód platformy, která se nachází na víc počítačů clustery.

> [!WARNING]
> Při změně režimu clusteru hello aktuální cluster hello je odebrána z vašeho systému a vytvoření nového clusteru. Hello data uložená v clusteru hello je odstraněna, když změníte režim clusteru.
> 
> 

toochange hello režimu tooone uzlu clusteru, vyberte **přepínače clusteru režimu** v hello Service Fabric Local Cluster Manager.

![Přepnutí režimu clusteru][switch-cluster-mode]

Nebo změnit režim hello clusteru pomocí prostředí PowerShell:

1. Jako správce spusťte nové okno prostředí PowerShell.
2. Spusťte instalační skript clusteru hello ze složky sady SDK hello:
   
    ```powershell
    & "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1" -CreateOneNodeCluster
    ```
   
    Instalace clusteru bude chvíli trvat. Po dokončení instalace byste měli vidět výstup podobný tomuhle:
   
    ![Výstup po instalaci clusteru][cluster-setup-success-1-node]

## <a name="next-steps"></a>Další kroky
* Provedli jste nasazení a upgrade některých předem sestavených aplikací a teď si můžete zkusit [sestavit vlastní aplikaci v sadě Visual Studio](service-fabric-create-your-first-application-in-visual-studio.md).
* Všechny hello akce prováděné na místní cluster hello v tomto článku lze provést na [clusteru Azure](service-fabric-cluster-creation-via-portal.md) také.
* Hello upgradu, které jsme provedli v tomto článku se základní. V tématu hello [dokumentaci k upgradu](service-fabric-application-upgrade.md) toolearn více informací o hello výkonu a flexibilitě upgradů Service Fabric.

<!-- Images -->

[cluster-setup-success]: ./media/service-fabric-get-started-with-a-local-cluster/LocalClusterSetup.png
[extracted-app-package]: ./media/service-fabric-get-started-with-a-local-cluster/ExtractedAppPackage.png
[deploy-app-to-local-cluster]: ./media/service-fabric-get-started-with-a-local-cluster/DeployAppToLocalCluster.png
[deployed-app-ui]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-v1.png
[deployed-app-ui-v2]: ./media/service-fabric-get-started-with-a-local-cluster/DeployedAppUI-PostUpgrade.png
[sfx-app-instance]: ./media/service-fabric-get-started-with-a-local-cluster/SfxAppInstance.png
[sfx-two-app-instances-different-partitions]: ./media/service-fabric-get-started-with-a-local-cluster/SfxTwoAppInstances-DifferentPartitionCount.png
[ps-getsfapp]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFApp.png
[ps-getsfsvc]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc.png
[ps-getsfpartitions]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFPartitions.png
[ps-appupgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/PS-AppUpgradeProgress.png
[ps-getsfsvc-postupgrade]: ./media/service-fabric-get-started-with-a-local-cluster/PS-GetSFSvc-PostUpgrade.png
[sfx-upgradeprogress]: ./media/service-fabric-get-started-with-a-local-cluster/SfxUpgradeOverview.png
[sfx-service-overview]: ./media/service-fabric-get-started-with-a-local-cluster/sfx-service-overview.png
[sfe-delete-application]: ./media/service-fabric-get-started-with-a-local-cluster/sfe-delete-application.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
[switch-cluster-mode]: ./media/service-fabric-get-started-with-a-local-cluster/switch-cluster-mode.png
