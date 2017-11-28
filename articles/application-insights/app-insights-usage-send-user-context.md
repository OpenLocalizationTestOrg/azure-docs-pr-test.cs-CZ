---
title: "využití tooenable kontext uživatele aaaSending vyskytne ve službě Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: 0e6c2348f53a3ea970060334179b0dd070925e82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
#  <a name="sending-user-context-tooenable-usage-experiences-in-azure-application-insights"></a><span data-ttu-id="4b724-103">Vyskytne odesílání využití tooenable kontext uživatele ve službě Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="4b724-103">Sending user context tooenable usage experiences in Azure Application Insights</span></span>

## <a name="tracking-users"></a><span data-ttu-id="4b724-104">Sledování uživatelů</span><span class="sxs-lookup"><span data-stu-id="4b724-104">Tracking users</span></span>

<span data-ttu-id="4b724-105">Application Insights umožňuje vám toomonitor a sledovat uživatele provede sadu nástrojů využití produktu:</span><span class="sxs-lookup"><span data-stu-id="4b724-105">Application Insights enables you toomonitor and track your users through a set of product usage tools:</span></span> 
* [<span data-ttu-id="4b724-106">Uživatelé, relace, události</span><span class="sxs-lookup"><span data-stu-id="4b724-106">Users, Sessions, Events</span></span>](https://docs.microsoft.com/azure/application-insights/app-insights-usage-segmentation)
* [<span data-ttu-id="4b724-107">Trychtýře</span><span class="sxs-lookup"><span data-stu-id="4b724-107">Funnels</span></span>](https://docs.microsoft.com/azure/application-insights/usage-funnels)
* [<span data-ttu-id="4b724-108">Uchování</span><span class="sxs-lookup"><span data-stu-id="4b724-108">Retention</span></span>](https://docs.microsoft.com/azure/application-insights/app-insights-usage-retention)
* <span data-ttu-id="4b724-109">Kohorty</span><span class="sxs-lookup"><span data-stu-id="4b724-109">Cohorts</span></span>
* [<span data-ttu-id="4b724-110">Workbooks</span><span class="sxs-lookup"><span data-stu-id="4b724-110">Workbooks</span></span>](https://docs.microsoft.com/azure/application-insights/app-insights-usage-workbooks)

<span data-ttu-id="4b724-111">V pořadí tootrack, co uživatel provede v čase Application Insights pro jednotlivé uživatele nebo relace potřebuje ID.</span><span class="sxs-lookup"><span data-stu-id="4b724-111">In order tootrack what a user does over time, Application Insights needs an ID for each user or session.</span></span> <span data-ttu-id="4b724-112">Zahrňte tyto ID každé vlastní zobrazení události nebo stránky.</span><span class="sxs-lookup"><span data-stu-id="4b724-112">Include these IDs in every custom event or page view.</span></span>
- <span data-ttu-id="4b724-113">Uživatelé, nálevky, uchovávání a kohorty: zahrnují ID uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b724-113">Users, Funnels, Retention, and Cohorts: Include user ID.</span></span>
- <span data-ttu-id="4b724-114">Relace: Zahrnují ID relace.</span><span class="sxs-lookup"><span data-stu-id="4b724-114">Sessions: Include session ID.</span></span>

<span data-ttu-id="4b724-115">Pokud vaše aplikace je integrována hello [JavaScript SDK](https://docs.microsoft.com/azure/application-insights/app-insights-javascript#set-up-application-insights-for-your-web-page), automaticky sledován ID uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b724-115">If your app is integrated with hello [JavaScript SDK](https://docs.microsoft.com/azure/application-insights/app-insights-javascript#set-up-application-insights-for-your-web-page), user ID is tracked automatically.</span></span>

## <a name="choosing-user-ids"></a><span data-ttu-id="4b724-116">Výběr ID uživatele</span><span class="sxs-lookup"><span data-stu-id="4b724-116">Choosing user IDs</span></span>

<span data-ttu-id="4b724-117">ID uživatele by měla zachovávají tootrack relace uživatele chování uživatelů v průběhu času.</span><span class="sxs-lookup"><span data-stu-id="4b724-117">User IDs should persist across user sessions tootrack how users behave over time.</span></span> <span data-ttu-id="4b724-118">Existují různé přístupy pro zachování ID hello.</span><span class="sxs-lookup"><span data-stu-id="4b724-118">There are various approaches for persisting hello ID.</span></span>
- <span data-ttu-id="4b724-119">Definice uživatele, který už máte ve službě.</span><span class="sxs-lookup"><span data-stu-id="4b724-119">A definition of a user that you already have in your service.</span></span>
- <span data-ttu-id="4b724-120">Služba hello má přístup tooa prohlížeče, pak může předat soubor cookie s ID hello prohlížeče v ní.</span><span class="sxs-lookup"><span data-stu-id="4b724-120">If hello service has access tooa browser, it can pass hello browser a cookie with an ID in it.</span></span> <span data-ttu-id="4b724-121">Hello ID zachová pro soubor cookie hello zůstává v prohlížeči hello uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b724-121">hello ID will persist for as long as hello cookie remains in hello user's browser.</span></span>
- <span data-ttu-id="4b724-122">V případě potřeby můžete použít nové ID každé relaci, ale výsledky hello o uživatelích, bude omezena.</span><span class="sxs-lookup"><span data-stu-id="4b724-122">If necessary, you can use a new ID each session, but hello results about users will be limited.</span></span> <span data-ttu-id="4b724-123">Například nebudete moct toosee jak v průběhu času mění chování uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b724-123">For example, you won't be able toosee how a user's behavior changes over time.</span></span>

<span data-ttu-id="4b724-124">Hello ID musí být identifikátor Guid nebo jiný řetězec dostatečně komplexní tooidentify každého uživatele jedinečné.</span><span class="sxs-lookup"><span data-stu-id="4b724-124">hello ID should be a Guid or another string complex enough tooidentify each user uniquely.</span></span> <span data-ttu-id="4b724-125">Například to může být dlouho náhodné číslo.</span><span class="sxs-lookup"><span data-stu-id="4b724-125">For example, it could be a long random number.</span></span>

<span data-ttu-id="4b724-126">Pokud hello ID obsahuje osobní identifikační informace o uživateli hello, není odpovídající hodnotu toosend tooApplication Insights jako ID uživatele.</span><span class="sxs-lookup"><span data-stu-id="4b724-126">If hello ID contains personally identifying information about hello user, it is not an appropriate value toosend tooApplication Insights as a user ID.</span></span> <span data-ttu-id="4b724-127">Můžete odeslat toto ID jako [ověřit ID uživatele](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users), ale nesplňuje požadavek na ID uživatele hello pro scénáře použití.</span><span class="sxs-lookup"><span data-stu-id="4b724-127">You can send such an ID as an [authenticated user ID](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users), but it does not fulfill hello user ID requirement for usage scenarios.</span></span>

## <a name="aspnet-apps-set-user-context-in-an-itelemetryinitializer"></a><span data-ttu-id="4b724-128">Aplikace ASP.NET: Nastavit kontext uživatele v ITelemetryInitializer</span><span class="sxs-lookup"><span data-stu-id="4b724-128">ASP.NET Apps: Set user context in an ITelemetryInitializer</span></span>

<span data-ttu-id="4b724-129">Vytvořte inicializátoru telemetrie, jak je podrobně popsaná v [zde](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer)a nastavte hello Context.User.Id a hello Context.Session.Id.</span><span class="sxs-lookup"><span data-stu-id="4b724-129">Create a telemetry initializer, as described in detail [here](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer), and set hello Context.User.Id and hello Context.Session.Id.</span></span>

<span data-ttu-id="4b724-130">Tento příklad nastaví hello uživatelské ID tooan identifikátor, který vyprší po hello relaci.</span><span class="sxs-lookup"><span data-stu-id="4b724-130">This example sets hello user ID tooan identifier that expires after hello session.</span></span> <span data-ttu-id="4b724-131">Pokud je to možné použijte ID uživatele, která je uchována napříč relacemi.</span><span class="sxs-lookup"><span data-stu-id="4b724-131">If possible, use a user ID that persists across sessions.</span></span>

<span data-ttu-id="4b724-132">*C#*</span><span class="sxs-lookup"><span data-stu-id="4b724-132">*C#*</span></span>

```C#

    using System;
    using System.Web;
    using Microsoft.ApplicationInsights.Channel;
    using Microsoft.ApplicationInsights.Extensibility;

    namespace MvcWebRole.Telemetry
    {
      /*
       * Custom TelemetryInitializer that sets hello user ID.
       *
       */
      public class MyTelemetryInitializer : ITelemetryInitializer
      {
        public void Initialize(ITelemetry telemetry)
        {
            // For a full experience, track each user across sessions. For an incomplete view of user 
            // behavior within a session, store user ID on hello HttpContext Session.
            // Set hello user ID if we haven't done so yet.
            if (HttpContext.Current.Session["UserId"] == null)
            {
                HttpContext.Current.Session["UserId"] = Guid.NewGuid();
            }

            // Set hello user id on hello Application Insights telemetry item.
            telemetry.Context.User.Id = (string)HttpContext.Current.Session["UserId"];

            // Set hello session id on hello Application Insights telemetry item.
            telemetry.Context.Session.Id = HttpContext.Current.Session.SessionID;
        }
      }
    }
```

## <a name="next-steps"></a><span data-ttu-id="4b724-133">Další kroky</span><span class="sxs-lookup"><span data-stu-id="4b724-133">Next steps</span></span>
- <span data-ttu-id="4b724-134">využití tooenable vyskytne, zahájit odesílání [vlastních událostí](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) nebo [stránky zobrazení](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span><span class="sxs-lookup"><span data-stu-id="4b724-134">tooenable usage experiences, start sending [custom events](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) or [page views](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span></span>
- <span data-ttu-id="4b724-135">Pokud už odesílat vlastní události nebo zobrazení stránky, prozkoumejte hello využití nástroje toolearn jak uživatelé používat služby.</span><span class="sxs-lookup"><span data-stu-id="4b724-135">If you already send custom events or page views, explore hello Usage tools toolearn how users use your service.</span></span>
    * [<span data-ttu-id="4b724-136">Přehled využití</span><span class="sxs-lookup"><span data-stu-id="4b724-136">Usage overview</span></span>](app-insights-usage-overview.md)
    * [<span data-ttu-id="4b724-137">Uživatelů, relací a události</span><span class="sxs-lookup"><span data-stu-id="4b724-137">Users, Sessions, and Events</span></span>](app-insights-usage-segmentation.md)
    * [<span data-ttu-id="4b724-138">Trychtýře</span><span class="sxs-lookup"><span data-stu-id="4b724-138">Funnels</span></span>](usage-funnels.md)
    * [<span data-ttu-id="4b724-139">Uchování</span><span class="sxs-lookup"><span data-stu-id="4b724-139">Retention</span></span>](app-insights-usage-retention.md)
    * [<span data-ttu-id="4b724-140">Workbooks</span><span class="sxs-lookup"><span data-stu-id="4b724-140">Workbooks</span></span>](app-insights-usage-workbooks.md)
