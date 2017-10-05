---
title: Integrace SCOM s Application Insights | Microsoft Docs
description: "Pokud si uživatelé SCOM, sledovat výkon a diagnostikovat problémy s nástrojem Application Insights. Komplexní řídicí panely, inteligentní výstrahy, výkonné diagnostické nástroje a analýzy dotazy."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 606e9d03-c0e6-4a77-80e8-61b75efacde0
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/12/2016
ms.author: bwren
ms.openlocfilehash: 9c205465981fabdbb696cdc44f765532bbb992b5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="application-performance-monitoring-using-application-insights-for-scom"></a><span data-ttu-id="2f0b6-104">Sledování výkonu aplikací pomocí Application Insights pro SCOM</span><span class="sxs-lookup"><span data-stu-id="2f0b6-104">Application Performance Monitoring using Application Insights for SCOM</span></span>
<span data-ttu-id="2f0b6-105">Pokud používáte System Center Operations Manager (SCOM) ke správě vašich serverů, můžete sledovat výkon a diagnostikovat problémy s výkonem za pomoci [Azure Application Insights](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="2f0b6-105">If you use System Center Operations Manager (SCOM) to manage your servers, you can monitor performance and diagnose performance issues with the help of [Azure Application Insights](app-insights-asp-net.md).</span></span> <span data-ttu-id="2f0b6-106">Application Insights monitoruje webové aplikace příchozí požadavky a odchozí REST a volání SQL, výjimkami a protokolu trasování.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-106">Application Insights monitors your web application's incoming requests, outgoing REST and SQL calls, exceptions, and log traces.</span></span> <span data-ttu-id="2f0b6-107">Poskytuje řídicí panely pomocí metriky grafů a inteligentní výstrahy, a také výkonné diagnostické vyhledávání a analytické dotazy přes tuto telemetrii.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-107">It provides dashboards with metric charts and smart alerts, as well as powerful diagnostic search and analytical queries over this telemetry.</span></span> 

<span data-ttu-id="2f0b6-108">Můžete přepnout na Application Insights monitorování pomocí sady management pack SCOM.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-108">You can switch on Application Insights monitoring by using an SCOM management pack.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="2f0b6-109">Než začnete</span><span class="sxs-lookup"><span data-stu-id="2f0b6-109">Before you start</span></span>
<span data-ttu-id="2f0b6-110">Předpokládáme:</span><span class="sxs-lookup"><span data-stu-id="2f0b6-110">We assume:</span></span>

* <span data-ttu-id="2f0b6-111">Seznamte se s SCOM a že používáte ke správě vašeho IIS SCOM 2012 R2 nebo 2016 webových serverů.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-111">You're familiar with SCOM, and that you use SCOM 2012 R2 or 2016 to manage your IIS web servers.</span></span>
* <span data-ttu-id="2f0b6-112">Již jste nainstalovali na serverech webové aplikace, které chcete monitorovat pomocí nástroje Application Insights.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-112">You have already installed on your servers a web application that you want to monitor with Application Insights.</span></span>
* <span data-ttu-id="2f0b6-113">Je aplikace framework verze rozhraní .NET 4.5 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-113">App framework version is .NET 4.5 or later.</span></span>
* <span data-ttu-id="2f0b6-114">Máte přístup k odběru v [Microsoft Azure](https://azure.com) a můžete přihlásit k [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="2f0b6-114">You have access to a subscription in [Microsoft Azure](https://azure.com) and can sign in to the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="2f0b6-115">Vaše organizace může mít odběr a do něj může přidat účet Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-115">Your organization may have a subscription, and can add your Microsoft account to it.</span></span>

<span data-ttu-id="2f0b6-116">(Vývojový tým může vytvořit [Application Insights SDK](app-insights-asp-net.md) do webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-116">(The development team might build the [Application Insights SDK](app-insights-asp-net.md) into the web app.</span></span> <span data-ttu-id="2f0b6-117">Tento čas sestavení instrumentace je dává větší flexibilitu při psaní vlastní telemetrii.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-117">This build-time instrumentation gives them greater flexibility in writing custom telemetry.</span></span> <span data-ttu-id="2f0b6-118">Ale nezáleží: provedením kroků popsaných v tomto poli s nebo bez SDK integrovanou.)</span><span class="sxs-lookup"><span data-stu-id="2f0b6-118">However, it doesn't matter: you can follow the steps described here either with or without the SDK built in.)</span></span>

## <a name="one-time-install-application-insights-management-pack"></a><span data-ttu-id="2f0b6-119">(Jednou) Instalovat sadu management pack Application Insights</span><span class="sxs-lookup"><span data-stu-id="2f0b6-119">(One time) Install Application Insights management pack</span></span>
<span data-ttu-id="2f0b6-120">Na počítači, kde spuštění nástroje Operations Manager:</span><span class="sxs-lookup"><span data-stu-id="2f0b6-120">On the machine where you run Operations Manager:</span></span>

1. <span data-ttu-id="2f0b6-121">Odinstalujte všechny předchozí verzi sady management pack:</span><span class="sxs-lookup"><span data-stu-id="2f0b6-121">Uninstall any old version of the management pack:</span></span>
   1. <span data-ttu-id="2f0b6-122">V nástroji Operations Manager otevřete správy, sad Management Pack.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-122">In Operations Manager, open Administration, Management Packs.</span></span> 
   2. <span data-ttu-id="2f0b6-123">Odstraňte staré verze.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-123">Delete the old version.</span></span>
2. <span data-ttu-id="2f0b6-124">Stáhněte a nainstalujte sadu management pack z katalogu.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-124">Download and install the management pack from the catalog.</span></span>
3. <span data-ttu-id="2f0b6-125">Restartujte nástroj Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-125">Restart Operations Manager.</span></span>

## <a name="create-a-management-pack"></a><span data-ttu-id="2f0b6-126">Vytvořit sadu management pack</span><span class="sxs-lookup"><span data-stu-id="2f0b6-126">Create a management pack</span></span>
1. <span data-ttu-id="2f0b6-127">V nástroji Operations Manager, otevřete **vytváření**, **rozhraní .NET... with Application Insights**, **Průvodce přidáním monitorování**a znovu vyberte **rozhraní .NET... s aplikací Statistika**.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-127">In Operations Manager, open **Authoring**, **.NET...with Application Insights**, **Add Monitoring Wizard**, and again choose **.NET...with Application Insights**.</span></span>
   
    ![](./media/app-insights-scom/020.png)
2. <span data-ttu-id="2f0b6-128">Název konfigurace po vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-128">Name the configuration after your app.</span></span> <span data-ttu-id="2f0b6-129">(Máte instrumentace jednu aplikaci najednou.)</span><span class="sxs-lookup"><span data-stu-id="2f0b6-129">(You have to instrument one app at a time.)</span></span>
   
    ![](./media/app-insights-scom/030.png)
3. <span data-ttu-id="2f0b6-130">Na stejné stránce průvodce vytvořte novou sadu management pack nebo vyberte balíček, který jste předtím vytvořili pro Application Insights.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-130">On the same wizard page, either create a new management pack, or select a pack that you created for Application Insights earlier.</span></span>
   
     <span data-ttu-id="2f0b6-131">(Application Insights [sada management pack](https://technet.microsoft.com/library/cc974491.aspx) je šablona, ze kterého můžete vytvořit instanci.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-131">(The Application Insights [management pack](https://technet.microsoft.com/library/cc974491.aspx) is a template, from which you create an instance.</span></span> <span data-ttu-id="2f0b6-132">Můžete opakovaně použít stejnou instanci později.)</span><span class="sxs-lookup"><span data-stu-id="2f0b6-132">You can reuse the same instance later.)</span></span>

    ![Na kartě Obecné vlastnosti zadejte název aplikace.](./media/app-insights-scom/040.png)

1. <span data-ttu-id="2f0b6-136">Vyberte jednu aplikaci, kterou chcete monitorovat.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-136">Choose one app that you want to monitor.</span></span> <span data-ttu-id="2f0b6-137">Vyhledávací funkce hledá mezi aplikace nainstalované na serverech.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-137">The search feature searches among apps installed on your servers.</span></span>
   
    ![K tomu, co k monitorování klikněte na tlačítko Přidat, zadejte část názvu aplikace, klikněte na tlačítko Hledat, vyberte aplikaci a pak přidat OK.](./media/app-insights-scom/050.png)
   
    <span data-ttu-id="2f0b6-139">Volitelné pole obor monitorování umožňuje určit podmnožinu vašim serverům, pokud nechcete, aby ke sledování aplikace na všech serverech.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-139">The optional Monitoring scope field can be used to specify a subset of your servers, if you don't want to monitor the app in all servers.</span></span>
2. <span data-ttu-id="2f0b6-140">Na další stránce průvodce musíte nejprve zadejte svoje přihlašovací údaje pro přihlášení do služby Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-140">On the next wizard page, you must first provide your credentials to sign in to Microsoft Azure.</span></span>
   
    <span data-ttu-id="2f0b6-141">Na této stránce zvolte prostředek Application Insights, kam chcete data telemetrie analyzovat a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-141">On this page, you choose the Application Insights resource where you want the telemetry data to be analyzed and displayed.</span></span> 
   
   * <span data-ttu-id="2f0b6-142">Pokud aplikace byla nakonfigurována pro službu Application Insights během vývoje, vyberte svůj existující prostředek.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-142">If the application was configured for Application Insights during development, select its existing resource.</span></span>
   * <span data-ttu-id="2f0b6-143">Jinak vytvořte nový prostředek s názvem aplikace.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-143">Otherwise, create a new resource named for the app.</span></span> <span data-ttu-id="2f0b6-144">Pokud existují další aplikace, které jsou součástí ve stejném systému, umístí je ve stejné skupině prostředků, aby přístup k telemetrii snazší správa.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-144">If there are other apps that are components of the same system, put them in the same resource group, to make access to the telemetry easier to manage.</span></span>
     
     <span data-ttu-id="2f0b6-145">Toto nastavení můžete později změnit.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-145">You can change these settings later.</span></span>
     
     ![Na kartě Nastavení Application Insights klikněte na tlačítko "přihlášení" a zadejte přihlašovací údaje účtu Microsoft Azure.](./media/app-insights-scom/060.png)
3. <span data-ttu-id="2f0b6-148">Dokončete průvodce.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-148">Complete the wizard.</span></span>
   
    ![Kliknutí na Vytvořit](./media/app-insights-scom/070.png)

<span data-ttu-id="2f0b6-150">Tento postup opakujte pro každou aplikaci, kterou chcete monitorovat.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-150">Repeat this procedure for each app that you want to monitor.</span></span>

<span data-ttu-id="2f0b6-151">Pokud budete potřebovat později změnit nastavení, otevřete znovu vlastnosti monitorování z okna pro vytváření obsahu.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-151">If you need to change settings later, re-open the properties of the monitor from the Authoring window.</span></span>

![V vytváření obsahu, vyberte monitorování výkonu aplikací .NET pomocí nástroje Application Insights, vyberte monitoru a klikněte na položku Vlastnosti.](./media/app-insights-scom/080.png)

## <a name="verify-monitoring"></a><span data-ttu-id="2f0b6-153">Ověřte monitorování</span><span class="sxs-lookup"><span data-stu-id="2f0b6-153">Verify monitoring</span></span>
<span data-ttu-id="2f0b6-154">Monitorování nainstalovanou vyhledávání pro vaši aplikaci na každém serveru.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-154">The monitor that you have installed searches for your app on every server.</span></span> <span data-ttu-id="2f0b6-155">Pokud zjistí aplikace a konfiguruje monitorování stavu Application Insights pro sledování aplikace.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-155">Where it finds the app, it configures Application Insights Status Monitor to monitor the app.</span></span> <span data-ttu-id="2f0b6-156">V případě potřeby nainstaluje nejprve monitorování stavu na serveru.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-156">If necessary, it first installs Status Monitor on the server.</span></span>

<span data-ttu-id="2f0b6-157">Můžete ověřit, které instance aplikace má najít:</span><span class="sxs-lookup"><span data-stu-id="2f0b6-157">You can verify which instances of the app it has found:</span></span>

![V monitorování, otevřete službu Application Insights](./media/app-insights-scom/100.png)

## <a name="view-telemetry-in-application-insights"></a><span data-ttu-id="2f0b6-159">Zobrazení telemetrie Application Insights</span><span class="sxs-lookup"><span data-stu-id="2f0b6-159">View telemetry in Application Insights</span></span>
<span data-ttu-id="2f0b6-160">V [portál Azure](https://portal.azure.com), přejděte k prostředku pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-160">In the [Azure portal](https://portal.azure.com), browse to the resource for your app.</span></span> <span data-ttu-id="2f0b6-161">Můžete [zobrazili grafy znázorňující telemetrie](app-insights-dashboards.md) z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-161">You [see charts showing telemetry](app-insights-dashboards.md) from your app.</span></span> <span data-ttu-id="2f0b6-162">(Pokud se ještě zobrazovat na hlavní stránce ještě, klikněte na živý datový proud metriky.)</span><span class="sxs-lookup"><span data-stu-id="2f0b6-162">(If it hasn't shown up on the main page yet, click Live Metrics Stream.)</span></span>

## <a name="next-steps"></a><span data-ttu-id="2f0b6-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2f0b6-163">Next steps</span></span>
* <span data-ttu-id="2f0b6-164">[Nastavení řídicího panelu](app-insights-dashboards.md) sdružujícího nejdůležitější grafy monitorování tato a další aplikace.</span><span class="sxs-lookup"><span data-stu-id="2f0b6-164">[Set up a dashboard](app-insights-dashboards.md) to bring together the most important charts monitoring this and other apps.</span></span>
* [<span data-ttu-id="2f0b6-165">Další informace o metriky</span><span class="sxs-lookup"><span data-stu-id="2f0b6-165">Learn about metrics</span></span>](app-insights-metrics-explorer.md)
* [<span data-ttu-id="2f0b6-166">Nastavení výstrah</span><span class="sxs-lookup"><span data-stu-id="2f0b6-166">Set up alerts</span></span>](app-insights-alerts.md)
* [<span data-ttu-id="2f0b6-167">Diagnostika problémů s výkonem</span><span class="sxs-lookup"><span data-stu-id="2f0b6-167">Diagnosing performance issues</span></span>](app-insights-detect-triage-diagnose.md)
* [<span data-ttu-id="2f0b6-168">Efektivní analytické dotazy</span><span class="sxs-lookup"><span data-stu-id="2f0b6-168">Powerful Analytics queries</span></span>](app-insights-analytics.md)
* [<span data-ttu-id="2f0b6-169">Testy dostupnosti webu</span><span class="sxs-lookup"><span data-stu-id="2f0b6-169">Availability web tests</span></span>](app-insights-monitor-web-app-availability.md)

