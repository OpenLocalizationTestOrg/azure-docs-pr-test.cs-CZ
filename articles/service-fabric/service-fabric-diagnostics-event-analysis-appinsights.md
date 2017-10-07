---
title: "aaaAzure analýza události služby prostředků infrastruktury pomocí služby Application Insights | Microsoft Docs"
description: "Další informace o vizualizaci a analýzu událostí pomocí Application Insights pro monitorování a Diagnostika Azure Service Fabric clusterů."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/26/2017
ms.author: dekapur
ms.openlocfilehash: 59bb5a409f2951e5b2034049e782dd0da67f933c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="event-analysis-and-visualization-with-application-insights"></a>Analýza události a vizualizace s Application Insights

Azure Application Insights je rozšiřitelná platforma pro monitorování aplikací a diagnostiku. Zahrnuje výkonné analýzy a zadávat dotazy na nástroj, přizpůsobitelné řídicích panelů a vizualizací a další možnosti, včetně automatizované výstrahy. Je doporučeno platforma pro monitorování a Diagnostika pro Service Fabric aplikace a služby hello.

## <a name="setting-up-application-insights"></a>Nastavení služby Application Insights

### <a name="creating-an-ai-resource"></a>Vytváření prostředek AI

toocreate AI prostředek, head přes toohello Azure Marketplace a vyhledejte "Application Insights". Měl by se zobrazit jako první řešení hello (je v kategorii "Web + mobilní"). Klikněte na tlačítko **vytvořit** při hledání v pravém prostředků hello (potvrďte, že cestu k odpovídá hello obrázek níže).

![Nový prostředek Application Insights](media/service-fabric-diagnostics-event-analysis-appinsights/create-new-ai-resource.png)

Je nutné toofill se některé informace tooprovision hello prostředků správně. V hello *typ aplikace* pole, použijte "Webové aplikace ASP.NET" Pokud budete používat některé z Service Fabric je programovací modely nebo publikování clusteru toohello aplikace .NET. Pokud budete nasazovat hosta spustitelných souborů a kontejnery, použijte "Obecné". Obecně platí tookeep "Webové aplikace ASP.NET" toousing výchozí možnosti otevřete v budoucí hello. je název Hello až tooyour předvoleb a skupině prostředků hello i předplatného jsou změnit po nasazení hello prostředku. Doporučujeme, aby vaše AI prostředek je ve hello stejné skupině prostředků jako cluster. Pokud potřebujete další informace, najdete v tématu [vytvořte prostředek Application Insights](../application-insights/app-insights-create-new-resource.md)

Je nutné hello klíč instrumentace AI tooconfigure AI nástrojem agregace událostí. AI prostředku je nastaveno (trvá několik minut po ověření nasazení hello), přejděte tooit a najde hello **vlastnosti** části na levém navigačním panelu hello. Nové okno se otevře zobrazující *klíč INSTRUMENTACE*. Pokud potřebujete toochange hello předplatné nebo skupinu prostředků hello prostředku, je možné ji provést zde také.

### <a name="configuring-ai-with-wad"></a>Konfigurace s WAD AI

Existují dva způsoby primární toosend data z WAD tooAzure AI, které se dosáhne přidáním AI podřízený toohello WAD konfigurace aplikace, podle popisu v [v tomto článku](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).

#### <a name="add-an-ai-instrumentation-key-when-creating-a-cluster-in-azure-portal"></a>Při vytváření clusteru na portálu Azure přidejte kód instrumentace AI

![Přidání AIKey](media/service-fabric-diagnostics-event-analysis-appinsights/azure-enable-diagnostics.png)

Při vytváření clusteru, pokud je vypnuté Diagnostika "Na", tooenter volitelné pole klíč instrumentace Application Insights zobrazí. Pokud vaše AI IKey sem vkládáte, podřízený hello AI bude automaticky nakonfigurován pro vás v šabloně hello Resource Manager, který je použité toodeploy vašeho clusteru.

#### <a name="add-hello-ai-sink-toohello-resource-manager-template"></a>Přidat hello šablony Resource Manageru toohello podřízený AI

V hello "WadCfg" hello šablony Resource Manageru přidejte "Podřízený" zahrnutím hello následující dvě změny:

1. Přidáte konfiguraci podřízený hello:

    ```json
    "SinksConfig": {
        "Sink": [
            {
                "name": "applicationInsights",
                "ApplicationInsights": "***ADD INSTRUMENTATION KEY HERE***"
            }
        ]
    }

    ```

2. Zahrnout hello podřízený hello DiagnosticMonitorConfiguration podle přidání hello následující řádek v "DiagnosticMonitorConfiguration" hello "WadCfg":

    ```json
    "sinks": "applicationInsights"
    ```

V obou fragmentů kódu hello výše hello název "applicationInsights" byl použité toodescribe hello jímky. Toto není požadavek a tak dlouho, dokud hello název podřízený hello je součástí "jímky", můžete nastavit hello název tooany řetězec.

V současné době se protokoly z clusteru hello zobrazí jako trasování v prohlížeči na AI protokolu. Vzhledem k tomu, že většina hello trasování pocházejících z platformy hello je úrovně "Informační", byste také zvážit změna hello podřízený konfigurace tooonly odeslat protokoly typu "Kritický" nebo "Chyba". Stačí přidáním podřízený tooyour "Kanály", jak je předvedeno v [v tomto článku](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).

>[!NOTE]
>Pokud používáte nesprávné IKey AI portálu nebo v šabloně Resource Manager, budete mít toomanually, změňte klíč hello a aktualizovat hello cluster / zavést jej znovu. 

### <a name="configuring-ai-with-eventflow"></a>Konfigurace s EventFlow AI

Pokud používáte EventFlow tooaggregate událostí, ujistěte se, zda text hello tooimport `Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights`balíček NuGet. Hello následující má toobe součástí hello *výstupy* části hello *eventFlowConfig.json*:

```json
"outputs": [
    {
        "type": "ApplicationInsights",
        // (replace hello following value with your AI resource's instrumentation key)
        "instrumentationKey": "00000000-0000-0000-0000-000000000000"
    }
]
```

Zda toomake hello požadované změny proveďte v filtry a také zahrnovat všechny ostatní vstupy (spolu s jejich odpovídajících balíčky NuGet).

## <a name="aisdk"></a>AI. SADA SDK

Obecně je doporučeno toouse EventFlow a WAD jako řešení agregace, protože umožňují pro více modulární toodiagnostics přístup a monitorování, tj. Pokud chcete toochange vaše výstupy z EventFlow, vyžaduje žádný skutečný tooyour změn instrumentace, právě soubor jednoduchých úprav tooyour konfigurace. Pokud však rozhodnout tooinvest pomocí Application Insights a nejsou pravděpodobně toochange tooa různé platformy, by měla vypadat do aplikace pomocí AI na novou sadu SDK pro agregaci událostí a jejich odesláním tooAI. To znamená, že nebude mít tooconfigure EventFlow toosend tooAI vaše data, ale místo toho nainstaluje balíček Service Fabric NuGet hello ApplicationInsight. Naleznete informace o balíčku hello [zde](https://github.com/Microsoft/ApplicationInsights-ServiceFabric).

[Podpora služby Application Insights pro Mikroslužeb a kontejnery](https://azure.microsoft.com/app-insights-microservices/) uvedeny některé hello nových funkcí, které se pracuje (aktuálně stále v beta), které umožňují toohave bohatší se na poli možnosti monitorování s AI. Jedná se o závislost sledování (používá se při vytváření AppMap služeb a aplikací v clusteru a hello komunikace mezi nimi) a lepší korelace trasování pocházejících z vašich služeb (pomáhá v lepší přesným rozpoznáním problém v hello pracovní postup aplikace nebo služby).

Pokud vývoji v rozhraní .NET a bude pravděpodobně možné pomocí některé z programovací modely Service Fabric a jsou ochotni toouse AI jako svou platformu pro vizualizaci a analýzu dat událostí a protokolů, pak doporučujeme přejít přes hello AI SDK trasy jako monitorování a Diagnostika pracovního postupu. Čtení [to](../application-insights/app-insights-asp-net-more.md) a [to](../application-insights/app-insights-asp-net-trace-logs.md) tooget začít s pomocí AI toocollect a zobrazit protokoly.

## <a name="navigating-hello-ai-resource-in-azure-portal"></a>Navigace hello AI prostředků na portálu Azure

Po nakonfigurování AI jako výstupu událostí a protokoly informace by měly spuštění tooshow v prostředku AI za pár minut. Přejděte toohello AI prostředku, který bude trvat jste toohello AI prostředků řídicího panelu. Klikněte na tlačítko **vyhledávání** v hello AI panelu toosee hello nejnovější trasování které byl přijat a mít toofilter toobe prostřednictvím je.

*Průzkumníku metrik* je užitečným nástrojem pro vytvoření vlastní řídicí panely podle metriky, které vaše aplikace, služby a cluster může být vytváření sestav. V tématu [zkoumat metriky ve službě Application Insights](../application-insights/app-insights-metrics-explorer.md) tooset až několik grafy pro sami na základě hello dat shromažďujete.

Kliknutím na tlačítko **Analytics** přejdete toohello Application Insights Analytics portál, kde může dotazovat událostí a trasování s větší rozsah a volitelnost. Další informace o to v [Analytics ve službě Application Insights](../application-insights/app-insights-analytics.md).

## <a name="next-steps"></a>Další kroky

* [Nastavení výstrah v AI](../application-insights/app-insights-alerts.md) toobe oznámení o změnách v výkonu a využití
* [Inteligentní detekce ve službě Application Insights](../application-insights/app-insights-proactive-diagnostics.md) provede proaktivní analýzu hello telemetrie odesílány tooAI toowarn o potenciálním problémům s výkonem
