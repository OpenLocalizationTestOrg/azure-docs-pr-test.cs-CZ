---
title: "Automatizace správy aplikací Azure Service Fabric | Microsoft Docs"
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
ms.openlocfilehash: fb3c2a77ea887289eebf343e223c190781d0e4c2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="automate-the-application-lifecycle-using-powershell"></a>Automatizovat životní cyklus aplikace pomocí prostředí PowerShell
Mnoho aspektů [životního cyklu aplikace Service Fabric](service-fabric-application-lifecycle.md) lze automatizovat.  Tento článek ukazuje, jak pomocí prostředí PowerShell pro automatizaci běžných úkolů pro nasazení, upgrade, odebrání a testování aplikací pro Azure Service Fabric.  Spravovat a rozhraní API HTTP pro správu aplikací jsou také k dispozici. V tématu [životní cyklus aplikace](service-fabric-application-lifecycle.md) Další informace.  

## <a name="prerequisites"></a>Požadavky
Předtím, než můžete přesunout úkoly v článku, nezapomeňte:

* Seznamte se s Service Fabric konceptů popsaných v [technický přehled Service Fabric](service-fabric-technical-overview.md).
* [Instalace modulu runtime, sady SDK a nástroje pro](service-fabric-get-started.md), který nainstaluje taky **ServiceFabric** modul prostředí PowerShell.
* [Povolit spuštění skriptu PowerShell](service-fabric-get-started.md#enable-powershell-script-execution).
* Spusťte místní cluster.  Spusťte nové okno prostředí PowerShell jako správce a spusťte instalační skript clusteru z této složky sady SDK:`& "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"`
* Než spustíte všechny příkazy prostředí PowerShell v tomto článku, nejdřív připojit k místní cluster Service Fabric pomocí [Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps):`Connect-ServiceFabricCluster localhost:19000`
* Následující úkoly vyžadují balíček pro nasazení aplikace v1 a v2 balíček aplikace pro upgrade. Stažení [ **WordCount** ukázkové aplikace](http://aka.ms/servicefabricsamples) (umístěný ve ukázky Začínáme). Sestavení a balíčku aplikace v sadě Visual Studio (klikněte pravým tlačítkem na **WordCount** v Průzkumníku řešení a vyberte **balíček**). Zkopírujte balíček v1 v `C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug` k `C:\Temp\WordCount`. Kopírování `C:\Temp\WordCount` k `C:\Temp\WordCountV2`, vytváření balíčku aplikace v2 pro upgrade. Otevřete `C:\Temp\WordCountV2\ApplicationManifest.xml` v textovém editoru. V **ApplicationManifest** elementu, změny **ApplicationTypeVersion** atribut z "1.0.0" na "2.0.0" Aktualizovat číslo verze aplikace. Uložte soubor změněné ApplicationManifest.xml.

## <a name="task-deploy-a-service-fabric-application"></a>Úloha: Nasazení aplikace Service Fabric
Jakmile jste vytvořené a zabalené aplikace (nebo stáhnout balíček aplikace), můžete nasadit aplikaci do místní cluster Service Fabric. Součástí nasazení je nahrávání balíčku aplikace, registrace typu aplikace a vytvoření instance aplikace. Při nasazení nové aplikace do clusteru, postupujte podle pokynů v této části.

### <a name="step-1-upload-the-application-package"></a>Krok 1: Nahrání balíčku aplikace
Nahrávání balíčku aplikace do úložiště bitové kopie vloží ho do umístění dostupné pro interní komponenty Service Fabric.  Balíček aplikace obsahuje nezbytné manifest aplikace, služby manifesty a kódu, konfigurace a data balíčky k vytvoření aplikace a instance služby. Pokud chcete ověřit balíček aplikace místně, použijte [Test ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/test-servicefabricapplicationpackage) rutiny.  [Kopie ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) příkaz odesílá balíček. Například:

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCount\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

### <a name="step-2-register-the-application-type"></a>Krok 2: Registrace typu aplikace
Registrace balíčku aplikace je typ aplikace a verze, které jsou deklarované v manifestu aplikace k dispozici pro použití. Čtení systému balíček odeslali v kroku 1, zkontrolujte balíček (ekvivalentní ke spuštění [Test ServiceFabricApplicationPackage](/powershell/servicefabric/vlatest/test-servicefabricapplicationpackage) místně), zpracovávají obsah balíčku a zkopírujte balíček zpracovaná na interní systému umístění.  Spustit [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) rutiny:

```powershell
Register-ServiceFabricApplicationType WordCount
```
Chcete-li zobrazit všechny typy aplikací zaregistrován v clusteru, spusťte [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) rutiny:

```powershell
Get-ServiceFabricApplicationType
```

### <a name="step-3-create-the-application-instance"></a>Krok 3: Vytvoření instance aplikace
Aplikace se dá vytvořit instance pomocí verze typu aplikace, který byl úspěšně zaregistrován pomocí [New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) příkaz. Název každé aplikace je deklarován v nasazení čas a musí začínat **prostředků infrastruktury:** scheme a být jedinečné pro každou instanci aplikace. Název typu aplikace a verze typu aplikace, které jsou deklarované v **ApplicationManifest.xml** souboru balíčku aplikace. Pokud žádné výchozí služby byly definovány v manifestu aplikace cílového typu aplikace, pak těch, které jsou v tuto chvíli vytvořit.

```powershell
New-ServiceFabricApplication fabric:/WordCount WordCount 1.0.0
```

[Get-ServiceFabricApplication](/powershell/servicefabric/vlatest/get-servicefabricapplication) příkaz vypíše všechny instance aplikace, které byly úspěšně vytvořeny, spolu s jejich celkovým stavem. [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps) příkaz vypíše všechny instance služby, které byly úspěšně vytvořeny v rámci instance dané aplikace. Jsou uvedeny výchozí služby (pokud existuje).

```powershell
Get-ServiceFabricApplication

Get-ServiceFabricApplication | Get-ServiceFabricService
```

## <a name="task-upgrade-a-service-fabric-application"></a>Úloha: Upgrade aplikace Service Fabric
Můžete upgradovat dříve nasazené aplikace Service Fabric se balíček aktualizované aplikace. Tato úloha provede upgrade aplikace WordCount, který byl nasazen v "úkolů: nasazení aplikace Service Fabric." Pročtěte [upgradu aplikace Service Fabric](service-fabric-application-upgrade.md) Další informace.

Pro zjednodušení v tomto příkladu byla aktualizována jenom číslo verze aplikace v WordCountV2 balíčku aplikace vytvořené v požadavky. Realističtější scénář zahrnovat aktualizuje vaše soubory kódu, konfigurace nebo dat služby a pak znovu sestavit a zabalení aplikace s aktualizovanou verzi čísla.  

### <a name="step-1-upload-the-updated-application-package"></a>Krok 1: Nahrání balíčku aktualizované aplikace
Aplikace WordCount v1 je připraven k upgradu. Pokud jste otevře okno prostředí PowerShell jako správce a zadejte [ **Get-ServiceFabricApplication**](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps), najdete v části nasazení této verze 1.0.0 typu aplikace WordCount.  

Nyní zkopírujte balíček aktualizované aplikace do úložiště bitové kopie Service Fabric (kde jsou balíčky aplikací uložené pomocí Service Fabric). Parametr **ApplicationPackagePathInImageStore** informuje o tom, kde ji můžete najít balíček aplikace Service Fabric. Následující příkaz zkopíruje balíček aplikace pro **WordCountV2** v úložišti bitové kopie:  

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCountV2\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCountV2

```
### <a name="step-2-register-the-updated-application-type"></a>Krok 2: Registrace typu aktualizované aplikace
Dalším krokem je zaregistrovat novou verzi aplikace Service Fabric, které lze provést pomocí [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) rutiny:

```powershell
Register-ServiceFabricApplicationType WordCountV2
```

### <a name="step-3-start-the-upgrade"></a>Krok 3: Spuštění upgradu
Různé upgradu parametrů, vypršení časových limitů a kritéria lze použít k upgradu aplikace. Pročtěte [parametry upgradu aplikace](service-fabric-application-upgrade-parameters.md) a [procesu upgradu](service-fabric-application-upgrade.md) dokumenty Další informace. Všechny instance a služby by měl být *pořádku* po upgradu.  Nastavte **HealthCheckStableDuration** na 60 sekund (tak, že služby jsou v pořádku po dobu minimálně 20 sekund před upgradem k další upgradovací doméně).  Také nastavit **UpgradeDomainTimeout** na 1200 sekund a **UpgradeTimeout** na 3000 sekund. Nakonec nastavte **UpgradeFailureAction** k **vrácení zpět**, který požaduje, aby Service Fabric vrátí zpět na předchozí verzi aplikace Pokud došlo k selhání při upgradu.

Nyní můžete spustit upgradu aplikace pomocí [Start-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) rutiny:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/WordCount -ApplicationTypeVersion 2.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000  -FailureAction Rollback -Monitored
```

Všimněte si, že název aplikace je stejný jako název aplikace nasazené předtím v1.0.0 (fabric: / WordCount). Service Fabric používá k identifikaci, která aplikace je získávání upgradovat tento název. Pokud jste nastavili vypršení časových limitů jako příliš krátká, můžete se setkat časový limit selhání zpráva, že problém. Odkazovat na [řešení potíží s aplikací upgrady](service-fabric-application-upgrade-troubleshooting.md), nebo zvyšte vypršení časových limitů.

### <a name="step-4-check-upgrade-progress"></a>Krok 4: Průběh upgradu zkontrolujte
Průběh upgradu aplikace můžete monitorovat pomocí [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md), nebo pomocí [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) rutiny:

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/WordCount
```

Za několik minut [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) rutiny ukazuje, že byly upgradovány všechny upgradu domény (Dokončit).

## <a name="task-test-a-service-fabric-application"></a>Úloha: Testovací aplikace Service Fabric
Vývojáři k zápisu vysoce kvalitních služeb, musí být může způsobit chyby nespolehlivé infrastruktury k testování stabilitu své služby. Service Fabric nabízí vývojářům možnost způsobit selhání akce a testování služeb v případě selhání pomocí scénářů testovací chaos a převzetí služeb při selhání.  Pročtěte [Úvod ke službě analýza selhání](service-fabric-testability-overview.md) Další informace.

### <a name="step-1-run-the-chaos-test-scenario"></a>Krok 1: Spuštění scénáře chaos testu
Scénáře testu chaos generuje chyby napříč celý cluster Service Fabric. Tento scénář komprimaci chyb obecně vidět přes měsíců nebo let několik hodin. Kombinace prokládaná chyb míru vysokou odolnost vyhledá náročnějších případech, které by jinak vynechat. Následující příklad spustí scénáře testu chaos po dobu 60 minut.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```

### <a name="step-2-run-the-failover-test-scenario"></a>Krok 2: Spuštění scénáře testu převzetí služeb při selhání
Převzetí služeb při selhání testování scénář cíle oddíl specifické služby namísto celého clusteru a dalším službám opustí neovlivní. Tento scénář iteruje posloupnost simulované chyb při ověřování služby průběhu obchodní logiky. Chyby při ověřování služby označuje potíže, které potřebuje další šetření. Testovací převzetí služeb při selhání indukuje pouze jeden selhání současně, a scénáře chaos testu, který může být nutné několik chyb. Následující příklad spouští test převzetí služeb při selhání po dobu 60 minut topologie fabric: / WordCount/WordCountService služby.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/WordCount/WordCountService"

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindUniformInt64 -PartitionKey 1
```

## <a name="task-remove-a-service-fabric-application"></a>Úloha: Odeberte aplikace Service Fabric
Odstranění instance nasazené aplikace, typ zřízené aplikace odebrat z clusteru a odebrání balíčku aplikace úložiště bitových kopií.

### <a name="step-1-remove-an-application-instance"></a>Krok 1: Odebrání instance aplikace
Instance aplikace už je potřeba, trvale odstraníte ho pomocí [odebrat ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) rutiny. Tím se také automaticky odebere všechny služby náležící k aplikaci, trvale odebrat všechny služby stavu. Tato operace se nedá vrátit a nelze ji obnovit stav aplikace.

```powershell
Remove-ServiceFabricApplication fabric:/WordCount
```

### <a name="step-2-unregister-the-application-type"></a>Krok 2: Zrušení registrace typu aplikace
Pokud již nepotřebujete konkrétní verzi typ aplikace, zrušení její registrace pomocí [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) rutiny. Zrušení registrace nepoužívá typy uvolní prostor úložiště používá balíček aplikace v úložišti bitové kopie. Typ aplikace může být registrace, dokud nejsou žádné aplikace, u ní nebo čekající upgrady aplikací odkazující na jeho instanci.

```powershell
Unregister-ServiceFabricApplicationType WordCount 1.0.0
```

### <a name="step-3-remove-the-application-package"></a>Krok 3: Odebrání balíčku aplikace
Po zrušení registrace typu aplikace balíčku aplikace můžete odebrat z úložiště imagí pomocí [odebrat ServiceFabricApplicationPackage](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) rutiny.

```powershell
Remove-ServiceFabricApplicationPackage -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

## <a name="next-steps"></a>Další kroky
[Životní cyklus aplikace Service Fabric](service-fabric-application-lifecycle.md)

[Nasazení aplikace](service-fabric-deploy-remove-applications.md)

[Upgrade aplikace](service-fabric-application-upgrade.md)

[Rutiny Azure Service Fabric](/powershell/azure/overview?view=azureservicefabricps)

