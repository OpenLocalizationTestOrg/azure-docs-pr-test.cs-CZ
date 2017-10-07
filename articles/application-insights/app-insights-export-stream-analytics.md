---
title: "aaaExport pomocí služby Stream Analytics z Azure Application Insights | Microsoft Docs"
description: "Stream Analytics můžete nepřetržitě transformace, filtr a hello data trasy, které exportujete z Application Insights."
services: application-insights
documentationcenter: 
author: noamben
manager: carmonm
ms.assetid: 31594221-17bd-4e5e-9534-950f3b022209
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: bwren
ms.openlocfilehash: fda9b64f588c520833b2669eafdf650efc3de6be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-stream-analytics-tooprocess-exported-data-from-application-insights"></a>Použití Stream Analytics tooprocess exportovaná data ze služby Application Insights
[Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) je hello ideální nástroj pro zpracování dat [exportovaný z Application Insights](app-insights-export-telemetry.md). Stream Analytics můžete načítat data z různých zdrojů. Můžete transformovat a filtrování dat hello a pak směruje tooa řadu jímky.

V tomto příkladu vytvoříme adaptéru, která vezme všechna data ze služby Application Insights, přejmenuje a zpracovává některá pole hello a prostřednictvím kanálu předá ho do Power BI.

> [!WARNING]
> Existují mnohem lepší a jednodušší [doporučené způsoby, jak toodisplay Application Insights data v Power BI](app-insights-export-power-bi.md). Hello cesta zde znázorněný je právě tooillustrate příklad jak tooprocess exportovat data.
> 
> 

![Blokový diagram pro export prostřednictvím SA tooPBI](./media/app-insights-export-stream-analytics/020.png)

## <a name="create-storage-in-azure"></a>Vytvořte úložiště v Azure
Průběžné export vždy výstupy účet úložiště Azure tooan dat, proto musíte nejprve toocreate hello úložiště.

1. Vytvořit účet úložiště "classic" v rámci vašeho předplatného v hello [portál Azure](https://portal.azure.com).
   
   ![Na portálu Azure zvolte nový, Data, úložiště](./media/app-insights-export-stream-analytics/030.png)
2. Vytvoření kontejneru
   
    ![V novém úložišti hello vyberte kontejnery, klikněte na dlaždici hello kontejnery a potom přidat](./media/app-insights-export-stream-analytics/040.png)
3. Zkopírovat přístupový klíč k úložišti hello
   
    Budete je potřebovat brzy tooset až hello vstupní toohello stream analytics služby.
   
    ![V úložišti hello otevře se nastavení klíče a proveďte kopii hello primární přístupový klíč](./media/app-insights-export-stream-analytics/045.png)

## <a name="start-continuous-export-tooazure-storage"></a>Spustit průběžné export tooAzure úložiště
[Průběžné export](app-insights-export-telemetry.md) přesune data z Application Insights do úložiště Azure.

1. V hello portálu Azure vyhledejte prostředek Application Insights toohello, který jste vytvořili pro vaše aplikace.
   
    ![Vyberte možnost Procházet, Application Insights, vaše aplikace](./media/app-insights-export-stream-analytics/050.png)
2. Vytvořte průběžné export.
   
    ![Vyberte nastavení, průběžné exportu, přidejte](./media/app-insights-export-stream-analytics/060.png)

    Vyberte účet úložiště hello, které jste vytvořili dříve:

    ![Nastavit cíl exportu hello](./media/app-insights-export-stream-analytics/070.png)

    Nastavte hello typy událostí, které se mají toosee:

    ![Vyberte typy událostí](./media/app-insights-export-stream-analytics/080.png)

1. Některá data hromadí let. Sledujte a umožnit lidem nějakou dobu používat vaši aplikaci. Telemetrická data se odešlou a zobrazí statistické grafy v [metriky explorer](app-insights-metrics-explorer.md) a jednotlivé události v [diagnostické vyhledávání](app-insights-diagnostic-search.md). 
   
    A navíc hello dat bude exportovat tooyour úložiště. 
2. Zkontrolujte hello exportovat data. V sadě Visual Studio, vyberte **zobrazení / cloudu Explorer**a otevřete Azure nebo úložiště. (Pokud nemáte tento příkaz nabídky, je třeba tooinstall hello Azure SDK: Otevřete dialogové okno Nový projekt hello a otevřete Visual C# / Cloud / získat Microsoft Azure SDK pro .NET.)
   
    ![](./media/app-insights-export-stream-analytics/04-data.png)
   
    Poznamenejte si hello běžné součástí hello název cesty, které je odvozeno z názvu a instrumentace klíč aplikace hello. 

Hello události se zapisují tooblob soubory ve formátu JSON. Každý soubor může obsahovat jeden nebo více událostí. Proto rádi bychom znali data události hello tooread a filtrování hello pole, která má být. Jsou k dispozici všechny typy věcí, které můžeme udělat s daty hello, ale naše plán dnes je toouse Stream Analytics toopipe hello data tooPower BI.

## <a name="create-an-azure-stream-analytics-instance"></a>Vytvoření instance služby Azure Stream Analytics
Z hello [portál Azure Classic](https://manage.windowsazure.com/), vyberte službu Azure Stream Analytics hello a vytvořit novou úlohu služby Stream Analytics:

![](./media/app-insights-export-stream-analytics/090.png)

![](./media/app-insights-export-stream-analytics/100.png)

Když je vytvořena nová úloha hello, rozbalte položku Podrobnosti:

![](./media/app-insights-export-stream-analytics/110.png)

### <a name="set-blob-location"></a>Nastavení umístění objektu blob
Nastavte tootake vstup z objektu blob služby průběžné Export:

![](./media/app-insights-export-stream-analytics/120.png)

Nyní budete potřebovat hello primární přístupový klíč z vašeho účtu úložiště, který jste si předtím poznamenali. Nastavením této hodnoty jako hello klíč účtu úložiště.

![](./media/app-insights-export-stream-analytics/130.png)

### <a name="set-path-prefix-pattern"></a>Sada cesta předpona vzoru
![](./media/app-insights-export-stream-analytics/140.png)

**Být jisti tooset hello formát data tooYYYY-MM-DD (s pomlčkami).**

Hello cesta předpony vzor Určuje, kde Stream Analytics vyhledá hello vstupní soubory v úložišti hello. Je třeba tooset ho toocorrespond toohow průběžné exportovat ukládá hello data. Nastavte takto:

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

V tomto příkladu:

* `webapplication27`je název hello hello prostředek Application Insights **malá všechny**.
* `1234...`je klíč instrumentace hello hello prostředku Application Insights **vynechání pomlčky**. 
* `PageViews`je hello typ dat chcete tooanalyze. dostupné typy Hello závisí na hello filtr, který nastavíte v průběžné exportovat. Zkontrolujte hello exportovaná data toosee hello jiné typy k dispozici a zobrazí hello [Exportovat datový model](app-insights-export-data-model.md).
* `/{date}/{time}`vzor zapsána oznámena.

> [!NOTE]
> Zkontrolujte toomake úložiště hello se, že můžete získat cestu hello správné.
> 
> 

### <a name="finish-initial-setup"></a>Dokončit počáteční nastavení
Zkontrolujte formát serializace hello:

![Potvrďte a zavřete průvodce](./media/app-insights-export-stream-analytics/150.png)

Zavřete průvodce hello a počkejte toocomplete hello instalační program.

> [!TIP]
> Použijte hello Ukázkový příkaz toodownload některá data. Umožňuje zachovat ji jako ukázka toodebug testovací dotazu.
> 
> 

## <a name="set-hello-output"></a>Výstup hello sady
Nyní vyberte úlohu a nastavte výstup hello.

![Vyberte nový kanál hello, klikněte na výstupy, přidat, Power BI](./media/app-insights-export-stream-analytics/160.png)

Zadejte vaše **pracovní nebo školní účet** tooauthorize tooaccess Stream Analytics prostředku Power BI. Pak vytvořte název pro výstup hello a pro datovou sadu Power BI hello cíl a tabulku.

![Skladová tři názvy](./media/app-insights-export-stream-analytics/170.png)

## <a name="set-hello-query"></a>Sada hello dotazu
dotaz Hello řídí hello překlad ze vstupní toooutput.

![Vyberte hello úlohu a klikněte na dotaz. Vložte následující ukázka hello.](./media/app-insights-export-stream-analytics/180.png)

Použijte hello testovací funkce toocheck získat výstup hello správné. Poskytněte hello ukázková data, která trvala ze stránky hello vstupy. 

### <a name="query-toodisplay-counts-of-events"></a>Dotaz toodisplay počet událostí
Vložte tento dotaz:

```SQL

    SELECT
      flat.ArrayValue.name,
      count(*)
    INTO
      [pbi-output]
    FROM
      [export-input] A
    OUTER APPLY GetElements(A.[event]) as flat
    GROUP BY TumblingWindow(minute, 1), flat.ArrayValue.name
```

* Export vstup je alias hello jsme vám Dal toohello vstupní datový proud
* alias pro výstup hello, které jsme definovali je pbi-output
* Používáme [vnější použít GetElements](https://msdn.microsoft.com/library/azure/dn706229.aspx) vzhledem k tomu, že název události hello je ve vnořené arrray JSON. Potom vyberte vyskladnění hello hello název události, spolu s počtem hello počet instancí s tímto názvem v hello časové období. Hello [Group By](https://msdn.microsoft.com/library/azure/dn835023.aspx) klauzule skupiny hello elementy do časových období 1 minuta.

### <a name="query-toodisplay-metric-values"></a>Hodnoty metriky toodisplay dotazu
```SQL

    SELECT
      A.context.data.eventtime,
      avg(CASE WHEN flat.arrayvalue.myMetric.value IS NULL THEN 0 ELSE  flat.arrayvalue.myMetric.value END) as myValue
    INTO
      [pbi-output]
    FROM
      [export-input] A
    OUTER APPLY GetElements(A.context.custom.metrics) as flat
    GROUP BY TumblingWindow(minute, 1), A.context.data.eventtime

``` 

* Tento dotaz prochází do hello metriky telemetrie tooget hello čas události a metriky hodnotu hello. Hello metriky hodnoty jsou uvnitř pole, takže používáme hello vnější GetElements použít vzor tooextract hello řádků. "myMetric" v tomto případě je název hello metrika hello. 

### <a name="query-tooinclude-values-of-dimension-properties"></a>Dotaz tooinclude hodnoty vlastností dimenze
```SQL

    WITH flat AS (
    SELECT
      MySource.context.data.eventTime as eventTime,
      InstanceId = MyDimension.ArrayValue.InstanceId.value,
      BusinessUnitId = MyDimension.ArrayValue.BusinessUnitId.value
    FROM MySource
    OUTER APPLY GetArrayElements(MySource.context.custom.dimensions) MyDimension
    )
    SELECT
     eventTime,
     InstanceId,
     BusinessUnitId
    INTO AIOutput
    FROM flat

```

* Tento dotaz obsahuje hodnoty vlastností dimenze hello bez v závislosti na konkrétní dimenzi probíhá na pevnou index v poli dimenze hello.

## <a name="run-hello-job"></a>Spustit úlohu hello
Vyberte datum v hello po toostart hello úlohu. 

![Vyberte hello úlohu a klikněte na dotaz. Vložte následující ukázka hello.](./media/app-insights-export-stream-analytics/190.png)

Počkejte, dokud je spuštěná úloha hello.

## <a name="see-results-in-power-bi"></a>Zobrazte výsledky v Power BI
> [!WARNING]
> Existují mnohem lepší a jednodušší [doporučené způsoby, jak toodisplay Application Insights data v Power BI](app-insights-export-power-bi.md). Hello cesta zde znázorněný je právě tooillustrate příklad jak tooprocess exportovat data.
> 
> 

Otevřít Power BI s použitím pracovní nebo školní účet a vyberte hello datovou sadu a tabulky, který jste definovali jako výstup hello úlohy Stream Analytics hello.

![V Power BI vyberte pole a datové sady.](./media/app-insights-export-stream-analytics/200.png)

Teď můžete použít tuto datovou sadu v sestavy a řídicí panely v [Power BI](https://powerbi.microsoft.com).

![V Power BI vyberte pole a datové sady.](./media/app-insights-export-stream-analytics/210.png)

## <a name="no-data"></a>Žádná data?
* Zkontrolujte, které [formátu datum hello sady](#set-path-prefix-pattern) správně tooYYYY-MM-DD (s pomlčkami).

## <a name="video"></a>Video
Noam Ben Zeev ukazuje, jak tooprocess export dat pomocí služby Stream Analytics.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Export-to-Power-BI-from-Application-Insights/player]
> 
> 

## <a name="next-steps"></a>Další kroky
* [Průběžný export](app-insights-export-telemetry.md)
* [Podrobný datový model referenční informace pro hello typy a hodnoty vlastností.](app-insights-export-data-model.md)
* [Application Insights](app-insights-overview.md)

