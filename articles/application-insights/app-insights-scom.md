---
title: "aaaSCOM integrace s nástrojem Application Insights | Microsoft Docs"
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
ms.openlocfilehash: ee9ad462610fd916098a0e292c5bd44f2a873c10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-performance-monitoring-using-application-insights-for-scom"></a><span data-ttu-id="ef0c2-104">Sledování výkonu aplikací pomocí Application Insights pro SCOM</span><span class="sxs-lookup"><span data-stu-id="ef0c2-104">Application Performance Monitoring using Application Insights for SCOM</span></span>
<span data-ttu-id="ef0c2-105">Pokud používáte System Center Operations Manager (SCOM) toomanage vašich serverů, můžete sledovat výkon a diagnostikovat problémy s výkonem hello pomoci [Azure Application Insights](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="ef0c2-105">If you use System Center Operations Manager (SCOM) toomanage your servers, you can monitor performance and diagnose performance issues with hello help of [Azure Application Insights](app-insights-asp-net.md).</span></span> <span data-ttu-id="ef0c2-106">Application Insights monitoruje webové aplikace příchozí požadavky a odchozí REST a volání SQL, výjimkami a protokolu trasování.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-106">Application Insights monitors your web application's incoming requests, outgoing REST and SQL calls, exceptions, and log traces.</span></span> <span data-ttu-id="ef0c2-107">Poskytuje řídicí panely pomocí metriky grafů a inteligentní výstrahy, a také výkonné diagnostické vyhledávání a analytické dotazy přes tuto telemetrii.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-107">It provides dashboards with metric charts and smart alerts, as well as powerful diagnostic search and analytical queries over this telemetry.</span></span> 

<span data-ttu-id="ef0c2-108">Můžete přepnout na Application Insights monitorování pomocí sady management pack SCOM.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-108">You can switch on Application Insights monitoring by using an SCOM management pack.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="ef0c2-109">Než začnete</span><span class="sxs-lookup"><span data-stu-id="ef0c2-109">Before you start</span></span>
<span data-ttu-id="ef0c2-110">Předpokládáme:</span><span class="sxs-lookup"><span data-stu-id="ef0c2-110">We assume:</span></span>

* <span data-ttu-id="ef0c2-111">Jste obeznámeni s SCOM a že používáte SCOM 2012 R2 nebo 2016 toomanage vaší služby IIS webových serverů.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-111">You're familiar with SCOM, and that you use SCOM 2012 R2 or 2016 toomanage your IIS web servers.</span></span>
* <span data-ttu-id="ef0c2-112">Již jste nainstalovali na serverech webové aplikace, které chcete toomonitor s Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-112">You have already installed on your servers a web application that you want toomonitor with Application Insights.</span></span>
* <span data-ttu-id="ef0c2-113">Je aplikace framework verze rozhraní .NET 4.5 nebo novější.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-113">App framework version is .NET 4.5 or later.</span></span>
* <span data-ttu-id="ef0c2-114">Máte předplatné tooa přístup [Microsoft Azure](https://azure.com) a může podepsat v toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ef0c2-114">You have access tooa subscription in [Microsoft Azure](https://azure.com) and can sign in toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="ef0c2-115">Vaše organizace může mít odběr a můžete přidat vaše tooit účet Microsoft.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-115">Your organization may have a subscription, and can add your Microsoft account tooit.</span></span>

<span data-ttu-id="ef0c2-116">(hello vývojový tým může vytvořit hello [Application Insights SDK](app-insights-asp-net.md) do hello webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-116">(hello development team might build hello [Application Insights SDK](app-insights-asp-net.md) into hello web app.</span></span> <span data-ttu-id="ef0c2-117">Tento čas sestavení instrumentace je dává větší flexibilitu při psaní vlastní telemetrii.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-117">This build-time instrumentation gives them greater flexibility in writing custom telemetry.</span></span> <span data-ttu-id="ef0c2-118">Ale nezáleží: můžete provést kroky hello zde popsané s nebo bez hello SDK integrovanou.)</span><span class="sxs-lookup"><span data-stu-id="ef0c2-118">However, it doesn't matter: you can follow hello steps described here either with or without hello SDK built in.)</span></span>

## <a name="one-time-install-application-insights-management-pack"></a><span data-ttu-id="ef0c2-119">(Jednou) Instalovat sadu management pack Application Insights</span><span class="sxs-lookup"><span data-stu-id="ef0c2-119">(One time) Install Application Insights management pack</span></span>
<span data-ttu-id="ef0c2-120">Na počítači hello kde spuštění nástroje Operations Manager:</span><span class="sxs-lookup"><span data-stu-id="ef0c2-120">On hello machine where you run Operations Manager:</span></span>

1. <span data-ttu-id="ef0c2-121">Odinstalujte všechny předchozí verzi sady hello management pack:</span><span class="sxs-lookup"><span data-stu-id="ef0c2-121">Uninstall any old version of hello management pack:</span></span>
   1. <span data-ttu-id="ef0c2-122">V nástroji Operations Manager otevřete správy, sad Management Pack.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-122">In Operations Manager, open Administration, Management Packs.</span></span> 
   2. <span data-ttu-id="ef0c2-123">Odstraňte stará verze hello.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-123">Delete hello old version.</span></span>
2. <span data-ttu-id="ef0c2-124">Stáhněte a nainstalujte hello management pack z katalogu hello.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-124">Download and install hello management pack from hello catalog.</span></span>
3. <span data-ttu-id="ef0c2-125">Restartujte nástroj Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-125">Restart Operations Manager.</span></span>

## <a name="create-a-management-pack"></a><span data-ttu-id="ef0c2-126">Vytvořit sadu management pack</span><span class="sxs-lookup"><span data-stu-id="ef0c2-126">Create a management pack</span></span>
1. <span data-ttu-id="ef0c2-127">V nástroji Operations Manager, otevřete **vytváření**, **rozhraní .NET... with Application Insights**, **Průvodce přidáním monitorování**a znovu vyberte **rozhraní .NET... s aplikací Statistika**.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-127">In Operations Manager, open **Authoring**, **.NET...with Application Insights**, **Add Monitoring Wizard**, and again choose **.NET...with Application Insights**.</span></span>
   
    ![](./media/app-insights-scom/020.png)
2. <span data-ttu-id="ef0c2-128">Konfigurace názvu hello po vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-128">Name hello configuration after your app.</span></span> <span data-ttu-id="ef0c2-129">(Současně mít tooinstrument jednu aplikaci.)</span><span class="sxs-lookup"><span data-stu-id="ef0c2-129">(You have tooinstrument one app at a time.)</span></span>
   
    ![](./media/app-insights-scom/030.png)
3. <span data-ttu-id="ef0c2-130">Hello na stejné stránce průvodce, buď vytvořit novou správy aktualizací Service pack nebo vyberte balíček, který jste předtím vytvořili pro Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-130">On hello same wizard page, either create a new management pack, or select a pack that you created for Application Insights earlier.</span></span>
   
     <span data-ttu-id="ef0c2-131">(hello Application Insights [sada management pack](https://technet.microsoft.com/library/cc974491.aspx) je šablona, ze kterého můžete vytvořit instanci.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-131">(hello Application Insights [management pack](https://technet.microsoft.com/library/cc974491.aspx) is a template, from which you create an instance.</span></span> <span data-ttu-id="ef0c2-132">Můžete opakovaně použít stejnou instanci později hello.)</span><span class="sxs-lookup"><span data-stu-id="ef0c2-132">You can reuse hello same instance later.)</span></span>

    ![Hello karta Obecné vlastnosti zadejte název hello aplikace hello.](./media/app-insights-scom/040.png)

1. <span data-ttu-id="ef0c2-136">Vyberte jednu aplikaci, které chcete toomonitor.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-136">Choose one app that you want toomonitor.</span></span> <span data-ttu-id="ef0c2-137">Hello vyhledávací funkce hledá mezi aplikace nainstalované na serverech.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-137">hello search feature searches among apps installed on your servers.</span></span>
   
    ![Na kartě co tooMonitor klikněte na tlačítko Přidat, zadejte část názvu aplikace hello, klikněte na tlačítko Hledat, hello aplikaci a pak přidat, zvolte OK.](./media/app-insights-scom/050.png)
   
    <span data-ttu-id="ef0c2-139">Hello volitelné monitorování oboru pole lze použít toospecify podmnožinu vašim serverům, pokud nechcete, aby aplikace hello toomonitor ve všech serverech.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-139">hello optional Monitoring scope field can be used toospecify a subset of your servers, if you don't want toomonitor hello app in all servers.</span></span>
2. <span data-ttu-id="ef0c2-140">Na další stránce průvodce hello nejprve je nutné zadat vaše přihlašovací údaje toosign v tooMicrosoft Azure.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-140">On hello next wizard page, you must first provide your credentials toosign in tooMicrosoft Azure.</span></span>
   
    <span data-ttu-id="ef0c2-141">Na této stránce zvolte prostředek Application Insights hello místo hello telemetrická data toobe analyzovat a zobrazit.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-141">On this page, you choose hello Application Insights resource where you want hello telemetry data toobe analyzed and displayed.</span></span> 
   
   * <span data-ttu-id="ef0c2-142">Pokud aplikace hello bylo nakonfigurované pro službu Application Insights během vývoje, vyberte svůj existující prostředek.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-142">If hello application was configured for Application Insights during development, select its existing resource.</span></span>
   * <span data-ttu-id="ef0c2-143">Jinak vytvořte nový prostředek s názvem aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-143">Otherwise, create a new resource named for hello app.</span></span> <span data-ttu-id="ef0c2-144">Pokud existují další aplikace, které jsou součástí z hello stejném systému, jejich umístění v hello stejnou skupinu prostředků, toomake přístup toohello telemetrie jednodušší toomanage.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-144">If there are other apps that are components of hello same system, put them in hello same resource group, toomake access toohello telemetry easier toomanage.</span></span>
     
     <span data-ttu-id="ef0c2-145">Toto nastavení můžete později změnit.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-145">You can change these settings later.</span></span>
     
     ![Na kartě Nastavení Application Insights klikněte na tlačítko "přihlášení" a zadejte přihlašovací údaje účtu Microsoft Azure.](./media/app-insights-scom/060.png)
3. <span data-ttu-id="ef0c2-148">Dokončení hello průvodce.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-148">Complete hello wizard.</span></span>
   
    ![Kliknutí na Vytvořit](./media/app-insights-scom/070.png)

<span data-ttu-id="ef0c2-150">Tento postup opakujte pro každou aplikaci, které chcete toomonitor.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-150">Repeat this procedure for each app that you want toomonitor.</span></span>

<span data-ttu-id="ef0c2-151">Pokud budete později potřebovat toochange nastavení, znovu ho otevřete vlastnosti hello hello monitorování z okna hello vytváření obsahu.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-151">If you need toochange settings later, re-open hello properties of hello monitor from hello Authoring window.</span></span>

![V vytváření obsahu, vyberte monitorování výkonu aplikací .NET pomocí nástroje Application Insights, vyberte monitoru a klikněte na položku Vlastnosti.](./media/app-insights-scom/080.png)

## <a name="verify-monitoring"></a><span data-ttu-id="ef0c2-153">Ověřte monitorování</span><span class="sxs-lookup"><span data-stu-id="ef0c2-153">Verify monitoring</span></span>
<span data-ttu-id="ef0c2-154">monitorování Hello nainstalovanou vyhledávání pro vaši aplikaci na každém serveru.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-154">hello monitor that you have installed searches for your app on every server.</span></span> <span data-ttu-id="ef0c2-155">Pokud zjistí aplikace hello konfiguruje aplikace hello toomonitor monitorování stavu Application Insights.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-155">Where it finds hello app, it configures Application Insights Status Monitor toomonitor hello app.</span></span> <span data-ttu-id="ef0c2-156">V případě potřeby nainstaluje nejprve monitorování stavu na serveru hello.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-156">If necessary, it first installs Status Monitor on hello server.</span></span>

<span data-ttu-id="ef0c2-157">Můžete ověřit, která instance aplikace hello najde:</span><span class="sxs-lookup"><span data-stu-id="ef0c2-157">You can verify which instances of hello app it has found:</span></span>

![V monitorování, otevřete službu Application Insights](./media/app-insights-scom/100.png)

## <a name="view-telemetry-in-application-insights"></a><span data-ttu-id="ef0c2-159">Zobrazení telemetrie Application Insights</span><span class="sxs-lookup"><span data-stu-id="ef0c2-159">View telemetry in Application Insights</span></span>
<span data-ttu-id="ef0c2-160">V hello [portál Azure](https://portal.azure.com), procházet toohello prostředků pro vaši aplikaci.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-160">In hello [Azure portal](https://portal.azure.com), browse toohello resource for your app.</span></span> <span data-ttu-id="ef0c2-161">Můžete [zobrazili grafy znázorňující telemetrie](app-insights-dashboards.md) z vaší aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-161">You [see charts showing telemetry](app-insights-dashboards.md) from your app.</span></span> <span data-ttu-id="ef0c2-162">(Pokud se ještě zobrazovat na hlavní stránku hello ještě, klikněte na živý datový proud metriky.)</span><span class="sxs-lookup"><span data-stu-id="ef0c2-162">(If it hasn't shown up on hello main page yet, click Live Metrics Stream.)</span></span>

## <a name="next-steps"></a><span data-ttu-id="ef0c2-163">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ef0c2-163">Next steps</span></span>
* <span data-ttu-id="ef0c2-164">[Nastavení řídicího panelu](app-insights-dashboards.md) toobring společně hello nejdůležitější grafy monitorování tato a další aplikace.</span><span class="sxs-lookup"><span data-stu-id="ef0c2-164">[Set up a dashboard](app-insights-dashboards.md) toobring together hello most important charts monitoring this and other apps.</span></span>
* [<span data-ttu-id="ef0c2-165">Další informace o metriky</span><span class="sxs-lookup"><span data-stu-id="ef0c2-165">Learn about metrics</span></span>](app-insights-metrics-explorer.md)
* [<span data-ttu-id="ef0c2-166">Nastavení výstrah</span><span class="sxs-lookup"><span data-stu-id="ef0c2-166">Set up alerts</span></span>](app-insights-alerts.md)
* [<span data-ttu-id="ef0c2-167">Diagnostika problémů s výkonem</span><span class="sxs-lookup"><span data-stu-id="ef0c2-167">Diagnosing performance issues</span></span>](app-insights-detect-triage-diagnose.md)
* [<span data-ttu-id="ef0c2-168">Efektivní analytické dotazy</span><span class="sxs-lookup"><span data-stu-id="ef0c2-168">Powerful Analytics queries</span></span>](app-insights-analytics.md)
* [<span data-ttu-id="ef0c2-169">Testy dostupnosti webu</span><span class="sxs-lookup"><span data-stu-id="ef0c2-169">Availability web tests</span></span>](app-insights-monitor-web-app-availability.md)

