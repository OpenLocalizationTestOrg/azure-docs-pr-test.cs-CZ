---
title: "aaaConvert toomicroservices aplikací Azure Cloud Services | Microsoft Docs"
description: "Tato příručka porovná Cloud Services – webové a rolí pracovního procesu a toohelp bezstavové služby Service Fabric migrovat z cloudové služby tooService prostředků infrastruktury."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 5880ebb3-8b54-4be8-af4b-95a1bc082603
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: c43b11623b2ba7f6069782a8b7e030c82572a6e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="guide-tooconverting-web-and-worker-roles-tooservice-fabric-stateless-services"></a>Průvodce tooconverting Web a rolí pracovního procesu tooService Fabric bezstavové služby
Tento článek popisuje, jak toomigrate vaše cloudové služby Web a rolí pracovního procesu tooService Fabric bezstavové služby. Toto je nejjednodušší cesta migrace hello z cloudové služby tooService prostředků infrastruktury pro aplikace, jejichž přehled architektury přechází toostay zhruba hello stejné.

## <a name="cloud-service-project-tooservice-fabric-application-project"></a>Cloudové služby projektu tooService prostředků infrastruktury aplikace projektu
 Projekt cloudové služby a aplikace Service Fabric projektu mají podobnou strukturou a obě představují hello nasazení jednotky pro vaše aplikace – to znamená, že každý definují hello kompletní balíček, který je nasazený toorun vaší aplikace. Cloudové služby projektu obsahuje jeden nebo více webu nebo rolí pracovního procesu. Podobně projektu aplikace Service Fabric obsahuje jednu nebo více služeb. 

Hello rozdíl je, že párům projekt cloudové služby hello hello nasazení aplikace se nasazení virtuálního počítače a proto obsahuje nastavení konfigurace virtuálního počítače v, zatímco projektu aplikace Service Fabric hello pouze definuje aplikaci, která bude nasazena Sada tooa existujících virtuálních počítačů v clusteru Service Fabric. samotný cluster Service Fabric Hello je nasazen pouze jednou, buď prostřednictvím šablonu Resource Manageru nebo prostřednictvím hello portál Azure a více Service Fabric může být aplikace nasazeny tooit.

![Porovnání projektu Service Fabric a cloudové služby][3]

## <a name="worker-role-toostateless-service"></a>Služba toostateless Role pracovního procesu
Role pracovního procesu koncepčně, představuje bezstavového zatížení, což znamená, každá instance úlohy hello je stejný jako a požadavky se dají směrované tooany instance kdykoli. Každá instance není očekávaný tooremember hello předchozí požadavek. Stav této úlohy hello funguje na spravuje úložiště služby externí stavu, například Azure Table Storage nebo Azure DB dokumentu. V Service Fabric tento typ úlohy je reprezentována bezstavové služby. Nejjednodušší způsob toomigrating Hello tooService Role pracovního procesu Fabric lze provést převod tooa kód Role pracovního procesu bezstavové služby.

![TooStateless Role pracovního procesu služby][4]

## <a name="web-role-toostateless-service"></a>Webová služba toostateless Role
Podobné tooWorker Role, webové Role také reprezentuje bezstavového zatížení, a proto koncepčně příliš může být namapované tooa bezstavové služby Service Fabric. Ale na rozdíl od webových rolí, Service Fabric nepodporuje službu IIS. toomigrate webovou aplikaci z bezstavové služby Role webový tooa vyžaduje první přesunutí tooa webové rozhraní, které může být samoobslužně hostovaná a není závislá na System.Web, například 1 jádro ASP.NET nebo služby IIS.

| **Aplikace** | **Podporuje se** | **Cesty migrace** |
| --- | --- | --- |
| ASP.NET – webové formuláře |Ne |Převést tooASP.NET základní 1 MVC |
| ASP.NET MVC |Pomocí nástroje Migrace |Upgrade tooASP.NET základní 1 MVC |
| Webové rozhraní API technologie ASP.NET |Pomocí nástroje Migrace |Použít vlastním hostováním server nebo ASP.NET Core 1 |
| ASP.NET Core 1 |Ano |Není k dispozici |

## <a name="entry-point-api-and-lifecycle"></a>Vstupní bod rozhraní API a životního cyklu
Rozhraní API nabídka podobně jako vstupní body služby Role pracovního procesu a Service Fabric: 

| **Vstupní bod** | **Role pracovního procesu** | **Služba Service Fabric** |
| --- | --- | --- |
| Zpracování |`Run()` |`RunAsync()` |
| Spuštění virtuálního počítače |`OnStart()` |Není k dispozici |
| Zastavení virtuálního počítače |`OnStop()` |Není k dispozici |
| Otevřete naslouchací proces pro požadavky klientů |Není k dispozici |<ul><li> `CreateServiceInstanceListener()`pro bezstavové</li><li>`CreateServiceReplicaListener()`pro stateful</li></ul> |

### <a name="worker-role"></a>Role pracovního procesu
```C#

using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
        }

        public override bool OnStart()
        {
        }

        public override void OnStop()
        {
        }
    }
}

```

### <a name="service-fabric-stateless-service"></a>Bezstavové služby Service Fabric
```C#

using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

namespace Stateless1
{
    public class Stateless1 : StatelessService
    {
        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
        }

        protected override Task RunAsync(CancellationToken cancelServiceInstance)
        {
        }
    }
}

```

Mají obě primární "spustit" přepsání ve které toobegin zpracování. Combine služby Service Fabric `Run`, `Start`, a `Stop` do jeden vstupní bod, `RunAsync`. Služby by měl začínat práce při `RunAsync` spustí a by se měla zastavit pracovat, když hello `RunAsync` signalizace CancellationToken metody. 

Existuje několik hlavní rozdíly mezi hello životního cyklu a životního cyklu služeb rolí pracovního procesu a Service Fabric:

* **Životní cyklus:** hello největších rozdílem je, že Role pracovního procesu je virtuální počítač a stejně tak životního cyklu vázanou toohello virtuální počítač, který zahrnuje události spustí nebo zastaví hello virtuálních počítačů. Služba Service Fabric má životního cyklu, která je oddělená od životního cyklu hello virtuálních počítačů, takže nebude obsahovat událostí při hello hostitele virtuálního počítače nebo počítače spuštění a zastavení, protože nesouvisí.
* **Doba života:** instance Role pracovního procesu bude recyklovat, pokud hello `Run` metoda ukončí. Hello `RunAsync` metoda ve službě Service Fabric ale můžete spustit toocompletion a instance služby hello zůstanou nahoru. 

Service Fabric představuje vstupní bod instalaci volitelné komunikace pro služby, které naslouchat žádostem klienta. Hello RunAsync i komunikace vstupní bod jsou volitelné přepsání v Service Fabric služeb - služby může zvolit tooonly naslouchání tooclient požadavky, nebo spustit pouze smyčku zpracování, nebo obě -, které je důvod, proč hello metodě RunAsync je povoleno tooexit bez restartování instance služby hello, protože ho může pokračovat toolisten pro požadavky klientů.

## <a name="application-api-and-environment"></a>Aplikace rozhraní API a prostředí
Hello cloudové služby prostředí rozhraní API poskytuje informace a funkce pro hello aktuální instance virtuálního počítače a také informace o další instance rolí virtuálních počítačů. Service Fabric obsahuje informace související s tooits runtime a některé informace o hello uzlu služby je v aktuálně spuštěna. 

| **Úloha prostředí** | **Cloud Services** | **Service Fabric** |
| --- | --- | --- |
| Nastavení konfigurace a oznámení o změně |`RoleEnvironment` |`CodePackageActivationContext` |
| Lokální úložiště |`RoleEnvironment` |`CodePackageActivationContext` |
| Informace o koncovém |`RoleInstance` <ul><li>Aktuální instance:`RoleEnvironment.CurrentRoleInstance`</li><li>Další role a instance:`RoleEnvironment.Roles`</li> |<ul><li>`NodeContext`pro aktuální uzel adresu</li><li>`FabricClient`a `ServicePartitionResolver` pro koncový bod zjišťování služby</li> |
| Emulace prostředí |`RoleEnvironment.IsEmulated` |Není k dispozici |
| Událost souběžných změny |`RoleEnvironment` |Není k dispozici |

## <a name="configuration-settings"></a>Nastavení konfigurace
Nastavení konfigurace v cloudové služby jsou nastavené pro roli virtuálního počítače a použít tooall instance dané role virtuálních počítačů. Tato nastavení jsou páry klíč hodnota nastavená v souborech ServiceConfiguration.*.cscfg a je možné přistupovat přímo prostřednictvím RoleEnvironment. V Service Fabric nastavení se týká jednotlivě tooeach služby a aplikace tooeach, nikoli tooa virtuální počítač, protože virtuální počítač může být hostitelem více služeb a aplikací. Služba se skládá ze tří balíčky:

* **Kód:** obsahuje hello služby spustitelné soubory, binární soubory, knihovny DLL a další soubory služba potřebuje toorun.
* **Konfigurace:** všechny konfigurační soubory a nastavení služby.
* **Data:** statických dat soubory spojené s hello služby.

Každý z těchto balíčků může být nezávisle verzí a upgradovaný. Podobné tooCloud služby, konfigurační balíček je možné programově přistupovat pomocí rozhraní API a události jsou k dispozici toonotify hello službu ke změně konfigurace balíčku. Soubor souborech Settings.xml slouží pro klíč hodnota konfigurace a programový přístup podobné toohello aplikace v oddílu nastavení souboru App.config. Ale na rozdíl od cloudové služby, konfigurační balíček Service Fabric může obsahovat všechny konfigurační soubory v libovolném formátu toho, jestli je XML, JSON, YAML nebo vlastní binární formát. 

### <a name="accessing-configuration"></a>Přístup ke konfiguraci
#### <a name="cloud-services"></a>Cloud Services
Nastavení konfigurace ze ServiceConfiguration.*.cscfg je možné přistupovat prostřednictvím `RoleEnvironment`. Tato nastavení jsou globálně dostupnou tooall instancí rolí ve hello stejného nasazení cloudové služby.

```C#

string value = RoleEnvironment.GetConfigurationSettingValue("Key");

```

#### <a name="service-fabric"></a>Service Fabric
Každá služba má svůj vlastní balíček individuální konfigurace. Všechny aplikace v clusteru je dostupné žádné předdefinované mechanismus pro globální nastavení konfigurace. Pokud používáte Service Fabric speciální souborech Settings.xml konfigurační soubor v rámci konfigurace balíčku, hodnoty v souborech Settings.xml lze přepsat na úrovni aplikace hello, aby možné nastavení konfigurace na úrovni aplikace.

Nastavení konfigurace se přistupuje v rámci každá instance služby prostřednictvím služby hello `CodePackageActivationContext`.

```C#

ConfigurationPackage configPackage = this.Context.CodePackageActivationContext.GetConfigurationPackageObject("Config");

// Access Settings.xml
KeyedCollection<string, ConfigurationProperty> parameters = configPackage.Settings.Sections["MyConfigSection"].Parameters;

string value = parameters["Key"]?.Value;

// Access custom configuration file:
using (StreamReader reader = new StreamReader(Path.Combine(configPackage.Path, "CustomConfig.json")))
{
    MySettings settings = JsonConvert.DeserializeObject<MySettings>(reader.ReadToEnd());
}

```

### <a name="configuration-update-events"></a>Události aktualizace konfigurace
#### <a name="cloud-services"></a>Cloud Services
Hello `RoleEnvironment.Changed` událostí je použité toonotify v hello prostředí, například o změně konfigurace dojde při změně všech instancí rolí. Toto je aktualizace konfigurace použité tooconsume bez recyklace instance rolí nebo restartování pracovní proces.

```C#

RoleEnvironment.Changed += RoleEnvironmentChanged;

private void RoleEnvironmentChanged(object sender, RoleEnvironmentChangedEventArgs e)
{
   // Get hello list of configuration changes
   var settingChanges = e.Changes.OfType<RoleEnvironmentConfigurationSettingChange>();
foreach (var settingChange in settingChanges) 
   {
      Trace.WriteLine("Setting: " + settingChange.ConfigurationSettingName, "Information");
   }
}

```

#### <a name="service-fabric"></a>Service Fabric
Každý hello tři typy balíčků ve službě - kód, konfigurace a Data - mít události, které instanci služby balíček aktualizace, přidat nebo odebrat. Služby mohou obsahovat více balíčků každého typu. Služba může mít například více balíčků konfigurace se každý jednotlivě verzí a rozšiřitelný. 

Tyto události jsou k dispozici tooconsume změny v balíčky služeb bez restartování instance služby hello.

```C#

this.Context.CodePackageActivationContext.ConfigurationPackageModifiedEvent +=
                    this.CodePackageActivationContext_ConfigurationPackageModifiedEvent;

private void CodePackageActivationContext_ConfigurationPackageModifiedEvent(object sender, PackageModifiedEventArgs<ConfigurationPackage> e)
{
    this.UpdateCustomConfig(e.NewPackage.Path);
    this.UpdateSettings(e.NewPackage.Settings);
}

```

## <a name="startup-tasks"></a>Spuštění úlohy
Spuštění úlohy se akcí, které předtím, než aplikaci spustí. Úloha spuštění je obvykle používanými toorun skripty instalace použitím zvýšených oprávnění. Cloudové služby a Service Fabric podporu spuštění úloh. Hello hlavní rozdíl je, že v cloudové služby, úloha spuštění se vázanou tooa virtuální počítač je součástí instanci role, zatímco v Service Fabric je úloha spuštění služby vázanou tooa, která není vázaná tooany konkrétní virtuální počítač.

| Cloud Services | Service Fabric |
| --- | --- | --- |
| Umístění konfigurace |ServiceDefinition.csdef |
| Oprávnění |"omezený" nebo "se zvýšenými oprávněními" |
| Pořadí úloh |"jednoduché", "pozadí", "popředí" |

### <a name="cloud-services"></a>Cloud Services
V cloudové služby je konfigurovat vstupní bod spuštění pro jednotlivé role v ServiceDefinition.csdef. 

```xml

<ServiceDefinition>
    <Startup>
        <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
            <Environment>
                <Variable name="MyVersionNumber" value="1.0.0.0" />
            </Environment>
        </Task>
    </Startup>
    ...
</ServiceDefinition>

```

### <a name="service-fabric"></a>Service Fabric
V Service Fabric je nakonfigurovaný vstupní bod spuštění pro službu v ServiceManifest.xml:

```xml

<ServiceManifest>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>Startup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    ...
</ServiceManifest>

``` 

## <a name="a-note-about-development-environment"></a>Poznámka o vývojového prostředí
Cloudové služby a služby infrastruktury jsou integrované pomocí sady Visual Studio s šablony projektů a podpora pro ladění, konfigurace a nasazení místně a tooAzure. Cloudové služby a Service Fabric také poskytuje prostředí runtime místní vývoj. Hello rozdílem je, že při hello cloudové služby modulu runtime vývoj emuluje hello prostředí Azure, ve kterém běží, Service Fabric nepoužívá emulátor – používá hello dokončení modulu runtime Service Fabric. Hello Service Fabric prostředí, ve kterém můžete spustit na místním vývojovém počítači je hello stejné prostředí, které běží v produkčním prostředí.

## <a name="next-steps"></a>Další kroky
Další informace o Service Fabric Reliable Services a hello základní rozdíly mezi cloudové služby a toounderstand Architektura aplikace Service Fabric jak tootake výhod hello úplná sada funkcí Service Fabric.

* [Začínáme se službami Reliable Services Service Fabric](service-fabric-reliable-services-quick-start.md)
* [Koncepční Průvodce toohello rozdíly mezi cloudové služby a Service Fabric](service-fabric-cloud-services-migration-differences.md)

<!--Image references-->
[3]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/service-fabric-cloud-service-projects.png
[4]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/worker-role-to-stateless-service.png
