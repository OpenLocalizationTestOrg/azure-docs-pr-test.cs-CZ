---
title: "aaaTroubleshoot cloudové služby pomocí Application Insights | Microsoft Docs"
description: "Zjistěte, jak cloudové služby tootroubleshoot problémy pomocí Application Insights tooprocess dat z Azure Diagnostics."
services: cloud-services
documentationcenter: .net
author: sbtron
manager: timlt
editor: tysonn
ms.assetid: e93f387b-ef29-4731-ae41-0676722accb6
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2017
ms.author: saurabh
ms.openlocfilehash: 972924d9e6d1fe33d5c19b006d482de52ffb0ef7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-cloud-services-using-application-insights"></a>Řešení potíží s Cloud Services pomocí Application Insights
S [Azure SDK 2.8](https://azure.microsoft.com/downloads/) a rozšíření Azure diagnostiky 1.5, může odesílat data diagnostiky Azure pro cloudové služby přímo tooApplication statistiky. Hello protokoly se shromažďují Azure Diagnostics&mdash;včetně protokoly aplikací, protokoly událostí systému Windows, protokoly trasování událostí pro Windows a čítače výkonu&mdash;lze odeslat tooApplication statistiky. Potom můžete vizualizovat tyto informace portálu Application Insights hello uživatelského rozhraní. Potom můžete hello Application Insights SDK tooget přehled metriky a protokoly, které pocházejí z vaší aplikace, a také hello systému a data na úrovni infrastruktury, který přichází z Azure Diagnostics.

## <a name="configure-azure-diagnostics-toosend-data-tooapplication-insights"></a>Konfigurace Azure Diagnostics toosend data tooApplication statistiky
Postupujte podle těchto kroků tooset až tooApplication Azure Diagnostics data vaše cloudové služby projektu toosend statistiky.

1. V Průzkumníku řešení Visual Studio, klikněte pravým tlačítkem na roli a vyberte **vlastnosti** tooopen hello Role designer.

    ![Vlastnosti rolí Průzkumník řešení][1]

2. V hello **diagnostiky** části hello Role Návrhář, vyberte hello **odeslání diagnostiky dat tooApplication Insights** možnost.

    ![Návrhář role odeslání diagnostiky dat tooapplication statistiky][2]

3. Hello dialogové okno pole, která se objeví vyberte prostředek Application Insights hello, který chcete toosend hello data Azure diagnostics. Dialogové okno Hello vám umožní tooselect existující prostředek Application Insights ze svého předplatného, nebo toomanually zadejte kód instrumentace pro prostředek Application Insights. Další informace o vytváření prostředek Application Insights najdete v tématu [vytvořte nový prostředek Application Insights](../application-insights/app-insights-create-new-resource.md).

    ![Vyberte prostředek application insights][3]

    Po přidání prostředku Application Insights hello hello klíč instrumentace pro tento prostředek je uloženo jako nastavení konfigurace služby s názvem hello **APPINSIGHTS_INSTRUMENTATIONKEY**. Můžete změnit tato nastavení konfigurace pro každou konfiguraci služby nebo prostředí. toodo Ano, vyberte jinou konfiguraci z hello **konfigurace služby** seznamu a zadejte nový klíč instrumentace pro danou konfiguraci.

    ![Vyberte konfiguraci služby][4]

    Hello **APPINSIGHTS_INSTRUMENTATIONKEY** rozšíření diagnostiky hello tooconfigure sady Visual Studio s hello odpovídající Application Insights informací o prostředku během publikování se používá nastavení konfigurace. nastavení konfigurace Hello je pohodlný způsob definování různých instrumentace klíčů pro různé služby konfigurace. Visual Studio se převede tohoto nastavení a vložte ji do veřejné konfigurace rozšíření hello diagnostiky během hello publikování procesu. toosimplify hello proces konfigurace rozšíření diagnostiky hello v prostředí PowerShell, výstup hello balíčku ze sady Visual Studio také obsahuje hello veřejné konfigurace XML se hello příslušný klíč instrumentace Application Insights. Hello veřejné konfigurační soubory jsou vytvořeny ve složce rozšíření hello a postupujte podle vzoru hello *PaaSDiagnostics.&lt; RoleName&gt;. PubConfig.xml*. Všechna nasazení pomocí prostředí PowerShell můžete použít tento vzor toomap každou roli tooa konfigurace.

4) toosend Azure diagnostics tooconfigure všechny protokoly úroveň chyb a čítače výkonu shromážděné hello Azure diagnostics agenta tooApplication statistiky, povolit hello **odeslání diagnostiky dat tooApplication Insights** možnost. 

    Pokud chcete toofurther konfigurace, jaká data se odesílají tooApplication statistiky, je nutné ručně upravit hello *diagnostics.wadcfgx* soubor pro každou roli. V tématu [konfigurace Azure Diagnostics toosend data tooApplication Insights](#configure-azure-diagnostics-to-send-data-to-application-insights) toolearn více o ruční aktualizaci konfigurace hello.

Při hello Cloudová služba je nakonfigurované toosend Azure diagnostics data tooapplication statistiky, můžete nasadit tooAzure normálně, ujistěte se, že je povolená hello rozšíření Azure diagnostics. Další informace najdete v tématu [publikování cloudové služby pomocí sady Visual Studio](../vs-azure-tools-publishing-a-cloud-service.md).  

## <a name="viewing-azure-diagnostics-data-in-application-insights"></a>Zobrazení dat Azure diagnostics ve službě Application Insights
Hello Azure diagnostická telemetrie se zobrazí v hello prostředek nakonfigurovaný pro cloudové služby Application Insights.

Typy protokolu Azure diagnostics mapování tooApplication Statistika koncepty těmito způsoby:

* Čítače výkonu zobrazují jako vlastní metriky ve službě Application Insights.
* Protokoly událostí systému Windows se zobrazí jako trasování a vlastní události ve službě Application Insights.
* Aplikace protokoly, protokoly trasování událostí pro Windows a žádné infrastruktury diagnostické protokoly jsou zobrazeny jako trasování ve službě Application Insights.

tooview Azure diagnostická data ve službě Application Insights, proveďte jednu z následujících hello:

* Použití [Průzkumníku metrik](../application-insights/app-insights-metrics-explorer.md) toovisualize všechny vlastní výkonu čítače nebo počty různých typů událostí protokolu událostí systému Windows.

    ![Vlastní metriky v Průzkumníku metrik][5]

* Použití [vyhledávání](../application-insights/app-insights-diagnostic-search.md) toosearch přes protokoly trasování hello poslal Azure Diagnostics. Například pokud nezpracovanou výjimku způsobila hello role toocrash a recyklace, informace o výjimkách hello se zobrazí v hello *aplikace* kanál *protokolu událostí systému Windows*. Můžete použít toolook vyhledávání v hello Chyba protokolu událostí systému Windows a získat trasování hello úplné zásobníku pro hello výjimky toohelp najít hello příčina problému hello.

    ![Hledání trasování][6]

## <a name="next-steps"></a>Další kroky
* [Přidat hello Application Insights SDK tooyour Cloudová služba](../application-insights/app-insights-cloudservices.md) toosend data o požadavky, výjimky, závislosti a jakékoli vlastní telemetrii z vaší aplikace. V kombinaci s daty Azure Diagnostics hello, tyto informace, které lze získat úplné zobrazení vaší aplikace a systému, ve hello stejného zdroje Přehled aplikace.  

<!--Image references-->
[1]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/solution-explorer-properties.png
[2]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-sendtoappinsights.png
[3]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/select-appinsights-resource.png
[4]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-appinsights-serviceconfig.png
[5]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/metrics-explorer-custom-metrics.png
[6]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/search-windowseventlog-error.png
