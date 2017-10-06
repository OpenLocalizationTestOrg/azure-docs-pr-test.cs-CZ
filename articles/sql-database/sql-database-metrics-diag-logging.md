---
title: "aaaAzure SQL databáze metriky & protokolování diagnostiky | Microsoft Docs"
description: "Další informace o konfiguraci využití prostředků toostore prostředků Azure SQL Database, připojení a statistik provádění dotazů."
services: sql-database
documentationcenter: 
author: vvasic
manager: jhubbard
editor: 
ms.assetid: 89c2a155-c2fb-4b67-bc19-9b4e03c6d3bc
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: vvasic
ms.openlocfilehash: e6f9e24992ca4f84f701e1ef858e98dc7b481e28
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-metrics-and-diagnostics-logging"></a>Azure SQL Database metrik a protokolování diagnostiky 
Databáze SQL Azure můžete emitování metriky a diagnostické protokoly pro snazší monitorování. Můžete nakonfigurovat využití prostředků toostore Azure SQL Database, pracovníků a relací a připojení do jednoho z těchto prostředků Azure:
- **Azure Storage:** Pro archivaci obrovských objemů telemetrických dat za nízkou cenu.
- **Azure centra událostí**: pro integraci telemetrie databáze SQL Azure s vlastní řešení monitorování nebo aktivní kanálů
- **Azure Log Analytics**: pro předinstalované hello řešení s vytváření sestav, výstrahy a zmírnění možnosti monitorování 

    ![Architektura](./media/sql-database-metrics-diag-logging/architecture.png)

## <a name="enable-logging"></a>Povolit protokolování

Ve výchozím nastavení není povoleno metrik a protokolování diagnostiky. Můžete povolit a spravovat metrik a protokolování diagnostiky pomocí jedné z následujících metod hello:
- portál Azure
- PowerShell
- Azure CLI
- REST API 
- Šablona Resource Manageru

Pokud povolíte metrik a protokolování diagnostiky, je potřeba toospecify hello prostředků Azure, kde se shromažďují vybraná data. Dostupné možnosti:
- Log Analytics
- Centrum událostí
- Azure Storage 

Můžete zřídit nového prostředku Azure nebo vybrat existující prostředek. Po výběru hello prostředků úložiště, musíte toospecify které toocollect data. Mezi dostupné možnosti patří:

- **[1 minutu metriky](sql-database-metrics-diag-logging.md#1-minute-metrics)**  – obsahuje procento DTU, omezení jednotek DTU, procento využití procesoru, fyzické číst procento, protokolu zapisovat procento, bylo úspěšné nebo neúspěšné/blokováno připojení brány firewall, procento relací, procento pracovních procesů úložiště, procento úložiště, XTP procento úložiště

Pokud zadáte centra událostí nebo azurestorage účet, můžete zadat toospecify zásady uchovávání data, která je starší než vybrané časové období se odstraní. Pokud zadáte analýzy protokolů, zásady uchovávání informací hello závisí na hello vybraná cenová úroveň. Další informace o [analýzy protokolů ceny](https://azure.microsoft.com/pricing/details/log-analytics/). 

Doporučujeme, abyste si přečetli obou hello [přehled metriky v Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md) a [přehled o Azure diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) články toogain představu o není pouze jak tooenable protokolování, ale hello metriky a protokolu kategorií podporuje hello různé služby Azure.

### <a name="azure-portal"></a>portál Azure

tooenable metriky a shromažďování diagnostických protokolů v hello portál Azure, přejděte tooyour Azure SQL database nebo stránky elastického fondu a pak klikněte na tlačítko **nastavení pro diagnostiku**.

   ![Povolit v hello portálu Azure](./media/sql-database-metrics-diag-logging/enable-portal.png)

### <a name="powershell"></a>PowerShell

tooenable metrik a protokolování diagnostiky pomocí prostředí PowerShell, použijte hello následující příkazy:

- úložiště tooenable diagnostických protokolů v účtu úložiště, použijte tento příkaz:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   Hello ID účtu úložiště je id prostředku hello toowhich účet úložiště hello chcete toosend hello protokoly.

- tooenable vysílání datového proudu diagnostické protokoly tooan centra událostí, použijte tento příkaz:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   Hello ID pravidla Service Bus je řetězec s Tento formát:

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 

- tooenable odesílání diagnostických protokolů pracovní prostor analýzy protokolů tooa, použijte tento příkaz:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of hello log analytics workspace] -Enabled $true
   ```

- Můžete získat id prostředku hello pracovní prostor analýzy protokolů pomocí hello následující příkaz:

   ```powershell
   (Get-AzureRmOperationalInsightsWorkspace).ResourceId
   ```

Tyto parametry tooenable můžete kombinovat více možností výstup.

### <a name="cli"></a>Rozhraní příkazového řádku

tooenable metrik a protokolování pomocí diagnostiky hello rozhraní příkazového řádku Azure, hello použijte následující příkazy:

- úložiště tooenable diagnostických protokolů v účtu úložiště, použijte tento příkaz:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   Hello ID účtu úložiště je id prostředku hello toowhich účet úložiště hello chcete toosend hello protokoly.

- tooenable vysílání datového proudu diagnostické protokoly tooan centra událostí, použijte tento příkaz:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   Hello ID pravidla Service Bus je řetězec s Tento formát:

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- tooenable odesílání diagnostických protokolů pracovní prostor analýzy protokolů tooa, použijte tento příkaz:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of hello log analytics workspace> --enabled true
   ```

Tyto parametry tooenable můžete kombinovat více možností výstup.

### <a name="rest-api"></a>REST API

Přečtěte si informace o tom příliš[změnit nastavení pro diagnostiku pomocí hello REST API služby Azure monitorování](https://msdn.microsoft.com/library/azure/dn931931.aspx). 

### <a name="resource-manager-template"></a>Šablona Resource Manageru

Přečtěte si informace o tom příliš[povolit nastavení pro diagnostiku při vytváření prostředků pomocí šablony Resource Manageru](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md). 

## <a name="stream-into-log-analytics"></a>Datový proud do analýzy protokolů 
Diagnostické protokoly a Azure SQL Database metriky Streamovat do pomocí možnosti integrované "Odeslat tooLog Analytics" hello hello portálu nebo povolením analýzy protokolů v nastavení diagnostiky pomocí rutin prostředí Azure PowerShell, rozhraní příkazového řádku Azure nebo REST Azure monitorování analýzy protokolů ROZHRANÍ API.

### <a name="installation-overview"></a>Přehled instalace

Monitorování firemního vozového Azure SQL Database je jednoduchý analýzy protokolů. Vyžadují se tři kroky:

1.  Vytvořte prostředek analýzy protokolů
2.  Konfigurace databáze toorecord metriky a diagnostických protokolů do hello vytvořili analýzy protokolů
3.  Nainstalujte **Azure SQL Analytics** řešení z Galerie v analýzy protokolů

### <a name="create-log-analytics-resource"></a>Vytvořte prostředek analýzy protokolů

1. Klikněte na tlačítko **nový** v levé nabídce hello.
2. Klikněte na tlačítko **monitorování + správy**
3. Klikněte na tlačítko **protokolu analýzy**
4. Vyplňte formulář analýzy protokolů hello s hello další vyžadované informace: název pracovního prostoru, předplatné, skupinu prostředků, umístění a cenovou úroveň.

   ![analýzy protokolů](./media/sql-database-metrics-diag-logging/log-analytics.png)

### <a name="configure-databases-toorecord-metrics-and-diagnostic-logs"></a>Konfigurace databáze toorecord metriky a diagnostických protokolů

Hello nejjednodušší způsob, jak tooconfigure kde databází záznam jejich metriky je prostřednictvím hello portálu Azure. V hello portálu Azure, přejděte tooyour prostředků Azure SQL Database a klikněte na tlačítko **nastavení diagnostiky**. 

### <a name="install-hello-azure-sql-analytics-solution-from-gallery"></a>Nainstalujte řešení hello analýzy SQL Azure z Galerie  

1. Jakmile je vytvořen hello prostředků analýzy protokolů a je do ní toku dat, nainstalujte řešení analýzy SQL Azure. To lze provést prostřednictvím hello **řešení Galerie** , které můžete najít na domovskou stránku hello OMS a v nabídce straně hello. V galerii hello, najít a klikněte na tlačítko **Azure SQL Analytics** řešení a klikněte na tlačítko **přidat**.

   ![řešení monitorování](./media/sql-database-metrics-diag-logging/monitoring-solution.png)

2. Na domovské stránce OMS, volá se nová dlaždice **Azure SQL Analytics** se zobrazí. Výběr tuto dlaždici se otevře řídicí panel Azure SQL Analytics hello.

### <a name="using-azure-sql-analytics-solution"></a>Pomocí řešení analýzy Azure SQL

Azure SQL Analytics je hierarchická řídicí panel, který vám umožní toonavigate prostřednictvím hierarchie hello prostředků Azure SQL Database. Tato funkce umožňuje vám toodo souhrnné monitorování ale také vám umožňuje tooscope vaší monitorování hello toojust správné nastavení prostředků.
Řídicí panel obsahuje seznamy hello různých prostředků ve sloupci hello vybraný zdroj. Například pro vybrané předplatné uvidíte hello všechny servery, elastické fondy a databází, které patří toohello vybrané předplatné. Kromě toho pro elastické fondy a databází, se zobrazí metriky využití prostředků hello tohoto prostředku. To zahrnuje grafy pro DTU, procesoru, vstupně-výstupní operace, protokolu, relace, pracovníci, připojení a úložiště v GB.

## <a name="stream-into-azure-event-hub"></a>Datový proud do centra událostí Azure

Diagnostické protokoly a Azure SQL Database metriky Streamovat do centra událostí pomocí možnosti integrované "centra událostí tooan datového proudu" hello hello portálu nebo povolením Id Service Bus pravidla v nastavení diagnostiky pomocí rutin prostředí Azure PowerShell, rozhraní příkazového řádku Azure nebo REST Azure monitorování ROZHRANÍ API. 

### <a name="what-toodo-with-metrics-and-diagnostic-logs-in-event-hub"></a>Jaké toodo s metriky a diagnostických protokolů v Centru událostí?
Jakmile je pomocí datového proudu vysílána hello vybraná data do centra událostí, jste jeden krok blíž tooenabling pokročilé scénáře monitorování. Služba Event Hubs slouží jako hello "přední dveře" pro kanál událostí, a jakmile jsou data shromážděna do centra událostí, lze je transformovat a uložené pomocí kteréhokoli poskytovatele služeb, analýzu v reálném čase nebo adaptérů pro dávkování či ukládání. Služba Event Hubs oddělí hello produkce datového proudu událostí od spotřeby těchto události, hello, aby příjemci událostí přístup hello události svého vlastního plánu. Další informace o Centru událostí najdete v tématu:

- [Co je Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)?
- [Začínáme s Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)


Můžete použít hello streamování schopností několika způsoby:

-   Zobrazit stav služby pomocí vysílání datového proudu tooPowerBI "aktivní cesta" data - pomocí služby Event Hubs, Stream Analytics a PowerBI, můžete snadno převést metriky a diagnostiky dat do téměř přehledy v reálném čase na služeb Azure. Přehled o tom, jak tooset se službou Event Hubs, zpracování dat pomocí služby Stream Analytics a použít PowerBI jako výstup, najdete v části [Stream Analytics a Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md).
-   Datový proud protokolů výrobců toothird protokolování telemetrie datové proudy a – Event Hubs pomocí vysílání datového proudu je můžete získat metriky a diagnostických protokolů v toodifferent řešení pro monitorování a protokolu analýzu třetích stran. 
-   Vytvořte vlastní telemetrii a protokolování platforma – Pokud už máte uživatelské telemetrie platformy nebo jsou právě přemýšlíte o vytváření jeden hello vysoce škálovatelné publikování a odběru, že povaha Event Hubs vám umožní tooflexibly ingestování diagnostické protokoly. V tématu [Dana Rosanova Průvodce toousing Event Hubs v globálním měřítku telemetrie platformu](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="stream-into-azure-storage"></a>Datový proud do úložiště Azure

Azure SQL Database metriky a diagnostické protokoly mohou být uloženy do Azure Storage pomocí předdefinovaných možnost "Archivu tooa účet úložiště" hello v hello portál Azure nebo povolením Azure Storage v nastavení diagnostiky pomocí rutin prostředí Azure PowerShell, rozhraní příkazového řádku Azure nebo Azure Monitorování REST API.

### <a name="schema-of-metrics-and-diagnostic-logs-in-hello-storage-account"></a>Schéma metriky a diagnostických protokolů v účtu úložiště hello

Jakmile nastavíte metriky a shromažďování diagnostických protokolů, kontejner úložiště se vytvoří v účtu úložiště hello, kterou jste vybrali při hello první řádky dat jsou k dispozici. Struktura Hello tyto objekty BLOB je:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ databases/{database_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```
    
Nebo jednodušeji:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/{resource Id}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Například může být název objektu blob pro 1 minutu metriky:

```powershell
insights-metrics-minute/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.SQL/ servers/Server1/databases/database1/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

V případě, že chcete toorecord hello data z hello elastického fondu, název objektu blob se trochu liší:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ elasticPools/{elastic_pool_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

### <a name="download-metrics-and-logs-from-azure-storage"></a>Stažení metriky a protokoly ze služby Azure storage

V tématu [stáhnout metriky a diagnostické protokoly ze služby Azure Storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)

## <a name="1-minute-metrics"></a>1 minutu metriky

| |  |
|---|---|
|**Prostředek**|**Metriky**|
|Databáze|Procento DTU DTU používá, omezení jednotek DTU, procento využití procesoru, procento fyzické dat pro čtení, zápisu protokolu procento, bylo úspěšné nebo neúspěšné/blokováno připojení brány firewall, procento relací, procento pracovních procesů, úložiště, procento úložiště, XTP procento úložiště, zablokování |
|Elastický fond|Procento eDTU eDTU používá, omezení eDTU, procento využití procesoru, fyzické čtení procento, protokolu zapisovat procento, procento relací, procento pracovních procesů, úložiště, procento úložiště, limit úložiště, XTP procento úložiště |
|||

## <a name="next-steps"></a>Další kroky

- Čtení obou hello [přehled metriky v Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md) a [přehled o Azure diagnostické protokoly](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) články toogain představu o není pouze jak tooenable protokolování, ale hello metriky a protokolu kategorií podporuje hello různé služby Azure.
- Přečtěte si tyto články toolearn o službě event hubs:
   - [Co je Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md)?
   - [Začínáme s Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
- V tématu [stáhnout metriky a diagnostické protokoly ze služby Azure Storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs)
