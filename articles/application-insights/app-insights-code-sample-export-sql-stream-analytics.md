---
title: "Export tooSQL ze služby Azure Application Insights | Microsoft Docs"
description: "Nepřetržitě exportujte tooSQL Application Insights dat pomocí služby Stream Analytics."
services: application-insights
documentationcenter: 
author: noamben
manager: carmonm
ms.assetid: 48903032-2c99-4987-9948-d6e4559b4a63
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/06/2015
ms.author: bwren
ms.openlocfilehash: 58b579499113751a088dc7e66cbec71529773322
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-export-toosql-from-application-insights-using-stream-analytics"></a>Návod: Export tooSQL ze služby Application Insights pomocí služby Stream Analytics
Tento článek ukazuje, jak toomove vaše telemetrická data z [Azure Application Insights] [ start] do Azure SQL database pomocí [průběžné exportovat] [ export] a [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). 

Průběžné export přesune telemetrická data do úložiště Azure ve formátu JSON. Jsme budete objekty JSON hello pomocí Azure Stream Analytics analyzovat a vytvořte řádky v tabulce databáze.

(Obecně platí, průběžné Export je způsob, jak toodo hello vlastní analýzu hello telemetrie aplikace odeslat tooApplication statistiky. Vám může přizpůsobit tento toodo ukázkový kód jiných věcí pomocí telemetrie hello exportovali, jako je například agregace dat.)

Začneme s hello předpokládá, že už máte aplikaci hello se mají toomonitor.

V tomto příkladu použijeme hello stránky zobrazení dat, ale hello stejného vzoru lze snadno rozšířit tooother datové typy, jako jsou vlastní události a výjimky. 

## <a name="add-application-insights-tooyour-application"></a>Přidání Application Insights tooyour aplikace
tooget spuštění:

1. [Nastavte Application Insights pro webové stránky](app-insights-javascript.md). 
   
    (V tomto příkladu budete zaměříme na zpracování stránky zobrazení dat z hello klientský prohlížeč, ale můžete také nastavit Application Insights pro hello na straně serveru vaší [Java](app-insights-java-get-started.md) nebo [ASP.NET](app-insights-asp-net.md) aplikace a proces žádosti závislosti a další telemetrií serveru.)
2. Publikování aplikace a sledujte telemetrii dat zobrazovaných v prostředku Application Insights.

## <a name="create-storage-in-azure"></a>Vytvořte úložiště v Azure
Průběžné export vždy výstupy účet úložiště Azure tooan dat, proto musíte nejprve toocreate hello úložiště.

1. Vytvořit účet úložiště v rámci vašeho předplatného v hello [portál Azure][portal].
   
    ![Na portálu Azure zvolte nový, Data, úložiště. Vyberte Classic, vyberte vytvořit. Zadejte název úložiště.](./media/app-insights-code-sample-export-sql-stream-analytics/040-store.png)
2. Vytvoření kontejneru
   
    ![V novém úložišti hello vyberte kontejnery, klikněte na dlaždici hello kontejnery a potom přidat](./media/app-insights-code-sample-export-sql-stream-analytics/050-container.png)
3. Zkopírovat přístupový klíč k úložišti hello
   
    Budete je potřebovat brzy tooset až hello vstupní toohello stream analytics služby.
   
    ![V úložišti hello otevře se nastavení klíče a proveďte kopii hello primární přístupový klíč](./media/app-insights-code-sample-export-sql-stream-analytics/21-storage-key.png)

## <a name="start-continuous-export-tooazure-storage"></a>Spustit průběžné export tooAzure úložiště
1. V hello portálu Azure vyhledejte prostředek Application Insights toohello, který jste vytvořili pro vaše aplikace.
   
    ![Vyberte možnost Procházet, Application Insights, vaše aplikace](./media/app-insights-code-sample-export-sql-stream-analytics/060-browse.png)
2. Vytvořte průběžné export.
   
    ![Vyberte nastavení, průběžné exportu, přidejte](./media/app-insights-code-sample-export-sql-stream-analytics/070-export.png)

    Vyberte účet úložiště hello, které jste vytvořili dříve:

    ![Nastavit cíl exportu hello](./media/app-insights-code-sample-export-sql-stream-analytics/080-add.png)

    Nastavte hello typy událostí, které se mají toosee:

    ![Vyberte typy událostí](./media/app-insights-code-sample-export-sql-stream-analytics/085-types.png)


1. Některá data hromadí let. Sledujte a umožnit lidem nějakou dobu používat vaši aplikaci. Telemetrická data se odešlou a zobrazí statistické grafy v [metriky explorer](app-insights-metrics-explorer.md) a jednotlivé události v [diagnostické vyhledávání](app-insights-diagnostic-search.md). 
   
    A navíc hello dat bude exportovat tooyour úložiště. 
2. Zkontrolujte hello export dat, buď v portálu hello – volba **Procházet**, vyberte svůj účet úložiště a potom **kontejnery** - nebo v sadě Visual Studio. V sadě Visual Studio, vyberte **zobrazení / cloudu Explorer**a otevřete Azure nebo úložiště. (Pokud nemáte tento příkaz nabídky, je třeba tooinstall hello Azure SDK: Otevřete dialogové okno Nový projekt hello a otevřete Visual C# / Cloud / získat Microsoft Azure SDK pro .NET.)
   
    ![V sadě Visual Studio otevřete prohlížeč Server, Azure, úložiště](./media/app-insights-code-sample-export-sql-stream-analytics/087-explorer.png)
   
    Poznamenejte si hello běžné součástí hello název cesty, které je odvozeno z názvu a instrumentace klíč aplikace hello. 

Hello události se zapisují tooblob soubory ve formátu JSON. Každý soubor může obsahovat jeden nebo více událostí. Proto rádi bychom znali data události hello tooread a filtrování hello pole, která má být. Jsou k dispozici všechny typy věcí, které můžeme udělat s daty hello, ale naše plán dnes je toouse Stream Analytics toomove hello data tooa databáze SQL. Která znamená, že snadno toorun spoustu zajímavé dotazy.

## <a name="create-an-azure-sql-database"></a>Vytvoření databáze Azure SQL
Znovu se spouští ze svého předplatného v [portál Azure][portal], vytvořit databázi hello (a nový server, pokud již máte jeden) toowhich napíšete hello data.

![Nový dat, SQL](./media/app-insights-code-sample-export-sql-stream-analytics/090-sql.png)

Zkontrolujte, zda že server databáze hello umožňuje přístup tooAzure služby:

![Procházet, servery, server, nastavení, brána Firewall, povolit přístup tooAzure](./media/app-insights-code-sample-export-sql-stream-analytics/100-sqlaccess.png)

## <a name="create-a-table-in-azure-sql-db"></a>Vytvoření tabulky v databázi SQL Azure
Připojte databázi toohello vytvořené v předchozí části hello s vaší nástroj pro správu upřednostňované. V tomto návodu použijeme [nástroje správy systému SQL Server](https://msdn.microsoft.com/ms174173.aspx) (SSMS).

![](./media/app-insights-code-sample-export-sql-stream-analytics/31-sql-table.png)

Vytvořit nový dotaz a spusťte hello následující T-SQL:

```SQL

CREATE TABLE [dbo].[PageViewsTable](
    [pageName] [nvarchar](max) NOT NULL,
    [viewCount] [int] NOT NULL,
    [url] [nvarchar](max) NULL,
    [urlDataPort] [int] NULL,
    [urlDataprotocol] [nvarchar](50) NULL,
    [urlDataHost] [nvarchar](50) NULL,
    [urlDataBase] [nvarchar](50) NULL,
    [urlDataHashTag] [nvarchar](max) NULL,
    [eventTime] [datetime] NOT NULL,
    [isSynthetic] [nvarchar](50) NULL,
    [deviceId] [nvarchar](50) NULL,
    [deviceType] [nvarchar](50) NULL,
    [os] [nvarchar](50) NULL,
    [osVersion] [nvarchar](50) NULL,
    [locale] [nvarchar](50) NULL,
    [userAgent] [nvarchar](max) NULL,
    [browser] [nvarchar](50) NULL,
    [browserVersion] [nvarchar](50) NULL,
    [screenResolution] [nvarchar](50) NULL,
    [sessionId] [nvarchar](max) NULL,
    [sessionIsFirst] [nvarchar](50) NULL,
    [clientIp] [nvarchar](50) NULL,
    [continent] [nvarchar](50) NULL,
    [country] [nvarchar](50) NULL,
    [province] [nvarchar](50) NULL,
    [city] [nvarchar](50) NULL
)

CREATE CLUSTERED INDEX [pvTblIdx] ON [dbo].[PageViewsTable]
(
    [eventTime] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON)

```

![](./media/app-insights-code-sample-export-sql-stream-analytics/34-create-table.png)

V této ukázce používáme data ze zobrazení stránky. toosee hello jiná data dostupná, zkontrolujte výstupu JSON a najdete v části hello [Exportovat datový model](app-insights-export-data-model.md).

## <a name="create-an-azure-stream-analytics-instance"></a>Vytvoření instance služby Azure Stream Analytics
Z hello [portál Azure Classic](https://manage.windowsazure.com/), vyberte službu Azure Stream Analytics hello a vytvořit novou úlohu služby Stream Analytics:

![](./media/app-insights-code-sample-export-sql-stream-analytics/37-create-stream-analytics.png)

![](./media/app-insights-code-sample-export-sql-stream-analytics/38-create-stream-analytics-form.png)

Když je vytvořena nová úloha hello, rozbalte položku Podrobnosti:

![](./media/app-insights-code-sample-export-sql-stream-analytics/41-sa-job.png)

#### <a name="set-blob-location"></a>Nastavení umístění objektu blob
Nastavte tootake vstup z objektu blob služby průběžné Export:

![](./media/app-insights-code-sample-export-sql-stream-analytics/42-sa-wizard1.png)

Nyní budete potřebovat hello primární přístupový klíč z vašeho účtu úložiště, který jste si předtím poznamenali. Nastavením této hodnoty jako hello klíč účtu úložiště.

![](./media/app-insights-code-sample-export-sql-stream-analytics/46-sa-wizard2.png)

#### <a name="set-path-prefix-pattern"></a>Sada cesta předpona vzoru
![](./media/app-insights-code-sample-export-sql-stream-analytics/47-sa-wizard3.png)

Zda tooset hello formát data byla příliš**rrrr-MM-DD** (s **pomlčky**).

Hello cesta předpony vzor Určuje, jak Stream Analytics vyhledá hello vstupní soubory v úložišti hello. Je třeba tooset ho toocorrespond toohow průběžné exportovat ukládá hello data. Nastavte takto:

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

V tomto příkladu:

* `webapplication27`je název hello hello prostředku Application Insights **vše na malá písmena**. 
* `1234...`je klíč instrumentace hello hello prostředek Application Insights **s pomlčkami odebrat**. 
* `PageViews`hello typu dat chceme tooanalyze. dostupné typy Hello závisí na hello filtr, který nastavíte v průběžné exportovat. Zkontrolujte hello exportovaná data toosee hello jiné typy k dispozici a zobrazí hello [Exportovat datový model](app-insights-export-data-model.md).
* `/{date}/{time}`vzor zapsána oznámena.

Název hello tooget a iKey prostředku Application Insights, otevřete Essentials na stránku s jeho přehled nebo otevřete nastavení.

#### <a name="finish-initial-setup"></a>Dokončit počáteční nastavení
Zkontrolujte formát serializace hello:

![Potvrďte a zavřete průvodce](./media/app-insights-code-sample-export-sql-stream-analytics/48-sa-wizard4.png)

Zavřete průvodce hello a počkejte toocomplete hello instalační program.

> [!TIP]
> Použijte hello ukázka funkce toocheck, že jste správně nastavili hello vstupní cesta. Pokud se nezdaří: Zkontrolujte, zda je data v úložišti hello hello ukázka časovém rozmezí jste zvolili. Upravte definici vstupní hello a zkontrolujte nastavení hello účet úložiště, cesta předponu a datum formátu správně.
> 
> 

## <a name="set-query"></a>Sada dotazu
Otevřete část dotazu hello:

![V služby stream analytics vyberte dotazu](./media/app-insights-code-sample-export-sql-stream-analytics/51-query.png)

Nahraďte hello výchozí dotaz s:

```SQL

    SELECT flat.ArrayValue.name as pageName
    , flat.ArrayValue.count as viewCount
    , flat.ArrayValue.url as url
    , flat.ArrayValue.urlData.port as urlDataPort
    , flat.ArrayValue.urlData.protocol as urlDataprotocol
    , flat.ArrayValue.urlData.host as urlDataHost
    , flat.ArrayValue.urlData.base as urlDataBase
    , flat.ArrayValue.urlData.hashTag as urlDataHashTag
      ,A.context.data.eventTime as eventTime
      ,A.context.data.isSynthetic as isSynthetic
      ,A.context.device.id as deviceId
      ,A.context.device.type as deviceType
      ,A.context.device.os as os
      ,A.context.device.osVersion as osVersion
      ,A.context.device.locale as locale
      ,A.context.device.userAgent as userAgent
      ,A.context.device.browser as browser
      ,A.context.device.browserVersion as browserVersion
      ,A.context.device.screenResolution.value as screenResolution
      ,A.context.session.id as sessionId
      ,A.context.session.isFirst as sessionIsFirst
      ,A.context.location.clientip as clientIp
      ,A.context.location.continent as continent
      ,A.context.location.country as country
      ,A.context.location.province as province
      ,A.context.location.city as city
    INTO
      AIOutput
    FROM AIinput A
    CROSS APPLY GetElements(A.[view]) as flat


```

Všimněte si, že nejprve hello několik vlastností jsou konkrétní toopage dat zobrazení. Export jiné typy telemetrických dat bude mít jiné vlastnosti. V tématu hello [podrobné referenční model dat pro hello typy a hodnoty vlastností.](app-insights-export-data-model.md)

## <a name="set-up-output-toodatabase"></a>Nastavit toodatabase výstup
Vyberte SQL jako výstup hello.

![V služby stream analytics vyberte výstupy](./media/app-insights-code-sample-export-sql-stream-analytics/53-store.png)

Zadejte databáze SQL hello.

![Zadejte podrobnosti hello databáze](./media/app-insights-code-sample-export-sql-stream-analytics/55-output.png)

Zavřete průvodce hello a čekat na oznámení, že byla nastavena výstup hello.

## <a name="start-processing"></a>Spuštění zpracování
Spuštění úlohy hello z panelu hello akce:

![V služby stream analytics klikněte na tlačítko Start](./media/app-insights-code-sample-export-sql-stream-analytics/61-start.png)

Můžete vybrat, zda zpracování toostart hello data od teď nebo toostart starší daty. Hello druhé je užitečné, pokud jste předtím průběžné exportovat už běží nějakou dobu.

![V služby stream analytics klikněte na tlačítko Start](./media/app-insights-code-sample-export-sql-stream-analytics/63-start.png)

Po několika minutách přejděte zpět tooSQL nástroje pro správu serveru a podívejte se na hello dat odesílaných v. Například použijte dotaz takto:

    SELECT TOP 100 *
    FROM [dbo].[PageViewsTable]


## <a name="related-articles"></a>Související články
* [Export tooPowerBI pomocí služby Stream Analytics](app-insights-export-power-bi.md)
* [Podrobný datový model referenční informace pro hello typy a hodnoty vlastností.](app-insights-export-data-model.md)
* [Průběžné Export ve službě Application Insights](app-insights-export-telemetry.md)
* [Application Insights](https://azure.microsoft.com/services/application-insights/)

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[export]: app-insights-export-telemetry.md
[metrics]: app-insights-metrics-explorer.md
[portal]: http://portal.azure.com/
[start]: app-insights-overview.md

