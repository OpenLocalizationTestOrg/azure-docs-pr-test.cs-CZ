---
title: "Správa aplikací Azure Service Fabric aaaAutomate | Microsoft Docs"
description: "Nasazení, upgrade, testování a odebrání aplikace Service Fabric pomocí prostředí PowerShell."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: f03f4294-571d-4262-8a77-cc8b481b959d
ms.service: service-fabric
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/16/2017
ms.author: ryanwi
redirect_url: /azure/service-fabric/service-fabric-deploy-remove-applications
ms.openlocfilehash: a64a39dbee26be8ac15fee767a90fd06bfe4b896
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automate-hello-application-lifecycle-using-powershell"></a>Automatizovat životního cyklu aplikace hello pomocí prostředí PowerShell
Mnoho aspektů hello [životního cyklu aplikace Service Fabric](service-fabric-application-lifecycle.md) lze automatizovat.  Tento článek ukazuje, jak toouse prostředí PowerShell tooautomate běžné úkoly pro nasazení, upgrade, odebrání a testování aplikací pro Azure Service Fabric.  Spravovat a rozhraní API HTTP pro správu aplikací jsou také k dispozici. V tématu [životní cyklus aplikace](service-fabric-application-lifecycle.md) Další informace.  

## <a name="prerequisites"></a>Požadavky
Před přesunete toohello úlohy v článku hello, nezapomeňte:

* Seznamte se s hello Service Fabric konceptů popsaných v [technický přehled Service Fabric](service-fabric-technical-overview.md).
* [Instalace hello runtime, sadu SDK a nástrojů pro](service-fabric-get-started.md), který se nainstaluje také hello **ServiceFabric** modul prostředí PowerShell.
* [Povolit spuštění skriptu PowerShell](service-fabric-get-started.md#enable-powershell-script-execution).
* Spusťte místní cluster.  Spusťte nové okno prostředí PowerShell jako správce a spusťte instalační skript clusteru hello ze složky sady SDK hello:`& "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"`
* Než spustíte všechny příkazy prostředí PowerShell v tomto článku, nejprve připojení toohello místní cluster Service Fabric pomocí [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps):`Connect-ServiceFabricCluster localhost:19000`
* Hello následující úkoly vyžadují toodeploy balíček aplikace v1 a v2 balíček aplikace pro upgrade. Stáhnout hello [ **WordCount** ukázkové aplikace](http://aka.ms/servicefabricsamples) (umístěný ve ukázky hello Začínáme). Sestavení a balíček aplikace hello v sadě Visual Studio (klikněte pravým tlačítkem na **WordCount** v Průzkumníku řešení a vyberte **balíček**). Zkopírování balíčku v1 hello v `C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug` příliš`C:\Temp\WordCount`. Kopírování `C:\Temp\WordCount` příliš`C:\Temp\WordCountV2`, vytváření balíčku aplikace hello v2 pro upgrade. Otevřete `C:\Temp\WordCountV2\ApplicationManifest.xml` v textovém editoru. V hello **ApplicationManifest** element, změna hello **ApplicationTypeVersion** atribut z "1.0.0" příliš "2.0.0" číslo verze aplikace hello tooupdate. Uložte soubor ApplicationManifest.xml hello změnit.

## <a name="task-deploy-a-service-fabric-application"></a>Úloha: Nasazení aplikace Service Fabric
Jakmile jste vytvořené a zabalené aplikace hello (nebo stáhnout balíček aplikace hello), můžete nasadit aplikaci hello do místní cluster Service Fabric. Součástí nasazení je odeslání balíčku aplikace hello, registrace typu aplikace hello a vytvoření instance aplikace hello. Použijte hello pokyny v této části toodeploy do nového clusteru tooa aplikace.

### <a name="step-1-upload-hello-application-package"></a>Krok 1: Nahrání balíčku aplikace hello
Odesílání toohello balíčku aplikace hello úložiště image store vloží ho komponenty Service Fabric přístupné toointernal umístění.  balíček aplikace Hello obsahuje hello nezbytné manifest aplikace, služby manifesty a kódu, konfiguraci a data balíčky toocreate hello aplikace a instance služby. Pokud chcete tooverify hello balíček aplikace místně, použijte hello [Test ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/test-servicefabricapplicationpackage) rutiny.  Hello [kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) příkaz nahrávání hello balíčku. Například:

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCount\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

### <a name="step-2-register-hello-application-type"></a>Krok 2: Registrace typu aplikace hello
Registrace balíčku aplikace hello umožňuje hello typ a verze aplikace deklarované v manifestu aplikace hello k dispozici pro použití. Hello systém přečte hello balíček odeslali v kroku hello 1, zkontrolujte balíček hello (ekvivalentní toorunning [Test ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/test-servicefabricapplicationpackage) místně), zpracovat hello balíček obsah a zkopírujte hello zpracovat balíček tooan Interní systémové umístění.  Spustit hello [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) rutiny:

```powershell
Register-ServiceFabricApplicationType WordCount
```
všechny typy aplikací hello zaregistrován v hello clusteru, spusťte hello toosee [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) rutiny:

```powershell
Get-ServiceFabricApplicationType
```

### <a name="step-3-create-hello-application-instance"></a>Krok 3: Vytvoření instance aplikace hello
Aplikace se dá vytvořit instance pomocí verze typu aplikace, který byl úspěšně zaregistrován pomocí hello [New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) příkaz. Hello názvu každé aplikace je deklarován v nasazení čas a musí začínat hello **prostředků infrastruktury:** scheme a být jedinečné pro každou instanci aplikace. Hello název typu aplikace a verze typu aplikace jsou deklarované v hello **ApplicationManifest.xml** souboru balíčku aplikace hello. Pokud žádné výchozí služby byly definovány manifest aplikace hello hello typ cílové aplikace, pak těch, které jsou v tuto chvíli vytvořit.

```powershell
New-ServiceFabricApplication fabric:/WordCount WordCount 1.0.0
```

Hello [Get-ServiceFabricApplication](/powershell/servicefabric/vlatest/get-servicefabricapplication) příkaz vypíše všechny instance aplikace, které byly úspěšně vytvořeny, spolu s jejich celkovým stavem. Hello [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps) příkaz vypíše všechny hello instancí služby, které byly úspěšně vytvořeny v rámci instance dané aplikace. Jsou uvedeny výchozí služby (pokud existuje).

```powershell
Get-ServiceFabricApplication

Get-ServiceFabricApplication | Get-ServiceFabricService
```

## <a name="task-upgrade-a-service-fabric-application"></a>Úloha: Upgrade aplikace Service Fabric
Můžete upgradovat dříve nasazené aplikace Service Fabric se balíček aktualizované aplikace. Tato úloha provede upgrade aplikace WordCount hello, který byl nasazen v "úkolů: nasazení aplikace Service Fabric." Pročtěte [upgradu aplikace Service Fabric](service-fabric-application-upgrade.md) Další informace.

jednoduché věcí tookeep v tomto příkladu pouze hello číslo verze aplikace byla aktualizována v balíčku aplikace hello WordCountV2 vytvořené v hello požadavky. Realističtější scénář zahrnovat aktualizuje vaše soubory kódu, konfigurace nebo dat služby a pak znovu sestavit a zabalení aplikace hello s aktualizovanou verzi čísla.  

### <a name="step-1-upload-hello-updated-application-package"></a>Krok 1: Nahrání balíčku aktualizované aplikace hello
Hello v1 aplikace WordCount je připraven toobe upgradovat. Pokud jste otevře okno prostředí PowerShell jako správce a zadejte [ **Get-ServiceFabricApplication**](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps), najdete v části nasazení této verze 1.0.0 hello typ aplikace WordCount.  

Aktualizována hello kopie aplikace balíčku toohello Service Fabric úložiště bitových kopií (kde jsou hello balíčky aplikací uložené pomocí Service Fabric). Hello parametr **ApplicationPackagePathInImageStore** informuje Service Fabric, kde ji můžete najít balíček aplikace hello. Dobrý den, následující příkaz kopie hello balíčku aplikace příliš**WordCountV2** v úložišti bitové kopie hello:  

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCountV2\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCountV2

```
### <a name="step-2-register-hello-updated-application-type"></a>Krok 2: Registrace hello aktualizovat typ aplikace
Hello dalším krokem je tooregister hello nové verze aplikace hello s Service Fabric, které lze provést pomocí hello [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) rutiny:

```powershell
Register-ServiceFabricApplicationType WordCountV2
```

### <a name="step-3-start-hello-upgrade"></a>Krok 3: Spuštění upgradu hello
Různé parametry upgradu, vypršení časových limitů a kritéria můžou být použité tooapplication upgrady. Pročtěte hello [parametry upgradu aplikace](service-fabric-application-upgrade-parameters.md) a [procesu upgradu](service-fabric-application-upgrade.md) toolearn další dokumenty. Všechny instance a služby by měl být *pořádku* po upgradu hello.  Sada hello **HealthCheckStableDuration** too60 sekund (tak, aby pro aspoň 20 sekund před provedením upgradu hello toohello další upgradovací doména hello služby jsou v pořádku).  Také sadu hello **UpgradeDomainTimeout** too1200 sekund a hello **UpgradeTimeout** too3000 sekund. Nakonec nastavte hello **UpgradeFailureAction** příliš**vrácení zpět**, které požadavky, které Service Fabric se vrátí k původní hello aplikace toohello předchozí verze, pokud během upgradu došlo k selhání.

Nyní můžete spustit upgradu aplikace hello pomocí hello [Start-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) rutiny:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/WordCount -ApplicationTypeVersion 2.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000  -FailureAction Rollback -Monitored
```

Všimněte si, že název aplikace hello je hello stejné jako hello nasadili v1.0.0 název aplikace (fabric: / WordCount). Service Fabric používá tento název tooidentify aplikaci, pro kterou je získávání upgradovat. Pokud jste nastavili hello vypršení časových limitů toobe příliš krátký, můžete se setkat chybová zpráva o vypršení časového limitu, stavy hello problém. Odkazovat příliš[řešení potíží s aplikací upgrady](service-fabric-application-upgrade-troubleshooting.md), nebo zvyšte hello vypršení časových limitů.

### <a name="step-4-check-upgrade-progress"></a>Krok 4: Průběh upgradu zkontrolujte
Průběh upgradu aplikace můžete monitorovat pomocí [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md), nebo pomocí hello [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) rutiny:

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/WordCount
```

Za několik minut, hello [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) rutiny ukazuje, že byly upgradovány všechny upgradu domény (Dokončit).

## <a name="task-test-a-service-fabric-application"></a>Úloha: Testovací aplikace Service Fabric
toowrite vysoce kvalitních služeb, vývojáři potřebovat toobe možné tooinduce nespolehlivé infrastruktury chyb tootest hello stabilitu svých služeb. Service Fabric nabízí vývojářům hello možnost tooinduce selhání akce a testovací služeb v hello přítomnost chyb pomocí hello testovací scénáře chaos a převzetí služeb při selhání.  Pročtěte [Úvod toohello selhání Analysis Service](service-fabric-testability-overview.md) Další informace.

### <a name="step-1-run-hello-chaos-test-scenario"></a>Krok 1: Spuštění hello chaos testovací scénář
scénáře testu chaos Hello generuje chyby napříč hello celý cluster Service Fabric. scénář Hello komprimaci chyb obecně vidět přes měsíců nebo let tooa několik hodin. kombinace Hello prokládaná chyb míru vysokou odolnost vyhledá náročnějších případech, které by jinak vynechat. Vítejte v následujícím příkladu spustí hello chaos testovací scénář po dobu 60 minut.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```

### <a name="step-2-run-hello-failover-test-scenario"></a>Krok 2: Spuštění hello scénáře testu převzetí služeb při selhání
převzetí služeb při selhání Hello testování scénář cíle oddíl specifické služby namísto celého clusteru hello a jiných služeb opustí neovlivní. scénář Hello iteruje posloupnost simulované chyb při ověřování služby průběhu obchodní logiky. Chyby při ověřování služby označuje potíže, které potřebuje další šetření. testovací převzetí služeb při selhání Hello indukuje pouze jeden selhání současně, jako názvem na rozdíl od toohello chaos testovací scénář, který může být nutné několik chyb. Vítejte v následujícím příkladu spustí hello testovací převzetí služeb při selhání po dobu 60 minut proti hello fabric: / WordCount/WordCountService služby.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/WordCount/WordCountService"

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindUniformInt64 -PartitionKey 1
```

## <a name="task-remove-a-service-fabric-application"></a>Úloha: Odeberte aplikace Service Fabric
Odstranění instance nasazené aplikace, odstraňte typ hello zřizovat aplikace z clusteru hello a odebrání balíčku aplikace hello hello úložiště bitových kopií.

### <a name="step-1-remove-an-application-instance"></a>Krok 1: Odebrání instance aplikace
Instance aplikace už je potřeba, trvale odstraníte ho pomocí hello [odebrat ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) rutiny. Tím se také automaticky odebere všechny služby patřící toohello aplikace, trvale odebrat všechny služby stavu. Tato operace se nedá vrátit a nelze ji obnovit stav aplikace.

```powershell
Remove-ServiceFabricApplication fabric:/WordCount
```

### <a name="step-2-unregister-hello-application-type"></a>Krok 2: Zrušení registrace typu aplikace hello
Pokud již nepotřebujete konkrétní verzi typ aplikace, zrušení její registrace pomocí hello [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) rutiny. Zrušení registrace nepoužívá typy verzích využitého prostoru úložiště pomocí balíčku aplikace hello v úložišti bitové kopie hello. Typ aplikace může být registrace, dokud nejsou žádné aplikace, u ní nebo čekající upgrady aplikací odkazující na jeho instanci.

```powershell
Unregister-ServiceFabricApplicationType WordCount 1.0.0
```

### <a name="step-3-remove-hello-application-package"></a>Krok 3: Odebrání balíčku aplikace hello
Po zrušení registrace typu aplikace hello balíčku aplikace hello můžete odebrat z úložiště bitových kopií hello pomocí hello [odebrat ServiceFabricApplicationPackage](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) rutiny.

```powershell
Remove-ServiceFabricApplicationPackage -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

## <a name="next-steps"></a>Další kroky
[Životní cyklus aplikace Service Fabric](service-fabric-application-lifecycle.md)

[Nasazení aplikace](service-fabric-deploy-remove-applications.md)

[Upgrade aplikace](service-fabric-application-upgrade.md)

[Rutiny Azure Service Fabric](/powershell/azure/overview?view=azureservicefabricps)

