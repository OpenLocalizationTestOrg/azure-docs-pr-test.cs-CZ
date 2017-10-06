---
title: "aaaUsing Elasticsearch jako úložiště trasování aplikace Service Fabric | Microsoft Docs"
description: "Popisuje, jak můžete použít aplikace Service Fabric Elasticsearch a Kibana toostore, rejstřík a vyhledávání pomocí trasování aplikací (protokoly)"
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
ms.openlocfilehash: b5977c54e69319e3caa376e44a02f971b66a3254
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-elasticsearch-as-a-service-fabric-application-trace-store"></a>Uložení Elasticsearch použijte jako trasování aplikace Service Fabric
## <a name="introduction"></a>Úvod
Tento článek popisuje, jak [Azure Service Fabric](https://azure.microsoft.com/documentation/services/service-fabric/) aplikace můžete použít **Elasticsearch** a **Kibana** pro úložiště trasování aplikací, indexování a vyhledávání. [Elasticsearch](https://www.elastic.co/guide/index.html) open source, distribuované a škálovatelné v reálném čase vyhledávání a analýzy modul, který je vhodná pro tuto úlohu. Je můžete nainstalovat na Windows a Linux virtuální počítače běžící v Microsoft Azure. Elasticsearch efektivně zpracovávat *strukturovaných* trasování vytvořeného pomocí technologie, jako třeba **trasování událostí pro Windows (ETW)**.

Trasování událostí pro Windows používá Service Fabric toosource diagnostické informace o běhu programu (trasování). Je, že hello doporučené metody pro toosource aplikace Service Fabric diagnostické informace, příliš. Pomocí hello shodný mechanismus umožňuje korelace mezi trasování zadaná runtime a zadané aplikace a díky při odstraňování problémů. Šablony projektu Service Fabric v sadě Visual Studio zahrnují protokolování API (podle hello .NET **EventSource** třída) který vysílá trasování ETW ve výchozím nastavení. Obecné informace o aplikace Service Fabric trasování pomocí trasování událostí pro Windows, najdete v části [monitorování a Diagnostika služby v instalačním programu místním počítači vývoj](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

Aby hello trasování tooshow nahoru v Elasticsearch potřebují mít toobe zaznamenané v uzlech clusteru Service Fabric hello v reálném čase (když je spuštěna aplikace hello) a odešle tooan Elasticsearch koncový bod. Existují dvě hlavní možnosti pro zaznamenání trasování:

* **Proces zaznamenávání trasování**  
  aplikace Hello nebo přesněji řečeno, proces služby je odpovědná za zasílání out hello diagnostických dat toohello trasování úložiště (Elasticsearch).
* **Zaznamenání mimo proces trasování**  
  Samostatným agentem je zaznamenání trasování z procesu služby hello nebo procesy a odesláním toohello trasování úložiště.

Níže jsme popisují, jak tooset až Elasticsearch v Azure, a popisují hello specialisté a cons pro obě možnosti zachycení a vysvětlují, jak tooconfigure Service Fabric služby toosend data tooElasticsearch.

## <a name="set-up-elasticsearch-on-azure"></a>Nastavit Elasticsearch v Azure
Hello tooset nejjednodušší způsob, jak si hello Elasticsearch služby v Azure je prostřednictvím [ **šablon Azure Resource Manageru**](../azure-resource-manager/resource-group-overview.md). O komplexní [šablony rychlý start Azure Resource Manageru pro Elasticsearch](https://github.com/Azure/azure-quickstart-templates/tree/master/elasticsearch) je k dispozici z úložiště Azure Quickstart šablony. Tato šablona používá samostatné úložiště účtů pro jednotky škálování (skupiny uzly). Můžete také vytvářet, klienta a serveru uzly s různými konfiguracemi a různé počty datových disků připojených.

Tady používáme jinou šablonu, nazývá **ES MultiNode** z hello [úložiště Azure diagnostických nástrojů](https://github.com/Azure/azure-diagnostics-tools). Tato šablona je snazší toouse a vytvoří cluster služby Elasticsearch chráněn základní ověřování protokolu HTTP. Než budete pokračovat, stáhněte hello úložiště z Githubu tooyour počítače (buď klonování úložiště hello nebo stažení souboru zip). Šablona Hello ES MultiNode se nachází ve složce hello s hello stejný název.

### <a name="prepare-a-machine-toorun-elasticsearch-installation-scripts"></a>Příprava počítače toorun Elasticsearch skripty instalace
Hello nejjednodušší způsob, jak toouse hello ES MultiNode šablona je pomocí poskytnutého skriptu prostředí Azure PowerShell názvem `CreateElasticSearchCluster`. toouse tento skript, budete potřebovat tooinstall moduly prostředí PowerShell a nástroj nazvaný **openssl**. Hello druhé je zapotřebí pro vytvoření klíče SSH, které můžou být použité tooadminister clusteru Elasticsearch vzdáleně.

`CreateElasticSearchCluster`skript je určená pro snadné použití pomocí šablony hello ES MultiNode z počítače s Windows. Je možné toouse hello šablony na počítač s jiným systémem než Windows, ale tento scénář je nad rámec tohoto článku hello.

1. Pokud jste nenainstalovali je již, nainstalujte [ **modulů prostředí Azure PowerShell**](http://aka.ms/webpi-azps). Po zobrazení výzvy klikněte na tlačítko **spustit**, pak **nainstalovat**. Azure PowerShell 1.3 nebo novější je povinný.
2. Hello **openssl** nástroj je součástí distribuce hello [ **Git pro Windows**](http://www.git-scm.com/downloads). Pokud jste tak ještě neučinili, nainstalujte [Git pro Windows](http://www.git-scm.com/downloads) nyní. (Možnosti instalace výchozí hello jsou OK).
3. Za předpokladu, že Git byl nainstalován, ale není zahrnutý v systémové cestě hello, otevřete okno Microsoft Azure PowerShell a spusťte následující příkazy hello:
   
    ```powershell
    $ENV:PATH += ";<Git installation folder>\usr\bin"
    $ENV:OPENSSL_CONF = "<Git installation folder>\usr\ssl\openssl.cnf"
    ```
   
    Nahraďte hello `<Git installation folder>` hello Git umístění na počítači; výchozí hodnota hello je **"C:\Program Files\Git"**. Všimněte si hello středníkem od začátku hello hello první cesty.
4. Ujistěte se, že jste přihlášeni tooAzure (prostřednictvím [ `Add-AzureRmAccount` ](https://msdn.microsoft.com/library/mt619267.aspx) rutiny) a zda jste vybrali hello předplatné, které by měl být použit toocreate cluster elastické vyhledávání. Můžete ověřit, že je vybrané správné předplatné pomocí `Get-AzureRmContext` a `Get-AzureRmSubscription` rutiny.
5. Pokud jste tak ještě neučinili, změňte hello aktuální adresář toohello ES MultiNode.

### <a name="run-hello-createelasticsearchcluster-script"></a>Spusťte skript CreateElasticSearchCluster hello
Před spuštěním skriptu hello, otevřete hello `azuredeploy-parameters.json` souboru a ověřte nebo zadejte hodnoty pro parametry skriptu hello. jsou k dispozici Hello následující parametry:

| Název parametru | Popis |
| --- | --- |
| dnsNameForLoadBalancerIP |Hello název, který je použité toocreate hello veřejně viditelný DNS název clusteru elastické vyhledávání hello (přidáním hello oblast Azure domény toohello zadaný název). Například pokud je tato hodnota parametru "myBigCluster" a hello vybrali oblast Azure je západní USA, hello výsledný název DNS pro hello cluster je myBigCluster.westus.cloudapp.azure.com. <br /><br />Tento název slouží také jako název kořenové pro mnoho artefakty přidružené hello elastické vyhledávání clusterem, jako jsou názvy data. |
| adminUsername |Název Hello hello správce účtu pro správu clusteru elastické vyhledávání hello (odpovídající klíče SSH jsou automaticky generovány.). |
| dataNodeCount |Hello počet uzlů v clusteru elastické vyhledávání hello. aktuální verze Hello hello skriptu nerozlišuje mezi uzly dat a dotazu. všechny uzly přehrát obě role. Výchozí hodnoty too3 uzlů. |
| dataDiskSize |velikost Hello, datových disků (v GB), který je přidělen pro každý uzel data. Každý uzel přijme 4 datových disků, výhradně vyhrazené tooElastic službu vyhledávání. |
| Oblast |Hello název oblasti Azure, kde by měl být umístěn hello elastické vyhledávání clusteru. |
| esUserName |Hello uživatelské jméno hello uživatele, který je nakonfigurován toohave přístup tooES clusteru (subjektu tooHTTP základní ověřování). Hello heslo není součástí souboru parametrů a je třeba zadat při `CreateElasticSearchCluster` vyvolání skriptu. |
| vmSizeDataNodes |Hello velikost virtuálního počítače Azure pro uzly clusteru elastické vyhledávání. TooStandard_D2 výchozí hodnoty. |

Teď je připraven toorun hello skriptu. Problém hello následující příkaz:

```powershell
CreateElasticSearchCluster -ResourceGroupName <es-group-name> -Region <azure-region> -EsPassword <es-password>
```

kde 

| Název parametru skriptu | Popis |
| --- | --- |
| `<es-group-name>` |Hello název skupiny prostředků Azure hello, která bude obsahovat všechny prostředky clusteru elastické vyhledávání. |
| `<azure-region>` |Název Hello hello oblast Azure, kde by se měl vytvořit hello elastické vyhledávání clusteru. |
| `<es-password>` |Hello heslo pro hello elastické hledat uživatele. |

> [!NOTE]
> Pokud dojde NullReferenceException z rutiny Test-AzureResourceGroup hello, že jste zapomněli toolog na tooAzure (`Add-AzureRmAccount`).
> 
> 

Pokud dojde k chybě z skriptu hello a zjistíte, hello chyba způsobila hodnotu parametru nesprávný šablony, opravte hello soubor parametrů a znovu spusťte hello skript s názvem skupiny prostředků jiného. Také můžete opakovaně použít hello stejný název skupiny prostředků a mít hello skript vyčištění hello starý přidáním hello `-RemoveExistingResourceGroup` parametr toohello skriptu volání.

### <a name="result-of-running-hello-createelasticsearchcluster-script"></a>Výsledek skriptu CreateElasticSearchCluster hello
Po spuštění hello `CreateElasticSearchCluster` skriptu, bude vytvořena následující hlavní artefakty hello. V tomto příkladu předpokládáme, že jste už použili "myBigCluster" jako hodnota hello hello `dnsNameForLoadBalancerIP` parametr a že hello oblasti, kde jste vytvořili hello clusteru je západní USA.

| Artefaktů | Název, umístění a poznámky |
| --- | --- |
| Klíč SSH pro vzdálenou správu |myBigCluster.key soubor (ve hello adresář, ze které hello se spouštěl CreateElasticSearchCluster). <br /><br />Tento soubor klíče může být použité tooconnect toohello správce uzlu a (prostřednictvím Správce uzel hello) toodata uzly v clusteru hello. |
| Správce uzlu |myBigCluster admin.westus.cloudapp.azure.com <br /><br />Vyhrazený virtuální počítač pro vzdálenou správu clusteru Elasticsearch – hello pouze jeden, který umožňuje externí připojení SSH. Běží na hello nemá stejné virtuální síti jako všechny uzly clusteru Elasticsearch hello, ale nejde spustit žádné služby, Elasticsearch. |
| Datové uzly |myBigCluster1... myBigCluster*N* <br /><br />Datové uzly, které jsou spuštěny služby Elasticsearch a Kibana. Můžete připojit přes SSH tooeach uzlu, ale pouze prostřednictvím hello správce uzlu. |
| Elasticsearch clusteru |http://myBigCluster.westus.cloudapp.Azure.com/ES/ <br /><br />Hello primární koncový bod pro cluster Elasticsearch hello (Poznámka hello /es přípona). Je chráněn základní ověřování protokolu HTTP (hello přihlašovací údaje byly hello zadané parametry esUserName nebo esPassword hello ES MultiNode šablony). Hello cluster má také hello head modulu plug-in nainstalovaný (http://myBigCluster.westus.cloudapp.azure.com/es/_plugin/head) pro správu základní cluster. |
| Kibana služby |http://myBigCluster.westus.cloudapp.Azure.com <br /><br />Hello Kibana služby je nastavený tooshow data z hello vytvořit Elasticsearch clusteru. Je chráněn hello stejné přihlašovací údaje ověřování jako hello cluster sám sebe. |

## <a name="in-process-versus-out-of-process-trace-capturing"></a>V procesu versus zaznamenávání mimo proces trasování
V hello Úvod jsme uvedených dva základní způsoby shromažďování diagnostických dat: v rámci procesu a mimo proces. Každý má silné a slabé stránky.

Výhody hello **proces zaznamenávání trasování** zahrnují:

1. *Jednoduchá konfigurace a nasazení*
   
   * Konfigurace Hello shromažďování diagnostických dat je právě součástí konfigurace aplikace hello. Je snadno tooalways zachovat ji "synchronizována" s hello rest aplikace hello.
   * Konfigurace pro aplikaci nebo službě,-je snadno dosažitelné.
   * Trasování mimo proces zaznamenávání obvykle vyžaduje samostatné nasazení a konfiguraci diagnostiky agenta hello, což je velmi Správce úloh a potenciální příčinu chyby. technologie konkrétní agenta Hello často umožňuje jenom jednu instanci hello agenta na virtuální počítač (uzel). To znamená že konfigurace pro kolekci hello konfigurace diagnostiky hello sdílí všechny aplikace a služby spuštěné v tomto uzlu.
2. *Flexibilita*
   
   * Hello aplikace může odesílat hello data bez ohledu na potřebuje toogo, dokud není klientské knihovny, která podporuje systém úložiště dat hello cílové. Můžete přidat nové jímky podle potřeby.
   * Komplexní zachycení, filtrování a agregace dat pravidla mohou být implementována.
   * Trasování mimo proces zachycení je často omezena jímky hello dat, které hello agent podporuje. Někteří agenti jsou extensible.
3. *Data aplikací toointernal přístup a kontext*
   
   * diagnostické subsystému Hello běžících v rámci procesu aplikace nebo služby hello můžete snadno posílení hello trasování s kontextové informace.
   * V hello přístupu mimo proces, hello data musí být odeslána tooan agent prostřednictvím některé mechanismus komunikace mezi procesy, jako je například trasování událostí pro Windows. Tento mechanismus může použít další omezení.

Výhody hello **trasování mimo proces zaznamenávání** zahrnují:

1. *Hello možnost toomonitor hello aplikaci a shromažďování výpisy stavu systému*
   
   * V procesu trasování zaznamenávání mohou neúspěšné, pokud aplikace hello selže toostart nebo dojde k chybě. Nezávislého agenta má mnohem větší šanci zachycení zásadní informace o odstraňování potíží.<br /><br />
2. *Vyspělosti, odolné a principy výkonu*
   
   * Agenta vyvinuté dodavatelem platformy (například Microsoft Azure Diagnostics agent) byl subjektu toorigorous testování a boj-posílení zabezpečení.
   * Pomocí trasování v procesu zachycení musí dát pozor tooensure že hello aktivitu odesílání diagnostických dat z aplikační proces narušovat hlavní úlohy hello aplikace nebo nastat problémy načasování nebo výkonu. Nezávisle spuštěné agenta je méně náchylná toothese problémy a je speciálně navrženého toolimit jeho dopad na hello systému.

Je možné toocombine a benefit z obou přístupů. Ve skutečnosti může být hello nejlepší řešení pro mnoho aplikací.

Tady používáme hello **Microsoft.Diagnostic.Listeners knihovny** a trasování v procesu hello zaznamenání dat toosend z clusteru Service Fabric aplikace tooan Elasticsearch.

## <a name="use-hello-listeners-library-toosend-diagnostic-data-tooelasticsearch"></a>Použít hello naslouchací procesy knihovny toosend diagnostických dat tooElasticsearch
Knihovna Microsoft.Diagnostic.Listeners Hello je součástí aplikace Service Fabric PartyCluster ukázka. toouse ho:

1. Stáhněte si [hello PartyCluster ukázka](https://github.com/Azure-Samples/service-fabric-dotnet-management-party-cluster) z Githubu.
2. Zkopírujte hello Microsoft.Diagnostics.Listeners a Microsoft.Diagnostics.Listeners.Fabric projekty (celý složky) z hello PartyCluster ukázka toohello řešení složku hello aplikace, která by měla toosend hello data tooElasticsearch .
3. Otevřít řešení hello cíl, klikněte pravým tlačítkem na uzel řešení hello v hello Průzkumníku řešení a zvolte **přidat existující projekt**. Přidejte hello Microsoft.Diagnostics.Listeners projekt toohello řešení. Opakujte stejný hello hello Microsoft.Diagnostics.Listeners.Fabric projektu.
4. Přidáte odkaz na projekt z vaší služby projekty toohello dva přidané projekty. (Každá služba, která by měla toosend data tooElasticsearch by měl odkazovat Microsoft.Diagnostics.EventListeners a Microsoft.Diagnostics.EventListeners.Fabric).
   
    ![Projekt odkazuje na tooMicrosoft.Diagnostics.EventListeners a Microsoft.Diagnostics.EventListeners.Fabric knihovny][1]

### <a name="service-fabric-general-availability-release-and-microsoftdiagnosticstracing-nuget-package"></a>Verze služby Fabric obecné dostupnosti a balíček Microsoft.Diagnostics.Tracing Nuget
Aplikace sestavené s verzí Service Fabric obecné dostupnosti (2.0.135 vydané 31. března 2016) cílové **rozhraní .NET Framework 4.5.2**. Tato verze je hello nejvyšší verzi hello nepodporuje v Azure v době hello hello GA verze rozhraní .NET Framework. Tato verze hello framework bohužel chybí určité EventListener rozhraní API této hello Microsoft.Diagnostics.Listeners musí knihovny. Protože EventSource (hello komponenta, která základem hello protokolování rozhraní API v aplikacích prostředků infrastruktury) a EventListener jsou úzce spojeny, každý projekt, který používá hello Microsoft.Diagnostics.Listeners knihovny, musíte použít alternativní implementace EventSource. Tato implementace poskytuje hello **balíček Microsoft.Diagnostics.Tracing Nuget**, vytvořené společností Microsoft. balíček Hello je plně zpětně kompatibilní s EventSource součástí hello framework, takže beze změn kódu by měla být nutné než změny odkazované oboru názvů.

toostart pomocí hello Microsoft.Diagnostics.Tracing implementace třídy hello EventSource, postupujte takto pro každý projekt služby, který potřebuje tooElasticsearch toosend dat:

1. Klikněte pravým tlačítkem na projekt služby hello a zvolte **spravovat balíčky Nuget**.
2. Přepínač zdroje balíčku nuget.org toohello (Pokud již není vybrána) a vyhledejte "**Microsoft.Diagnostics.Tracing**".
3. Nainstalujte hello `Microsoft.Diagnostics.Tracing.EventSource` balíčku (a její závislosti).
4. Otevřete hello **ServiceEventSource.cs** nebo **ActorEventSource.cs** souborů ve vašem projektu service a nahraďte hello `using System.Diagnostics.Tracing` direktivy nad hello soubor s hello `using Microsoft.Diagnostics.Tracing` – direktiva.

Tyto kroky nebude nutné jednou hello **rozhraní .NET Framework 4.6** podporuje Microsoft Azure.

### <a name="elasticsearch-listener-instantiation-and-configuration"></a>Vytváření instancí Elasticsearch naslouchací proces a konfigurace
Hello poslední krok pro odesílání diagnostických dat tooElasticsearch je toocreate instance `ElasticSearchListener` a nakonfigurujte ho s Elasticsearch data připojení. naslouchací proces Hello automaticky zaznamená všechny události vyvolané prostřednictvím EventSource tříd definovaných v projektu služby hello. Potřebuje toobe zachování během doby života hello služby hello, takže hello nejlepší umístit toocreate, který je v kódu inicializace služby hello. Zde je, jak může vypadat hello inicializace kód pro bezstavové služby po hello potřebné změny (přidání zdůraznit v komentářích počínaje `****`):

```csharp
using System;
using System.Diagnostics;
using System.Fabric;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Runtime;

// **** Add hello following directives
using Microsoft.Diagnostics.EventListeners;
using Microsoft.Diagnostics.EventListeners.Fabric;

namespace Stateless1
{
    internal static class Program
    {
        /// <summary>
        /// This is hello entry point of hello service host process.
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

                // hello ServiceManifest.XML file defines one or more service type names.
                // Registering a service maps a service type name tooa .NET type.
                // When Service Fabric creates an instance of this service type,
                // an instance of hello class is created in this host process.

                ServiceRuntime.RegisterServiceAsync("Stateless1Type", 
                    context => new Stateless1(context)).GetAwaiter().GetResult();

                ServiceEventSource.Current.ServiceTypeRegistered(Process.GetCurrentProcess().Id, typeof(Stateless1).Name);

                // Prevents this host process from terminating so services keep running.
                Thread.Sleep(Timeout.Infinite);

                // **** Ensure that hello ElasticSearchListner instance is not garbage-collected prematurely
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

Data připojení Elasticsearch by mělo být uvedeno v samostatném oddílu v konfiguračním souboru služby hello (**PackageRoot\Config\Settings.xml**). Hello název oddílu hello musí odpovídat toohello předaná hodnota toohello `FabricConfigurationProvider` konstruktoru, například:

```xml
<Section Name="ElasticSearchEventListener">
  <Parameter Name="serviceUri" Value="http://myBigCluster.westus.cloudapp.azure.com/es/" />
  <Parameter Name="userName" Value="(ES user name)" />
  <Parameter Name="password" Value="(ES user password)" />
  <Parameter Name="indexNamePrefix" Value="myapp" />
</Section>
```
Hello hodnoty `serviceUri`, `userName` a `password` parametry odpovídají adresa koncového bodu clusteru Elasticsearch toohello, Elasticsearch uživatelské jméno a heslo, v uvedeném pořadí. `indexNamePrefix`hello předpona pro indexy Elasticsearch; Knihovna Microsoft.Diagnostics.Listeners Hello se vytvoří nový index pro svoje data každý den.

### <a name="verification"></a>Ověření
A to je vše! Nyní vždy, když se spustí služba hello, spustí odesílání trasování toohello Elasticsearch služby zadaný v konfiguraci hello. Můžete to ověřit pomocí uživatelského rozhraní Kibana přidruženého k instanci Elasticsearch cíl hello hello otevírání. V našem příkladu adresa stránku hello je http://myBigCluster.westus.cloudapp.azure.com/. Zkontrolujte, zda indexy s předponou názvu hello zvolené pro hello `ElasticSearchListener` instance skutečně byly vytvořeny a naplněny s daty.

![Zobrazuje Kibana PartyCluster aplikace události][2]

## <a name="next-steps"></a>Další kroky
* [Další informace o diagnostice a monitorování služby Service Fabric](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

<!--Image references-->
[1]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/listener-lib-references.png
[2]: ./media/service-fabric-diagnostics-how-to-use-elasticsearch/kibana.png
