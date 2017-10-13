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
# <a name="event-analysis-and-visualization-with-application-insights"></a><span data-ttu-id="0d6c6-103">Analýza události a vizualizace s Application Insights</span><span class="sxs-lookup"><span data-stu-id="0d6c6-103">Event analysis and visualization with Application Insights</span></span>

<span data-ttu-id="0d6c6-104">Azure Application Insights je rozšiřitelná platforma pro monitorování aplikací a diagnostiku.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-104">Azure Application Insights is an extensible platform for application monitoring and diagnostics.</span></span> <span data-ttu-id="0d6c6-105">Zahrnuje výkonné analýzy a zadávat dotazy na nástroj, přizpůsobitelné řídicích panelů a vizualizací a další možnosti, včetně automatizované výstrahy.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-105">It includes a powerful analytics and querying tool, customizable dashboard and visualizations, and further options including automated alerting.</span></span> <span data-ttu-id="0d6c6-106">Je doporučeno platforma pro monitorování a Diagnostika pro Service Fabric aplikace a služby hello.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-106">It is hello recommended platform for monitoring and diagnostics for Service Fabric applications and services.</span></span>

## <a name="setting-up-application-insights"></a><span data-ttu-id="0d6c6-107">Nastavení služby Application Insights</span><span class="sxs-lookup"><span data-stu-id="0d6c6-107">Setting up Application Insights</span></span>

### <a name="creating-an-ai-resource"></a><span data-ttu-id="0d6c6-108">Vytváření prostředek AI</span><span class="sxs-lookup"><span data-stu-id="0d6c6-108">Creating an AI Resource</span></span>

<span data-ttu-id="0d6c6-109">toocreate AI prostředek, head přes toohello Azure Marketplace a vyhledejte "Application Insights".</span><span class="sxs-lookup"><span data-stu-id="0d6c6-109">toocreate an AI resource, head over toohello Azure Marketplace, and search for "Application Insights".</span></span> <span data-ttu-id="0d6c6-110">Měl by se zobrazit jako první řešení hello (je v kategorii "Web + mobilní").</span><span class="sxs-lookup"><span data-stu-id="0d6c6-110">It should show up as hello first solution (it is under category "Web + Mobile").</span></span> <span data-ttu-id="0d6c6-111">Klikněte na tlačítko **vytvořit** při hledání v pravém prostředků hello (potvrďte, že cestu k odpovídá hello obrázek níže).</span><span class="sxs-lookup"><span data-stu-id="0d6c6-111">Click **Create** when you are looking at hello right resource (confirm that your path matches hello image below).</span></span>

![Nový prostředek Application Insights](media/service-fabric-diagnostics-event-analysis-appinsights/create-new-ai-resource.png)

<span data-ttu-id="0d6c6-113">Je nutné toofill se některé informace tooprovision hello prostředků správně.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-113">You will need toofill out some information tooprovision hello resource correctly.</span></span> <span data-ttu-id="0d6c6-114">V hello *typ aplikace* pole, použijte "Webové aplikace ASP.NET" Pokud budete používat některé z Service Fabric je programovací modely nebo publikování clusteru toohello aplikace .NET.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-114">In hello *Application Type* field, use "ASP.NET web application" if you will be using any of Service Fabric's programming models or publishing a .NET application toohello cluster.</span></span> <span data-ttu-id="0d6c6-115">Pokud budete nasazovat hosta spustitelných souborů a kontejnery, použijte "Obecné".</span><span class="sxs-lookup"><span data-stu-id="0d6c6-115">Use "General" if you will be deploying guest executables and containers.</span></span> <span data-ttu-id="0d6c6-116">Obecně platí tookeep "Webové aplikace ASP.NET" toousing výchozí možnosti otevřete v budoucí hello.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-116">In general, default toousing "ASP.NET web application" tookeep your options open in hello future.</span></span> <span data-ttu-id="0d6c6-117">je název Hello až tooyour předvoleb a skupině prostředků hello i předplatného jsou změnit po nasazení hello prostředku.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-117">hello name is up tooyour preference, and both hello resource group and subscription are changeable post-deployment of hello resource.</span></span> <span data-ttu-id="0d6c6-118">Doporučujeme, aby vaše AI prostředek je ve hello stejné skupině prostředků jako cluster.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-118">We recommend that your AI resource is in hello same resource group as your cluster.</span></span> <span data-ttu-id="0d6c6-119">Pokud potřebujete další informace, najdete v tématu [vytvořte prostředek Application Insights](../application-insights/app-insights-create-new-resource.md)</span><span class="sxs-lookup"><span data-stu-id="0d6c6-119">If you need more information, please see [Create an Application Insights resource](../application-insights/app-insights-create-new-resource.md)</span></span>

<span data-ttu-id="0d6c6-120">Je nutné hello klíč instrumentace AI tooconfigure AI nástrojem agregace událostí.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-120">You need hello AI Instrumentation Key tooconfigure AI with your event aggregation tool.</span></span> <span data-ttu-id="0d6c6-121">AI prostředku je nastaveno (trvá několik minut po ověření nasazení hello), přejděte tooit a najde hello **vlastnosti** části na levém navigačním panelu hello.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-121">Once your AI resource is set up (takes a few minutes after hello deployment is validated), navigate tooit and find hello **Properties** section on hello left navigation bar.</span></span> <span data-ttu-id="0d6c6-122">Nové okno se otevře zobrazující *klíč INSTRUMENTACE*.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-122">A new blade will open up that shows an *INSTRUMENTATION KEY*.</span></span> <span data-ttu-id="0d6c6-123">Pokud potřebujete toochange hello předplatné nebo skupinu prostředků hello prostředku, je možné ji provést zde také.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-123">If you need toochange hello subscription or resource group of hello resource, it can be done here as well.</span></span>

### <a name="configuring-ai-with-wad"></a><span data-ttu-id="0d6c6-124">Konfigurace s WAD AI</span><span class="sxs-lookup"><span data-stu-id="0d6c6-124">Configuring AI with WAD</span></span>

<span data-ttu-id="0d6c6-125">Existují dva způsoby primární toosend data z WAD tooAzure AI, které se dosáhne přidáním AI podřízený toohello WAD konfigurace aplikace, podle popisu v [v tomto článku](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).</span><span class="sxs-lookup"><span data-stu-id="0d6c6-125">There are two primary ways toosend data from WAD tooAzure AI, which is achieved by adding an AI sink toohello WAD configuration, as detailed in [this article](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).</span></span>

#### <a name="add-an-ai-instrumentation-key-when-creating-a-cluster-in-azure-portal"></a><span data-ttu-id="0d6c6-126">Při vytváření clusteru na portálu Azure přidejte kód instrumentace AI</span><span class="sxs-lookup"><span data-stu-id="0d6c6-126">Add an AI Instrumentation Key when creating a cluster in Azure portal</span></span>

![Přidání AIKey](media/service-fabric-diagnostics-event-analysis-appinsights/azure-enable-diagnostics.png)

<span data-ttu-id="0d6c6-128">Při vytváření clusteru, pokud je vypnuté Diagnostika "Na", tooenter volitelné pole klíč instrumentace Application Insights zobrazí.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-128">When creating a cluster, if Diagnostics is turned "On", an optional field tooenter an Application Insights Instrumentation key will show.</span></span> <span data-ttu-id="0d6c6-129">Pokud vaše AI IKey sem vkládáte, podřízený hello AI bude automaticky nakonfigurován pro vás v šabloně hello Resource Manager, který je použité toodeploy vašeho clusteru.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-129">If you paste your AI IKey here, hello AI sink will be automatically configured for you in hello Resource Manager template that is used toodeploy your cluster.</span></span>

#### <a name="add-hello-ai-sink-toohello-resource-manager-template"></a><span data-ttu-id="0d6c6-130">Přidat hello šablony Resource Manageru toohello podřízený AI</span><span class="sxs-lookup"><span data-stu-id="0d6c6-130">Add hello AI Sink toohello Resource Manager template</span></span>

<span data-ttu-id="0d6c6-131">V hello "WadCfg" hello šablony Resource Manageru přidejte "Podřízený" zahrnutím hello následující dvě změny:</span><span class="sxs-lookup"><span data-stu-id="0d6c6-131">In hello "WadCfg" of hello Resource Manager template, add a "Sink" by including hello following two changes:</span></span>

1. <span data-ttu-id="0d6c6-132">Přidáte konfiguraci podřízený hello:</span><span class="sxs-lookup"><span data-stu-id="0d6c6-132">Add hello sink configuration:</span></span>

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

2. <span data-ttu-id="0d6c6-133">Zahrnout hello podřízený hello DiagnosticMonitorConfiguration podle přidání hello následující řádek v "DiagnosticMonitorConfiguration" hello "WadCfg":</span><span class="sxs-lookup"><span data-stu-id="0d6c6-133">Include hello Sink in hello DiagnosticMonitorConfiguration by adding hello following line in "DiagnosticMonitorConfiguration" of hello "WadCfg":</span></span>

    ```json
    "sinks": "applicationInsights"
    ```

<span data-ttu-id="0d6c6-134">V obou fragmentů kódu hello výše hello název "applicationInsights" byl použité toodescribe hello jímky.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-134">In both hello code snippets above, hello name "applicationInsights" was used toodescribe hello sink.</span></span> <span data-ttu-id="0d6c6-135">Toto není požadavek a tak dlouho, dokud hello název podřízený hello je součástí "jímky", můžete nastavit hello název tooany řetězec.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-135">This is not a requirement and as long as hello name of hello sink is included in "sinks", you can set hello name tooany string.</span></span>

<span data-ttu-id="0d6c6-136">V současné době se protokoly z clusteru hello zobrazí jako trasování v prohlížeči na AI protokolu.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-136">Currently, logs from hello cluster will show up as traces in AI's log viewer.</span></span> <span data-ttu-id="0d6c6-137">Vzhledem k tomu, že většina hello trasování pocházejících z platformy hello je úrovně "Informační", byste také zvážit změna hello podřízený konfigurace tooonly odeslat protokoly typu "Kritický" nebo "Chyba".</span><span class="sxs-lookup"><span data-stu-id="0d6c6-137">Since most of hello traces coming from hello platform are of level "Informational", you can also consider changing hello sink configuration tooonly send logs of type "Critical" or "Error".</span></span> <span data-ttu-id="0d6c6-138">Stačí přidáním podřízený tooyour "Kanály", jak je předvedeno v [v tomto článku](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).</span><span class="sxs-lookup"><span data-stu-id="0d6c6-138">This can be done by adding "Channels" tooyour sink, as demonstrated in [this article](../monitoring-and-diagnostics/azure-diagnostics-configure-application-insights.md).</span></span>

>[!NOTE]
><span data-ttu-id="0d6c6-139">Pokud používáte nesprávné IKey AI portálu nebo v šabloně Resource Manager, budete mít toomanually, změňte klíč hello a aktualizovat hello cluster / zavést jej znovu.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-139">If you use an incorrect AI IKey either in portal or in your Resource Manager template, you will have toomanually change hello key and update hello cluster / redeploy it.</span></span> 

### <a name="configuring-ai-with-eventflow"></a><span data-ttu-id="0d6c6-140">Konfigurace s EventFlow AI</span><span class="sxs-lookup"><span data-stu-id="0d6c6-140">Configuring AI with EventFlow</span></span>

<span data-ttu-id="0d6c6-141">Pokud používáte EventFlow tooaggregate událostí, ujistěte se, zda text hello tooimport `Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights`balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-141">If you are using EventFlow tooaggregate events, make sure tooimport hello `Microsoft.Diagnostics.EventFlow.Output.ApplicationInsights`NuGet package.</span></span> <span data-ttu-id="0d6c6-142">Hello následující má toobe součástí hello *výstupy* části hello *eventFlowConfig.json*:</span><span class="sxs-lookup"><span data-stu-id="0d6c6-142">hello following has toobe included in hello *outputs* section of hello *eventFlowConfig.json*:</span></span>

```json
"outputs": [
    {
        "type": "ApplicationInsights",
        // (replace hello following value with your AI resource's instrumentation key)
        "instrumentationKey": "00000000-0000-0000-0000-000000000000"
    }
]
```

<span data-ttu-id="0d6c6-143">Zda toomake hello požadované změny proveďte v filtry a také zahrnovat všechny ostatní vstupy (spolu s jejich odpovídajících balíčky NuGet).</span><span class="sxs-lookup"><span data-stu-id="0d6c6-143">Make sure toomake hello required changes in your filters, as well as include any other inputs (along with their respective NuGet packages).</span></span>

## <a name="aisdk"></a><span data-ttu-id="0d6c6-144">AI. SADA SDK</span><span class="sxs-lookup"><span data-stu-id="0d6c6-144">AI.SDK</span></span>

<span data-ttu-id="0d6c6-145">Obecně je doporučeno toouse EventFlow a WAD jako řešení agregace, protože umožňují pro více modulární toodiagnostics přístup a monitorování, tj. Pokud chcete toochange vaše výstupy z EventFlow, vyžaduje žádný skutečný tooyour změn instrumentace, právě soubor jednoduchých úprav tooyour konfigurace.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-145">It is generally recommended toouse EventFlow and WAD as aggregation solutions, because they allow for a more modular approach toodiagnostics and monitoring, i.e. if you want toochange your outputs from EventFlow, it requires no change tooyour actual instrumentation, just a simple modification tooyour config file.</span></span> <span data-ttu-id="0d6c6-146">Pokud však rozhodnout tooinvest pomocí Application Insights a nejsou pravděpodobně toochange tooa různé platformy, by měla vypadat do aplikace pomocí AI na novou sadu SDK pro agregaci událostí a jejich odesláním tooAI.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-146">If, however, you decide tooinvest in using Application Insights and are not likely toochange tooa different platform, you should look into using AI's new SDK for aggregating events and sending them tooAI.</span></span> <span data-ttu-id="0d6c6-147">To znamená, že nebude mít tooconfigure EventFlow toosend tooAI vaše data, ale místo toho nainstaluje balíček Service Fabric NuGet hello ApplicationInsight.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-147">This means that you will no longer have tooconfigure EventFlow toosend your data tooAI, but instead will install hello ApplicationInsight's Service Fabric NuGet package.</span></span> <span data-ttu-id="0d6c6-148">Naleznete informace o balíčku hello [zde](https://github.com/Microsoft/ApplicationInsights-ServiceFabric).</span><span class="sxs-lookup"><span data-stu-id="0d6c6-148">Details on hello package can be found [here](https://github.com/Microsoft/ApplicationInsights-ServiceFabric).</span></span>

<span data-ttu-id="0d6c6-149">[Podpora služby Application Insights pro Mikroslužeb a kontejnery](https://azure.microsoft.com/app-insights-microservices/) uvedeny některé hello nových funkcí, které se pracuje (aktuálně stále v beta), které umožňují toohave bohatší se na poli možnosti monitorování s AI.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-149">[Application Insights support for Microservices and Containers](https://azure.microsoft.com/app-insights-microservices/) shows you some of hello new features that are being worked on (currently still in beta), which allow you toohave richer out-of-the-box monitoring options with AI.</span></span> <span data-ttu-id="0d6c6-150">Jedná se o závislost sledování (používá se při vytváření AppMap služeb a aplikací v clusteru a hello komunikace mezi nimi) a lepší korelace trasování pocházejících z vašich služeb (pomáhá v lepší přesným rozpoznáním problém v hello pracovní postup aplikace nebo služby).</span><span class="sxs-lookup"><span data-stu-id="0d6c6-150">These include dependency tracking (used in building an AppMap of all your services and applications in a cluster and hello communication between them), and better correlation of traces coming from your services (helps in better pinpointing an issue in hello workflow of an app or service).</span></span>

<span data-ttu-id="0d6c6-151">Pokud vývoji v rozhraní .NET a bude pravděpodobně možné pomocí některé z programovací modely Service Fabric a jsou ochotni toouse AI jako svou platformu pro vizualizaci a analýzu dat událostí a protokolů, pak doporučujeme přejít přes hello AI SDK trasy jako monitorování a Diagnostika pracovního postupu.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-151">If you are developing in .NET and will likely be using some of Service Fabric's programming models, and are willing toouse AI as your platform for visualizing and analyzing event and log data, then we recommend that you go via hello AI SDK route as your monitoring and diagnostics workflow.</span></span> <span data-ttu-id="0d6c6-152">Čtení [to](../application-insights/app-insights-asp-net-more.md) a [to](../application-insights/app-insights-asp-net-trace-logs.md) tooget začít s pomocí AI toocollect a zobrazit protokoly.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-152">Read [this](../application-insights/app-insights-asp-net-more.md) and [this](../application-insights/app-insights-asp-net-trace-logs.md) tooget started with using AI toocollect and display your logs.</span></span>

## <a name="navigating-hello-ai-resource-in-azure-portal"></a><span data-ttu-id="0d6c6-153">Navigace hello AI prostředků na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="0d6c6-153">Navigating hello AI resource in Azure portal</span></span>

<span data-ttu-id="0d6c6-154">Po nakonfigurování AI jako výstupu událostí a protokoly informace by měly spuštění tooshow v prostředku AI za pár minut.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-154">Once you have configured AI as an output for your events and logs, information should start tooshow up in your AI resource in a few minutes.</span></span> <span data-ttu-id="0d6c6-155">Přejděte toohello AI prostředku, který bude trvat jste toohello AI prostředků řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-155">Navigate toohello AI resource, which will take you toohello AI resource dashboard.</span></span> <span data-ttu-id="0d6c6-156">Klikněte na tlačítko **vyhledávání** v hello AI panelu toosee hello nejnovější trasování které byl přijat a mít toofilter toobe prostřednictvím je.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-156">Click **Search** in hello AI taskbar toosee hello latest traces that it has received, and toobe able toofilter through them.</span></span>

<span data-ttu-id="0d6c6-157">*Průzkumníku metrik* je užitečným nástrojem pro vytvoření vlastní řídicí panely podle metriky, které vaše aplikace, služby a cluster může být vytváření sestav.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-157">*Metrics Explorer* is a useful tool for creating custom dashboards based on metrics that your applications, services, and cluster may be reporting.</span></span> <span data-ttu-id="0d6c6-158">V tématu [zkoumat metriky ve službě Application Insights](../application-insights/app-insights-metrics-explorer.md) tooset až několik grafy pro sami na základě hello dat shromažďujete.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-158">See [Exploring Metrics in Application Insights](../application-insights/app-insights-metrics-explorer.md) tooset up a few charts for yourself based on hello data you are collecting.</span></span>

<span data-ttu-id="0d6c6-159">Kliknutím na tlačítko **Analytics** přejdete toohello Application Insights Analytics portál, kde může dotazovat událostí a trasování s větší rozsah a volitelnost.</span><span class="sxs-lookup"><span data-stu-id="0d6c6-159">Clicking **Analytics** will take you toohello Application Insights Analytics portal, where you can query events and traces with greater scope and optionality.</span></span> <span data-ttu-id="0d6c6-160">Další informace o to v [Analytics ve službě Application Insights](../application-insights/app-insights-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="0d6c6-160">Read more about this at [Analytics in Application Insights](../application-insights/app-insights-analytics.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0d6c6-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="0d6c6-161">Next steps</span></span>

* <span data-ttu-id="0d6c6-162">[Nastavení výstrah v AI](../application-insights/app-insights-alerts.md) toobe oznámení o změnách v výkonu a využití</span><span class="sxs-lookup"><span data-stu-id="0d6c6-162">[Set up Alerts in AI](../application-insights/app-insights-alerts.md) toobe notified about changes in performance or usage</span></span>
* <span data-ttu-id="0d6c6-163">[Inteligentní detekce ve službě Application Insights](../application-insights/app-insights-proactive-diagnostics.md) provede proaktivní analýzu hello telemetrie odesílány tooAI toowarn o potenciálním problémům s výkonem</span><span class="sxs-lookup"><span data-stu-id="0d6c6-163">[Smart Detection in Application Insights](../application-insights/app-insights-proactive-diagnostics.md) performs a proactive analysis of hello telemetry being sent tooAI toowarn you of potential performance problems</span></span>