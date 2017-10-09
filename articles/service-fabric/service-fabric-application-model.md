---
title: model aplikace Service Fabric aaaAzure | Microsoft Docs
description: "Jak toomodel a popisují aplikace a služby v Service Fabric."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 17a99380-5ed8-4ed9-b884-e9b827431b02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: ryanwi
ms.openlocfilehash: 54c4d026e7d556be5f697d4a6f2ee886687e1c35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="model-an-application-in-service-fabric"></a>Model aplikace v Service Fabric
Tento článek obsahuje přehled aplikačního modelu hello Azure Service Fabric a jak toodefine aplikace a služby prostřednictvím manifest soubory.

## <a name="understand-hello-application-model"></a>Pochopení hello aplikačního modelu
Aplikace je kolekce základních služeb, které provádějí určité funkce nebo funkce. Služba provede kompletní a samostatné funkce a začít a spustit nezávisle na jiné služby.  Služba se skládá z kódu, konfigurace a data. Pro každou službu kódu se skládá z binární hello spustitelné soubory, konfigurace se skládá z nastavení služby, které je možné načíst v době běhu a data se skládá z libovolné statických dat toobe spotřebovávají hello služby. Jednotlivé komponenty v tomto modelu hierarchické aplikace může být verzí a upgradovali nezávisle.

![model aplikace Service Fabric Hello][appmodel-diagram]

Typ aplikace kategorizaci aplikace a skládá se z sady typy služeb. Typ služby je kategorizaci služby. Hello kategorizaci může mít různá nastavení a konfigurace, ale hello základní funkce zůstane hello stejné. Hello instancí služby jsou variacím konfigurace hello jinou službu systému hello stejný typ služby.  

Třídy (nebo "typů") aplikací a služeb, které jsou popsané prostřednictvím souborů XML (manifestů aplikace a služby manifesty).  manifesty Hello jsou hello šablony, kterými může být aplikace vytvořena z úložiště bitových kopií hello clusteru. Hello definice schématu pro soubor ServiceManifest.xml a ApplicationManifest.xml hello se instaluje s hello Service Fabric SDK a nástrojů pro příliš*C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*.

Hello kód pro spuštění jako samostatné procesy, i když jsou hostované pomocí hello stejného uzlu Service Fabric instance jinou aplikaci. Kromě toho je možné spravovat životní cyklus hello každá instance aplikace (například upgradovat) nezávisle. Hello následující diagram ukazuje, jak typy aplikací se skládají z typů služeb, které pak se skládají z kódu, konfigurace a data balíčky. diagram hello toosimplify, hello balíčků kódu/config/dat pouze pro `ServiceType4` se zobrazují, když každý typ služby by mělo zahrnovat některé nebo všechny ty typy balíčků.

![Typy aplikací Service Fabric a typů služeb][cluster-imagestore-apptypes]

Jsou dva různé soubory manifestu použité toodescribe aplikací a služeb: hello service manifest a manifest aplikace. Manifesty jsou podrobně popsány v následující části hello.

V clusteru hello aktivní může být jeden nebo více instancí typu služby. Například instance stavové služby nebo repliky, dosáhnout nastavením stavu replikace mezi replikami, které jsou umístěné na různých uzlech v clusteru hello vysokou spolehlivostí. Replikace v podstatě poskytuje redundanci pro toobe služby hello k dispozici i v případě selhání jednoho uzlu v clusteru. A [oddíly služby](service-fabric-concepts-partitioning.md) další vydělí jeho stav (a přístup vzory toothat stav) mezi uzly v clusteru hello.

Hello následující diagram znázorňuje hello vztah mezi aplikací a instance služby, oddíly a repliky.

![Oddíly a repliky v rámci služby][cluster-application-instances]

> [!TIP]
> Rozložení hello aplikací si můžete prohlédnout v clusteru pomocí nástroje Service Fabric Explorer hello k dispozici v http://&lt;yourclusteraddress&gt;: 19080/Explorer. Další informace najdete v tématu [vizualizace vašeho clusteru pomocí Service Fabric Exploreru](service-fabric-visualizing-your-cluster.md).
> 
> 

## <a name="describe-a-service"></a>Popis služby
manifest služby Hello deklarativně definuje hello služby typu a verzi. Určuje metadata služby, jako je například typ služby, Vlastnosti stavu, metriky Vyrovnávání zatížení, binární soubory služby a konfigurační soubory.  Jinými slovy, popisuje hello kód, konfigurace a data balíčky, které tvoří toosupport balíček služby jeden nebo více typů služeb. Zde je jednoduchý příklad manifest služby:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="MyCode" Version="CodeVersion1">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
    <EnvironmentVariables>
      <EnvironmentVariable Name="MyEnvVariable" Value=""/>
      <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
    </EnvironmentVariables>
  </CodePackage>
  <ConfigPackage Name="MyConfig" Version="ConfigVersion1" />
  <DataPackage Name="MyData" Version="DataVersion1" />
</ServiceManifest>
```

**Verze** atributy jsou nestrukturovaných řetězce a analyzovat není systémem hello. Verze atributy jsou použité tooversion jednotlivé komponenty pro upgrade.

**ServiceTypes** deklaruje, jaké typy služby podporuje **CodePackages** v tomto manifestu. Při vytváření instance služby pro jeden z těchto typů služeb aktivují se všechny balíčky kódu, které jsou deklarované v manifestu tento spuštěním jejich vstupní body. Výsledný procesy Hello jsou očekávané tooregister hello podporované služby typy za běhu. Typů služeb jsou deklarované v manifestu úroveň hello a není hello úrovni balíčku kódu. Takže pokud je více balíčků kódu, všechny aktivují se vždy, když systém hello hledá některého hello deklarovaný typů služeb.

**SetupEntryPoint** je privilegované vstupního bodu, který je spuštěn s hello stejné přihlašovací údaje jako Service Fabric (obvykle hello *LocalSystem* účtu) před další vstupní bod. spustitelný soubor Hello určeného **EntryPoint** je obvykle hostitele služby dlouho běžící hello. Hello přítomnost vstupní bod samostatného instalačního zabraňuje s toorun hello hostitele služby s vysokou úrovní oprávnění pro dlouhou dobu. spustitelný soubor Hello určeného **EntryPoint** běží **SetupEntryPoint** ukončí úspěšně. Pokud proces hello někdy ukončí nebo dojde k chybě, výsledná proces hello monitorovat a restartování (počínaje znovu **SetupEntryPoint**).  

Typické scénáře použití **SetupEntryPoint** jsou při spustit spustitelný soubor před spuštěním služby hello nebo k provedení operace se zvýšenými oprávněními. Například:

* Nastavení a inicializace proměnné prostředí, které hello potřebám spustitelný soubor služby. Toto není omezený tooonly spustitelné soubory zapisovat prostřednictvím programovací modely hello Service Fabric. Například npm.exe musí některé proměnné prostředí nakonfigurované pro nasazení aplikace node.js.
* Nastavení řízení přístupu nainstalováním certifikáty zabezpečení.

Další podrobnosti o tom tooconfigure hello **SetupEntryPoint** najdete v části [konfigurace hello zásady pro bod služby instalační položka](service-fabric-application-runas-security.md)

**EnvironmentVariables** poskytuje seznam proměnných prostředí, které jsou nastavené pro tento balíček kódu. Environmentment proměnné mohou být přepsána nastaveními v hello `ApplicationManifest.xml` tooprovide různé hodnoty pro různé služby instance. 

**DataPackage** deklaruje do složky s názvem podle hello **název** atribut, který obsahuje libovolné statických dat toobe spotřebovávají hello procesu v době běhu.

**ConfigPackage** deklaruje do složky s názvem podle hello **název** atribut, který obsahuje *souborech Settings.xml* souboru. soubor nastavení Hello obsahuje oddíly pár definovaný uživatelem, klíč hodnota nastavení, která čte hello proces zpět za běhu. Během upgradu, pokud pouze hello **ConfigPackage** **verze** došlo ke změně, pak není hello spuštěn proces restartovat. Místo toho zpětné volání upozorní hello procesu, které mění nastavení konfigurace, může být dynamicky znovu. Tady je příklad *souborech Settings.xml* souboru:

```xml
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSection">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
```

> [!NOTE]
> Manifest služby může obsahovat více kód, konfigurace a data balíčky. Každý z nich může být verzí nezávisle.
> 
> 

<!--
For more information about other features supported by service manifests, refer toohello following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Resources
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application"></a>Popis aplikace
manifest aplikace Hello deklarativně popisuje hello typ a verze aplikace. Určuje metadata služby složení například stabilní názvy, vytváření oddílů schématu, faktor počet/replikace instance, zásady zabezpečení nebo izolace, omezení umístění, konfigurace přepsání a typů základní služby. Hello Vyrovnávání zatížení domén do které se umístí hello aplikace jsou také popsány.

Proto manifest aplikace popisuje elementy na úrovni aplikace hello a odkazuje na jeden nebo více služby manifesty toocompose typ aplikace. Zde je jednoduchý příklad manifest aplikace:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ApplicationManifest
      ApplicationTypeName="MyApplicationType"
      ApplicationTypeVersion="AppManifestVersion1"
      xmlns="http://schemas.microsoft.com/2011/01/fabric"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example application manifest</Description>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyServiceManifest" ServiceManifestVersion="SvcManifestVersion1"/>
    <ConfigOverrides/>
    <EnvironmentOverrides CodePackageRef="MyCode"/>
  </ServiceManifestImport>
  <DefaultServices>
     <Service Name="MyService">
         <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
             <SingletonPartition/>
         </StatelessService>
     </Service>
  </DefaultServices>
</ApplicationManifest>
```

Jako služba manifesty **verze** atributy jsou nestrukturovaných řetězce a nejsou analyzovat hello systému. Verze atributy jsou také použít tooversion jednotlivé komponenty pro upgrade.

**ServiceManifestImport** obsahuje manifesty tooservice odkazy, které tvoří tento typ aplikace. Manifesty importované služba zjistit, jaké typy služby jsou platné v rámci tento typ aplikace. V rámci hello ServiceManifestImport přepsat hodnoty konfigurace v souborech Settings.xml a prostředí proměnné v ServiceManifest.xml soubory. 


**DefaultServices** deklaruje instance služby, které se automaticky vytvoří vždy, když aplikace je vytvořena instance pro tento typ aplikace. Výchozí služby jsou pouze pro vaše pohodlí a chovají se jako normální služby, každý po jejich vytvoření. Tyto jsou upgradovány spolu s jinými službami v instanci aplikace hello a může být odebrán také.

> [!NOTE]
> Manifest aplikace může obsahovat více manifestu importy služby a výchozí služby. Každý importu manifestu služby může být nezávisle verzí.
> 
> 

jak toomaintain jinou aplikaci a parametry služby pro jednotlivá prostředí, najdete v části toolearn [Správa parametry aplikace pro prostředí s více](service-fabric-manage-multiple-environment-app-configuration.md).

<!--
For more information about other features supported by application manifests, refer toohello following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->



## <a name="next-steps"></a>Další kroky
[Balíček aplikace](service-fabric-package-apps.md) a nastavit jej připraveni toodeploy.

[Nasazení a odebírat aplikace] [ 10] popisuje, jak instancemi aplikace toomanage toouse prostředí PowerShell.

[Správa aplikací parametry pro prostředí s více] [ 11] popisuje, jak tooconfigure parametrů a proměnných prostředí pro instance jinou aplikaci.

[Konfigurovat zásady zabezpečení pro vaši aplikaci] [ 12] popisuje, jak toorun služeb v rámci přístup toorestrict zásady zabezpečení.

[Aplikace hostující modely] [ 13] popisují vztah mezi nasazená služba a služba hostitelský proces repliky (nebo instance).

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png

<!--Link references--In actual articles, you only need a single period before hello slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
[13]: service-fabric-hosting-model.md
