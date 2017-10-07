---
title: aaaExport tooPower BI z Application Insights | Microsoft Docs
description: "Analytické dotazy lze zobrazit v Power BI."
services: application-insights
documentationcenter: 
author: noamben
manager: carmonm
ms.assetid: 7f13ea66-09dc-450f-b8f9-f40fdad239f2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: bwren
ms.openlocfilehash: 6668cd7f4e0fbf41695972617f5f8ec207356659
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="feed-power-bi-from-application-insights"></a>Kanálu Power BI z Application Insights
[Power BI](http://www.powerbi.com/) je sada nástrojů obchodní analýzy, které vám pomohou analyzovat data a sdílet uplatnitelné. Bohaté řídicí panely jsou k dispozici na každé zařízení. Můžete kombinovat data z mnoha zdrojů, včetně analytické dotazy z [Azure Application Insights](app-insights-overview.md).

Export dat tooPower Application Insights BI tři doporučené způsoby. Můžete je používat samostatně nebo společně.

* [**Power BI adaptér** ](#power-pi-adapter) -nastavit kompletní řídicí panel telemetrie z vaší aplikace. Předdefinovaná Hello sadu grafy, ale můžete přidat vlastní dotazy z jiných zdrojů.
* [**Export analytické dotazy** ](#export-analytics-queries) -zápisu žádný dotaz chcete pomocí Analytics a exportovat je tooPower BI. Tento dotaz můžete umístit na řídicím panelu společně s dalšími daty.
* [**Průběžné exportu a Stream Analytics** ](app-insights-export-stream-analytics.md) – to zahrnuje další pracovní tooset nahoru. Je užitečné, pokud chcete svá data, tookeep dlouhou dobu. V opačném hello jiné metody se nedoporučuje.

## <a name="power-bi-adapter"></a>Adaptér Power BI
Tato metoda vytvoří kompletní řídicí panel telemetrie. Předdefinovaná Hello počáteční datové sady, ale můžete přidat další tooit data.

### <a name="get-hello-adapter"></a>Získat adaptér hello
1. Přihlaste se příliš[Power BI](https://app.powerbi.com/).
2. Otevřete **získat Data**, **služby**, **Application Insights**
   
    ![Získat ze zdroje dat Application Insights](./media/app-insights-export-power-bi/power-bi-adapter.png)
3. Zadejte podrobnosti o hello prostředku Application Insights.
   
    ![Získat ze zdroje dat Application Insights](./media/app-insights-export-power-bi/azure-subscription-resource-group-name.png)
4. Počkejte, než minutu nebo dvě pro toobe hello data importovat.
   
    ![Adaptér Power BI](./media/app-insights-export-power-bi/010.png)

Můžete upravit řídicí panel hello, kombinace hello Application Insights grafy s u jiných zdrojů a se analytické dotazy. Je Galerie vizualizace můžete získat více grafů, kde každý graf obsahuje parametry, které můžete zadat.

Po importu počáteční hello, hello řídicí panel a sestavy hello pokračovat tooupdate denně. Můžete ovládat hello plán aktualizace pro datovou sadu hello.

## <a name="export-analytics-queries"></a>Export analytické dotazy
Tato trasa vám umožní toowrite žádné Analytics dotazu vám líbilo a poté exportujte tento řídicí panel Power BI tooa. (Můžete přidat řídicí panel toohello vytvořený adaptér hello.)

### <a name="one-time-install-power-bi-desktop"></a>Jednou: Nainstalujte Power BI Desktop
tooimport Application Insights dotaz, použít hello plochy verze Power BI. Ale pak ho můžete publikovat toohello web nebo tooyour prostoru cloudu Power BI. 

Nainstalujte [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).

### <a name="export-an-analytics-query"></a>Export dotaz Analytics
1. [Otevřete analýzy a napsat dotaz](app-insights-analytics-tour.md).
2. Testování a dotaz hello Upřesnit, až budete spokojeni s výsledky hello.

   **Ujistěte se, že hello dotazů běží správně v Analytics předtím, než ho exportovat.**
3. Na hello **exportovat** nabídce zvolte **Power BI (M)**. Uložte soubor textu hello.
   
    ![Export dotazu Power BI](./media/app-insights-export-power-bi/analytics-export-power-bi.png)
4. V Power BI Desktop vyberte **načíst Data, prázdné dotazu** a pak v hello dotazu editor, v části **zobrazení** vyberte **Advanced Editor dotazů**.

    Skript jazyka M hello exportovat vložení do hello Advanced editoru dotazů.

    ![Rozšířený dotaz editoru](./media/app-insights-export-power-bi/power-bi-import-analytics-query.png)

1. Můžete mít tooprovide pověření tooallow Power BI tooaccess Azure. Použijte toosign 'účet organizace, se pomocí účtu Microsoft.
   
    ![Zadejte přihlašovací údaje Azure tooenable Power BI toorun dotazu Application Insights](./media/app-insights-export-power-bi/power-bi-import-sign-in.png)

    (Pokud budete potřebovat přihlašovací údaje tooverify hello, použijte příkaz nabídky hello nastavení zdroje dat v hello editoru dotazů. Opatrní toospecify hello pověření, které používáte pro Azure, což může být liší od přihlašovacích údajů pro Power BI.)
2. Zvolte vizualizaci dotazu a vyberte hello pole osy x, y a segmentace dimenze.
   
    ![Vyberte vizualizace](./media/app-insights-export-power-bi/power-bi-analytics-visualize.png)
3. Publikujte pracovní prostor sestavy tooyour Power BI cloudu. Odtud můžete synchronizované verze vložit do jiných webových stránek.
   
    ![Vyberte vizualizace](./media/app-insights-export-power-bi/publish-power-bi.png)
4. Ručně aktualizujte hello sestavy v intervalech, nebo nastavení plánované aktualizace na stránce Možnosti hello.

## <a name="troubleshooting"></a>Řešení potíží

### <a name="401-or-403-unauthorized"></a>401 nebo 403 Neautorizováno 
To může dojít, pokud zatím není aktualizovaná refesh tokenu. Opakujte tyto kroky tooensure máte pořád přístup. Pokud máte přístup a přihlašovací údaje hello refershing nefunguje, otevřete prosím lístek podpory.

1. Přihlaste se k portálu Azure hello a ujistěte se, že dostanete hello prostředků
2. Zkuste toorefresh hello přihlašovací údaje pro hello řídicí panel

### <a name="502-bad-gateway"></a>502 Chybná brána
To je obvykle způsobeno dotazu analýzy, který vrátí příliš mnoho dat. Měli zkuste použít menší časový rozsah, nebo pomocí hello [před](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#ago) nebo [startofweek/startofmonth](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#startofweek) funguje pouze [projektu](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics-reference#project-operator) hello polí je nutné.

Pokud snižuje datovou sadu hello pocházejících z dotazu hello Analytics není splňují vaše požadavky měli byste zvážit použití hello [rozhraní API](https://dev.applicationinsights.io/documentation/overview) toopull větší datové sady. Tady jsou pokyny, jak exportovat tooconvert hello M-Query toouse hello rozhraní API.

1. Vytvoření [klíč rozhraní API](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID)
2. Aktualizace hello Power BI M skript, který jste exportovali z Analytics nahrazením hello ARM adresu URL s rozhraním API AI (viz následující příklad)
   * Nahraďte **https://management.azure.com/subscriptions/...**
   * s **https://api.applicationinsights.io/beta/apps/...**
3. Nakonec aktualizujte toobasic přihlašovacích údajů a používání vašeho klíče rozhraní API
  

**Existující skriptu**
 ```
 Source = Json.Document(Web.Contents("https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups//providers/microsoft.insights/components//api/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```
**Aktualizované skriptu**
 ```
 Source = Json.Document(Web.Contents("https://api.applicationinsights.io/beta/apps/<APPLICATION_ID>/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```

## <a name="about-sampling"></a>O vzorkování
Pokud vaše aplikace odešle velké množství dat, může funkce adaptivního vzorkování hello pracovat a odesílat pouze procento vaší telemetrie. Dobrý den, je-li hodnotu true, pokud jste ručně nastavili vzorkování v hello SDK nebo na přijímání stejné. [Přečtěte si další informace o vzorkování.](app-insights-sampling.md)


## <a name="next-steps"></a>Další kroky
* [Power BI - Další informace](http://www.powerbi.com/learning/)
* [Analýza kurzu](app-insights-analytics-tour.md)

