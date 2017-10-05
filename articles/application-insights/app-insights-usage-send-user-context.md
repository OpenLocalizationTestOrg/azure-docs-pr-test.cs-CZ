---
title: "Odesílání uživatelský kontext povolit použití možnosti v Azure Application Insights | Microsoft Docs"
description: "Sledovat, jak se uživatelé přesunou prostřednictvím služby po každé z nich přiřazení jedinečné, trvalé řetězec ID ve službě Application Insights."
services: application-insights
documentationcenter: 
author: abgreg
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: csharp
ms.topic: article
ms.date: 08/02/2017
ms.author: bwren
ms.openlocfilehash: 9350029c775643be0dcc679b0f4bb9238b5f8aca
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
#  <a name="sending-user-context-to-enable-usage-experiences-in-azure-application-insights"></a><span data-ttu-id="a5d38-103">Odesílání uživatelský kontext povolit použití možnosti v Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="a5d38-103">Sending user context to enable usage experiences in Azure Application Insights</span></span>

## <a name="tracking-users"></a><span data-ttu-id="a5d38-104">Sledování uživatelů</span><span class="sxs-lookup"><span data-stu-id="a5d38-104">Tracking users</span></span>

<span data-ttu-id="a5d38-105">Application Insights umožňuje monitorování a sledování uživatelům pomocí sady nástrojů pro použití produktu:</span><span class="sxs-lookup"><span data-stu-id="a5d38-105">Application Insights enables you to monitor and track your users through a set of product usage tools:</span></span> 
* [<span data-ttu-id="a5d38-106">Uživatelé, relace, události</span><span class="sxs-lookup"><span data-stu-id="a5d38-106">Users, Sessions, Events</span></span>](https://docs.microsoft.com/azure/application-insights/app-insights-usage-segmentation)
* [<span data-ttu-id="a5d38-107">Trychtýře</span><span class="sxs-lookup"><span data-stu-id="a5d38-107">Funnels</span></span>](https://docs.microsoft.com/azure/application-insights/usage-funnels)
* [<span data-ttu-id="a5d38-108">Uchování</span><span class="sxs-lookup"><span data-stu-id="a5d38-108">Retention</span></span>](https://docs.microsoft.com/azure/application-insights/app-insights-usage-retention)
* <span data-ttu-id="a5d38-109">Kohorty</span><span class="sxs-lookup"><span data-stu-id="a5d38-109">Cohorts</span></span>
* [<span data-ttu-id="a5d38-110">Workbooks</span><span class="sxs-lookup"><span data-stu-id="a5d38-110">Workbooks</span></span>](https://docs.microsoft.com/azure/application-insights/app-insights-usage-workbooks)

<span data-ttu-id="a5d38-111">Aby bylo možné sledovat, co uživatel provede v čase, Application Insights pro jednotlivé uživatele nebo relace potřebuje ID.</span><span class="sxs-lookup"><span data-stu-id="a5d38-111">In order to track what a user does over time, Application Insights needs an ID for each user or session.</span></span> <span data-ttu-id="a5d38-112">Zahrňte tyto ID každé vlastní zobrazení události nebo stránky.</span><span class="sxs-lookup"><span data-stu-id="a5d38-112">Include these IDs in every custom event or page view.</span></span>
- <span data-ttu-id="a5d38-113">Uživatelé, nálevky, uchovávání a kohorty: zahrnují ID uživatele.</span><span class="sxs-lookup"><span data-stu-id="a5d38-113">Users, Funnels, Retention, and Cohorts: Include user ID.</span></span>
- <span data-ttu-id="a5d38-114">Relace: Zahrnují ID relace.</span><span class="sxs-lookup"><span data-stu-id="a5d38-114">Sessions: Include session ID.</span></span>

<span data-ttu-id="a5d38-115">Pokud vaše aplikace je integrována [JavaScript SDK](https://docs.microsoft.com/azure/application-insights/app-insights-javascript#set-up-application-insights-for-your-web-page), automaticky sledován ID uživatele.</span><span class="sxs-lookup"><span data-stu-id="a5d38-115">If your app is integrated with the [JavaScript SDK](https://docs.microsoft.com/azure/application-insights/app-insights-javascript#set-up-application-insights-for-your-web-page), user ID is tracked automatically.</span></span>

## <a name="choosing-user-ids"></a><span data-ttu-id="a5d38-116">Výběr ID uživatele</span><span class="sxs-lookup"><span data-stu-id="a5d38-116">Choosing user IDs</span></span>

<span data-ttu-id="a5d38-117">ID uživatele by měla zachová napříč relacemi uživatele sledovat chování uživatele v průběhu času.</span><span class="sxs-lookup"><span data-stu-id="a5d38-117">User IDs should persist across user sessions to track how users behave over time.</span></span> <span data-ttu-id="a5d38-118">Existují různé přístupy k zachování ID.</span><span class="sxs-lookup"><span data-stu-id="a5d38-118">There are various approaches for persisting the ID.</span></span>
- <span data-ttu-id="a5d38-119">Definice uživatele, který už máte ve službě.</span><span class="sxs-lookup"><span data-stu-id="a5d38-119">A definition of a user that you already have in your service.</span></span>
- <span data-ttu-id="a5d38-120">Služba má přístup do prohlížeče, pak může předat soubor cookie s ID prohlížeče v ní.</span><span class="sxs-lookup"><span data-stu-id="a5d38-120">If the service has access to a browser, it can pass the browser a cookie with an ID in it.</span></span> <span data-ttu-id="a5d38-121">ID pro se uchová, dokud zůstane soubor cookie v prohlížeči uživatele.</span><span class="sxs-lookup"><span data-stu-id="a5d38-121">The ID will persist for as long as the cookie remains in the user's browser.</span></span>
- <span data-ttu-id="a5d38-122">V případě potřeby můžete použít nové ID každé relaci, ale výsledky o uživatelích, bude omezena.</span><span class="sxs-lookup"><span data-stu-id="a5d38-122">If necessary, you can use a new ID each session, but the results about users will be limited.</span></span> <span data-ttu-id="a5d38-123">Například nebudete moci zobrazit, jak v průběhu času mění chování uživatele.</span><span class="sxs-lookup"><span data-stu-id="a5d38-123">For example, you won't be able to see how a user's behavior changes over time.</span></span>

<span data-ttu-id="a5d38-124">ID musí být identifikátor Guid nebo jiný řetězec dostatečně složité, se jednoznačně označit každého uživatele.</span><span class="sxs-lookup"><span data-stu-id="a5d38-124">The ID should be a Guid or another string complex enough to identify each user uniquely.</span></span> <span data-ttu-id="a5d38-125">Například to může být dlouho náhodné číslo.</span><span class="sxs-lookup"><span data-stu-id="a5d38-125">For example, it could be a long random number.</span></span>

<span data-ttu-id="a5d38-126">Pokud ID obsahuje osobní identifikační informace o uživateli, se nejedná o správnou hodnotu k odeslání do Application Insights jako ID uživatele.</span><span class="sxs-lookup"><span data-stu-id="a5d38-126">If the ID contains personally identifying information about the user, it is not an appropriate value to send to Application Insights as a user ID.</span></span> <span data-ttu-id="a5d38-127">Můžete odeslat toto ID jako [ověřit ID uživatele](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users), ale nesplňuje požadavek na ID uživatele pro scénáře použití.</span><span class="sxs-lookup"><span data-stu-id="a5d38-127">You can send such an ID as an [authenticated user ID](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users), but it does not fulfill the user ID requirement for usage scenarios.</span></span>

## <a name="aspnet-apps-set-user-context-in-an-itelemetryinitializer"></a><span data-ttu-id="a5d38-128">Aplikace ASP.NET: Nastavit kontext uživatele v ITelemetryInitializer</span><span class="sxs-lookup"><span data-stu-id="a5d38-128">ASP.NET Apps: Set user context in an ITelemetryInitializer</span></span>

<span data-ttu-id="a5d38-129">Vytvořte inicializátoru telemetrie, jak je podrobně popsaná v [zde](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer)a nastavte Context.User.Id a Context.Session.Id.</span><span class="sxs-lookup"><span data-stu-id="a5d38-129">Create a telemetry initializer, as described in detail [here](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer), and set the Context.User.Id and the Context.Session.Id.</span></span>

<span data-ttu-id="a5d38-130">Tento příklad nastaví na identifikátor, který vyprší po relaci ID uživatele.</span><span class="sxs-lookup"><span data-stu-id="a5d38-130">This example sets the user ID to an identifier that expires after the session.</span></span> <span data-ttu-id="a5d38-131">Pokud je to možné použijte ID uživatele, která je uchována napříč relacemi.</span><span class="sxs-lookup"><span data-stu-id="a5d38-131">If possible, use a user ID that persists across sessions.</span></span>

<span data-ttu-id="a5d38-132">*C#*</span><span class="sxs-lookup"><span data-stu-id="a5d38-132">*C#*</span></span>

```C#

    using System;
    using System.Web;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that sets the user ID.
       *
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            // For a full experience, track each user across sessions. For an incomplete view of user 
            // behavior within a session, store user ID on the HttpContext Session.
            // Set the user ID if we haven't done so yet.
            if (HttpContext.Current.Session["UserId"] == null)
            {
                HttpContext.Current.Session["UserId"] = Guid.NewGuid();
            }

            // Set the user id on the Application Insights telemetry item.
            telemetry.Context.User.Id = (string)HttpContext.Current.Session["UserId"];

            // Set the session id on the Application Insights telemetry item.
            telemetry.Context.Session.Id = HttpContext.Current.Session.SessionID;
        }
      }
    }
```

## <a name="next-steps"></a><span data-ttu-id="a5d38-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a5d38-133">Next steps</span></span>
- <span data-ttu-id="a5d38-134">Pokud chcete povolit použití možnosti, zahájit odesílání [vlastních událostí](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) nebo [stránky zobrazení](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span><span class="sxs-lookup"><span data-stu-id="a5d38-134">To enable usage experiences, start sending [custom events](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) or [page views](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span></span>
- <span data-ttu-id="a5d38-135">Pokud jste již odeslat vlastní události nebo zobrazení stránky, prozkoumejte využití nástroje se dozvíte, jak uživatelé používat služby.</span><span class="sxs-lookup"><span data-stu-id="a5d38-135">If you already send custom events or page views, explore the Usage tools to learn how users use your service.</span></span>
    * [<span data-ttu-id="a5d38-136">Přehled využití</span><span class="sxs-lookup"><span data-stu-id="a5d38-136">Usage overview</span></span>](app-insights-usage-overview.md)
    * [<span data-ttu-id="a5d38-137">Uživatelů, relací a události</span><span class="sxs-lookup"><span data-stu-id="a5d38-137">Users, Sessions, and Events</span></span>](app-insights-usage-segmentation.md)
    * [<span data-ttu-id="a5d38-138">Trychtýře</span><span class="sxs-lookup"><span data-stu-id="a5d38-138">Funnels</span></span>](usage-funnels.md)
    * [<span data-ttu-id="a5d38-139">Uchování</span><span class="sxs-lookup"><span data-stu-id="a5d38-139">Retention</span></span>](app-insights-usage-retention.md)
    * [<span data-ttu-id="a5d38-140">Workbooks</span><span class="sxs-lookup"><span data-stu-id="a5d38-140">Workbooks</span></span>](app-insights-usage-workbooks.md)
