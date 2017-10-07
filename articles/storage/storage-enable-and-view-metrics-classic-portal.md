---
title: "aaaEnabling úložiště metriky v hello portálu Azure | Microsoft Docs"
description: "Jak tooenable úložiště metriky pro hello službám Blob, fronty, tabulky a souborů"
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 2fb5b229-f099-4334-92be-4e0e7dd257d7
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/03/2017
ms.author: robinsh
ms.openlocfilehash: 4c990371e08a6586d935b0535149eabd4960cfaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enabling-storage-metrics-and-viewing-metrics-data"></a>Zapnutí metrik úložiště a prohlížení dat metrik
[!INCLUDE [storage-selector-portal-enable-and-view-metrics](../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a>Přehled
Úložiště Metrics je povolena ve výchozím nastavení, když vytvoříte nový účet úložiště. Můžete nakonfigurovat monitorování pomocí buď hello [portálu Azure Classic](https://manage.windowsazure.com), prostředí Windows PowerShell nebo programově pomocí rozhraní API úložiště.

Pokud povolíte metriky úložiště, musíte vybrat dobu uchování dat hello: Tato doba určuje, jak dlouho hello úložiště služby udržuje hello metriky a poplatky, můžete pro hello místo požadované toostore je. Obvykle měli byste použít kratší doby uchování pro minutu metriky než hodinová metriky kvůli hello významné přebytečné místo požadované pro minutu metriky. Měli byste vybrat dobu uchovávání tak, aby dostatečný čas tooanalyze hello dat a stáhnout všechny metriky, které chcete tookeep pro offline analýzu nebo pro účely generování sestav. Mějte na paměti, že vám budou dostávat také pro stahování dat metriky z vašeho účtu úložiště.

## <a name="how-tooenable-storage-metrics-using-hello-azure-classic-portal"></a>Jak hello metriky tooenable úložiště pomocí portálu Azure Classic
V hello [portálu Azure Classic](https://manage.windowsazure.com), použijte stránku hello konfigurovat pro toocontrol účet úložiště Storage Metrics. Pro monitorování, můžete nastavit úroveň a doby uchování ve dnech pro všechny objekty BLOB, tabulek a front. V každém případě hello úroveň je jedna z následujících hello:

* Vypnutí – Nebyly shromážděny žádné metriky.
* Minimální – Storage Metrics shromažďuje základní sadu metriky jako vstupní/výstupní, dostupnosti, latence a procenta úspěšnosti, které se shromažďují pro služby objektů Blob, Table a Queue hello.
* Verbose – Shromažďuje metriky úložiště úplnou sadu metriky, které zahrnuje hello stejné metriky pro každý úložiště operace rozhraní API, kromě toohello úrovně služeb metriky. Podrobné metriky povolit blíže analyzovat problémy, ke kterým došlo během operací aplikace.

Všimněte si, že hello portálu Azure Classic neumožňuje aktuálně tooconfigure minutu metriky ve vašem účtu úložiště; je nutné povolit minutu metriky pomocí prostředí PowerShell nebo prostřednictvím kódu programu.

## <a name="how-tooenable-storage-metrics-using-powershell"></a>Jak tooenable metriky úložiště pomocí prostředí PowerShell
Pomocí prostředí PowerShell na váš místní počítač tooconfigure metriky úložiště ve vašem účtu úložiště pomocí hello prostředí Azure PowerShell rutinu Get-AzureStorageServiceMetricsProperty tooretrieve hello aktuální nastavení a hello rutiny Set-AzureStorageServiceMetricsProperty toochange hello aktuální nastavení.

Hello rutiny, které řídí metriky úložiště používají hello následující parametry:

* MetricsType možné hodnoty jsou hodin a minut.
* ServiceType možné hodnoty jsou objekt Blob, fronty a tabulky.
* MetricsLevel možné hodnoty jsou None (ekvivalentní tooOff v hello portálu Azure Classic), služba (ekvivalentní tooMinimal v hello portálu Azure Classic) a ServiceAndApi (ekvivalentní tooVerbose v hello portálu Azure Classic).

Například hello následující příkaz přepne na minutu metriky pro hello služby objektů blob v účtu úložiště výchozí s dobou uchování hello nastavit toofive dny:

```powershell
Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5
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
Pokud jste nakonfigurovali úložiště metriky toomonitor účtu úložiště, zaznamenává hello metriky v sadě dobře známé tabulek ve vašem účtu úložiště. Můžete stránku hello monitorování pro váš účet úložiště na portálu Azure Classic tooview hello hodinové metriky hello, jakmile budou k dispozici v grafu. Na tuto stránku hello portálu Azure Classic můžete:

* Vyberte které tooplot metriky v grafu hello (hello volba dostupné metriky závisí na tom, jestli jste zvolili, podrobný nebo minimální monitorování služby hello na stránce konfigurace hello).
* Vyberte časové rozmezí hello hello metriky zobrazené v grafu hello.
* Zvolte toouse absolutní nebo relativní škálování tooplot hello metriky.
* Konfigurace e-mailové výstrahy toonotify případě určité metriky dosáhne určitou hodnotu.

Pokud chcete toodownload hello metriky pro dlouhodobé uložení nebo tooanalyze je místně, budete potřebovat toouse nástroj nebo napsat kód tooread hello tabulky. Je nutné stáhnout hello minutu metriky pro analýzu. Pokud seznam všech tabulek hello ve vašem účtu úložiště, ale můžete je přejít přímo pomocí názvu tabulky Hello nejsou zobrazeny. Celou řadu nástrojů procházení úložiště jiných výrobců Uvědomte těchto tabulek a umožňují tooview je přímo (najdete v příspěvku blogu hello [Průzkumníci úložiště Microsoft Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) seznam dostupných nástrojů).

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

Nastavení výstrah v hello portálu Azure Classic na stránce monitorování hello byste měli zvážit, aby metriky úložiště může automaticky upozornit na důležité změny v chování hello vaší služby úložiště. Pokud toodownload nástroj Průzkumníka úložiště používáte ve formátu odděleného tato data metriky, můžete data hello tooanalyze Microsoft Excel. Najdete v příspěvku blogu hello [Průzkumníci úložiště Microsoft Azure](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/03/11/windows-azure-storage-explorers-2014.aspx) seznam nástrojů Průzkumníka úložiště k dispozici.

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

## <a name="next-steps"></a>Další kroky:
[Povolení analytika úložiště protokolování a přístup k datům protokolu](https://msdn.microsoft.com/library/dn782840.aspx)
