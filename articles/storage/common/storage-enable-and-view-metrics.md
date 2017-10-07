---
title: "aaaEnabling úložiště metriky v hello portálu Azure | Microsoft Docs"
description: "Jak tooenable úložiště metriky pro hello službám Blob, fronty, tabulky a souborů"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 0407adfc-2a41-4126-922d-b76e90b74563
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/14/2017
ms.author: robinsh
ms.openlocfilehash: f157dbdf9a256da7ab186f80db746b91d1a9beb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enabling-azure-storage-metrics-and-viewing-metrics-data"></a>Zapnutí metrik Azure Storage a prohlížení dat metrik
[!INCLUDE [storage-selector-portal-enable-and-view-metrics](../../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a>Přehled
Úložiště Metrics je povolena ve výchozím nastavení, když vytvoříte nový účet úložiště. Můžete nakonfigurovat monitorování prostřednictvím hello [portál Azure](https://portal.azure.com) nebo prostředí Windows PowerShell, nebo prostřednictvím kódu programu prostřednictvím knihovny klienta úložiště hello.

Můžete nakonfigurovat dobu uchování dat metrik hello: Tato doba určuje, jak dlouho hello úložiště služby udržuje hello metriky a poplatky, můžete pro hello místo požadované toostore je. Obvykle měli byste použít kratší doby uchování pro minutu metriky než hodinová metriky kvůli hello významné přebytečné místo požadované pro minutu metriky. Měli byste vybrat dobu uchovávání tak, aby dostatečný čas tooanalyze hello dat a stáhnout všechny metriky, které chcete tookeep pro offline analýzu nebo pro účely generování sestav. Mějte na paměti, že vám budou dostávat také pro stahování dat metriky z vašeho účtu úložiště.

## <a name="how-tooenable-metrics-using-hello-azure-portal"></a>Jak hello tooenable metriky pomocí portálu Azure
Postupujte podle těchto kroků tooenable metriky v hello [portál Azure](https://portal.azure.com):

1. Přejděte tooyour účet úložiště.
1. Vyberte **diagnostiky** na hello **nabídky** okna
1. Ujistěte se, že **stav** je nastaven příliš**na**.
1. Vyberte hello metriky pro služby hello chcete toomonitor.
1. Zadejte tooindicate zásady uchovávání informací jak dlouho data tooretain metriky a protokolu.
1. Vyberte **Uložit**.

Všimněte si, že hello [portál Azure](https://portal.azure.com) neumožňuje aktuálně tooconfigure minutu metriky ve vašem účtu úložiště; je nutné povolit minutu metriky pomocí prostředí PowerShell nebo prostřednictvím kódu programu.

## <a name="how-tooenable-metrics-using-powershell"></a>Jak tooenable metriky pomocí prostředí PowerShell
Pomocí prostředí PowerShell na váš místní počítač tooconfigure metriky úložiště ve vašem účtu úložiště pomocí hello prostředí Azure PowerShell rutinu Get-AzureStorageServiceMetricsProperty tooretrieve hello aktuální nastavení a hello rutiny Set-AzureStorageServiceMetricsProperty toochange hello aktuální nastavení.

Hello rutiny, které řídí metriky úložiště používají hello následující parametry:

* MetricsType: možné hodnoty jsou hodin a minut.
* ServiceType: možné hodnoty jsou objekt Blob, fronty a tabulky.
* MetricsLevel: možné hodnoty jsou None, služby a ServiceAndApi.

Například hello následující příkaz přepne na minutu metriky pro hello služby objektů Blob v účtu úložiště výchozí s dobou uchování hello nastavit toofive dny:

```powershell
Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5`
```

Hello následující příkaz načte hello aktuální hodinové metriky úrovně a jejich uchovávání dní hello služby objektů blob v účtu úložiště výchozí:

```powershell
Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob
```

Informace o tom, jak tooconfigure hello prostředí Azure PowerShell rutiny toowork ve vašem předplatném Azure a jak tooselect hello výchozí úložiště účtu toouse najdete v tématu: [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview).

## <a name="how-tooenable-storage-metrics-programmatically"></a>Jak tooenable metriky úložiště prostřednictvím kódu programu
Následující fragment kódu jazyka C# Hello ukazuje, jak tooenable metrik a protokolování pro použití služby Blob hello hello Klientská knihovna pro úložiště pro .NET:

```csharp
//Parse hello connection string for hello storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

// Create service client for credentialed access toohello Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Enable Storage Analytics logging and set retention policy too10 days.
ServiceProperties properties = new ServiceProperties();
properties.Logging.LoggingOperations = LoggingOperations.All;
properties.Logging.RetentionDays = 10;
properties.Logging.Version = "1.0";

// Configure service properties for metrics. Both metrics and logging must be set at hello same time.
properties.HourMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
properties.HourMetrics.RetentionDays = 10;
properties.HourMetrics.Version = "1.0";

properties.MinuteMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
properties.MinuteMetrics.RetentionDays = 10;
properties.MinuteMetrics.Version = "1.0";

// Set hello default service version toobe used for anonymous requests.
properties.DefaultServiceVersion = "2015-04-05";

// Set hello service properties.
blobClient.SetServiceProperties(properties);
```

## <a name="viewing-storage-metrics"></a>Zobrazení metriky úložiště
Po dokončení konfigurace účtu úložiště Storage Analytics metriky toomonitor, Storage Analytics zaznamenává hello metriky v sadě dobře známé tabulek ve vašem účtu úložiště. Grafy tooview hodinové metriky můžete nakonfigurovat v hello [portál Azure](https://portal.azure.com):

1. Přejděte tooyour účet úložiště v hello [portál Azure](https://portal.azure.com).
1. Vyberte **metriky** v hello **nabídky** okně hello služby jejichž metriky chcete tooview.
1. Vyberte **upravit** v grafu hello chcete tooconfigure.
1. V hello **upravit graf** okně, vyberte hello **časový rozsah**, **typ grafu**a hello metriky, které chcete zobrazit v grafu hello.
1. Vyberte **OK**.

Pokud chcete, aby toodownload hello metriky pro dlouhodobé uložení nebo tooanalyze je místně, budete muset:

* Použijte nástroj, který má informace o těchto tabulek a bude umožňují tooview a je stáhnout.
* Napsat vlastní aplikace nebo skriptu tooread a ukládání tabulek hello.

Celou řadu nástrojů procházení úložiště jiných výrobců Uvědomte těchto tabulek a umožňují tooview je přímo.
Najdete v tématu [Azure Storage Client Tools](storage-explorers.md) seznam dostupných nástrojů.

> [!NOTE]
> Počínaje verzí 0.8.0 hello [Microsoft Azure Storage Explorer](http://storageexplorer.com/), můžete zobrazit a stáhnout hello analytics metriky tabulky.
> 
> 

V pořadí tooaccess hello analytics tabulky prostřednictvím kódu programu, Všimněte si, hello analytics tabulky se nezobrazí Pokud seznam všech tabulek hello ve vašem účtu úložiště. Můžete k nim přistupovat přímo podle názvu, nebo použijte hello [CloudAnalyticsClient API](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.analytics.cloudanalyticsclient.aspx) v hello .NET klienta knihovny tooquery hello názvy tabulek.

### <a name="hourly-metrics"></a>Hodinové metriky
* $MetricsHourPrimaryTransactionsBlob
* $MetricsHourPrimaryTransactionsTable
* $MetricsHourPrimaryTransactionsQueue

### <a name="minute-metrics"></a>Minutu metriky
* $MetricsMinutePrimaryTransactionsBlob
* $MetricsMinutePrimaryTransactionsTable
* $MetricsMinutePrimaryTransactionsQueue

### <a name="capacity"></a>Kapacita
* $MetricsCapacityBlob

Můžete najít podrobnosti o hello schémata pro tyto tabulky v [schématu tabulky metriky Analytics úložiště](https://msdn.microsoft.com/library/azure/hh343264.aspx). Hello ukázka řádků níže zobrazit pouze podmnožinu sloupců hello k dispozici, ale ilustrovat některé důležité funkce hello způsob, jakým Storage Metrics uloží tyto metriky:

| Klíč oddílu | RowKey | časové razítko | TotalRequests | TotalBillableRequests | TotalIngress | TotalEgress | Dostupnost | AverageE2ELatency | AverageServerLatency | PercentSuccess |
| --- |:---:| ---:| --- | --- | --- | --- | --- | --- | --- | --- |
| 20140522T1100 |uživatel; Všechny |2014-05-22T11:01:16.7650250Z |7 |7 |4003 |46801 |100 |104.4286 |6.857143 |100 |
| 20140522T1100 |uživatel; QueryEntities |2014-05-22T11:01:16.7640250Z |5 |5 |2694 |45951 |100 |143.8 |7.8 |100 |
| 20140522T1100 |uživatel; QueryEntity |2014-05-22T11:01:16.7650250Z |1 |1 |538 |633 |100 |3 |3 |100 |
| 20140522T1100 |uživatel; UpdateEntity |2014-05-22T11:01:16.7650250Z |1 |1 |771 |217 |100 |9 |6 |100 |

V těchto datech minutu metriky příklad používá klíč oddílu hello hello čas na minutu řešení. klíč řádku Hello identifikuje hello typ informací uložených v řádku hello a tím se skládá ze dvou částí informace, typ přístupu hello a typ požadavku hello:

* Typ přístupu Hello je uživatele nebo systému, kde uživatel odkazuje tooall uživatelské požadavky toohello úložiště služby, a systém odkazuje toorequests provedené Storage Analytics.
* Typ požadavku Hello je všechny v takovém případě je souhrn řádku, nebo ji identifikuje hello konkrétní API jako třeba QueryEntity nebo UpdateEntity.

Ukázková data Hello výš ukazuje, které všechny hello zaznamenává pro jednu minutu (počínaje 11:00 AM), takže hello počet požadavků QueryEntities plus hello počet požadavků QueryEntity plus počet hello UpdateEntity požadavků, které dohromady tooseven, což je hello celkový zobrazený na řádek uživatele: všechny Hello. Podobně můžete odvodit hello průměrnou dobu začátku do konce 104.4286 na řádku uživatele: všechny hello pomocí výpočtu ((143.8 * 5) + 3 + 9) / 7.

## <a name="metrics-alerts"></a>Metriky výstrahy
Měli byste zvážit vytvoření výstrahy v hello [portál Azure](https://portal.azure.com) tak metriky úložiště automaticky vás může upozornit, důležité změny v chování hello vaší služby úložiště. Pokud toodownload nástroj Průzkumníka úložiště používáte ve formátu odděleného tato data metriky, můžete data hello tooanalyze Microsoft Excel. V tématu [Azure Storage Client Tools](storage-explorers.md) seznam nástrojů Průzkumníka úložiště k dispozici. Výstrahy můžete nakonfigurovat v hello **výstrah pravidla** okně přístupná **monitorování** v okně nabídce účtu úložiště hello.

> [!IMPORTANT]
> Může nastat zpoždění mezi událostí úložiště a když se zaznamenává hello odpovídající hodinových nebo minutu metriky data. V případě hello minutu metrik může několik minut dat zapsat najednou. To může způsobit tootransactions z dřívějších minut agregovaný do hello transakci pro hello aktuální minuty. V takovém případě hello výstraha, že službu nemusí mít všechna data dostupné metriky pro hello nakonfigurovat výstrahy interval, což může vést tooalerts neočekávaně aktivuje.
>

## <a name="accessing-metrics-data-programmatically"></a>Přístup k datům metriky prostřednictvím kódu programu
Hello následující výpis ukazuje ukázka C# kód, který přistupuje k hello minutu metriky pro řadu minut a zobrazí výsledky hello v okně konzoly. Používá hello knihovny pro úložiště Azure verze 4, který zahrnuje hello CloudAnalyticsClient třídu, která zjednodušuje přístupem hello metriky tabulek v úložišti.

```csharp
private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)
{
    // Convert hello dates toohello format used in hello PartitionKey
    var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");

    var services = Enum.GetValues(typeof(StorageService));
    foreach (StorageService service in services)
    {
        Console.WriteLine("Minute Metrics for Service {0} from {1} too{2} UTC", service, start, end);
        var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);
        var t = analyticsClient.GetMinuteMetricsTable(service);
        var opContext = new OperationContext();
        var query =
          from entity in metricsQuery
          // Note, you can't filter using hello entity properties Time, AccessType, or TransactionType
          // because they are calculated fields in hello MetricsEntity class.
          // hello PartitionKey identifies hello DataTime of hello metrics.
          where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0
        select entity;

        // Filter on "user" transactions after fetching hello metrics from Table Storage.
        // (StartsWith is not supported using LINQ with Azure table storage)
        var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));
        var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();
        Console.WriteLine(resultString);
    }
}

private static string MetricsString(MetricsEntity entity, OperationContext opContext)
{
    var entityProperties = entity.WriteEntity(opContext);
    var entityString =
      string.Format("Time: {0}, ", entity.Time) +
      string.Format("AccessType: {0}, ", entity.AccessType) +
      string.Format("TransactionType: {0}, ", entity.TransactionType) +
      string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));
    return entityString;
}
```

## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a>Jaké poplatky platit při povolení metrik úložiště?
Požadavky toocreate tabulka entity metrik, které jsou výši hello standardních sazeb použít tooall Azure Storage operace zápisu

Čtení a odstraňování požadavků podle dat toometrics klienta jsou taky fakturovatelný na standardních sazeb. Pokud jste nakonfigurovali zásadu uchovávání dat, vám není účtován když odstraní stará data metrik Azure Storage. Pokud odstraníte analytická data, váš účet účtovat pro operace odstranění hello.

kapacita Hello používá tabulky metriky hello je také fakturovatelný: můžete použít následující tooestimate hello množství kapacity, na které se používá k ukládání dat metriky hello:

* Pokud každou hodinu služba využívá každé rozhraní API v každé službě, pak přibližně 148KB dat je uložený každou hodinu do tabulky transakcí metriky hello Pokud jste povolili službu a úroveň API souhrnu.
* Pokud každou hodinu služba využívá každé rozhraní API v každé službě, pak přibližně 12KB dat je uložený každou hodinu do tabulky transakcí metriky hello Pokud jste povolili právě služby úrovni souhrnu.
* Hello kapacity pro objekty BLOB má dva řádky přidat každý den (za předpokladu, že uživatel, má vyjádřit výslovný souhlas pro protokoly): to znamená, že každý den hello velikost této tabulky zvyšuje úroveň až tooapproximately 300 bajtů.

## <a name="next-steps"></a>Další kroky
[Povolení protokolování a přístup k datům protokolu úložiště](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data)
