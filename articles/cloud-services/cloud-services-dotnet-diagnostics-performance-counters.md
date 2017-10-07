---
title: "aaaUse čítače výkonu v Azure Diagnostics | Microsoft Docs"
description: "Pomocí čítače výkonu v cloudové služby Azure nebo virtuální počítač toofind kritická místa a optimalizaci výkonu."
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: tysonn
ms.assetid: fc4c61e9-d49d-4ed9-a32c-b91cdf851909
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/29/2016
ms.author: robb
ms.openlocfilehash: f3250816c01fc6e164a6aae48da5035845e6d2b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-use-performance-counters-in-an-azure-application"></a>Vytváření a používání čítače výkonu v aplikaci Azure
Tento článek popisuje hello výhody a jak čítače výkonu tooput do aplikace Azure. Můžete je použít toocollect data, najít kritická místa a vyladit výkon systému a aplikací.

Čítače výkonu, které jsou k dispozici pro Windows Server, služba IIS a ASP.NET může také shromažďovat a použít toodetermine hello stavu Azure webové role, role pracovního procesu a virtuálních počítačů. Můžete také vytvořit a použít vlastní čítače výkonu.  

Můžete zkontrolovat data čítače výkonu

1. Přímo na hostiteli aplikace hello s nástrojem Sledování výkonu hello přistupovat pomocí vzdálené plochy
2. S pomocí hello Azure Management Pack nástroje System Center Operations Manager
3. Pomocí jiných nástrojů monitorování, které přístup hello diagnostických dat přenesených tooAzure úložiště. V tématu [úložiště a zobrazení diagnostických dat ve službě Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) Další informace.  

Další informace o sledování výkonu hello vaší aplikace v hello [portál Azure](http://portal.azure.com/), najdete v části [jak tooMonitor cloudových služeb](https://www.azure.com/manage/services/cloud-services/how-to-monitor-a-cloud-service/).

Další podrobné pokyny k vytváření protokolování a trasování strategie a používání diagnostiky a jiné problémy tootroubleshoot technik a optimalizaci aplikace Azure, najdete v části [řešení potíží s osvědčené postupy pro vývoj Azure Aplikace](https://msdn.microsoft.com/library/azure/hh771389.aspx).

## <a name="enable-performance-counter-monitoring"></a>Povolit monitorování čítače výkonu
Ve výchozím nastavení nejsou povoleny čítače výkonu. Spuštění úlohy nebo aplikace, musíte upravit diagnostiky výchozí hello čítače výkonu specifických hello tooinclude konfigurace agenta chcete toomonitor pro každou instanci role.

### <a name="performance-counters-available-for-microsoft-azure"></a>Čítače výkonu, které jsou k dispozici pro Microsoft Azure
Azure poskytuje podmnožinu hello čítačů výkonu, které jsou k dispozici pro Windows Server, služba IIS a hello zásobníku technologie ASP.NET. Hello následující tabulka uvádí některé čítače výkonu hello konkrétní týkající se aplikace Azure.

| Kategorie čítače: Objekt (Instance) | Název čítače | Referenční informace |
| --- | --- | --- |
| CLR – výjimky na rozhraní .NET (*globální*) |# Výjimek / s |Čítače výkonu výjimky |
| Využívání paměti rozhraním .NET CLR (*globální*) |Čas |Čítače výkonu paměti |
| ASP.NET |Restartování aplikace |Čítače výkonu pro technologii ASP.NET |
| ASP.NET |Čas provádění požadavku |Čítače výkonu pro technologii ASP.NET |
| ASP.NET |Odpojené požadavky |Čítače výkonu pro technologii ASP.NET |
| ASP.NET |Restartování pracovního procesu |Čítače výkonu pro technologii ASP.NET |
| Aplikace ASP.NET (**celkový**) |Celkový počet požadavků |Čítače výkonu pro technologii ASP.NET |
| Aplikace ASP.NET (**celkový**) |Počet požadavků za sekundu |Čítače výkonu pro technologii ASP.NET |
| Položku ASP.NET v4.0.30319 |Čas provádění požadavku |Čítače výkonu pro technologii ASP.NET |
| Položku ASP.NET v4.0.30319 |Čekací doba požadavku |Čítače výkonu pro technologii ASP.NET |
| Položku ASP.NET v4.0.30319 |Aktuální požadavky |Čítače výkonu pro technologii ASP.NET |
| Položku ASP.NET v4.0.30319 |Požadavky ve frontě |Čítače výkonu pro technologii ASP.NET |
| Položku ASP.NET v4.0.30319 |Zamítnuté požadavky |Čítače výkonu pro technologii ASP.NET |
| Memory (Paměť) |Počet MB k dispozici |Čítače výkonu paměti |
| Memory (Paměť) |Svěřené bajty |Čítače výkonu paměti |
| Procesor(_celkem) |% Času procesoru |Čítače výkonu pro technologii ASP.NET |
| TCPv4 |Počet selhání připojení |Objekt TCP |
| TCPv4 |Vytvořeno připojení |Objekt TCP |
| TCPv4 |Resetování připojení |Objekt TCP |
| TCPv4 |Segmenty odeslaných za sekundu |Objekt TCP |
| Interface(*) sítě |Přijaté bajty/s |Objekt rozhraní sítě |
| Interface(*) sítě |Odeslané bajty/s |Objekt rozhraní sítě |
| Síťové rozhraní (Microsoft sběrnice virtuálního počítače síťový adaptér _2) |Přijaté bajty/s |Objekt rozhraní sítě |
| Síťové rozhraní (Microsoft sběrnice virtuálního počítače síťový adaptér _2) |Odeslané bajty/s |Objekt rozhraní sítě |
| Síťové rozhraní (Microsoft sběrnice virtuálního počítače síťový adaptér _2) |Čítač Bajty celkem/s |Objekt rozhraní sítě |

## <a name="create-and-add-custom-performance-counters-tooyour-application"></a>Vytvořit a přidat vlastní výkonu čítače tooyour aplikace
Azure má toocreate podporu a upravte vlastní čítače výkonu pro webových rolí a rolí pracovního procesu. Hello čítače může být použité tootrack a monitorování chování specifické pro aplikaci. Můžete vytvořit a kategorie čítače výkonu vlastní a specifikátory odstranit z úloha spuštění, webovou roli nebo role pracovního procesu se zvýšenými oprávněními.

> [!NOTE]
> Kód, který provede změny čítače výkonu toocustom musí mít zvýšená oprávnění toorun. Pokud je kód hello v webovou roli nebo role pracovního procesu, hello role musí obsahovat značky hello <Runtime executionContext="elevated" /> v hello ServiceDefinition.csdef souboru pro hello role tooinitialize správně.
>
>

Můžete odeslat vlastní výkon čítač uložení dat v tooAzure hello Diagnostika agenta.

data čítače Hello standardní výkonu je generován hello, který zpracovává Azure. Data čítače výkonu vlastní musí být vytvořeny webovou aplikací role role nebo pracovního procesu. V tématu [typy čítačů výkonu](https://msdn.microsoft.com/library/z573042h.aspx) informace o typech hello dat, která mohou být uloženy ve vlastní čítače výkonu. V tématu [Ukázka čítače výkonu](http://code.msdn.microsoft.com/azure/) pro příklad, který vytvoří a nastaví data čítače výkonu vlastní ve webové roli.

## <a name="store-and-view-performance-counter-data"></a>Úložiště a zobrazení data čítače výkonu
Azure data čítače výkonu pomocí jiné diagnostické informace ukládá do mezipaměti. Tato data jsou k dispozici pro vzdálené monitorování hello role instance je spuštěn pomocí nástroje tooview přístup ke vzdálené ploše, jako je monitorování výkonu. toopersist hello data mimo hello instanci role, hello Diagnostika agenta musíte přenést hello data tooAzure úložiště. limit velikosti Hello data čítače výkonu hello do mezipaměti se dá nakonfigurovat v hello Diagnostika agenta, nebo může být nakonfigurován toobe součástí sdílené limit pro všechny hello diagnostická data. Další informace o nastavení hello velikost vyrovnávací paměti najdete v tématu [OverallQuotaInMB](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitorconfiguration.overallquotainmb.aspx) a [DirectoriesBufferConfiguration](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.directoriesbufferconfiguration.aspx). V tématu [úložiště a zobrazení diagnostických dat ve službě Azure Storage](https://msdn.microsoft.com/library/azure/hh411534.aspx) přehled nastavení hello agenta tootransfer data tooa účet úložiště pro diagnostiku.

Každá instance čítače výkonu nakonfigurované se zaznamenává v zadané vzorkovací frekvenci a hello Vzorkovat data se přenáší toohello účet úložiště tak, že žádost o přenos plánované nebo žádost o přenos na vyžádání. Automatické přenosy může být naplánovaná tak často, jak jednou za minutu. Data čítače výkonu přenesených hello Diagnostika agenta je uložena v tabulce, WADPerformanceCountersTable, v účtu úložiště hello. Tato tabulka může získat přístup a dotaz pomocí metod úložiště Azure úrovně standard rozhraní API. V tématu [Ukázka čítače výkonu Microsoft Azure](http://code.msdn.microsoft.com/Windows-Azure-PerformanceCo-7d80ebf9) příklad dotazování a zobrazení data čítače výkonu z tabulky WADPerformanceCountersTable hello.

> [!NOTE]
> V závislosti na hello Diagnostika agenta přenosu četnost a fronty latence hello nejnovější data čítače výkonu v účtu úložiště hello může být zastaralý několik minut.
>
>

## <a name="enable-performance-counters-using-diagnostics-configuration-file"></a>Povolit čítače výkonu pomocí diagnostiky konfiguračního souboru
Použijte následující postup tooenable čítače výkonu v aplikaci Azure hello.

## <a name="prerequisites"></a>Požadavky
Této části se předpokládá, že máte importovat monitorování diagnostiky hello do vaší aplikace a přidat hello diagnostiky konfigurační soubor tooyour řešení sady Visual Studio (diagnostics.wadcfg v SDK 2.4 a pod ním nebo diagnostics.wadcfgx v SDK 2.5 a vyšší). Informace o kroky 1 a 2 v [povolení diagnostiky Azure Cloud Services a virtuálních počítačů](cloud-services-dotnet-diagnostics.md)) Další informace.

## <a name="step-1-collect-and-store-data-from-performance-counters"></a>Krok 1: Shromažďovat a ukládat data z čítače výkonu
Po přidání, že hello diagnostiky souboru tooyour řešení sady Visual Studio, můžete nakonfigurovat hello kolekce a ukládání data čítače výkonu v aplikaci Azure. To se provádí přidáním soubor diagnostiky toohello čítačů výkonu. Diagnostická data, včetně čítačů výkonu, je nejprve shromažďují na instanci hello. Hello data je pak trvalou toohello WADPerformanceCountersTable tabulky v hello služby Azure Table, tak budete také potřebovat toospecify hello účet úložiště ve vaší aplikaci. Pokud testujete aplikace místně v hello výpočetní emulátor, můžete také ukládat diagnostická data místně v hello emulátoru úložiště. Než bude ukládat data diagnostiky, je nutné nejprve přejít toohello [portál Azure](http://portal.azure.com/) a vytvoření účtu úložiště classic. Osvědčeným postupem je toolocate svůj účet úložiště hello stejného geografického umístění jako vaše aplikace Azure. Podle zachování hello Azure aplikace a účet úložiště jsou v Dobrý den stejného geografického umístění, vyhněte se platícího náklady na externí šířku pásma a snižování latence.

### <a name="add-performance-counters-toohello-diagnostics-file"></a>Přidejte soubor diagnostiky toohello čítačů výkonu
Existuje mnoho čítačů, které můžete použít. Hello následující příklad ukazuje několik čítače výkonu, které se doporučují pro webové a pracovní role monitorování.

Otevřete soubor diagnostiky hello (diagnostics.wadcfg SDK 2.4 a nižší nebo diagnostics.wadcfgx v SDK 2.5 a vyšší) a přidejte následující toohello DiagnosticMonitorConfiguration element hello:

```xml
<PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
    <PerformanceCounterConfiguration counterSpecifier="\Memory\Available Bytes" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Processor(_Total)\% Processor Time" sampleRate="PT30S" />

    <!-- Use hello Process(w3wp) category counters in a web role -->

    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\% Processor Time" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Private Bytes" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\Process(w3wp)\Thread Count" sampleRate="PT30S" />

    <!-- Use hello Process(WaWorkerHost) category counters in a worker role.
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\% Processor Time" sampleRate="PT30S" />
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Private Bytes" sampleRate="PT30S" />
        <PerformanceCounterConfiguration counterSpecifier="\Process(WaWorkerHost)\Thread Count" sampleRate="PT30S" />
    -->

    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Interop(_Global_)\# of marshalling" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Loading(_Global_)\% Time Loading" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR LocksAndThreads(_Global_)\Contention Rate / sec" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Memory(_Global_)\# Bytes in all Heaps" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Networking(_Global_)\Connections Established" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Remoting(_Global_)\Remote Calls/sec" sampleRate="PT30S" />
    <PerformanceCounterConfiguration counterSpecifier="\.NET CLR Jit(_Global_)\% Time in Jit" sampleRate="PT30S" />
</PerformanceCounters>
```

Hello bufferQuotaInMB atribut, který určuje maximální množství hello úložiště systému souboru, který je k dispozici pro typ kolekce dat hello (Azure protokoly, protokoly služby IIS, atd.). Hello výchozí hodnota je 0. Když je dosaženo podílu hello, nejstarší data hello je odstranit, protože se přidá nová data. Hello součet všech vlastností bufferQuotaInMB hello musí být větší než hodnota hello hello OverallQuotaInMB atributu. Další podrobné informace pro určení toho, jak velké úložiště se bude vyžadovat hello sběr dat diagnostiky, naleznete v hello část instalace WAD [řešení potíží s osvědčené postupy pro vývoj aplikací Azure](https://msdn.microsoft.com/library/windowsazure/hh771389.aspx).

Hello scheduledTransferPeriod atribut, který určuje hello interval mezi naplánované přenosů dat, zaokrouhlit toohello nejbližší minutu. V následující příklady hello nastavení tooPT30M (30 minut). Hello přenos období tooa malá hodnota nastavení, jako je 1 minuta, bude nepříznivě ovlivnit výkon vaší aplikace v produkčním prostředí ale může být užitečná pro zobrazení diagnostiky rychle práce při testování. Hello naplánované přenos perioda musí být dostatečně malé, tooensure, diagnostická data nejsou přepsána na hello instanci, ale dostatečně velké na to, že nebude mít vliv hello výkon aplikace.

Určuje atribut counterSpecifier Hello toocollect čítače výkonu hello. Určuje atribut sampleRate Hello hello rychlost, jakou hello čítače výkonu se odeberou, v takovém případě 30 sekund.

Po přidání, že čítače výkonu hello, které chcete toocollect, uložte soubor změny toohello diagnostiky. Dále je nutné zadat účet toospecify hello úložiště, který bude formou hello diagnostická data.

### <a name="specify-hello-storage-account"></a>Zadejte účet úložiště hello
toopersist účet vaší diagnostické informace tooyour Azure Storage, je nutné zadat připojovací řetězec v souboru konfigurace (souboru ServiceConfiguration.cscfg) služby.

Pro Azure SDK 2.5 hello účet úložiště lze v souboru diagnostics.wadcfgx hello.

> [!NOTE]
> Tyto pokyny platí pouze tooAzure SDK 2.4 a níže. Pro Azure SDK 2.5 hello účet úložiště lze v souboru diagnostics.wadcfgx hello.
>
>

tooset hello připojovací řetězce:

1. Otevřete soubor ServiceConfiguration.Cloud.cscfg hello pomocí oblíbeném textovém editoru a sadu hello připojovací řetězec pro úložiště. Hello *AccountName* a *AccountKey* hodnoty se nacházejí v hello portál Azure hello panelu účet úložiště, v části přístupové klíče.

    ```xml
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<name>;AccountKey=<key>"/>
    </ConfigurationSettings>
    ```
2. Uložte soubor ServiceConfiguration.Cloud.cscfg hello.
3. Otevřete soubor ServiceConfiguration.Local.cscfg hello a ověřte, zda je UseDevelopmentStorage nastavena tootrue.

    ```xml
    <ConfigurationSettings>
      <Settingname="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true"/>
    </ConfigurationSettings>
    ```
   Teď, když jsou nastavené hello připojovací řetězce, aplikace se uchová účet úložiště tooyour diagnostiky dat při nasazení vaší aplikace.
4. Uložte a sestavení projektu a nasazení aplikace.

## <a name="step-2-optional-create-custom-performance-counters"></a>Krok 2: (Volitelné) vytvořit vlastní čítače výkonu
Kromě toho toohello předem definované čítače výkonu, můžete přidat že vlastní vlastního výkonu čítače toomonitor role web nebo worker. Vlastní čítače výkonu může být použité tootrack a monitorování chování specifické pro aplikaci a můžou vytvořit nebo odstranit v úloha spuštění, webovou roli nebo role pracovního procesu se zvýšenými oprávněními.

agent Azure diagnostics Hello aktualizuje hello konfiguraci čítače výkonnosti ze souboru .wadcfg hello jednu minutu po spuštění.  Pokud vytvoříte vlastní čítače výkonu v hello metoda OnStart a úlohami spuštění trvá déle než minutu tooexecute, vaše vlastní čítače výkonu nebudou byl vytvořen při hello Azure Diagnostics agent se pokusí tooload je.  V tomto scénáři uvidíte, že Azure Diagnostics správně zaznamená všechny diagnostická data s výjimkou vaše vlastní čítače výkonu.  tooresolve tento problém, vytvořte hello čítače výkonu v úloze pro spuštění nebo přesunout, že některé vaše úloha spuštění metoda OnStart toohello fungovat po vytvoření hello čítače výkonu.

Proveďte následující kroky toocreate jednoduché vlastní výkon čítač s názvem "\MyCustomCounterCategory\MyButton1Counter" hello:

1. Otevřete soubor definice služby (CSDEF) hello pro vaši aplikaci.
2. Přidejte hello Runtime element toohello WebRole nebo WorkerRole element tooallow provádění se zvýšenými oprávněními:

    ```xml
    <runtime executioncontext="elevated"/>
    ```
3. Uložte soubor hello.
4. Otevřete soubor diagnostiky hello (diagnostics.wadcfg SDK 2.4 a nižší nebo diagnostics.wadcfgx v SDK 2.5 a vyšší) a přidejte následující toohello DiagnosticMonitorConfiguration hello

    ```xml
    <PerformanceCounters bufferQuotaInMB="0" scheduledTransferPeriod="PT30M">
      <PerformanceCounterConfiguration counterSpecifier="\MyCustomCounterCategory\MyButton1Counter" sampleRate="PT30S"/>
    </PerformanceCounters>
    ```
5. Uložte soubor hello.
6. Před vyvoláním základní vytvořte kategorie čítače výkonu vlastní hello metoda OnStart hello vaši roli. OnStart. Hello následující příklad jazyka C# vytvoří vlastní kategorii, pokud ještě neexistuje:

    ```csharp
    public override bool OnStart()
    {
      if (!PerformanceCounterCategory.Exists("MyCustomCounterCategory"))
      {
         CounterCreationDataCollection counterCollection = new CounterCreationDataCollection();

         // add a counter tracking user button1 clicks
         CounterCreationData operationTotal1 = new CounterCreationData();
         operationTotal1.CounterName = "MyButton1Counter";
         operationTotal1.CounterHelp = "My Custom Counter for Button1";
         operationTotal1.CounterType = PerformanceCounterType.NumberOfItems32;
         counterCollection.Add(operationTotal1);

         PerformanceCounterCategory.Create(
           "MyCustomCounterCategory",
           "My Custom Counter Category",
           PerformanceCounterCategoryType.SingleInstance, counterCollection);

         Trace.WriteLine("Custom counter category created.");
      }
      else {
        Trace.WriteLine("Custom counter category already exists.");
      }

    return base.OnStart();
    }
    ```
7. Aktualizujte hello čítače v rámci vaší aplikace. Následující ukázka Hello aktualizací čítače výkonu vlastní Button1_Click událostí:

    ```csharp
    protected void Button1_Click(object sender, EventArgs e)
    {
      PerformanceCounter button1Counter = new PerformanceCounter(
        "MyCustomCounterCategory",
        "MyButton1Counter",
        string.Empty,
        false);
      button1Counter.Increment();
      this.Button1.Text = "Button 1 count: " +
        button1Counter.RawValue.ToString();
    }
    ```
8. Uložte soubor hello.  

Data čítače výkonu vlastní budou shromažďována teď hello Azure diagnostics monitorování.

## <a name="step-3-query-performance-counter-data"></a>Krok 3: Dotaz na data čítače výkonu
Jakmile se vaše aplikace je nasazená a spuštěna, bude zahájeno monitorování diagnostiky hello shromažďování čítačů výkonu a uložením tohoto úložiště tooAzure data. Pomocí nástrojů, jako je Průzkumníka serveru v sadě Visual Studio, [Azure Storage Explorer](http://azurestorageexplorer.codeplex.com/), nebo [Azure Diagnostics Manager](http://www.cerebrata.com/Products/AzureDiagnosticsManager/Default.aspx) podle Cerebrata čítače výkonu hello tooview data v hello Tabulka WADPerformanceCountersTable. Můžete také prostřednictvím kódu programu dotazovat pomocí služby Table hello [C#](../cosmos-db/table-storage-how-to-use-dotnet.md), [Java](../cosmos-db/table-storage-how-to-use-java.md), [Node.js](../cosmos-db/table-storage-how-to-use-nodejs.md), [Python](../cosmos-db/table-storage-how-to-use-python.md), [Ruby](../cosmos-db/table-storage-how-to-use-ruby.md), nebo [PHP](../cosmos-db/table-storage-how-to-use-php.md).

Hello následující C# příklad ukazuje základní dotaz hello WADPerformanceCountersTable tabulky a uloží soubor CSV tooa data diagnostiky hello. Po uložení souboru CSV tooa hello čítače výkonu můžete hello vytváření grafů možnosti v aplikaci Microsoft Excel nebo některé další data hello nástroj toovisualize. Být jisti tooadd tooMicrosoft.WindowsAzure.Storage.dll odkaz, který je součástí hello Azure SDK pro .NET října 2012 a novější. sestavení Hello je nainstalovaná toohello % Program Files%\Microsoft SDKs\Microsoft Azure.NET SDK\version-num\ref\ adresář.

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Table;
...

// Get hello connection string. When using Microsoft Azure Cloud Services, it is recommended
// you store your connection string using hello Microsoft Azure service configuration
// system (*.csdef and *.cscfg files). You can you use hello CloudConfigurationManager type
// tooretrieve your storage connection string.  If you're not using Cloud Services, it's
// recommended that you store hello connection string in your web.config or app.config file.
// Use hello ConfigurationManager type tooretrieve your storage connection string.

string connectionString = Microsoft.WindowsAzure.CloudConfigurationManager.GetSetting("StorageConnectionString");
//string connectionString = System.Configuration.ConfigurationManager.ConnectionStrings["StorageConnectionString"].ConnectionString;

// Get a reference toohello storage account using hello connection string.  You can also use hello development
// storage account (Storage Emulator) for local debugging.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
//CloudStorageAccount storageAccount = CloudStorageAccount.DevelopmentStorageAccount;

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "WADPerformanceCountersTable" table.
CloudTable table = tableClient.GetTableReference("WADPerformanceCountersTable");

// Create hello table query, filter on a specific CounterName, DeploymentId and RoleInstance.
TableQuery<PerformanceCountersEntity> query = new TableQuery<PerformanceCountersEntity>()
  .Where(
    TableQuery.CombineFilters(
      TableQuery.GenerateFilterCondition("CounterName", QueryComparisons.Equal, @"\Processor(_Total)\% Processor Time"),
      TableOperators.And,
      TableQuery.CombineFilters(
      TableQuery.GenerateFilterCondition("DeploymentId", QueryComparisons.Equal, "ec26b7a1720447e1bcdeefc41c4892a3"),
      TableOperators.And,
      TableQuery.GenerateFilterCondition("RoleInstance", QueryComparisons.Equal, "WebRole1_IN_0")
    )
  )
);

// Execute hello table query.
IEnumerable<PerformanceCountersEntity> result = table.ExecuteQuery(query);

// Process hello query results and build a CSV file.
StringBuilder sb = new StringBuilder("TimeStamp,EventTickCount,DeploymentId,Role,RoleInstance,CounterName,CounterValue\n");

foreach (PerformanceCountersEntity entity in result)
{
  sb.Append(entity.Timestamp + "," + entity.EventTickCount + "," + entity.DeploymentId + ","
    + entity.Role + "," + entity.RoleInstance + "," + entity.CounterName + "," + entity.CounterValue+"\n");
}

StreamWriter sw = File.CreateText(@"C:\temp\PerfCounters.csv");
sw.Write(sb.ToString());
sw.Close();
```

Entity se mapují tooC # objektů pomocí vlastní třídy odvozené od **TableEntity**. Hello následující kód definuje třídu entity představující čítače výkonu ve hello **WADPerformanceCountersTable** tabulky.

```csharp
public class PerformanceCountersEntity : TableEntity
{
  public long EventTickCount { get; set; }
  public string DeploymentId { get; set; }
  public string Role { get; set; }
  public string RoleInstance { get; set; }
  public string CounterName { get; set; }
  public double CounterValue { get; set; }
}
```

## <a name="next-steps"></a>Další kroky
[Zobrazit další články v Azure Diagnostics](../azure-diagnostics.md)
