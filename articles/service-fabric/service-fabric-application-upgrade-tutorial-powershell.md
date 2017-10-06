---
title: "upgrade aplikace Fabric aaaService pomocí prostředí PowerShell | Microsoft Docs"
description: "Tento článek vás provede hello činnost nasazení aplikace Service Fabric, změna hello kódu a zavádění upgrade pomocí prostředí PowerShell."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: 9bc75748-96b0-49ca-8d8a-41fe08398f25
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: f31212264de45c3b257a0efafb75c10c279b989f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="service-fabric-application-upgrade-using-powershell"></a>Upgrade aplikace Service Fabric pomocí prostředí PowerShell
> [!div class="op_single_selector"]
> * [PowerShell](service-fabric-application-upgrade-tutorial-powershell.md)
> * [Visual Studio](service-fabric-application-upgrade-tutorial.md)
> 
> 

<br/>

Hello nejčastěji používá a doporučený postup upgradu postupného upgradu hello monitorovány.  Azure Service Fabric monitoruje stav hello aplikace hello upgraduje na základě sady zásad stavu. Po upgradu domény služby aktualizace (UD), Service Fabric vyhodnotí hello stavu aplikací a buď pokračuje toohello další aktualizace domény nebo hello upgradu v závislosti na zásadách stavu hello se nezdaří.

Můžete provést upgrade monitorované aplikaci pomocí hello spravované nebo nativních rozhraní API prostředí PowerShell nebo REST. Pokyny k provedení upgradu pomocí sady Visual Studio, najdete v části [upgrade vaší aplikace pomocí sady Visual Studio](service-fabric-application-upgrade-tutorial.md).

Pomocí Service Fabric monitorovat postupné upgrady lze nakonfigurovat správce aplikací hello zásad vyhodnocení stavu hello, Service Fabric používá toodetermine, pokud aplikace hello je v pořádku. Kromě toho může správce hello nakonfigurovat toobe hello akce prováděné při vyhodnocování stavu hello selže (například provádění automatického vrácení zpět.) Tato část vás provede monitorovaných upgrade pro jednu z ukázky hello SDK, které používá prostředí PowerShell. Hello následující Microsoft Virtual Academy video vás také provede upgrade aplikace:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=OrHJH66yC_6406218965">
<img src="./media/service-fabric-application-upgrade-tutorial-powershell/AppLifecycleVid.png" WIDTH="360" HEIGHT="244">
</a></center>

## <a name="step-1-build-and-deploy-hello-visual-objects-sample"></a>Krok 1: Vytvoření a nasazení ukázkové vizuální objekty hello
Sestavení a publikování aplikace hello kliknutím pravým tlačítkem na projekt aplikace hello, **VisualObjectsApplication,** a výběrem hello **publikovat** příkaz.  Další informace najdete v tématu [kurz upgradu aplikace Service Fabric](service-fabric-application-upgrade-tutorial.md).  Alternativně můžete použít PowerShell toodeploy vaší aplikace.

> [!NOTE]
> Před všechny příkazy Service Fabric hello mohou být použity v prostředí PowerShell, musíte nejdřív tooconnect toohello clusteru pomocí hello `Connect-ServiceFabricCluster` rutiny. Podobně se předpokládá, že hello clusteru již byla nastavena na místním počítači. Najdete v článku hello na [nastavení vývojového prostředí Service Fabric](service-fabric-get-started.md).
> 
> 

Po sestavení hello projektu v sadě Visual Studio, můžete použít příkaz prostředí PowerShell hello [kopie ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/copy-servicefabricapplicationpackage) toocopy hello aplikace balíčku toohello úložiště bitových kopií. Pokud chcete tooverify hello balíček aplikace místně, použijte hello [Test ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/test-servicefabricapplicationpackage) rutiny. Hello dalším krokem je tooregister hello aplikace toohello modulu runtime Service Fabric pomocí hello [Register-ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/register-servicefabricapplicationtype) rutiny. Hello posledním krokem je toostart instance aplikace hello pomocí hello [New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) rutiny.  Tyto tři kroky jsou obdobou toousing hello **nasadit** položky nabídky v sadě Visual Studio.

Teď můžete použít [aplikace Service Fabric Explorer tooview hello clusteru a hello](service-fabric-visualizing-your-cluster.md). Hello aplikace má webová služba, která může být navigaci tooin Internet Explorer tak, že zadáte [http://localhost: 8081/visualobjects](http://localhost:8081/visualobjects) v panelu Adresa hello.  Měli byste vidět některé plovoucí vizuální objekty pohyb v úvodní obrazovka.  Kromě toho můžete použít [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) toocheck stav aplikace hello.

## <a name="step-2-update-hello-visual-objects-sample"></a>Krok 2: Aktualizace Ukázka vizuální objekty hello
Můžete si všimnout, že s verzí hello, který byl nasazen v kroku 1, není otočit hello vizuální objekty. Umožňuje upgradovat tento tooone aplikace, kde také otočit hello vizuální objekty.

Vyberte projekt VisualObjects.ActorService hello v rámci hello VisualObjects řešení a otevřete soubor StatefulVisualObjectActor.cs hello. V rámci tohoto souboru, přejděte metoda toohello `MoveObject`, komentář `this.State.Move()`a zrušte komentář u `this.State.Move(true)`. Tato změna otočí hello objekty po upgradu služby hello.

Potřebujeme tooupdate hello *ServiceManifest.xml* hello projektu v souboru (pod PackageRoot) **VisualObjects.ActorService**. Aktualizace hello *CodePackage* hello too2.0 verze služby a hello odpovídající řádky v hello *ServiceManifest.xml* souboru.
Můžete použít hello Visual Studio *upravit soubory manifestu* možnost po kliknutí pravým tlačítkem na hello řešení toomake hello souboru manifestu změny.

Po provedení změn hello, hello manifest by měl vypadat jako následující hello (zvýrazněné části zobrazit změny hello):

```xml
<ServiceManifestName="VisualObjects.ActorService" Version="2.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

<CodePackageName="Code" Version="2.0">
```

Nyní hello *ApplicationManifest.xml* souboru (v části hello **VisualObjects** projektu v části hello **VisualObjects** řešení) je aktualizovaný tooversion 2.0 Dobrý den  **VisualObjects.ActorService** projektu. Kromě toho je verze aplikace hello aktualizované too2.0.0.0 z 1.0.0.0. Hello *ApplicationManifest.xml* by měl vypadají hello následující fragment kódu:

```xml
<ApplicationManifestxmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VisualObjects" ApplicationTypeVersion="2.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

 <ServiceManifestRefServiceManifestName="VisualObjects.ActorService" ServiceManifestVersion="2.0" />
```


Nyní, sestavení projektu hello výběrem jenom hello **ActorService** projekt a potom kliknete pravým tlačítkem a vyberete hello **sestavení** možnost v sadě Visual Studio. Pokud vyberete **znovu vytvořit všechny**, hello verze pro všechny projekty, by měl aktualizovat, protože by změnily hello kódu. Další, můžeme aktualizovat aplikace kliknutím pravým tlačítkem na balíček hello ***VisualObjectsApplication***, výběrem hello nabídky Service Fabric a zvolením **balíček**. Tato akce vytvoří balíček aplikace, které se dá nasadit.  Aktualizovaná aplikace je připravená toobe nasazení.

## <a name="step-3--decide-on-health-policies-and-upgrade-parameters"></a>Krok 3: Rozhodněte o zásady stavu a upgradujte parametry
Seznamte se s hello [parametry upgradu aplikace](service-fabric-application-upgrade-parameters.md) a hello [procesu upgradu](service-fabric-application-upgrade.md) tooget dobrou znalost jazyka hello různé parametry, vypršení časových limitů a stavu kritérium použít upgradu . V tomto návodu je kritéria hodnocení stavu služby hello nastavit výchozí toohello (a doporučené) hodnoty, které znamená, že by měly být všechny služby a instance *pořádku* po upgradu hello.  

Však umožňuje zvýšit hello *HealthCheckStableDuration* too60 sekund (tak, aby aspoň 20 sekund před provedením upgradu hello toohello další aktualizace domény hello služby jsou v pořádku).  Umožňuje také nastavit hello *UpgradeDomainTimeout* toobe 1200 sekund a hello *UpgradeTimeout* toobe 3000 sekund.

Nakonec také nastavíme hello *UpgradeFailureAction* toorollback. Tato možnost vyžaduje Service Fabric tooroll back hello aplikace toohello předchozí verze, pokud zjistí jakékoli problémy při upgradu hello. Proto při spouštění upgradu hello (v kroku 4), hello zadat následující parametry jsou:

FailureAction = vrácení zpět

HealthCheckStableDurationSec = 60

UpgradeDomainTimeoutSec = 1200

UpgradeTimeout = 3000

## <a name="step-4-prepare-application-for-upgrade"></a>Krok 4: Příprava aplikací pro upgrade
Teď je vytvořená aplikace hello a připravena toobe upgradovat. Pokud jste otevře okno prostředí PowerShell jako správce a zadejte [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps), ponechte vás informuje, že je aplikace typu 1.0.0.0 **VisualObjects** který je nasazen.  

Hello balíčku aplikace je uložen pod následující hello relativní cestu, kde nekomprimovaným hello Service Fabric SDK - *Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug*. V tomto adresáři, kde je uložen balíček aplikace hello byste měli najít složku "Balíček". Zkontrolujte tooensure hello časová razítka, že je sestavení nejnovější hello (může být zapotřebí toomodify hello správně také cesty).

Teď umožňuje kopírování hello aktualizovat toohello balíčku aplikace úložiště bitových kopií prostředků infrastruktury služby (kde jsou hello balíčky aplikací uložené pomocí Service Fabric). Hello parametr *ApplicationPackagePathInImageStore* informuje Service Fabric, kde ji můžete najít balíček aplikace hello. Jsme umístili aplikace hello aktualizovat "VisualObjects\_V2" s hello následující příkaz (může být zapotřebí toomodify cesty znovu správně).

```powershell
Copy-ServiceFabricApplicationPackage  -ApplicationPackagePath .\Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug\Package
-ImageStoreConnectionString fabric:ImageStore   -ApplicationPackagePathInImageStore "VisualObjects\_V2"
```

dalším krokem Hello je tooregister tuto aplikaci s Service Fabric, která je možné provést pomocí hello [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) příkaz:

```powershell
Register-ServiceFabricApplicationType -ApplicationPathInImageStore "VisualObjects\_V2"
```

Pokud hello předchozí příkaz k neúspěchu, je pravděpodobné, že to musíte znovu vytvořit všechny služby. Jak je uvedeno v kroku 2, může mít tooupdate vaši webovou verzi.

## <a name="step-5-start-hello-application-upgrade"></a>Krok 5: Spuštění upgradu aplikace hello
Nyní jsme všechny aplikace hello toostart sadu upgradu pomocí hello [Start-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) příkaz:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/VisualObjects -ApplicationTypeVersion 2.0.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000   -FailureAction Rollback -Monitored
```


Hello název aplikace je hello stejná jak je popsáno v hello *ApplicationManifest.xml* souboru. Service Fabric používá tento název tooidentify aplikaci, pro kterou je získávání upgradovat. Pokud jste nastavili hello vypršení časových limitů toobe příliš krátký, můžete se setkat selhání zpráva, že stavy hello problém. Najdete v části řešení potíží s toohello, případně zvyšte hello vypršení časových limitů.

Nyní, při hello aplikace upgradu bude pokračovat, můžete sledovat pomocí Service Fabric Explorer nebo pomocí hello [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) příkaz prostředí PowerShell: 

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/VisualObjects
```

Za několik minut, stav hello, který jste získali pomocí hello předcházející příkaz prostředí PowerShell, stavu, aby byly upgradovány všechny aktualizace domény (Dokončit). A by měl zjistit, že hello vizuální objekty v okně prohlížeče spustili otáčení!

Pokuste se upgrade z verze 2 tooversion 3, nebo z verze 2 tooversion 1 jako cvičení. Přechod z verze 2 tooversion 1 také považuje upgrade. Přehrání s vypršení časových limitů a toomake zásady stavu sami obeznámeni s nimi. Při nasazování tooan clusteru Azure, hello parametry nutné toobe sadu správně. Můžete je dobré tooset hello vypršení časových limitů.

## <a name="next-steps"></a>Další kroky
[Upgrade vaší aplikace pomocí sady Visual Studio](service-fabric-application-upgrade-tutorial.md) vás provede upgrade aplikace pomocí sady Visual Studio.

Řídí, jak vaše aplikace upgraduje pomocí [upgrade parametry](service-fabric-application-upgrade-parameters.md).

Aby aplikace upgradů kompatibilní metodou učení jak toouse [serializace dat](service-fabric-application-upgrade-data-serialization.md).

Zjistěte, jak toouse pokročilé funkce při upgradu vaší aplikace tím, že odkazuje příliš[Advanced témata](service-fabric-application-upgrade-advanced.md).

Řešení běžných potíží v upgradů aplikací tím, že odkazuje toohello kroky v [řešení potíží s aplikací upgrady](service-fabric-application-upgrade-troubleshooting.md).

