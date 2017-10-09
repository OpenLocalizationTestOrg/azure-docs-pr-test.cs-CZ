---
title: aaaMicrosoft Dynamics CRM, Azure Application Insights | Microsoft Docs
description: "Získáte telemetrická data z Microsoft Dynamics CRM Online pomocí Application Insights. Postup instalace, získávání dat, vizualizace a export."
services: application-insights
documentationcenter: 
author: mazharmicrosoft
manager: carmonm
ms.assetid: 04c66338-687e-49e5-9975-be935f98f156
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/16/2017
ms.author: bwren
ms.openlocfilehash: a39398060d6553fb18a26c101f085b7d87443636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>Návod: Povolení Telemetrie pro aplikaci Microsoft Dynamics CRM Online pomocí Application Insights
Tento článek ukazuje, jak tooget telemetrická data z [Microsoft Dynamics CRM Online](https://www.dynamics.com/) pomocí [Azure Application Insights](https://azure.microsoft.com/services/application-insights/). Projdeme hello dokončení procesu přidání Application Insights skriptu tooyour aplikace, zaznamenávání dat a vizualizace dat sady.

> [!NOTE]
> [Procházet hello ukázkové řešení](https://dynamicsandappinsights.codeplex.com/).
> 
> 

## <a name="add-application-insights-toonew-or-existing-crm-online-instance"></a>Přidejte Application Insights toonew nebo existující instanci CRM Online
toomonitor vaší aplikace, přidání aplikace tooyour Application Insights SDK. Hello SDK odesílá telemetrii toohello [portál Application Insights](https://portal.azure.com), kde můžete používat naše efektivní analýzu a diagnostické nástroje, nebo exportovat hello data toostorage.

### <a name="create-an-application-insights-resource-in-azure"></a>Vytvořte prostředek Application Insights v Azure
1. Získat [účet ve službě Microsoft Azure](http://azure.com/pricing). 
2. Přihlaste se k hello [portál Azure](https://portal.azure.com) a přidat nový prostředek Application Insights. Toto je, kde bude zpracována a zobrazí data.
   
    ![Klikněte na tlačítko +, služby pro vývojáře, Application Insights.](./media/app-insights-sample-mscrm/01.png)
   
    Vyberte jako typ aplikace hello ASP.NET.
3. Otevřete stránku Začínáme hello a otevřete "monitorování a diagnostikovat na straně klienta".
   
    ![Fragment kódu pro vložení do webové stránky](./media/app-insights-sample-mscrm/03.png)

**Nechat otevřený hello znaková stránka** při hello další krok v jiném okně prohlížeče. Budete potřebovat kód hello brzy k dispozici. 

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>Vytvořte prostředek JavaScript webové aplikace Microsoft Dynamics CRM
1. Otevřete CRM Online instance a přihlaste se s oprávněními správce.
2. Otevřete Microsoft Dynamics CRM nastavení, přizpůsobení, přizpůsobit hello systému
   
    ![Nastavení aplikace Microsoft Dynamics CRM](./media/app-insights-sample-mscrm/04.png)
   
    ![Nastavení > Přizpůsobení](./media/app-insights-sample-mscrm/05.png)

    ![Přizpůsobení možnost hello systému](./media/app-insights-sample-mscrm/06.png)

1. Vytvořte prostředek JavaScript.
   
    ![Dialogové okno Nový webový prostředek](./media/app-insights-sample-mscrm/07.png)
   
    Zadejte jeho název, vyberte **skriptu (JScript)** a textovém editoru otevřete hello.
   
    ![Otevřete hello textového editoru](./media/app-insights-sample-mscrm/08.png)
2. Zkopírujte kód hello z Application Insights. Při kopírování Ujistěte se, že tooignore značek skriptu. Naleznete níže – snímek obrazovky:
   
    ![Nastavte klíč instrumentace](./media/app-insights-sample-mscrm/09.png)
   
    Hello kód obsahuje klíč instrumentace hello, který identifikuje prostředek vaší aplikace statistiky.
3. Uložte a publikovat.
   
    ![Uložte a publikování](./media/app-insights-sample-mscrm/10.png)

### <a name="instrument-forms"></a>Nástrojích formulářů
1. V aplikaci Microsoft CRM Online otevřete formulář účet hello
   
    ![Účet formuláře](./media/app-insights-sample-mscrm/11.png)
2. Otevřít formulář hello vlastnosti
   
    ![Vlastnosti formuláře](./media/app-insights-sample-mscrm/12.png)
3. Přidat hello JavaScript webové prostředky, který jste vytvořili
   
    ![Nabídka Přidat](./media/app-insights-sample-mscrm/13.png)
   
    ![Přidat prostředek webové hello](./media/app-insights-sample-mscrm/14.png)
4. Uložte a publikujte vlastní nastavení formuláře.

## <a name="metrics-captured"></a>Metriky zachycení
Nyní jste nastavili zachycování telemetrie pro formulář hello. Vždy, když se používají data odešlou tooyour prostředek Application Insights.

Tady jsou ukázky hello data, která se zobrazí.

#### <a name="application-health"></a>Stav aplikace
![Příklad čas načítání stránky](./media/app-insights-sample-mscrm/15.png)

![Příklad stránky zobrazení grafu](./media/app-insights-sample-mscrm/16.png)

Výjimky prohlížečů:

![Grafu výjimek prohlížeče](./media/app-insights-sample-mscrm/17.png)

Klikněte na graf tooget hello podrobněji:

![Seznamu výjimek](./media/app-insights-sample-mscrm/18.png)

#### <a name="usage"></a>Využití
![Uživatelů, relací a zobrazení stránek](./media/app-insights-sample-mscrm/19.png)

![Sesion grafy](./media/app-insights-sample-mscrm/20.png)

![Verze prohlížeče](./media/app-insights-sample-mscrm/21.png)

#### <a name="browsers"></a>Prohlížeče
![Rozdělení čas načítání stránky](./media/app-insights-sample-mscrm/22.png)

![Počet relací podle verze prohlížeče](./media/app-insights-sample-mscrm/23.png)

#### <a name="geolocation"></a>Informace o zeměpisné poloze
![Počet relací podle země](./media/app-insights-sample-mscrm/24.png)

![Relace a uživatelů podle země](./media/app-insights-sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>Požadavek na uvnitř stránku zobrazení
![Stránka zobrazení souhrnu](./media/app-insights-sample-mscrm/26.png)

![Vyhledávání na stránky zobrazení událostí](./media/app-insights-sample-mscrm/27.png)

![Podobně jako zobrazení stránky](./media/app-insights-sample-mscrm/28.png)

![Zobrazení vlastností stránky](./media/app-insights-sample-mscrm/29.png)

![Stránky na relaci](./media/app-insights-sample-mscrm/30.png)

## <a name="sample-code"></a>Ukázka kódu
[Procházet hello ukázkový kód](https://dynamicsandappinsights.codeplex.com/).

## <a name="power-bi"></a>Power BI
Můžete provést i hlubší analysis, pokud jste [exportovat hello data tooMicrosoft Power BI](app-insights-export-power-bi.md).

## <a name="sample-microsoft-dynamics-crm-solution"></a>Ukázkové aplikace Microsoft Dynamics CRM řešení
[Tady je implementována v aplikaci Microsoft Dynamics CRM hello ukázkové řešení](https://dynamicsandappinsights.codeplex.com/).

## <a name="learn-more"></a>Další informace
* [Co je Application Insights?](app-insights-overview.md)
* [Application Insights pro webové stránky](app-insights-javascript.md)
* [Další ukázky a návody](app-insights-code-samples.md)
