---
title: "Pomocí Elasticsearch jako úložiště trasování aplikace Service Fabric | Microsoft Docs"
description: "Popisuje, jak můžou aplikace Service Fabric pomocí Elasticsearch a Kibana index, úložiště a hledání prostřednictvím trasování aplikací (protokoly)"
services: service-fabric
documentationcenter: .net
author: karolz-ms
manager: adegeo
editor: 
ms.assetid: e59b0c39-e468-4d9e-b453-d5f2a8ad20d8
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/07/2017
ms.author: karolz@microsoft.com
redirect_url: /azure/service-fabric/service-fabric-diagnostics-event-aggregation-eventflow
ms.openlocfilehash: 2d2ceceea131b41ad1a1735aaa2a859d035ab098
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="use-elasticsearch-as-a-service-fabric-application-trace-store"></a>Uložení Elasticsearch použijte jako trasování aplikace Service Fabric
## <a name="introduction"></a>Úvod
Tento článek popisuje, jak [Azure Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric/) aplikace můžete použít **Elasticsearch** a **Kibana** pro úložiště trasování aplikací, indexování a vyhledávání. [Elasticsearch](https://www.elastic.co/guide/index.html) open source, distribuované a škálovatelné v reálném čase vyhledávání a analýzy modul, který je vhodná pro tuto úlohu. Je můžete nainstalovat na Windows a Linux virtuální počítače běžící v Microsoft Azure. Elasticsearch efektivně zpracovávat *strukturovaných* trasování vytvořeného pomocí technologie, jako třeba **trasování událostí pro Windows (ETW)**.

Trasování událostí pro Windows používá modulu runtime Service Fabric na zdroj diagnostické informace (trasování). Je doporučené metody pro aplikace Service Fabric na příliš zdrojového jejich diagnostické informace. Pomocí stejného mechanismu umožňuje korelace mezi trasování zadaná runtime a zadané aplikace a díky při odstraňování problémů. Šablony projektu Service Fabric v sadě Visual Studio zahrnují protokolování API (podle .NET **EventSource** třída) který vysílá trasování ETW ve výchozím nastavení. Obecné informace o aplikace Service Fabric trasování pomocí trasování událostí pro Windows, najdete v části [monitorování a Diagnostika služby v instalačním programu místním počítači vývoj](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

Pro trasování objeví v Elasticsearch potřebují k zaznamenané v uzlech clusteru Service Fabric v reálném čase (když aplikace běží) a odesílají do Elasticsearch koncový bod. Existují dvě hlavní možnosti pro zaznamenání trasování:

* **Proces zaznamenávání trasování**  
  Aplikace, nebo přesněji řečeno, proces služby je odpovědná za zasílání diagnostických dat trasování úložiště (Elasticsearch).
* **Zaznamenání mimo proces trasování**  
  Samostatným agentem je zaznamenání trasování z procesu služby nebo procesy a odesílá je do úložiště trasování.

Níže jsme popisují, jak nastavit Elasticsearch v Azure, popisují specialisté a cons pro obě možnosti zachycení a vysvětlují, jak nakonfigurovat službu Service Fabric k odesílání dat do Elasticsearch.

## <a name="set-up-elasticsearch-on-azure"></a>Nastavit Elasticsearch v Azure
Nejjednodušší způsob, jak nastavit službu Elasticsearch v Azure je prostřednictvím [ **šablon Azure Resource Manageru**](../azure-resource-manager/resource-group-overview.md). O komplexní [šablony rychlý start Azure Resource Manageru pro Elasticsearch](https://github.com/Azure/azure-quickstart-templates/tree/master/elasticsearch) je k dispozici z úložiště Azure Quickstart šablony. Tato šablona používá samostatné úložiště účtů pro jednotky škálování (skupiny uzly). Můžete také vytvářet, klienta a serveru uzly s různými konfiguracemi a různé počty datových disků připojených.

Tady používáme jinou šablonu, nazývá **ES MultiNode** z [úložiště Azure diagnostických nástrojů](https://github.com/Azure/azure-diagnostics-tools). Tato šablona je jednodušší použít, a vytvoří cluster služby Elasticsearch chráněn základní ověřování protokolu HTTP. Než budete pokračovat, stáhněte úložiště z Githubu k vašemu počítači (buď klonování úložiště nebo stažení souboru zip). ES-MultiNode šablony je umístěn ve složce se stejným názvem.

### <a name="prepare-a-machine-to-run-elasticsearch-installation-scripts"></a>Příprava počítačů ke spouštění skriptů instalace Elasticsearch
Nejjednodušší způsob, jak používat šablony ES MultiNode je pomocí poskytnutého skriptu prostředí Azure PowerShell názvem `CreateElasticSearchCluster`. Pokud chcete použít tento skript, musíte nainstalovat moduly prostředí PowerShell a nástroj nazvaný **openssl**. K tomu je potřeba pro vytvoření klíč SSH, který můžete použít ke správě clusteru Elasticsearch vzdáleně.

`CreateElasticSearchCluster`skript je určená pro snadné použití se šablonou ES MultiNode z počítače s Windows. Je možné použít šablonu na počítač s jiným systémem než Windows, ale tento scénář je nad rámec tohoto článku.

1. Pokud jste nenainstalovali je již, nainstalujte [ **modulů prostředí Azure PowerShell**](http://aka.ms/webpi-azps). Po zobrazení výzvy klikněte na tlačítko **spustit**, pak **nainstalovat**. Azure PowerShell 1.3 nebo novější je povinný.
2. **Openssl** nástroj je součástí distribuce [ **Git pro Windows**](http://www.git-scm.com/downloads). Pokud jste tak ještě neučinili, nainstalujte [Git pro Windows](http://www.git-scm.com/downloads) nyní. (Výchozí možnosti instalace jsou OK).
3. Za předpokladu, že Git byl nainstalován, ale není zahrnutý v systémové cestě, otevřete okno Microsoft Azure PowerShell a spusťte následující příkazy:
   
    ```powershell
    $ENV:PATH += ";<Git installation folder>\usr\bin"
    $ENV:OPENSSL_CONF = "<Git installation folder>\usr\ssl\openssl.cnf"
    ```
   
    Nahraďte `<Git installation folder>` Git umístění na počítači; výchozí hodnota je **"C:\Program Files\Git"**. Všimněte si středníkem na začátku první cesty.
4. Ujistěte se, že jste přihlášeni do Azure (prostřednictvím [ `Add-AzureRmAccount` ](https://msdn.microsoft.com/library/mt619267.aspx) rutiny) a že jste vybrali odběr, který se má použít k vytvoření clusteru elastické vyhledávání. Můžete ověřit, že je vybrané správné předplatné pomocí `Get-AzureRmContext` a `Get-AzureRmSubscription` rutiny.
5. Pokud jste tak ještě neučinili, přejděte do složky ES MultiNode aktuální adresář.

### <a name="run-the-createelasticsearchcluster-script"></a>Spusťte skript CreateElasticSearchCluster
Než spustíte skript, otevřete `azuredeploy-parameters.json` souboru a ověřte nebo zadejte hodnoty pro parametry skriptu. Jsou k dispozici následující parametry:

| Název parametru | Popis |
| --- | --- |
| dnsNameForLoadBalancerIP |Název, který se používá k vytvoření veřejně viditelný název DNS pro cluster elastické vyhledávání (přidáním domény oblast Azure pro zadaný název). Například pokud je tato hodnota parametru "myBigCluster" a vybraný oblast Azure je západní USA, výsledný název DNS clusteru je myBigCluster.westus.cloudapp.azure.com. <br /><br />Tento název slouží také jako název kořenové pro mnoho artefakty přidružené clusteru elastické vyhledávání, například názvy uzlu data. |
| adminUsername |Název účtu správce pro správu clusteru elastické vyhledávání (odpovídající klíče SSH jsou automaticky generovány.). |
| dataNodeCount |Počet uzlů v clusteru elastické vyhledávání. Aktuální verze skriptu nerozlišuje mezi uzly dat a dotazu. všechny uzly přehrát obě role. Výchozí hodnota je 3 uzly. |
| dataDiskSize |Velikost datových disků (v GB), který je přidělen pro každý uzel data. Každý uzel přijme 4 datových disků, výhradně vyhrazený pro elastické vyhledávací službu. |
| Oblast |Název oblasti Azure, kde by měl být umístěn clusteru elastické vyhledávání. |
| esUserName |Uživatelské jméno uživatele, který je nakonfigurován tak, aby měl přístup ke clusteru ES (přičemž podléhá základní ověřování protokolem HTTP). Heslo není součástí souboru parametrů a je třeba zadat při `CreateElasticSearchCluster` vyvolání skriptu. |
| vmSizeDataNodes |Velikost virtuálního počítače Azure pro uzly clusteru elastické vyhledávání. Výchozí hodnota je Standard_D2. |

Nyní jste připraveni ke spuštění skriptu. Vydejte následující příkaz:

```powershell
CreateElasticSearchCluster -ResourceGroupName <es-group-name> -Region <azure-region> -EsPassword <es-password>
```

kde 

| Název parametru skriptu | Popis |
| --- | --- |
| `<es-group-name>` |Název skupiny prostředků Azure, která bude obsahovat všechny prostředky clusteru elastické vyhledávání. |
| `<azure-region>` |Název oblasti Azure, kde by měl být vytvořen cluster elastické vyhledávání. |
| `<es-password>` |Heslo pro elastické hledat uživatele. |

> [!NOTE]
> Pokud dojde NullReferenceException z rutiny Test-AzureResourceGroup, jste zapomněli pro přihlášení k Azure (`Add-AzureRmAccount`).
> 
> 

Pokud dojde k chybě z spouštění skriptu a zjistíte, že chyba způsobila hodnotu parametru nesprávný šablony, opravte soubor parametrů a znovu spusťte skript s názvem skupiny prostředků jiného. Můžete také použít stejný název skupiny prostředků a nechat skript vyčištění starý přidáním `-RemoveExistingResourceGroup` parametr vyvolání skriptu.

### <a name="result-of-running-the-createelasticsearchcluster-script"></a>Výsledek skriptu CreateElasticSearchCluster
Po spuštění `CreateElasticSearchCluster` skriptu, bude vytvořena následující hlavní artefakty. V tomto příkladu předpokládáme, že jste použili "myBigCluster" jako hodnotu `dnsNameForLoadBalancerIP` parametr a zda je oblast, kde jste vytvořili clusteru západní USA.

| Artefaktů | Název, umístění a poznámky |
| --- | --- |
| Klíč SSH pro vzdálenou správu |myBigCluster.key souboru (v adresáři, ze kterého se spouštěl CreateElasticSearchCluster). <br /><br />Tento soubor klíče slouží k připojení k uzlu správce a (prostřednictvím Správce uzel) pro data uzly v clusteru. |
| Správce uzlu |myBigCluster admin.westus.cloudapp.azure.com <br /><br />Vyhrazený virtuální počítač pro vzdálenou správu clusteru Elasticsearch – pouze ten, který umožňuje externí připojení SSH. Běží na stejné virtuální síti jako všechny uzly v clusteru Elasticsearch, ale nejde spustit žádné služby, Elasticsearch. |
| Datové uzly |myBigCluster1... myBigCluster*N* <br /><br />Datové uzly, které jsou spuštěny služby Elasticsearch a Kibana. Můžete připojit pomocí protokolu SSH do každého uzlu, ale pouze prostřednictvím Správce uzlu. |
| Elasticsearch clusteru |http://myBigCluster.westus.cloudapp.Azure.com/ES/ <br /><br />Primární koncový bod clusteru Elasticsearch (Poznámka příponou /es). Je chráněn základní ověřování protokolu HTTP (přihlašovací údaje byly zadané parametry esUserName nebo esPassword ES MultiNode šablony). Cluster má také hlavičky modulu plug-in nainstalovaný (http://myBigCluster.westus.cloudapp.azure.com/es/_plugin/head) pro správu základní cluster. |
| Kibana služby |http://myBigCluster.westus.cloudapp.Azure.com <br /><br />Službu Kibana jsou nastaveny na zobrazení dat z vytvořený Elasticsearch cluster. Je chráněn stejné ověřovací pověření jako samotného clusteru. |

## <a name="in-process-versus-out-of-process-trace-capturing"></a>V procesu versus zaznamenávání mimo proces trasování
V úvodu, jsme uvedených dva základní způsoby shromažďování diagnostických dat: v rámci procesu a mimo proces. Každý má silné a slabé stránky.

Výhody **proces zaznamenávání trasování** zahrnují:

1. *Jednoduchá konfigurace a nasazení*
   
   * Konfigurace shromažďování diagnostických dat je právě součástí konfigurace aplikace. Je snadné vždy synchronizujte jej "" s zbývající aplikace.
   * Konfigurace pro aplikaci nebo službě,-je snadno dosažitelné.
   * Trasování mimo proces zaznamenávání obvykle vyžaduje samostatné nasazení a konfigurace diagnostiky agenta, který je velmi Správce úloh a potenciální příčinu chyby. Technologie konkrétní agenta často umožňuje jenom jednu instanci agenta na virtuální počítač (uzel). To znamená že konfigurace pro kolekci konfigurace diagnostiky sdílí všechny aplikace a služby spuštěné v tomto uzlu.
2. *Flexibilita*
   
   * Aplikace může posílat data, bez ohledu na účelu, také je zde knihovna klienta, který podporuje systém cílových dat úložiště. Můžete přidat nové jímky podle potřeby.
   * Komplexní zachycení, filtrování a agregace dat pravidla mohou být implementována.
   * Trasování mimo proces zachycení je často omezena jímky dat, které podporuje agenta. Někteří agenti jsou extensible.
3. *Přístup k datům interní aplikace a kontext*
   
   * Subsystém diagnostiky běžících v rámci procesu aplikace nebo služba může snadno posílení trasování s kontextové informace.
   * V metodě mimo proces musí se poslat data agentu prostřednictvím některé mechanismus komunikace mezi procesy, jako je například trasování událostí pro Windows. Tento mechanismus může použít další omezení.

Výhody **trasování mimo proces zaznamenávání** zahrnují:

1. *Možnost monitorování výpisy stavu systému aplikaci a shromažďování*
   
   * Trasování v procesu zachycení může neúspěšné, pokud se nepodaří spustit nebo dojde k chybě aplikace. Nezávislého agenta má mnohem větší šanci zachycení zásadní informace o odstraňování potíží.<br /><br />
2. *Vyspělosti, odolné a principy výkonu*
   
   * Agenta vyvinuté dodavatelem platformy (například Microsoft Azure Diagnostics agent) byl souladu přísných testování a boj-posílení zabezpečení.
   * Pomocí trasování v procesu zachycení se musí dát pozor na zkontrolujte, že aktivitu odesílání diagnostických dat z aplikační proces narušovat hlavní úlohy aplikace nebo nastat problémy načasování nebo výkonu. Nezávisle spuštěné agenta je méně náchylná k těmto problémům a je určená speciálně pro omezení jeho dopad na systém.

Je možné kombinovat a těžit z obou přístupů. Ve skutečnosti může být nejlepším řešením pro mnoho aplikací.

Tady používáme **Microsoft.Diagnostic.Listeners knihovny** a zachycení k odesílání dat z aplikace Service Fabric do clusteru Elasticsearch trasování v procesu.

## <a name="use-the-listeners-library-to-send-diagnostic-data-to-elasticsearch"></a>Posílat diagnostická data do Elasticsearch pomocí knihovny – moduly naslouchání
Knihovna Microsoft.Diagnostic.Listeners je součástí aplikace Service Fabric PartyCluster ukázka. Pro použití:

1. Stáhněte si [ukázka PartyCluster](https://github.com/Azure-Samples/service-fabric-dotnet-management-party-cluster) z Githubu.
2. Zkopírujte projekty Microsoft.Diagnostics.Listeners a Microsoft.Diagnostics.Listeners.Fabric (celý složky) z adresáře ukázka PartyCluster ke složce řešení aplikace, která má posílat data do Elasticsearch.
3. Otevřete řešení cíl, klikněte pravým tlačítkem na uzel řešení v Průzkumníku řešení a zvolte **přidat existující projekt**. Přidáte projekt Microsoft.Diagnostics.Listeners k řešení. Opakujte stejný Microsoft.Diagnostics.Listeners.Fabric projektu.
4. Přidáte odkaz na projekt z vaší služby projekty do dvou přidané projektů. (Každá služba, která by měla při odesílání dat do Elasticsearch by měl odkazovat Microsoft.Diagnostics.EventListeners a Microsoft.Diagnostics.EventListeners.Fabric).
   
    ![Odkazy na projekt Microsoft.Diagnostics.EventListeners a Microsoft.Diagnostics.EventListeners.Fabric knihoven][1]

### <a name="service-fabric-general-availability-release-and-microsoftdiagnosticstracing-nuget-package"></a>Verze služby Fabric obecné dostupnosti a balíček Microsoft.Diagnostics.Tracing Nuget
Aplikace sestavené s verzí Service Fabric obecné dostupnosti (2.0.135 vydané 31. března 2016) cílové **rozhraní .NET Framework 4.5.2**. Tato verze je nejvyšší verze rozhraní .NET Framework v době verze GA nepodporuje v Azure. Tato verze rozhraní bohužel chybí určité EventListener rozhraní API, která potřebuje Microsoft.Diagnostics.Listeners knihovny. Protože EventSource (komponenta, která je základem protokolování rozhraní API v aplikacích prostředků infrastruktury) a EventListener jsou úzce spojeny, každý projekt, který používá knihovnu Microsoft.Diagnostics.Listeners musíte použít alternativní implementace EventSource. Tato implementace poskytuje **balíček Microsoft.Diagnostics.Tracing Nuget**, vytvořené společností Microsoft. Balíček není plně zpětně kompatibilní s EventSource součástí rozhraní, takže beze změn kódu by měla být nutné než změny odkazované oboru názvů.

Pokud chcete začít používat Microsoft.Diagnostics.Tracing implementace třídy EventSource, postupujte podle těchto kroků pro každý projekt služby, který potřebuje k odesílání dat do Elasticsearch:

1. Klikněte pravým tlačítkem na projekt služby a zvolte **spravovat balíčky Nuget**.
2. Přepnout na nuget.org zdroj balíčku (Pokud již není vybrána) a vyhledejte "**Microsoft.Diagnostics.Tracing**".
3. Nainstalujte `Microsoft.Diagnostics.Tracing.EventSource` balíčku (a její závislosti).
4. Otevřete **ServiceEventSource.cs** nebo **ActorEventSource.cs** souborů ve vašem projektu service a nahraďte `using System.Diagnostics.Tracing` direktivy nad soubor s `using Microsoft.Diagnostics.Tracing` – direktiva.

Tyto kroky nebude nutné jednou **rozhraní .NET Framework 4.6** podporuje Microsoft Azure.

### <a name="elasticsearch-listener-instantiation-and-configuration"></a>Vytváření instancí Elasticsearch naslouchací proces a konfigurace
Poslední krok pro odesílání diagnostických dat do Elasticsearch je vytvoření instance `ElasticSearchListener` a nakonfigurujte ho s Elasticsearch data připojení. Naslouchací proces automaticky zaznamená všechny události vyvolané prostřednictvím EventSource tříd definovaných v projektu služby. Je nutné aktivní po dobu životnosti služby, takže je nejlepší místo k jeho vytvoření v kódu inicializace služby. Zde je, jak může vypadat kód inicializace pro bezstavové služby po potřebné změny (přidání zdůraznit v komentářích počínaje `****`):

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Runtime;

// **** Add the following directives
using Microsoft.Diagnostics.EventListeners;
using Microsoft.Diagnostics.EventListeners.Fabric;

namespace Stateless1
{
    internal static class Program
    {
        /// <summary>
        /// This is the entry point of the service host process.
        /// </summary>        
        private static void Main()
        {
            try
            {
                // **** Instantiate ElasticSearchListener
                var configProvider = new FabricConfigurationProvider("ElasticSearchEventListener");
                ElasticSearchListener esListener = null;
                if (configProvider.HasConfiguration)
                {
                    esListener = new ElasticSearchListener(configProvider, new FabricHealthReporter("ElasticSearchEventListener"));
                }

                // The ServiceManifest.XML file defines one or more service type names.
                // Registering a service maps a service type name to a .NET type.
                // When Service Fabric creates an instance of this service type,
                // an instance of the class is created in this host process.

                ServiceRuntime.RegisterServiceAsync("Stateless1Type", 
                    context => new Stateless1(context)).GetAwaiter().GetResult();

                ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless1).Name);

                // Prevents this host process from terminating so services keep running.
                Thread.Sleep(Timeout.Infinite);

                // **** Ensure that the ElasticSearchListner instance is not garbage-collected prematurely
                GC.KeepAlive(esListener);
            }
            catch (Exception e)
            {
                ServiceEventSource.Current.ServiceHostInitializationFailed(e);
                throw;
            }
        }
    }
}
```

Data připojení Elasticsearch by mělo být uvedeno v samostatném oddílu v konfiguračním souboru služby (**PackageRoot\Config\Settings.xml**). Název oddílu musí odpovídat hodnotě předaný `FabricConfigurationProvider` konstruktoru, například:

```xml
<Section Name="ElasticSearchEventListener">
  <Parameter Name="serviceUri" Value="http://myBigCluster.westus.cloudapp.azure.com/es/" />
  <Parameter Name="userName" Value="(ES user name)" />
  <Parameter Name="password" Value="(ES user password)" />
  <Parameter Name="indexNamePrefix" Value="myapp" />
</Section>
```
Hodnoty `serviceUri`, `userName` a `password` parametry odpovídají adresa koncového bodu clusteru Elasticsearch, Elasticsearch uživatelské jméno a heslo, v uvedeném pořadí. `indexNamePrefix`je předpona pro indexy Elasticsearch; Knihovna Microsoft.Diagnostics.Listeners denně vytvoří nový index pro svoje data.

### <a name="verification"></a>Ověření
A to je vše! Nyní vždy, když je služba spuštěna, spustí odesílání trasování ke službě Elasticsearch zadaný v konfiguraci. Můžete ověřit, že to tak, že otevřete uživatelské rozhraní Kibana přidružený k instanci Elasticsearch cíl. V našem příkladu je adresu stránky http://myBigCluster.westus.cloudapp.azure.com/. Zkontrolujte, zda indexy s předponou názvu zvolené pro `ElasticSearchListener` instance skutečně byly vytvořeny a naplněny s daty.

![Zobrazuje Kibana PartyCluster aplikace události][2]

## <a name="next-steps"></a>Další kroky
* [Další informace o diagnostice a monitorování služby Service Fabric](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

<!--Image references-->
[1]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/listener-lib-references.png
[2]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/kibana.png
